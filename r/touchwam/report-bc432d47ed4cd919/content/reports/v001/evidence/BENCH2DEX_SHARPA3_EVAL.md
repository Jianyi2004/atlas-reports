# 三个完整 Sharpa 任务的测评

支持 task26 罐头摆盘、task32 烤盘准备、task73 完整四块拼图。
单个三任务模型根据原始任务指令选择行为，保留同一套 EEF/手指动作及归一化。

## 本次固定模型

- 训练目录：`outputs/bench2dex_sharpa3_alpha/2026-10-03_17-47-21`
- 导出：`outputs/bench2dex_sharpa3_alpha_step11000_export`
- 权重：`checkpoint_step_11000.safetensors`，24,813,767,464 bytes。
- SHA256：`e6c774b0e747dc3447c5a4e170795edc5393c62080b06632b140f6db10958298`
- 这是无触觉的三任务 OpenWAM；TouchWAM V1 尚未训练。

导出使用硬链接固定权重，训练仍可继续保存/轮换其他 checkpoint。
配置、训练统计、tokenizer、URDF、主动关节映射和任务指令一并导出。

## 4090 拉取

用户指定的大文件传输方向是 **4090 主动拉取 A100**。
从 A100 仅 SSH 登录 4090 执行控制命令；在 4090 上执行：

```bash
scp -p -r -o BatchMode=yes -o ClearAllForwardings=yes \
  ys:/data_all/intern10/workspace/TouchWAM/OpenWAM/outputs/bench2dex_sharpa3_alpha_step11000_export \
  /home/zjy/workspace/TouchWAM/OpenWAM/outputs/
```

4090 的 `ys` 是 `10.249.43.100` 直连，没有 Azure ProxyJump；24.8 GB 模型不通过
A100→Azure→4090 的 SSH 控制链路传输。实际传输先写入独立 staging 目录，SHA256 和
大小校验通过才发布最终目录。不要在传输未完成时启动模型。

## 协议

每个任务 50 个不同初始化，严格使用原始记录 main 0–24 + `_1` 25–49。
这些源文件是否通过训练数据筛选不影响测评场景选择，避免只测成功演示的选择偏差。
只导出 `scene_generalization_sample` 元数据，不携带专家动作或 RGB。

| 任务 | 控制步数上限 | 原始训练组场景 | 验证组场景 | 未用于训练/验证的组 |
| --- | ---: | ---: | ---: | ---: |
| 26 | 871 | 45 | 5 | 0 |
| 32 | 852 | 14 | 2 | 34 |
| 73 | 1213 | 38 | 4 | 8 |

步数来自官方 `metrics.expert_time_step × 1.5 / 3`，向上取整；控制 20 Hz、物理 60 Hz。
保留原场景成功条件、0.5 s 稳定成功提前结束、任务种子偏移、完整四块拼图判定。
使用训练中出现的初始化会单独标记，整体 SR 不等于独立未见场景 SR。

本轮 `none`：3×50=150 回合。每个任务的成功率可与相同协议的 None 列核对。
论文跨 None/Equi./Inv./Full 的平均 SR 需要四个 profile 全跑，共 600 回合。
仍需确认基线的 checkpoint、动作规划频率等条件才能作模型能力比较。

## 启动与查看

在 A100 准备场景元数据：

```bash
.venv/bin/python scripts/prepare_bench2dex_sharpa3_eval.py \
  --output-dir assets/benchmark_data/bench2dex_sharpa3_eval50
```

在 4090 的 `/home/zjy/workspace/TouchWAM/OpenWAM`：

```bash
CKPT_DIR="$PWD/outputs/bench2dex_sharpa3_alpha_step11000_export" \
  bash scripts/serve_bench2dex.sh

python3 scripts/eval_bench2dex_sharpa3.py \
  --ckpt-dir outputs/bench2dex_sharpa3_alpha_step11000_export \
  --anchor-root assets/benchmark_data/bench2dex_sharpa3_eval50 \
  --output-dir outputs/eval_sharpa3_step11000_none50
```

