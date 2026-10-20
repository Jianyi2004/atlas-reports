# SharpaDex v1.0：两个新增episode的独立质量复核

固定版本：`c8032fd891e3f6f19b0d4886793579c1dce5eea7`。分别选取 `hammer_nails/season_POC22048_2026_05_20_16_40_10_train` 与 `insert_plug_into_socket/season_POC22007_2026_03_18_13_58_40_train` 的 **v2.1 episode 0**。仅下载两条Parquet及配套小型元数据，并发2；没有下载视频。API的100,000条截断siblings仅用于定位已知文件，未用于推断全库规模。选择为方便抽样，不能据此推断全库质量缺陷率。

两个样例都通过基本数值完整性核验：13个数值字段均无null、NaN或Inf；60D触觉实际存储为float32，全部60维都随时间变化，没有整列恒零或恒定；去掉全零首帧后，每维仍分别至少变化682次和289次。动作与观察并非逐元素复制，65D joint action及12D TCP action都没有任何一行与对应观察完全相等。

| 核验项 | hammer_nails | insert_plug_into_socket |
|---|---:|---:|
| 帧数 | 703 | 335 |
| 帧覆盖时长 N/30 | 23.433秒 | 11.167秒 |
| 首末时间戳跨度 | 23.400秒 | 11.133秒 |
| 60D触觉数值范围（单位未核实） | −8.621至36.954 | −2.125至24.103 |
| joint action/observation平均绝对差 | 0.007323 | 0.032112 |
| TCP action/observation平均绝对差 | 0.006105 | 0.011695 |
| 细粒度语言覆盖 | 638/703，90.75% | 265/335，79.10% |
| 尾部仅全局语言的帧 | 638–702，共65帧 | 265–334，共70帧 |

需要保留的具体细节：

- **触觉首帧都是60维全零**。后续还分别有4对、2对相邻触觉行完全重复；后帧索引为hammer的244、248、437、646，plug的170、288。它们可能涉及初始化、对齐或保持旧值，但现有导出文件不足以判断原因，不能直接称为真实零接触或丢帧。
- 两条的joint torque维58–64、24D TCP state的维6–11和18–23全零，与文档中的占位槽一致。hammer还有torque维14、15、22、23、27全零，action维7–28（左手）及61–62全零；plug的action维14、18全零。任务文字主要描述右手操作，**单轨迹恒零不能直接视作传感器失效**。也不能把文档“58维carry data”写成每条轨迹58维均非零。
- `frame_index`均从0连续递增，`timestamp`与`frame_index/30`最大偏差分别小于0.90和0.45微秒，符合float32舍入。24D TCP state的两个pose片段与12D observed TCP pose逐元素相等；其force/torque槽不应作为真实腕部六轴力使用。

**语言区间在这两条样例中是双端包含 `[start_step, end_step]`**：例如hammer的0–287对应subtask索引0，第288帧改为1；plug的0–130对应0，第131帧改为1。8个片段的每一帧都与lookup文本一致，无重叠。末段结束后，`subtask_index=4`映射到完整`global_task`，不是“缺失值”或第五个细粒度阶段。因此非空subtask索引覆盖率不能替代细粒度分段覆盖率。

`subtask_index`是该season/export的lookup表ID，与episode内的`subtask-001`等segment ID不同，也不是统一skill类别ID。hammer season中tasks索引0/1文本相同，subtasks索引4/5文本也相同，不能直接把lookup行数当独立语义数量。两条的`task_index`均能正确查到global task；annotations、episodes及Parquet帧数一致。额外读取到的两season元数据分别为68/110个episode、46,875/42,874帧，info、annotation帧数和episode长度总和彼此一致；这不表示已核验其他episode的数值或视频。

**尚不能证实的物理语义和同步：** `fx/fy/fz/tx/ty/tz`命名与动态实数本身不能证明单位为N/N·m、标定准确、零点/坐标系已知，亦不能证明这些量是直接测量而非由触觉图像估算。所读README/info/modality没有提供足以核实这些问题的传感器型号、标定矩阵/误差和力矩参考点；TCP的`rotation_type`仍为null，单位与参考坐标系也不能从维度猜测。当前只能独立确认导出帧级对齐、均匀时间轴和语言映射，不能核实真实硬件同步、相机/触觉时延、原始采样率、掉帧及插值规则。`v40_validation.json`声称视频帧数匹配和TCP按原始`aligned_index`精确导出，这是发布者校验声明，本次没有原始时钟与源流作独立复验。`score=S`及成功条件也只是标注，本次未看视频验证成功和分段语义。

证据文件：`download_manifest.json`保存固定版本URL、字节数与SHA-256；`audit_summary.json`及两任务的`audit.json`保存所有列的统计、60维逐通道变化、动作差异、时间与分段端点检查。复现脚本为`fetch.py`与`audit.py`。

固定版本样例：[hammer Parquet](https://huggingface.co/datasets/Sharpa-Robotics/SharpaDex-v1.0/blob/c8032fd891e3f6f19b0d4886793579c1dce5eea7/hammer_nails/season_POC22048_2026_05_20_16_40_10_train/lerobot_v2.1/data/chunk-000/episode_000000.parquet)、[plug Parquet](https://huggingface.co/datasets/Sharpa-Robotics/SharpaDex-v1.0/blob/c8032fd891e3f6f19b0d4886793579c1dce5eea7/insert_plug_into_socket/season_POC22007_2026_03_18_13_58_40_train/lerobot_v2.1/data/chunk-000/episode_000000.parquet)。
