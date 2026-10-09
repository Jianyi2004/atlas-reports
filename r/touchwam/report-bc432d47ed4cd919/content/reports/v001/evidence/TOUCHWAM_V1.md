# TouchWAM V1：Bench2Dex 三任务触觉适配

## 实现范围

这是一个可选择的模型架构版本。保留 Wan VAE、文本/状态条件、视频与动作双流、
30 层共享注意力、原 flow-matching 目标和 Alpha 80 维动作表示。

新增约 **2,800,806 个可训练参数**（默认 Alpha 配置）：

- 每个指尖共享小 CNN，2 层带时间因果 mask 的 Transformer；带左右手、手指和时间 ID。
- 4 帧历史含当前帧，10 指尖，共 40 个 256 维 token；投影到动作条件的 1024 维空间。
- 动作层 `[4, 9, 14, 19, 24, 29]`（从 0 编号）加入并行触觉条件注意力。
- 复用原 Q、输出投影、K/V 和 Q/K 归一化；只在触觉 K/V 上加 rank=16 LoRA 增量。
- 每层 `tanh(g)` 门控初始化为 0，原条件注意力与触觉注意力分别做 softmax，并使用
  同一份进入条件注意力之前的动作特征。残差相加后再进入原 FFN。

```
h' = h + CA_original(norm(h), C) + tanh(g) * CA_tactile(norm(h), Z_touch)
h_next = original_FFN_and_residual(h')
```

默认 `adapter_only` 模式中，原参数只冻结 `requires_grad`，下游计算仍保留梯度图。触觉可以通过原联合注意力间接
影响未来的视频流，因此保留视频与动作两项 loss。第一步只有 gate 能获得非零梯度；
gate 打开后 encoder、projection 和 LoRA 开始学习，这是零门控初始化的预期行为。

没有新增触觉 VAE、第三条大型 DiT、未来触觉生成或接触预测辅助 loss。
架构可运行不代表成功率已经超过 OpenWAM，仍需配对的 50 次/任务仿真评测。

![TouchWAM V1](docs/touchwam/architecture_v1.png)

## 输入、输出和数据处理

| 字段 | 单条样本形状 | 说明 |
| --- | --- | --- |
| RGB | 9 张 384×320 图 | 与原三任务训练一致的三视角拼图；原始 33 帧 stride 4 |
| prompt | 文本 | 使用对应任务指令 |
| proprio | `[1,80]` | 原 Alpha 状态表示和共享归一化 |
| action target / output | `[32,80]` | 双臂 EEF 位置/旋转 + 手指关节；58 个有效维，其余保留占位 |
| tactile_maps | `[4,10,2,32,32]` | 从旧到新 `t-3,t-2,t-1,t`；通道 0 距离米，通道 1 接触 mask |
| tactile_valid | `[4,10]` bool | 独立标记传感器/历史是否存在；True 且零接触仍是有效信息 |

batch 时在上述形状前增加 `B`。推理生成动作 chunk，亦可按原设置解码预测视频。
V1 不输出触觉预测。

指尖顺序固定为右手 thumb/index/middle/ring/pinky，然后左手对应五指。
读取 `robot/tactile/distance_along_normal_m/<site>` 与 `contact_mask/<site>`，不把
量化的 `tacmap` 灰度当作力。240×240 → 32×32 使用“接触像素距离均值 + 最大池化接触
mask”，保留稀疏接触；编码器以 0.015 m 缩放距离到 [-1,1]，与记录传感器量程一致。
距离是射线几何量，不能解释为标定接触力。

窗口沿用 Alpha manifest 中按无效帧、时间断点、回零边界切开的 segments；历史不会跨
segment 或读取未来。缺少的历史填零并置 valid=False。非有限值的指尖帧整体置无效。

三任务采用同一份 manifest、训练/验证分组、训练集统计和等任务采样：

| 任务 | 训练轨迹 | 验证轨迹 | 总计 |
| --- | ---: | ---: | ---: |
| 26 canned food tray | 90 | 10 | 100 |
| 32 baking tray prep | 28 | 4 | 32 |
| 73 full jigsaw | 76 | 8 | 84 |
| 合计 | 194 | 22 | 216 |

这些是当前筛选后数据集的数量，不是论文每个任务的全部下载数量。
已检查全部 216 条记录的触觉字段形状与量程，覆盖 159,290 个原始帧；未逐帧做物理
触觉质量审计。

## 数据准备

在 OpenWAM 仓库根目录运行；全部是 CPU 操作。

```bash
# 检查所有源文件的字段、长度和量程
OMP_NUM_THREADS=1 .venv/bin/python scripts/prepare_bench2dex_tactile_v1.py --audit-only

# 可选：预处理全部触觉地图，避免训练时重复解压 240x240 原始数据
OMP_NUM_THREADS=1 .venv/bin/python scripts/prepare_bench2dex_tactile_v1.py --workers 8
```