需要完整四 profile 时，在新输出目录启动，增加：
`--profiles none cov_only inv_only inv_cov`。

结果目录：

- 根目录 `summary.json`：三任务完成数、成功数、分项 SR，完成后给 macro SR。
- `<task>/<profile>/summary.json`：当前进度、Wilson 95% 区间和各数据组分项。
- `<task>/<profile>/per_episode.jsonl`：每回合原始结果。
- `<task>/<profile>/episodes/0001/attempt_01/`：仿真日志、录像、IK 诊断。
- `plan.json`：固定模型 SHA256、任务和协议；不同参数不能混入已有结果。

模型服务握手会报告实际加载文件名、大小及已校验的导出 SHA256；客户端与导出目录
一致才允许动作查询，避免把旧 task86 服务误当作新模型。任务失败计入分母且不重跑；
仅无有效结果的基础设施异常尝试两次。一个任务基础设施失败会记录错误并继续其他任务，
不会把缺失回合伪装成成功或完整结果。

模型保持 10 次去噪、32 步 chunk、执行 16 步重新规划，与之前 task86 的模型设置一致。
每回合新仿真进程，模型服务复用；A100 训练不需要暂停。

## 原始关节命令与物理限位的适配修复

2026-10-04 首次实际推理发现，训练的 EEF 标签来自 `FK(action/commanded)`，
而部分原始肘关节命令超出物理限位。task32 训练数据有 11016/17134 个有效帧
包含越限机械臂命令；仿真中的实际关节仍停在物理限位。直接用物理限位约束 IK
无法反解这些 EEF 标签，旧适配器会保持原位，不能把这种故障归因于模型。

`scripts/prepare_bench2dex_ik_limits.py` 仅从训练组有效帧提取原始命令范围。
导出元数据中的 `arm_command_limits` 显式启用修复：先尝试原物理边界 IK；失败时
在训练命令范围内反解，成功后将输出关节目标裁剪回原 URDF 物理限位。
只有左/右 A4 的反解下界扩展为 -2.701858/-2.593850 rad；仿真输出下界仍为
-2.094395 rad。任意不可解目标仍保持原位。未声明该字段的旧模型保持原行为。

每回合记录 `ik_projected_arm_targets` 和 `ik_failed_arm_targets`，测评计划固定
此范围及 `ik_adapter=reconstruct_train_commands_then_clip_v1`，避免混合适配器结果。
抽查三个任务各 24 个专家机械臂目标：旧 IK 失败 1/11/1 个，修复后均为 0，
所有输出目标在原物理限位内。这是有限的接口验证，不是任务成功率。
逆解存在冗余，裁剪后的轨迹不保证与原专家命令完全一致。

修复前输出 `eval_sharpa3_step11000_none50_20261004_161304` 保留为诊断，
正式结果从新的输出目录重新计数，不复用旧回合。最新目录由
`outputs/eval_sharpa3_active.json` 指向。

后续训练版本应在转换时明确处理原始命令限位，并重新训练/对照；本轮权重和
训练进程不变，修复只在测评命令转换接口生效。

## 播放测评录像

`recordings/{success,failure}/episode_*.hdf5` 保存 JPEG 相机帧，不是直接可播放的 MP4。
在 4090 使用 OpenWAM 环境执行以下导出，可在不中断测评的情况下处理已完成回合：

```bash
python scripts/export_bench2dex_eval_videos.py --run-dir <本次输出目录> --watch
```

MP4 位于 `<本次输出目录>/videos/<task>/<profile>/{success,failure}/episode_XXXXXX.mp4`。
每个视频横向排列 stereo_left、wrist_left、wrist_right 三路观测，采用 H.264；
默认录制帧率为 20/3≈6.67 FPS，播放时长对应仿真时间。原录制只有每路 240×180
像素、JPEG quality=25，导出不会恢复未保存的细节，也不是模型预测的未来视频。
`videos/index.json` 列出录像、成功标记、约束违规标记和导出错误；`--watch` 只读
已完成回合，随着测评继续自动导出新回合，测评结束或退出后停止。编码使用 CPU
两个线程，原始 HDF5 和测评结果保持不变。
