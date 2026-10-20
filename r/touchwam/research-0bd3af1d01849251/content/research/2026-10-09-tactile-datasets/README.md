# 触觉数据集调研（基础2026-10-09；实际样例2026-10-19；SharpaDex增补2026-10-20）

从 [report.html](report.html) 阅读报告，或打开 [report.md](report.md)。
[datasets.csv](datasets.csv) 是完整可筛选总表，[summary.json](summary.json) 是自动统计。

主统计按独立采集来源家族去重；区分真实人类ego、机器人、工具、人类其他实测与实验台。
仅有官方入口的B级与实际文件目录已核实的A级分别列出；不声称已下载所有数据。

`catalog.py` 是经人工复核的条目源，`build_report.py` 生成报告、表格、来源索引和统计。
修改条目后运行：`python build_report.py`。

`sources/` 保存本次公开页面／API的成功及失败请求，不是触觉数据本体。
HF信息部分经hf-mirror读取，GitHub内容部分经jsDelivr读取；报告保留官方原地址。
检索日志仅是发现过程记录，不应当作论文事实的独立依据。

## 2026-10-19 实际样例与时长

全部72个条目均有抽样结果记录，63个条目有已关联来源的样例展示（含静态、镜像、派生与复用），另1个来源未证实的同名候选样例，33个条目有量化时长证据；各口径及子集范围单列。

`trajectory_manifest.json` / `trajectory_records/` 保存下载路径、校验和、对齐方法与限制；`dataset_durations.csv` 是时长总表，`trajectory_audits/` 保存选定的详细审计。`trajectories/` 为可独立播放的本地媒体。原始下载文件保存在研究工作目录下的 `trajectory_work/`，未将数GB原始文件作为网页启动资源。

## 全量时长规划

`duration_estimates/`逐条保留数值、范围、假设、访问状态及证据；`duration_estimates.csv`为完整表，`duration_totals.json`为去重分类汇总。连续录制、静态帧等效、条件开放和汇编复用分开统计。