不生成缓存也可以训练，默认 reader 直接读取原 HDF5。长期训练建议先生成缓存；
全部地图约 12.2 GiB（float32），另有很小的有效性 mask，不复制 RGB。
支持按完整 episode 续跑，缓存 manifest 绑定源 manifest 的 SHA256、源文件大小/修改
时间和输入 schema。`--limit-episodes 1` 可试转换，但部分缓存不能用于完整三任务训练。
`--workers` 按轨迹并行转换，每个 CPU 进程一个计算线程；只有主进程更新缓存 manifest，
并用目录锁避免重复启动两个转换程序。转换前后校验源文件大小和修改时间。
此操作不需要停止现有 GPU 训练。

## 启动第一阶段训练

选定一个**固定的三任务无触觉 OpenWAM checkpoint 文件**。先为比较组保留相同起点；
如果文件处于滚动清理目录，实验归档时应复制/硬链接权重，并携带同目录配置、统计和
tokenizer。脚本要求具体 `.safetensors` 文件，不会自动选择变化中的“最新”。

```bash
bash scripts/run_bench2dex_touchwam_v1.sh \
  /absolute/path/to/fixed/checkpoint_step_N.safetensors

# 已完成上面的完整缓存时增加：
bash scripts/run_bench2dex_touchwam_v1.sh \
  /absolute/path/to/fixed/checkpoint_step_N.safetensors \
  dataloader.tactile_cache_root=assets/benchmark_data/bench2dex_sharpa3_tactile_v1
```

预设为 8 卡、每卡 batch 16、累积 2（global batch 256）、BF16/ZeRO2、梯度检查点；
冻结原参数，只训练新模块，LR=1e-4、cosine、5% warmup。
训练上限 6000 **microsteps**，即 3000 次参数更新；每 1000 microsteps 保存，最多保留
最近 10 份完整 checkpoint；正常结束还保存最后一步。这是首轮实验预算，尚未通过
验证集或成功率调优，也不是论文给出的触觉模型最佳步数。

输出位于 `outputs/bench2dex_sharpa3_touchwam_v1/<timestamp>/`。
上述脚本在前台输出日志，若要后台运行可用 tmux 并自行 `tee` 保存输出。
脚本只启动新训练，不自动停止现有训练。

### 三任务首轮运行配置（2026-10-04）

`scripts/run_bench2dex_touchwam_v1_sharpa3.sh` 固定首轮实验的启动参数：

- 基础权重：`outputs/bench2dex_sharpa3_alpha_step13000_touch_base/checkpoint_step_13000.safetensors`。
- 基础权重 SHA256：`aea1783084fac18513838b5700ef7762d9e4adf1f9ba5a46a979520f1adcc26c`。
- 数据：全部 216 条触觉缓存；194 train、22 val；沿用原 Alpha 动作、分组及统计。
- 固定随机种子 42，其他超参数沿用上述 6000 microstep / global batch 256 配置。
- `performance_rank_*.jsonl` 每 10 步记录 loss、梯度范数、显存、六个触觉 gate 及
  K/V LoRA up 权重范数，可检查零门控是否打开、新参数是否发生更新。

基础权重用硬链接另存并携带配置、归一化、tokenizer 和机器人几何，防止旧训练滚动
清理影响触觉模型的起点。无触觉基线分支、checkpoint 与正在运行的 4090 测评保留。

```bash
bash scripts/run_bench2dex_touchwam_v1_sharpa3.sh
```

需要衔接已有八卡训练时，通过 `training.handoff_ready_dir=<全新目录>` 先加载 CPU
权重。仅当缓存和检查通过、8 个 rank 均写出 ready 文件后，才停止已核实的旧训练
launcher 和 workers，随后写入该目录的 `release` 文件。不能复用旧 gate 目录。
当前挂载信息、实时日志路径和切换记录保存在输出根目录的 `active_run.json`。

从已训练 V1 checkpoint 新一轮 finetune：

```bash
bash scripts/run_bench2dex_touchwam_v1.sh \
  /absolute/path/to/touchwam/checkpoint_step_N.safetensors \
  training.initialize_touch_from_openwam=false
```

这是权重热启动，不恢复优化器。精确 resume 需完整 accelerate state，见版本管理文档。

## 从官方 Alpha 开始联合微调（2026-10-05）

独立预设 `train_touchwam_v1_from_alpha` 从官方
`/data_all/share/checkpoints/OpenWAM/OpenWAM-Alpha-Pretrain-Foundation-Model/checkpoint_step_154000.safetensors`
初始化全部原版参数，触觉模块新初始化，gate 为零；Bench2Dex step 从 0 计数，
优化器重新创建，`resume_ckpt_path=null`。
官方源权重 SHA256：`180a02653118b0f96da28a9cae9ec7b4c1c1e6cd0e4608b56c7b4884f8001d3d`。

```bash
bash scripts/run_bench2dex_touchwam_v1_from_alpha.sh
```

此预设使用 `model.architecture.training_mode=joint_finetune`：视频 DiT、动作 DiT、
状态投影和触觉模块共同训练；VAE、文本等预处理编码器继续冻结。只训练触觉模块的旧
方案依赖已适配 Bench2Dex 的主干，不适合作为本轮从通用权重开始的对照。

