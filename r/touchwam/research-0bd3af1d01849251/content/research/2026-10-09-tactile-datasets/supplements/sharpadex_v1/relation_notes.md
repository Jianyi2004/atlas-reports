# SharpaDex v1.0 与 Sharpa North-FTP-1（R16）关系核查

核查日期：2026-10-20。本笔记只比较现有下载样例、官方数据卡、已缓存FTP论文及轻量目录，不下载大规模数据，不改Atlas或原调研报告。

## 结论

**可以确认两者属于同一个Sharpa North采集家族，但目前不能确认SharpaDex v1.0是旧Sharpa North-FTP-1的严格超集、简单改名重发布，或完全不重叠的新采集。两者的规模不能直接相加。**

SharpaDex是一个更大规模的官方公开集合，并采用LeRobot导出；FTP中的Sharpa集合采用FTP统一Zarr表示。相同的机器人平台、设备编号和采集目录惯例只能支持“同家族”，不能证明“同轨迹”。反过来，目录名或任务名不同也不能证明完全独立。

建议报告将SharpaDex作为新的官方发布入口重点介绍，与R16互相引用，并明确“同采集家族、轨迹重叠量未核实”。若需保守统计独立小时，优先使用经新仓库元数据核实的SharpaDex时长，旧R16暂不再额外加到这项之上；这不是认定旧库已全部包含，只是避免未经去重相加。

## 证据对照

| 项目 | 旧R16：Sharpa North-FTP-1 | SharpaDex v1.0 | 能支持的判断 |
|---|---|---|---|
| 官方声明规模 | FTP论文称约4,000条long-horizon dexterous demonstrations | README称28,993条、59任务、495 seasons、32,396,172帧，30FPS约300小时 | 发布规模不同，不能称同一份仅改文件格式的数据；论文条数和新仓库条数不能相加当独立采集总量 |
| 采集平台/来源 | FTP论文明确Sharpa North、Sharpa Wave、Sharpa DTC；致谢Sharpa提供该数据 | 实际annotation原始路径含`/Data/North/SharpaOpenData/`；README为双7DoF机械臂、双22DoF手和指尖触觉 | 有直接证据支持同属Sharpa North采集家族 |
| 采集季次命名 | 已下载样例：`tube_season_POC22007_2026_02_09_15_57_16_train.zarr` | `task/season_POC22xxx_YYYY_MM_DD_HH_MM_SS_train/lerobot_v2.1或v3.0` | 共享采集命名惯例和POC设备ID，不能单凭这一点判定轨迹重复 |
| 发布格式 | FTP分卷tar中的Zarr；示例视觉224×224，触觉经过FTP标准化 | LeRobot v2.1/v3.0；6路视频；60D触觉数值、65D状态/动作等 | 变换后的视觉/触觉字节和形状不能用于直接排除同源 |
| 新旧目录交叉核对 | 仅完整核实旧库的一项season：Feb 9的tube；100条、68,142帧，下载完整episode0为729帧 | 已核实完整495个season目录；日期名范围2026-03-10至2026-07-07；没有Feb 9样例或任何2026年2月season | 目前可见目录不能支持“新版完整包含旧版”；但只掌握旧版一个season，不能由此证明整个旧库与新版互斥 |
| 原始轨迹标识 | 现有FTP样例没有可与新库匹配的`source_uid`或原始episode目录 | annotation保存`source_uid`、`anno_path`、`v40_original_episode_index` | 新库具备去重线索；需要旧库同级原始标识才能量化交集 |
| 官方继承说明 | 论文未提SharpaDex名称 | 新README未提FTP、North-FTP-1、与旧库的重叠或继承关系 | 没有官方直接证据能把两者写成已确认的扩展/重发关系 |

## 已实际读取的新库来源标识

来源：`clean_plate_with_eraser/season_POC22027_2026_04_17_11_01_59_train/lerobot_v2.1/meta/annotations.jsonl`，下载记录的revision为`c8032fd891e3f6f19b0d4886793579c1dce5eea7`。

episode 0示例：

