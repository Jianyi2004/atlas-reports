# OpenWAM Alpha：Bench2Dex 三个 IIWA7 + Sharpa 完整任务

## 本轮数据（2026-10-03）

| 任务 | 原始回放 | 离线终态核验通过 | 训练 / 验证轨迹 | 训练 / 验证同源组 |
|---|---:|---:|---:|---:|
| 26 Canned Food Tray Line Arrangement（罐头摆盘） | 100 | 100 | 90 / 10 | 45 / 5 |
| 32 Baking Tray Prep with Tools（烤盘准备） | 100 | 32 | 28 / 4 | 14 / 2 |
| 73 Jigsaw Puzzle Assembly（完整拼图，四块活动拼图） | 100 | 84 | 76 / 8 | 38 / 4 |
| 总计 | 300 | 216 | 194 / 22 | 97 / 11 |

使用 `teleopdata/dataset/<task>/replay-generalization`。没有混入 task86 简化拼图。
同编号 `episode_000000` 与 `episode_000000_1` 归同一组，防止划分泄漏；
“216 条文件”不表示 216 个独立初始化。每个任务按同源组约 90%/10% 划分，seed=42。

### 为什么不能直接筛选旧成功标记

这 300 条的 `collection_mode=scripted_hold`、`demo_eligible=false`。
当前采集器对此模式直接写入 false。旧 success 标记为 task26 0/100、task32 96/100、
task73 0/100，与现有场景/成功检查代码不一致；例如旧拼图指令和当前四块组装目标不同。

`scripts/audit_bench2dex_sharpa.py` 直接调用当前 Bench2Dex YAML 的
`metrics.terminal.raw_condition` 及其原有成功检查器，未修改容差或成功条件。
用记录的物体位置、姿态、线速度、角速度，在有效且连续的 20Hz 帧上检查目标持续至少
0.5s，并排除 homing。时间跨度为 0.5s，即至少 11 个连续记录点。

这是保守的离线演示筛选，不是仿真成功率：不能还原两个记录点之间的 60Hz 物理状态，
也不能把未通过审计的轨迹全部称为真实失败（部分停留时间不足或采样边界不完整）。
没有据此重写原始 success/demo_eligible 标记。审计记录源文件大小、mtime、场景与检查器
SHA256，转换前检查一致性，并把原标记和核验证据一起保存在 manifest 中。

没有按旧安全标记过滤：保留的 task26/32/73 中分别有 2/8/2 条
`safety_hard_violation=true`。本轮采用任务终态完成作为模仿学习的筛选标准，
后续评测仍须独立报告任务成功率和安全指标。

### 坐标与模型数据

继承 task86 已验证的 Alpha 转换：

- measured qpos → URDF FK → state；commanded joints → FK → action。
- 每侧：末端 xyz（米）+ rot6d（旋转矩阵前两列）+ Sharpa 20 个手指关节（弧度）。
- 双侧 raw58，使用共同的机器人 `link_0` 坐标系，末端为 `left_hand_C_MC`、
  `multi_right_hand_C_MC`；映射到 Alpha80 的 `0-8,10-29,34-42,44-63`，其余位置 mask。
- 20Hz 的连续 33 帧窗口，32 步动作、首帧状态；RGB 每 4 帧取一张，共 9 张。
- 正面、左腕、右腕三视角组合成 384×320；语言指令分别来自三个当前场景 YAML。
- 排除无效、非有限值、回零帧，窗口不跨越时间断点。
- 三个任务共享一份只在训练划分上拟合的 `eef/eef_state` 统计。rot6d 不做经验范围缩放。
- 数值 NPZ 与审计文件约 67MiB；RGB 继续原位读取 HDF5，需保留原始数据目录。

task32 的 replay 相机定义仍记录旧安装角度，但逐帧外参与任务26/73使用的回放安装角度一致。
只在独立 FK 审计中显式采用左 `[-0.1571,0.314,-1.571]`、
右 `[-0.1571,-0.314,-1.571]`，并保存原定义与实际审计定义。
每条接收轨迹仍要求最大位置误差 <1e-4m、最大角度误差 <1e-3rad；不移动 RGB 或动作时间索引。

### 采样

真实训练窗口数分别为 54,730 / 16,238 / 60,834。
每个虚拟 epoch 各任务取 60,834 个窗口，共 182,502，三任务各 1/3。
每个任务先打乱遍历完整窗口再补采样，每个 epoch 重新打乱，避免永远遗漏长任务后半段。
验证集不重复采样，训练器当前不自动做仿真验证。