训练数据与无触觉基线完全相同：216 条轨迹（194 train / 22 val），同一 split、
归一化、等任务采样、seed 42、三视角 RGB 和 EEF + 手指关节动作。额外读取现有触觉缓存。
沿用 `run_bench2dex_sharpa3.sh` 的主干微调预算及超参数：8 卡、每卡 batch 16、累积 2、
global batch 256，统一 LR=1e-5、constant schedule、BF16/ZeRO2、梯度检查点，
18000 microsteps（9000 optimizer updates），每 1000 microsteps 保存、保留最近 10 份。
不另存完整优化器状态；输出目录 `outputs/bench2dex_sharpa3_touchwam_v1_from_alpha/<timestamp>/`。

模式改变的是可训练参数集合，不改变 V1 的 forward、权重 key 或输入 schema；旧配置
未设置 `training_mode` 时默认 `adapter_only`，旧 V1 checkpoint 仍严格兼容。
模式保存在运行 `config.yaml` 和 performance 日志中。原版 OpenWAM、旧触觉实验及结果保留。
启动记录与检查结果见新输出根目录 `active_run.json`、`preflight.json`。

比较时应选**相同 Bench2Dex microstep/更新次数**（例如双方 step11000）并使用相同
测评场景/种子；官方文件名的 step154000 是预训练计数，不计入本轮微调预算。
先前旧触觉版的 6000-step 结果不能替代本轮实验结果。

## 推理接口

模型选择由 checkpoint 的 config 驱动，现有 deploy loader 会构建 V1 并严格加载。
观察字典新增 `tactile_maps` 和 `tactile_valid`，经 server/policy/engine 送入 `generate()`。
V1 一次观察只编码触觉一次，所有去噪步复用 token；新观察会重新编码，不跨 episode
复用触觉。server 握手的 `tactile` 字段提供 schema、顺序、历史长度等。

4090 仿真客户端需要在每个 **20 Hz 观察帧**采集触觉，并使用
`openwam.tactile.sharpa_v1.SharpaTactileHistoryV1` 累积历史，episode reset 时 `reset()`。
执行动作 chunk 期间也必须更新历史，不能把四次重新规划误当作四帧 20 Hz 触觉。
`append(timestamp_s, distance_by_site, contact_by_site)` 返回上述两个字段，numpy 数组
通过 JSON 发送时调用 `.tolist()`。未提供触觉时 V1 显式走原模型路径，方便消融。

Bench2Dex 测评客户端已接入实时 Sharpa sensor：`run_policy.py` 在每个 20 Hz 观察
时刻采集原生 240×240 数据，`benchmarks/bench2dex/policy.py` 使用上述共享预处理器
构建历史并发送。模型握手决定是否需要触觉；TouchWAM 正式测评缺少传感器数据时
直接报错，不允许静默退回无触觉。每回合记录有效传感器帧数、接触帧数、完整历史
帧数；批量测评检查其与实际动作步数一致。

`launch_bench2dex_sharpa3_eval.py` 对 TouchWAM 先执行三个任务各 32 步的真实模型
检查，确认触觉有效及历史连续后，按每任务 10 回合轮流执行完整的 50 回合计划。
轮流执行不改变场景、随机种子、动作时限或成功判据；中断后继续使用相同参数和
输出目录即可跳过已完成回合。`none` 模式的 150 回合结果不是论文四种扰动条件
的汇总成功率。每个 `summary.json` 明确包含目标数、完成数和完成标记。

本轮部署与定时关机的记录位于工作区 `setup/touchwam_eval_20261005/plan.json`，
4090 的后台任务位于 `outputs/touchwam_eval_20261005_job/`；结果和 MP4 持续回传到
A100 的 `outputs/4090_results/eval_touchwam_v1_step6000_none50_20261005/`。
实验比较时应确认输入 valid 和接触统计，保证相同 checkpoint 起点、数据 split、
额外更新次数及 replanning 频率。建议增加同预算无触觉继续微调组和 tactile-shuffle 组。

## 核心文件与验证

- `openwam/model/architectures/touchwam/v1.py`：版本注册、输入、冻结、权重迁移/校验。
- `openwam/model/architectures/touchwam/modules_v1.py`：编码器、适配器和动作扩展。
- `openwam/tactile/sharpa_v1.py`：离线/在线一致的输入预处理和历史缓冲。
- `openwam/dataloader/bench2dex_tactile.py`：因果历史读取与原三任务混合复用。
- `configs/train_touchwam_v1.yaml`：训练预设；模型结构参数位于 `configs/model/touchwam_v1.yaml`。
- `tests/test_touchwam_v1.py`、`test_touchwam_migration.py`、`test_bench2dex_tactile.py`：
  零门控数值一致性、梯度穿过冻结参数、检查点重计算、严格迁移、缺失/无接触、因果性、
  缓存一致性、真实三任务样本及推理接口测试。

测试使用 CPU 小模型（真实 ActionDiT/MoT，mock 视频 backbone）；另外读取实际训练
checkpoint 的 tensor header 验证 826 个动作/状态参数名与形状完全匹配。
CPU 测试不能替代完整 5B 模型 GPU 训练或仿真成功率测试。
