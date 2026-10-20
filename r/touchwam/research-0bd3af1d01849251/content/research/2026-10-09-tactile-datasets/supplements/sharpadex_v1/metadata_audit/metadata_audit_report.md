# SharpaDex-v1.0 公开规模元数据审计

审计时间：2026-10-19T22:42:53.837179+00:00（UTC；北京时间2026-10-20）。仓库版本：`c8032fd891e3f6f19b0d4886793579c1dce5eea7`。

已从非递归根目录及59个任务目录确认495个season，再实际读取全部495份`lerobot_v3.0/meta/info.json`。全部返回HTTP 200，`X-Repo-Commit`一致。根目录及各任务目录均完整处理分页；未使用被100000条上限截断的API siblings认定剩余任务缺失。

| 项目 | 实际聚合结果 | 与README比较 |
|---|---:|---|
| 任务 | 59 | 一致 |
| season | 495 | 一致 |
| episode | 28,993 | 一致 |
| 帧 | 32,396,172 | 一致 |
| FPS | 全部495份均30 | 一致 |
| 发布时长 | 299.964555556小时 | 四舍五入300.0小时，一致 |

计算公式：32,396,172 ÷ 30 ÷ 3,600 = **299.964555556小时**。只计算v3.0一个导出；v2.1和v3.0按官方说明是同一批演示，不能相加。59个任务逐项的season、episode、frames均与README任务表完全一致，差异数：0。

所有season均为相同schema：19个字段，其中6个video字段。全部`observation.state`/`action`为65D、joint_torque为65D、TCP state为24D、observed/commanded TCP pose各12D、numeric tactile为60D。4路RGB均480×480，tactile_deform为480×1200，tactile_raw为480×1600；所有video metadata均30FPS、H.264。schema包括dtype/shape/names/video info逐项哈希一致。train split全部为0:total_episodes。

`meta.total_tasks`在447个season中为1、47个season中为2、1个season中为3。这是各season内部LeRobot任务标签数量；59是根目录任务类别数。不能累加内部标签数得到全库任务类别数，本次未读取所有task标签文本来统计全局去重语句。

按全库帧数与episode数计算，平均每条episode约37.246秒。这里只能算均值，不能由season info推出每条episode时长分布。season的episode数量范围1–530，中位数34；season时间范围约35.333秒–4.676小时，中位数0.412583小时。

## 时长最多的10项任务

| 任务 | season | episode | 小时 |
|---|---:|---:|---:|
| `iron_garment` | 33 | 517 | 21.090287 |
| `deal_playing_cards` | 23 | 1088 | 19.209667 |
| `place_coffee_filter_in_dripper` | 21 | 784 | 16.537315 |
| `pack_blueberries_in_bag` | 17 | 485 | 14.198306 |
| `play_tic_tac_toe` | 16 | 521 | 12.199194 |
| `fold_and_close_box` | 10 | 697 | 11.425176 |
| `reposition_and_stack_blocks` | 15 | 1007 | 9.901241 |
| `assemble_gears_on_base` | 11 | 1488 | 9.102991 |
| `hang_garment_on_hanger` | 15 | 521 | 9.067583 |
| `hammer_nails` | 4 | 1071 | 7.978407 |

## 存储量口径

仓库API的`usedStorage`为7,694,665,428,461字节，即7.694665 TB（十进制）或6.998257 TiB。这是仓库口径，**不是单一导出的实际下载大小**，可能包括两个导出、附带资产及历史对象。未扫描全部大文件树逐文件求和。

所有495份info中的`data_files_size_in_mb=100`、`video_files_size_in_mb=200`均为LeRobot文件切分目标配置，不是各season实际数据量，不可相加估算存储。

本次实际下载495份info JSON共7,821,060字节，未下载大视频。

## 证据与限制

- `season_inventory.json`：从非递归目录发现的全部59任务/495season。
- `task_trees/`：59个任务的原始目录结果与分页信息。
- `infos/`及`responses/`：495份原始info与HTTP状态、commit、SHA256、下载字节。
- `season_metadata_summary.json`、`audit_totals.json`：逐season/逐task/全库聚合。
- `metadata_findings.json`、`task_statistics.csv`：离线复核与任务分布。
- `audit_metadata.py`：最多4并发、可缓存重跑；`analyze_metadata.py`：离线复算。

这是公开metadata规模核查，未逐行读取全部Parquet或解码全部视频；不能证明全库每帧同步质量、传感器有效率、所有大文件完整可下载。语言片段数量和实际零填充维度也没有由本次info审计独立验证。