## 训练配置

从 `OpenWAM-Alpha-Pretrain-Foundation-Model/checkpoint_step_154000.safetensors`
初始化一个共享模型；没有从 task86 专项模型继续微调。

| 参数 | 值 |
|---|---|
| GPU | 0–7，8×A100 80GB |
| 每卡 batch / 累积 | 16 / 2 |
| 全局有效 batch | 256 |
| 学习率 | 1e-5，constant |
| 精度 / 分片 | BF16 / DeepSpeed ZeRO2 |
| 最大 microstep | 18,000（约 9,000 次优化器更新） |
| 保存 | 每 1,000 microstep，最近 10 份，另有结束时保存 |
| 保存内容 | 权重，每份约 24.8GB；不保存约百GB的 optimizer 全状态 |

18,000 是三任务首轮实验预算，不是论文承诺的收敛点。参考已有约6–7s/microstep，
总墙钟粗估30–35小时，加上保存与启动开销。以新运行实际速度为准。

```bash
cd /data_all/intern10/workspace/TouchWAM/OpenWAM
# GPU 空闲时新开训练；当前已有运行时请勿重复执行
bash scripts/run_bench2dex_sharpa3.sh

# 显式修改预算
bash scripts/run_bench2dex_sharpa3.sh training.max_steps=24000

# 需要继续时从指定完整权重 warm-start：优化器与计数会重置
bash scripts/run_bench2dex_sharpa3.sh \
  training.finetune_ckpt_path=/absolute/run/checkpoint_step_10000.safetensors \
  training.max_steps=6000
```

配置：`configs/dataloader/bench2dex_sharpa3_alpha.yaml`；运行元数据、PID、日志、实际输出目录
见 `outputs/bench2dex_sharpa3_alpha/active_run.json`。第一次换任务使用
`training.handoff_ready_dir` 让新8进程先在CPU加载完成；全部 ready、旧完整checkpoint可读后才
停止旧launcher，旧worker退出后立即 release。该开关默认关闭，普通启动无需它。

重新转换需指定新的输出目录（拒绝覆盖已冻结数据）：

```bash
.venv/bin/python scripts/audit_bench2dex_sharpa.py --output /absolute/new_source_audit.json
.venv/bin/python scripts/prepare_bench2dex_sharpa_mix.py \
  --audit /absolute/new_source_audit.json --output-dir /absolute/new_conversion
```

## 后续与论文比较

本轮训练覆盖论文中该本体的三个完整任务。后续在4090分别测 task26、32、73，
每个任务每个 profile 50 回合；None / Equi. / Inv. / Full 共4种配置，即每任务200回合、
三任务共600回合，才对应表中四配置汇总SR。仅 None 的150回合可做初步比较。

正式测评要固定权重、按任务切换语言指令/场景、使用本轮共享归一化统计，并核对冻结的
成功检查器版本、anchor选择、随机种子、控制预算和成功持续时间。不沿用 task86 的任务号或
600控制步上限作为三个长任务的默认协议。仍需给出训练中见过/未见过场景的分项。
当前训练数据经过筛选且保留验证划分，与论文训练数据规模不完全相同，比较时应同时披露。

## 验证

```bash
.venv/bin/python -m pytest -q tests/test_bench2dex_alpha.py \
  tests/test_bench2dex_sharpa_mix.py tests/test_training_handoff.py
```

转换/混合采样7项、CPU切换门控2项测试已通过；Hydra启动配置解析通过。
数据检查覆盖共享统计、同源组隔离、每任务完整窗口覆盖、epoch重洗、三种语言指令、
实际RGB读取、动作/状态形状、有效槽位与数值有限性。

已启动运行：`outputs/bench2dex_sharpa3_alpha/2026-10-03_17-47-21`，
launcher PID `2096595`。8个rank实际完成前向/反向和优化器更新，loss/梯度有限，
无OOM，每卡显存约58GB。详见运行目录 `launch_verification.json`。
旧task86在microstep3374结束，最新完整 `checkpoint_step_3000.safetensors`
保留在 `outputs/bench2dex_task86_alpha_continue6000/2026-10-03_11-29-35`；
该3000是从原step6000权重启动后重新计数。3374时尚未写出的更新没有保存。