- `source_uid`: `4b4ce4d1b52e1551`
- `num_frames`: 1250
- 原始任务名：`ClearPlate`
- `anno_path`: `/mnt/netdata/Team/Academic/Data/North/SharpaOpenData/ClearPlate/season_POC22027_2026_04_17_11_01_59_train/POC22027_2026_04_17_11_03_13_train/anno.json`
- `v40_original_language_cache`: 位于`/Data/North/SharpaOpenData_lerobot/clean_plate_with_eraser/.../lerobot_v2.1/meta/annotation_cache.jsonl`
- `v40_original_episode_index`: 1，而当前`episode_index`: 0。
- `v40_language_source`: `previous_published_lerobot`。

最后一项只说明语言标注来自此前的LeRobot发布/缓存，**不能解释为来自FTP**。同时，episode导出索引已经重编号，跨版本不能直接拿`episode_000000`认定同一原始轨迹。

## 完整新库季次目录核对

目录证据来自同组agent生成的`metadata_audit/season_inventory.json`，通过根目录和各任务非递归tree取得：59任务、495季次。按季次名解析的日期分布：

| 月份 | 季次数 |
|---|---:|
| 2026-03 | 70 |
| 2026-04 | 45 |
| 2026-05 | 167 |
| 2026-06 | 202 |
| 2026-07 | 11 |
| 合计 | 495 |

最早2026-03-10，最晚2026-07-07；与现有旧FTP样例`POC22007_2026_02_09_15_57_16`没有精确目录匹配。日期来自目录名，不是对所有原始传感器绝对时间戳的审计。

初始`files.json`恰好只有100,000条，被HF数据集信息接口的文件清单上限截断，只到`iron_garment`中途，因此没有使用该部分清单的缺失结果作为最终判断依据。

## 统计及后续去重规则

1. **SharpaDex v2.1与v3.0不能相加。** 官方README明确两者是同一批28,993条示范的两个导出。
2. **SharpaDex与FTP Sharpa目前不能直接相加。** 缺少旧4,000条的完整来源ID清单；既没有证实全重叠，也没有证实零重叠。
3. 优先按原始`source_uid`、原采集episode目录和season设备/时间标识匹配。任务名称已经出现`ClearPlate`→`clean_plate_with_eraser`的规范化，不能只比较英文任务字符串。
4. 只有当来源ID不保留时，再用动作/关节序列和同一视角的视频抽样做变换容忍的匹配；FTP已缩放/标准化，文件哈希不同不等于不同采集。
5. 新库明确30FPS，不足以追认旧FTP转换包也是30Hz。旧R16的物理采样率仍未直接核实，不应用新版频率倒填旧版时长。
6. FTP论文Table7的Sharpa条目`3.29`是**Sample-Scale**，`9.0%`是训练重采样后的Final-Percentage，二者都不是该子库小时数，不能据此另推出时长。

## 来源与本地证据

- [SharpaDex官方数据卡](https://huggingface.co/datasets/Sharpa-Robotics/SharpaDex-v1.0)
- [新库实际样例annotation（固定revision）](https://huggingface.co/datasets/Sharpa-Robotics/SharpaDex-v1.0/blob/c8032fd891e3f6f19b0d4886793579c1dce5eea7/clean_plate_with_eraser/season_POC22027_2026_04_17_11_01_59_train/lerobot_v2.1/meta/annotations.jsonl)
- [FTP-1论文：§2.4、Appendix C、Table7、致谢](https://arxiv.org/html/2606.13102)
- [FTP公开Sharpa数据入口](https://huggingface.co/datasets/MJJJJ1064/FTP-1-Dataset/tree/main/sharpa)
- 新证据：`readme_mirror.md`、`sample_download.json`、`sample/meta/annotations.jsonl`、`numeric_audit.json`、`metadata_audit/season_inventory.json`。
- 旧证据：`research/tactile_dataset_survey_2026-10-09/trajectory_work/R16/tar_index.json`、`episode0.npz`、`sample.zarr/meta/episode_ends`、`paper.html`和`trajectory_work/records/R16.json`。
