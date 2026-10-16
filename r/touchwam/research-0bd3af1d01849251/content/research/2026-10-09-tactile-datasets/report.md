# 公开触觉数据集调研报告

**面向：自建触觉世界模型｜检索截止：2026年10月9日｜版本：1.0**

> 配图更新：2026-10-16。本版新增81张图片，覆盖56个目录条目，其中51个属于主统计来源，8个人类ego来源均已配图。4组数值触觉样例由本报告读取公开数据后绘制。数据规模与开放状态仍沿用2026-10-09核验口径。
> 图片保留来源、采集／仿真／预测标识；图片版权归原作者。点击HTML中的图片可放大查看。

## 1. 结论与数量

本次登记 **71 个条目**，其中 **55 个独立真实触觉采集来源家族**有可核实的数据文件目录或官方数据发布入口：**人类 ego 8 个、机器人真机 23 个、人操持工具／UMI 5 个、其他人类实测 6 个、实物探测／标定／混合实验台 13 个**。此外，TouchScale 和 ToucHD 是 **2 个条件开放家族**，单独计入时合计 **57 个**。

这个结论回答的是“本轮检索发现了多少有公开发布证据的来源”，**不是全网已经穷尽的绝对总量，也不是55个均已成功匿名下载、均有无用途限制的开源许可证**。主数中的34个看到实际数据文件目录，另外21个确认了官方入口／脚本但没有验证大归档。需要最严格的可核实文件目录口径时，请使用A列。

| 类别 | A：已见文件目录 | B：已见官方入口 | 主统计 A+B | 另有条件开放 |
|---|---:|---:|---:|---:|
| E · 人类ego＋实测触觉 | 4 | 4 | **8** | 1 |
| R · 机器人真机触觉 | 12 | 11 | **23** | 0 |
| U · 人操持工具／UMI | 4 | 1 | **5** | 0 |
| H · 其他人类实测（非ego或视角未确认） | 4 | 2 | **6** | 0 |
| P · 实物探测／标定／混合实验台 | 10 | 3 | **13** | 1 |
| 合计 | **34** | **21** | **55** | **2** |

### 1.1 用户关心的ego和真机分别是多少

- **人类 ego＋实测触觉：8个；含需接受条款的TouchScale则9个。** 其中Touch and Go是人手持触觉探头，测到局部探头接触；其余7个是人手／手套或手—压力板录制。ego只说明视角，并不保证全手压力、连续手姿态或机器人action。
- **机器人真机触觉：23个。** 若把可向机器人重定向的人操持UMI／夹爪采集一并作为操作数据来源，则为 **28个＝23＋5**，但其中5个不是机器人本体执行。
- **其他真实物理触觉：19个＝6个人类非ego／视角未确认＋13个探测／实验台家族。** 若“真机”只是泛指非仿真的实测数据，那么非ego部分为 **47个＝23＋5＋6＋13**；报告不将47个称为47个机器人数据集。

### 1.2 “公开”与“开源”要分开

按本次逐条读取的数据卡／数据专用许可，55个主统计来源中，**23个标明无NC（非商业）限制的数据许可，11个明确有非商业限制，21个的数据专用许可尚未确认或需追溯上游条款**。这里的23个包含MIT、Apache、CC BY、CC BY-SA、GPL及CDLA等不同条款，并不意味着可以免署名、免履行共享条件。许可统计是对发布声明的整理，不是对作者全部上游权利的独立审计。

| 类别 | 已标明无NC限制 | 明确有NC限制 | 数据许可未确认／上游待查 |
|---|---:|---:|---:|
| 人类ego＋实测触觉 | 2 | 3 | 3 |
| 机器人真机触觉 | 7 | 3 | 13 |
| 人操持工具／UMI | 3 | 1 | 1 |
| 其他人类实测（非ego或视角未确认） | 4 | 1 | 1 |
| 实物探测／标定／混合实验台 | 7 | 3 | 3 |

最适合起步的组合取决于目标传感器：人手压力路线优先看OpenTouch、EgoTouch、ActionSense及OpenGraph样例；机器人动作条件路线优先看RDP、HapTile，再扩展FreeTacMan／Touch in the Wild工具示范和LET-Dex／VTouch等不同本体。跨传感器编码则重点看TacQuad、HPT、TaF和TacBench。以上是基于字段与采集协议的适配建议，尚未进行统一模型训练比较。

## 2. 检索方法、证据和计数规则

### 2.1 纳入的“触觉”是什么

主目录纳入直接测量接触的压力／力阵列、触觉皮肤、BioTac／OptoForce等接触传感器，以及GelSight、DIGIT等通过弹性体形变成像的光学触觉。**光学触觉图像记录真实接触，但图像像素本身不等于已标定的力值。** 实物实验台、手持探头、人类手套和机器人都可纳入；按采集方式互斥分组。

仅有腕部六维F/T、本体关节力矩，或传感器意义不清的state量，放入力觉扩展；只有人手／物体姿态、几何接触标签、热成像接触印迹、视频推断的伪触觉，放入相关数据附录。纯仿真不进入真实触觉数量。混合库只计真实部分。

### 2.2 证据等级与核查深度

| 等级 | 本报告完成的核查 | 不能由此推出的结论 |
|---|---|---|
| A | 官方或由官方／论文可追溯的发布平台可见实际数据文件，如ZIP、TAR、HDF5、Zarr、Parquet、视频等 | 未下载并校验全量文件；目录存在不保证每个归档完整、内容符合论文全部规模 |
| B | 官方项目／README提供明确下载URL、逐文件链接或下载脚本 | 未核实归档payload；旧链接可能需登录、限流、已迁移或失效 |
| C | 有真实数据发布及文件目录，但API表明需接受条款／提供资料 | 未申请、未登录接受协议，不算无条件公开访问 |
| D | 有论文、代码或候选归档，但下载、独立来源、模态或分类证据尚不足 | 不据名称、论文实验或“coming soon”推断已开源 |

检索以作者项目、论文、GitHub README、HF／ModelScope数据卡、文件目录为主，并沿FTP-1、FoTa、Sparsh、AnyTouch等论文的来源表反向追踪。第三方awesome清单用于发现线索，不单独支持关键规模和开放结论。参考年份一般是论文／项目年份，不保证等于数据首次开放日期。

本次环境对GitHub和HF直连存在超时与速率限制：部分GitHub文件通过jsDelivr读取对应官方仓库副本，HF卡片和目录主要通过hf-mirror读取，并与作者项目／论文中的仓库名交叉核对。报告给出原始平台URL。缓存包含失败请求和搜索页，**缓存URL数量不是“独立权威证据数量”**。没有下载TB级数据，也没有声称已核实每一条轨迹的数值质量。

### 2.3 去重与分类规则

1. **计独立采集来源家族，不计仓库数量。** 同一来源的原始版、处理版、mini/full、单／双臂、不同分辨率、训练／测试、HF／Drive镜像合并。不同采集协议但被作者明确作为同一发布家族的版本，也按一家族保守统计。
2. **汇编不再加一次。** FTP-1、FoTa、Sparsh、Touch100k和yxma再编码包单列。能辨明独立新增采集的来源单独计，如Sharpa、TVL-HCT、Touch-Slide、TacBench原创监督部分；不能辨明的新增部分不为凑数加入。
3. **TVL拆来源，任务不拆数据集。** 机器人SSVTP和新增人采HCT是不同采集来源，分别计；TVL总包不再计。TacBench不同任务／传感器不全部拆成独立库。
4. **同一跨本体家族只归一类。** VTouch含青龙、D-WHEEL和PIKA，在机器人R类计一次，PIKA不再给U类加数。类别中的“机器人”不意味着这个混合家族的全部轨迹都由机器人采集。
5. **严格保留人类ego条件。** 固定相机、人手采集但没有视角说明的库不加E；机器人腕部相机也不属于人类ego。PX、FusionX V3等因此暂列H。VTDexManip的头戴采集与ego样例已通过作者项目采集图确认。
6. **不把不同物理量相加。** 帧／图像对／试验／episode／多相机stream-hour／人类操作小时彼此不直接换算；论文采集总量与当前开放量分列。

## 3. 主目录速览

编号链接跳转到详细说明；每条详细说明都包含数据入口、规模、模态、动作字段、许可及局限。CSV含全部71个条目，可按类别、证据与许可筛选。

| 编号 | 数据集／来源家族 | 分类 | 证据 | 首要入口 |
|---|---|---|---|---|
| [E01](#E01) | ActionSense | E | B | [项目](https://action-sense.csail.mit.edu/) |
| [E02](#E02) | EgoPressure | E | A | [官方数据](https://huggingface.co/datasets/eth-siplab/EgoPressure) |
| [E03](#E03) | OpenTouch | E | B | [项目](https://opentouch-tactile.github.io/) |
| [E04](#E04) | EgoTouch（TouchAnything数据） | E | A | [官方数据](https://huggingface.co/datasets/zhouzhoujy/EgoTouch) |
| [E05](#E05) | EgoTactile | E | A | [官方数据](https://huggingface.co/datasets/HustleHard/EgoTactile) |
| [E06](#E06) | OpenGraph Ego-Tactile Manipulation | E | A | [官方数据及格式](https://huggingface.co/datasets/OpenGraphLabs-Research/ego-tactile-manipulation) |
| [E07](#E07) | Touch and Go | E | B | [官方项目/许可](https://touch-and-go.github.io/) |
| [E08](#E08) | VTDexManip（真实人类预训练数据） | E | B | [项目](https://lqts.github.io/VTDexManip/) |
| [R01](#R01) | VisGel | R | B | [作者仓库/下载脚本](https://github.com/YunzhuLi/VisGel) |
| [R02](#R02) | Calandra抓取数据 / The Feeling of Success | R | B | [原项目](https://sites.google.com/view/the-feeling-of-success/) |
| [R03](#R03) | ObjectFolder-Real | R | B | [官方数据与结构](https://objectfolder.stanford.edu/objectfolder-real-download) |
| [R04](#R04) | Eagle Shoal / Visual-Tactile Dataset | R | B | [数据仓库/许可](https://github.com/tsinghua-rll/Visual-Tactile_Dataset) |
| [R05](#R05) | PoseIt | R | B | [官方数据说明/Drive](https://github.com/Robo-Touch/PoseIt) |
| [R06](#R06) | YCB-Sight（真实部分） | R | B | [官方数据/逐物体下载](https://github.com/Robo-Touch/YCB-Sight) |
| [R07](#R07) | RH20T（触觉配置） | R | A | [官方项目/下载](https://rh20t.github.io/) |
| [R08](#R08) | Reactive Diffusion Policy（RDP）数据 | R | A | [官方数据](https://huggingface.co/datasets/WendiChen/reactive_diffusion_policy_dataset) |
| [R09](#R09) | UniT数据 | R | A | [作者仓库/原始Drive](https://github.com/ZhengtongXu/UniT) |
| [R10](#R10) | VLA-Touch数据 | R | A | [官方数据](https://huggingface.co/datasets/AllenBi21/VLA-Touch) |
| [R11](#R11) | OmniViTac（OmniVTA数据） | R | A | [官方数据](https://huggingface.co/datasets/tars-robotics/OmniVitac) |
| [R12](#R12) | HapTile | R | A | [数据](https://huggingface.co/datasets/HapTile2026/HapTile) |
| [R13](#R13) | LET-Dex Dataset | R | A | [官方数据](https://huggingface.co/datasets/LejuRobotics/LET-Dex-Dataset) |
| [R14](#R14) | Humanoid Everyday（触觉部分） | R | A | [官方数据](https://huggingface.co/datasets/USC-PSI-Lab/humanoid-everyday) |
| [R15](#R15) | 白虎-VTouch / OpenLoong visuo-tactile | R | A | [官方数据](https://www.modelscope.cn/datasets/OpenLoong-Community/visuo-tactile) |
| [R16](#R16) | Sharpa North-FTP-1 | R | A | [公开数据](https://huggingface.co/datasets/MJJJJ1064/FTP-1-Dataset/tree/main/sharpa) |
| [R17](#R17) | Tactile MNIST（Real） | R | A | [真实序列数据](https://huggingface.co/datasets/TimSchneider42/tactile-mnist-touch-real-seq-t256-320x240) |
| [R18](#R18) | ARQ-CRISP Slip Detection Dataset | R | B | [数据及协议](https://github.com/ARQ-CRISP/slip_detection_dataset_2021) |
| [R19](#R19) | iCub Hybrid Touch Texture Dataset | R | B | [官方数据](https://github.com/clear-nus/TactileLearning) |
| [R20](#R20) | Doro/Jaco OptoForce Object Classification | R | B | [原作者数据仓库](https://github.com/philipmaus/Tactile_Object_Classification) |
| [R21](#R21) | BioTac_slide_20_50 | R | B | [官方数据](https://github.com/dexrob/BioTac_slide_20_50) |
| [R22](#R22) | Active Clothing Material Perception（Yuan18） | R | B | [原始数据归档](http://data.csail.mit.edu/active_clothing/Data_ICRA18.tar) |
| [R23](#R23) | SSVTP | R | A | [公开数据（TVL内ssvtp）](https://huggingface.co/datasets/mlfu7/Touch-Vision-Language-Dataset) |
| [U01](#U01) | FreeTacMan | U | A | [官方数据](https://huggingface.co/datasets/OpenDriveLab/FreeTacMan) |
| [U02](#U02) | Touch in the Wild | U | A | [官方数据](https://huggingface.co/datasets/binghaohuang-robot/touch_in_the_wild-dataset) |
| [U03](#U03) | exUMI | U | A | [官方数据](https://huggingface.co/datasets/silicx/exUMI) |
| [U04](#U04) | ViTaMIn | U | A | [作者数据](https://huggingface.co/datasets/chuanyune/ViTaMIn_dataset) |
| [U05](#U05) | Daimon-Infinity | U | B | [官方说明/许可](https://github.com/dmrobot-admin/Daimon-Infinity) |
| [H01](#H01) | STAG | H | B | [官方项目与下载](https://stag.csail.mit.edu/) |
| [H02](#H02) | PressureVisionDB | H | B | [官方代码/下载器/许可](https://github.com/facebookresearch/PressureVision) |
| [H03](#H03) | PX OmniSharing DB SampleData | H | A | [官方样例数据](https://huggingface.co/datasets/paxini/Omnisharing_DB_SampleData) |
| [H04](#H04) | FusionX Multimodal Sample Dataset V3 | H | A | [发布方数据](https://huggingface.co/datasets/touchtronix/FusionX-Multimodal-Sample-Data-V3) |
| [H05](#H05) | Tachin Fixed Viewpoint Tactile Dataset | H | A | [发布方数据/字段定义](https://huggingface.co/datasets/Tachintech/Fixed_Viewpoint_Tactile_Dataset) |
| [H06](#H06) | Tactile Gesture Language | H | A | [发布方数据](https://huggingface.co/datasets/Tachintech/Tactile_Gesture_Language) |
| [P01](#P01) | TVL-HCT（TVL新增人采部分） | P | A | [官方数据及修复说明](https://huggingface.co/datasets/mlfu7/Touch-Vision-Language-Dataset) |
| [P02](#P02) | TacQuad（AnyTouch） | P | A | [官方数据](https://huggingface.co/datasets/xxuan01/TacQuad) |
| [P03](#P03) | HPT / HTT Dataset | P | A | [官方数据/manifest](https://huggingface.co/datasets/AllenBi21/HTT-dataset) |
| [P04](#P04) | TaF-Dataset | P | A | [官方数据/字段/许可](https://huggingface.co/datasets/jiamig/taf-dataset) |
| [P05](#P05) | Touch-Slide | P | A | [官方数据](https://huggingface.co/datasets/facebook/touch-slide) |
| [P06](#P06) | YCB-Slide（Real） | P | B | [官方数据/下载](https://github.com/rpl-cmu/YCB-Slide) |
| [P07](#P07) | Sparsh / TacBench原创监督数据 | P | A | [DIGIT力/滑移](https://huggingface.co/datasets/facebook/digit-force-estimation) |
| [P08](#P08) | TactileTracking / NormalFlow Dataset | P | A | [官方数据](https://huggingface.co/datasets/joehjhuang/TactileTracking) |
| [P09](#P09) | GelSLAM Dataset | P | A | [官方数据及局限](https://huggingface.co/datasets/joehjhuang/GelSLAM_dataset) |
| [P10](#P10) | GelSight Sim2Real（真实A域） | P | B | [官方数据/协议](https://github.com/RVSATHU/GelSight-Sim2Real) |
| [P11](#P11) | PhysiCLeAR（Octopi 1.0/1.5） | P | B | [官方数据下载说明](https://github.com/clear-nus/octopi-1.5) |
| [P12](#P12) | Touch-Ex | P | A | [发布方数据及采集说明](https://huggingface.co/datasets/gemixin/touch-ex) |
| [P13](#P13) | Tactile Deformation Response | P | A | [发布方数据](https://huggingface.co/datasets/Tachintech/tactile-deformation-response) |


## 4. 人类ego视频＋真实触觉（8个）


<a id="E01"></a>

### E01 · ActionSense

**分类：**人类ego＋实测触觉　**参考年份：**2022　**证据：**B（已核实官方下载入口／脚本，归档未验证；计入主统计）

**数据／项目链接：**[项目](https://action-sense.csail.mit.edu/)；[数据下载](https://action-sense.csail.mit.edu/data.html)；[字段与质量说明](https://action-sense.csail.mit.edu/dataset_info.html)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 官方说明页：10 位参与者；778.0 分钟录制，其中543.5分钟为厨房活动；20类活动。完整可穿戴数据涉及9人。 |
| 传感器、模态与时序 | 第一视角眼动相机、双手导电织物触觉手套、身体IMU、手指跟踪、EMG、外部RGB/深度及声音；各流有时间信息。 |
| 动作与状态 | 有人体运动和活动标签；不是机器人控制命令。 |
| 对触觉世界模型的用途 | 厨房连续接触动态、跨模态预训练、长序列事件预测。 |
| 数据许可 | CC BY-NC-SA 4.0 |
| 限制、证据缺口与去重 | 官方注明一名参与者只有相机数据，初期参与者的Manus手指跟踪不可靠。不能把778分钟全部当成完整同步视频—姿态—触觉有效时长；网页目标25人不是已发布人数。 |

<div class="dataset-figures">
<figure class="dataset-figure" id="fig-e01-01">
<div class="figure-type"><strong>E01 · 图1</strong><span>采集装置</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/e01-01.webp" target="_blank" rel="noopener" aria-label="放大：ActionSense官方可穿戴采集装置：第一视角相机、双手触觉手套及人体运动传感器。各模态有效覆盖仍须按参与者核对。"><img src="figures/e01-01.webp" alt="ActionSense官方可穿戴采集装置：第一视角相机、双手触觉手套及人体运动传感器。各模态有效覆盖仍须按参与者核对。" width="1500" height="1469" loading="lazy" decoding="async"></a>
<figcaption>ActionSense官方可穿戴采集装置：第一视角相机、双手触觉手套及人体运动传感器。各模态有效覆盖仍须按参与者核对。<span class="figure-links"><a href="https://action-sense.csail.mit.edu/" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://action-sense.csail.mit.edu/images/wearable_sensors_v0.jpg" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
<figure class="dataset-figure wide" id="fig-e01-02">
<div class="figure-type"><strong>E01 · 图2</strong><span>数据概览</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/e01-02.webp" target="_blank" rel="noopener" aria-label="放大：ActionSense多模态厨房活动录制概览；展示视觉、人体运动及触觉的互补观测。"><img src="figures/e01-02.webp" alt="ActionSense多模态厨房活动录制概览；展示视觉、人体运动及触觉的互补观测。" width="2400" height="899" loading="lazy" decoding="async"></a>
<figcaption>ActionSense多模态厨房活动录制概览；展示视觉、人体运动及触觉的互补观测。<span class="figure-links"><a href="https://action-sense.csail.mit.edu/" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://action-sense.csail.mit.edu/images/overview.jpg" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="E02"></a>

### E02 · EgoPressure

**分类：**人类ego＋实测触觉　**参考年份：**2025　**证据：**A（已核实实际数据文件目录；计入主统计）

**数据／项目链接：**[官方数据](https://huggingface.co/datasets/eth-siplab/EgoPressure)；[工具/字段](https://github.com/eth-siplab/EgoPressure)；[论文](https://arxiv.org/abs/2409.02224)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 当前官方数据卡：21人、1,344序列、32手势×双手；每路约630K帧、8路合计约5M相机帧；约1.96TB。 |
| 传感器、模态与时序 | 头戴RGB-D与7个固定RGB-D视角，30Hz；Sensel Morph桌面压力板105×185原始网格，UV压力、MANO手网格、逐帧ego相机位姿。 |
| 动作与状态 | 手部姿态/网格是运动监督，没有机器人动作。 |
| 对触觉世界模型的用途 | 压力估计、接触区域与几何联合建模；适合做有计量依据的压力预测基准。 |
| 数据许可 | CC BY-NC-SA 4.0；MANO另受其许可约束 |
| 限制、证据缺口与去重 | 压力来自手接触桌面压力板，不是全手戴手套操作任意物体；不能把多相机帧数视作独立接触状态。论文与新版数据卡规模口径不同，本文以现有发布卡为准。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-e02-01">
<div class="figure-type"><strong>E02 · 图1</strong><span>采集装置</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/e02-01.webp" target="_blank" rel="noopener" aria-label="放大：EgoPressure论文图3：7个固定视角与1个ego相机组成的采集系统；接触压力来自桌面压力板。"><img src="figures/e02-01.webp" alt="EgoPressure论文图3：7个固定视角与1个ego相机组成的采集系统；接触压力来自桌面压力板。" width="2024" height="1122" loading="lazy" decoding="async"></a>
<figcaption>EgoPressure论文图3：7个固定视角与1个ego相机组成的采集系统；接触压力来自桌面压力板。<span class="figure-links"><a href="https://arxiv.org/html/2409.02224" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://arxiv.org/html/2409.02224v2/camera_setup.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
<figure class="dataset-figure wide" id="fig-e02-02">
<div class="figure-type"><strong>E02 · 图2</strong><span>数据样例</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/e02-02.webp" target="_blank" rel="noopener" aria-label="放大：EgoPressure论文标注样例（右手）：视觉、手部几何和压力监督。此处展示数据标注，不是将预测压力当传感器实测。"><img src="figures/e02-02.webp" alt="EgoPressure论文标注样例（右手）：视觉、手部几何和压力监督。此处展示数据标注，不是将预测压力当传感器实测。" width="2039" height="1097" loading="lazy" decoding="async"></a>
<figcaption>EgoPressure论文标注样例（右手）：视觉、手部几何和压力监督。此处展示数据标注，不是将预测压力当传感器实测。<span class="figure-links"><a href="https://arxiv.org/html/2409.02224" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://arxiv.org/html/2409.02224v2/F2.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="E03"></a>

### E03 · OpenTouch

**分类：**人类ego＋实测触觉　**参考年份：**2025　**证据：**B（已核实官方下载入口／脚本，归档未验证；计入主统计）

**数据／项目链接：**[项目](https://opentouch-tactile.github.io/)；[官方数据脚本](https://github.com/OpenTouch-MIT/opentouch/blob/main/scripts/download_data.sh)；[论文](https://arxiv.org/abs/2512.16842)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 论文：5.1小时同步录制、2,900精选片段（约3小时），14种环境。 |
| 传感器、模态与时序 | Aria第一视角视频、169触点全手压力、Rokoko手姿态，30Hz同步；动作、物体、抓型和自然语言标注。 |
| 动作与状态 | 硬件跟踪的人手运动轨迹；语义动作标签不是低层控制动作。 |
| 对触觉世界模型的用途 | 第一版人手触觉时序模型的优先来源，可研究视频—姿态—触觉联合预测。 |
| 数据许可 | 本次未确认数据专用许可证 |
| 限制、证据缺口与去重 | 已核实官方脚本列27个Drive文件ID，未完成大文件下载。论文表中约800 object types与正文/网页8,000 objects不能混用，故不把两者当成同一个物体数量指标。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-e03-01">
<div class="figure-type"><strong>E03 · 图1</strong><span>采集装置</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/e03-01.webp" target="_blank" rel="noopener" aria-label="放大：OpenTouch论文图4：Aria眼镜、手部跟踪与全手触觉采集及标注流程。语言标签来自后续自动标注。"><img src="figures/e03-01.webp" alt="OpenTouch论文图4：Aria眼镜、手部跟踪与全手触觉采集及标注流程。语言标签来自后续自动标注。" width="2040" height="552" loading="lazy" decoding="async"></a>
<figcaption>OpenTouch论文图4：Aria眼镜、手部跟踪与全手触觉采集及标注流程。语言标签来自后续自动标注。<span class="figure-links"><a href="https://arxiv.org/html/2512.16842v1" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://arxiv.org/html/2512.16842v1/opentouch_hardware.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
<figure class="dataset-figure wide" id="fig-e03-02">
<div class="figure-type"><strong>E03 · 图2</strong><span>数据样例</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/e03-02.webp" target="_blank" rel="noopener" aria-label="放大：OpenTouch论文图3：视频、手姿态与触觉对照。接触压力补充了遮挡、透明物体及仅看外观难以辨别的交互信息。"><img src="figures/e03-02.webp" alt="OpenTouch论文图3：视频、手姿态与触觉对照。接触压力补充了遮挡、透明物体及仅看外观难以辨别的交互信息。" width="2040" height="912" loading="lazy" decoding="async"></a>
<figcaption>OpenTouch论文图3：视频、手姿态与触觉对照。接触压力补充了遮挡、透明物体及仅看外观难以辨别的交互信息。<span class="figure-links"><a href="https://arxiv.org/html/2512.16842v1" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://arxiv.org/html/2512.16842v1/opentouch_dataexample.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="E04"></a>

### E04 · EgoTouch（TouchAnything数据）

**分类：**人类ego＋实测触觉　**参考年份：**2026　**证据：**A（已核实实际数据文件目录；计入主统计）

**数据／项目链接：**[官方数据](https://huggingface.co/datasets/zhouzhoujy/EgoTouch)；[项目/格式](https://github.com/Jianyi2004/TouchAnything)；[论文](https://arxiv.org/abs/2605.13083)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 作者当前README：208任务、1,891 episodes、约2M帧、1,000+物体；发布说明仍保留“上传中/可能不完整”。 |
| 传感器、模态与时序 | ego+双腕3路视频、双手压力、人手姿态与相机位姿。代码中的处理格式为30FPS、21×21压力网格、WiLoR双手21关节点及有效掩码。 |
| 动作与状态 | 人手运动估计和相机位姿；不能直接作为机器人关节控制标签。 |
| 对触觉世界模型的用途 | 双手接触、遮挡下多视角预测、跨视角和跨本体预训练。 |
| 数据许可 | 数据许可未确认；代码MIT不自动等于数据MIT |
| 限制、证据缺口与去重 | 原始传感器、16×16采集说明和21×21处理网格须区分；不能把WiLoR估计称作全量动捕真值。数据目录有Home/Office/Outdoor/Retail/Workbench和split.json，但未证明覆盖论文全部样本。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-e04-01">
<div class="figure-type"><strong>E04 · 图1</strong><span>采集装置</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/e04-01.webp" target="_blank" rel="noopener" aria-label="放大：EgoTouch论文图2：头戴和双腕相机、压力手套以及对齐后的多模态样例。公开处理代码中的手姿态包含WiLoR估计，应保留其有效掩码。"><img src="figures/e04-01.webp" alt="EgoTouch论文图2：头戴和双腕相机、压力手套以及对齐后的多模态样例。公开处理代码中的手姿态包含WiLoR估计，应保留其有效掩码。" width="1579" height="850" loading="lazy" decoding="async"></a>
<figcaption>EgoTouch论文图2：头戴和双腕相机、压力手套以及对齐后的多模态样例。公开处理代码中的手姿态包含WiLoR估计，应保留其有效掩码。<span class="figure-links"><a href="https://arxiv.org/html/2605.13083" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://arxiv.org/html/2605.13083v1/data_collection.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
<figure class="dataset-figure wide" id="fig-e04-02">
<div class="figure-type"><strong>E04 · 图2</strong><span>数据样例</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/e04-02.webp" target="_blank" rel="noopener" aria-label="放大：EgoTouch论文图3：多场景操作中的视角、手部姿态与接触压力示例。采集读数和模型估计字段需按正文区分。"><img src="figures/e04-02.webp" alt="EgoTouch论文图3：多场景操作中的视角、手部姿态与接触压力示例。采集读数和模型估计字段需按正文区分。" width="1937" height="670" loading="lazy" decoding="async"></a>
<figcaption>EgoTouch论文图3：多场景操作中的视角、手部姿态与接触压力示例。采集读数和模型估计字段需按正文区分。<span class="figure-links"><a href="https://arxiv.org/html/2605.13083" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://arxiv.org/html/2605.13083v1/example_of_touchanything.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="E05"></a>

### E05 · EgoTactile

**分类：**人类ego＋实测触觉　**参考年份：**2026　**证据：**A（已核实实际数据文件目录；计入主统计）

**数据／项目链接：**[官方数据](https://huggingface.co/datasets/HustleHard/EgoTactile)；[项目](https://egotactile.github.io/)；[论文](https://arxiv.org/abs/2606.09243)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 12位参与者、63种日常物体、7物体类别；本次未确认可用于总量相加的完整时长。 |
| 传感器、模态与时序 | 1280×720 ego视频、162位置压力，视觉与压力对齐15Hz；戴手套与裸手两子集。 |
| 动作与状态 | 数据卡未给连续机器人动作；主要是视频到压力监督。 |
| 对触觉世界模型的用途 | 抓握压力预测、跨参与者和跨物体泛化、压力时序表征。 |
| 数据许可 | CC BY-NC 4.0 |
| 限制、证据缺口与去重 | 裸手子集可见手没有戴手套：参考压力来自画面外另一只戴手套的手，以节拍器协调动作。裸手子集不应标成“可见手直接测量压力真值”。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-e05-01">
<div class="figure-type"><strong>E05 · 图1</strong><span>采集装置</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/e05-01.webp" target="_blank" rel="noopener" aria-label="放大：EgoTactile论文图2：戴手套与裸手设置及数据分布。裸手设置的参考压力由画面外另一只戴手套的手提供，不能当成可见裸手的直接压力真值。"><img src="figures/e05-01.webp" alt="EgoTactile论文图2：戴手套与裸手设置及数据分布。裸手设置的参考压力由画面外另一只戴手套的手提供，不能当成可见裸手的直接压力真值。" width="1590" height="603" loading="lazy" decoding="async"></a>
<figcaption>EgoTactile论文图2：戴手套与裸手设置及数据分布。裸手设置的参考压力由画面外另一只戴手套的手提供，不能当成可见裸手的直接压力真值。<span class="figure-links"><a href="https://arxiv.org/html/2606.09243v1" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://arxiv.org/html/2606.09243v1/dataset_overview.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
<figure class="dataset-figure" id="fig-e05-02">
<div class="figure-type"><strong>E05 · 图2</strong><span>触觉表示</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/e05-02.webp" target="_blank" rel="noopener" aria-label="放大：EgoTactile论文图7：稀疏触点序列与标准手形热图的映射；热图是传感器读数的派生表示，并非独立的稠密压力测量。"><img src="figures/e05-02.webp" alt="EgoTactile论文图7：稀疏触点序列与标准手形热图的映射；热图是传感器读数的派生表示，并非独立的稠密压力测量。" width="678" height="519" loading="lazy" decoding="async"></a>
<figcaption>EgoTactile论文图7：稀疏触点序列与标准手形热图的映射；热图是传感器读数的派生表示，并非独立的稠密压力测量。<span class="figure-links"><a href="https://arxiv.org/html/2606.09243v1" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://arxiv.org/html/2606.09243v1/pressure_representation.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="E06"></a>

### E06 · OpenGraph Ego-Tactile Manipulation

**分类：**人类ego＋实测触觉　**参考年份：**2026　**证据：**A（已核实实际数据文件目录；计入主统计）

**数据／项目链接：**[官方数据及格式](https://huggingface.co/datasets/OpenGraphLabs-Research/ego-tactile-manipulation)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 公开样例72 episodes、1.28小时、138,629帧。 |
| 传感器、模态与时序 | 1920×1080头戴视频；OGLO每手80、双手160压力通道；30Hz对齐并保留90Hz原始触觉，头/双腕IMU；LeRobot v3。 |
| 动作与状态 | 语义分段、物体/抓型标签；未见连续机器人action或完整手关节位姿字段。 |
| 对触觉世界模型的用途 | 小规模快速跑通时序数据管线；允许商业用途的ego压力样本。 |
| 数据许可 | CC BY 4.0 |
| 限制、证据缺口与去重 | 公开的是样例，不能推算其商业全库规模。动作边界由接触信号自动生成，数据卡明确不是人工真值；腕部IMU不等于腕部6DoF位置。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-e06-01">
<div class="figure-type"><strong>E06 · 图1</strong><span>采集装置</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/e06-01.webp" target="_blank" rel="noopener" aria-label="放大：OpenGraph官方数据卡中的OGLO触觉手套；每手80个压力通道，传感器分布应与原始字段映射一起使用。"><img src="figures/e06-01.webp" alt="OpenGraph官方数据卡中的OGLO触觉手套；每手80个压力通道，传感器分布应与原始字段映射一起使用。" width="2400" height="1348" loading="lazy" decoding="async"></a>
<figcaption>OpenGraph官方数据卡中的OGLO触觉手套；每手80个压力通道，传感器分布应与原始字段映射一起使用。<span class="figure-links"><a href="https://huggingface.co/datasets/OpenGraphLabs-Research/ego-tactile-manipulation/raw/main/README.md" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://huggingface.co/datasets/OpenGraphLabs-Research/ego-tactile-manipulation/resolve/main/assets/oglo_gloves.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
<figure class="dataset-figure wide" id="fig-e06-02">
<div class="figure-type"><strong>E06 · 图2</strong><span>数据样例</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/e06-02.webp" target="_blank" rel="noopener" aria-label="放大：OpenGraph官方动作分段示例。接触边界由触觉信号自动生成，图中分段不等于人工逐帧标注。"><img src="figures/e06-02.webp" alt="OpenGraph官方动作分段示例。接触边界由触觉信号自动生成，图中分段不等于人工逐帧标注。" width="2400" height="481" loading="lazy" decoding="async"></a>
<figcaption>OpenGraph官方动作分段示例。接触边界由触觉信号自动生成，图中分段不等于人工逐帧标注。<span class="figure-links"><a href="https://huggingface.co/datasets/OpenGraphLabs-Research/ego-tactile-manipulation/raw/main/README.md" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://huggingface.co/datasets/OpenGraphLabs-Research/ego-tactile-manipulation/resolve/main/assets/action_seg_storyboard.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="E07"></a>

### E07 · Touch and Go

**分类：**人类ego＋实测触觉　**参考年份：**2022　**证据：**B（已核实官方下载入口／脚本，归档未验证；计入主统计）

**数据／项目链接：**[官方项目/许可](https://touch-and-go.github.io/)；[官方数据](https://drive.google.com/drive/folders/1NDasyshDCL9aaQzxjn_-Q5MBURRT360B)；[论文](https://arxiv.org/abs/2211.12498)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 自然环境中的视觉—触觉录制；包括原始视频、接触片段与材质标签。不同预处理版帧数不同，本文不混用250K/262K等训练子集数字。 |
| 传感器、模态与时序 | 人类手持GelSight探测器触碰物体，同时记录第一视角视频；提供接触起始/材质信息。 |
| 动作与状态 | 没有可直接执行的机器人控制动作；也没有全手压力分布。 |
| 对触觉世界模型的用途 | 自然材质、跨模态编码器、视频条件未来触觉预测。 |
| 数据许可 | CC BY 4.0 |
| 限制、证据缺口与去重 | ego指相机视角，不代表测到人类皮肤或手套压力；触觉是探头局部接触。与Sparsh/FoTa/AnyTouch中的Touch and Go去重。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-e07-01">
<div class="figure-type"><strong>E07 · 图1</strong><span>采集装置</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/e07-01.webp" target="_blank" rel="noopener" aria-label="放大：Touch and Go官方项目展示的自然环境采集：人操持GelSight探头，记录局部触觉与第一视角视觉。它不是全手皮肤压力录制。"><img src="figures/e07-01.webp" alt="Touch and Go官方项目展示的自然环境采集：人操持GelSight探头，记录局部触觉与第一视角视觉。它不是全手皮肤压力录制。" width="1800" height="2400" loading="lazy" decoding="async"></a>
<figcaption>Touch and Go官方项目展示的自然环境采集：人操持GelSight探头，记录局部触觉与第一视角视觉。它不是全手皮肤压力录制。<span class="figure-links"><a href="https://touch-and-go.github.io/" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://touch-and-go.github.io/resources/images/human_collect2.jpg" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="E08"></a>

### E08 · VTDexManip（真实人类预训练数据）

**分类：**人类ego＋实测触觉　**参考年份：**2025　**证据：**B（已核实官方下载入口／脚本，归档未验证；计入主统计）

**数据／项目链接：**[项目](https://lqts.github.io/VTDexManip/)；[官方采集图](https://lqts.github.io/VTDexManip/resources/dataset.jpg)；[官方数据下载说明](https://github.com/LQTS/VTDexManipPretraining)；[论文](https://openreview.net/forum?id=jf7C7EGw21)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 官方项目确认10种日常操作、182物体；机器人评测另有6项仿真任务。 |
| 传感器、模态与时序 | 人类第一视角操作视频与稀疏触觉接触信号；官方采集示意图明确展示头戴设备及ego样例。真实预训练录制与仿真RL基准须分开。 |
| 动作与状态 | 不应把仿真RL的动作字段当成人类原始录制所附的机器人控制标签。 |
| 对触觉世界模型的用途 | 人类到灵巧手的表征迁移、接触有无和多指接触模式。 |
| 数据许可 | 归档内有License.txt，本次未解包确认条款 |
| 限制、证据缺口与去重 | 部分实验对触觉二值化，不能当作稠密连续力分布。官方OneDrive下载密码为vtdexmanip；只计真实人类数据一次，不把6个任务算6个数据集。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-e08-01">
<div class="figure-type"><strong>E08 · 图1</strong><span>采集与样例</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/e08-01.webp" target="_blank" rel="noopener" aria-label="放大：VTDexManip官方数据介绍图：头戴采集、触觉手套、10种操作样例与数据统计。图中的真实人类数据与另行发布的仿真机器人评测需分开。"><img src="figures/e08-01.webp" alt="VTDexManip官方数据介绍图：头戴采集、触觉手套、10种操作样例与数据统计。图中的真实人类数据与另行发布的仿真机器人评测需分开。" width="2400" height="1213" loading="lazy" decoding="async"></a>
<figcaption>VTDexManip官方数据介绍图：头戴采集、触觉手套、10种操作样例与数据统计。图中的真实人类数据与另行发布的仿真机器人评测需分开。<span class="figure-links"><a href="https://lqts.github.io/VTDexManip/" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://lqts.github.io/VTDexManip/resources/dataset.jpg" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>



## 5. 机器人真机触觉（23个）


<a id="R01"></a>

### R01 · VisGel

**分类：**机器人真机触觉　**参考年份：**2019　**证据：**B（已核实官方下载入口／脚本，归档未验证；计入主统计）

**数据／项目链接：**[作者仓库/下载脚本](https://github.com/YunzhuLi/VisGel)；[项目](https://visgel.csail.mit.edu/)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 官方代码提供data_seen约328GB、data_unseen约83.2GB、数据列表及演示集。 |
| 传感器、模态与时序 | 机器人GelSight接触视频与外部视觉视频，包含接触前后变化。 |
| 动作与状态 | 官方主要任务是视觉↔触觉预测；本次未确认发布中可用的控制action字段。 |
| 对触觉世界模型的用途 | 触觉生成、视觉—触觉时序对齐与表征预训练。 |
| 数据许可 | 本次未确认数据专用许可 |
| 限制、证据缺口与去重 | 项目主页本次403，但作者仓库的下载说明可读；FoTa的VisGel-small是抽样/去平坦图版，不能与原库重复计数或拿其帧数代表原库。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-r01-01">
<div class="figure-type"><strong>R01 · 图1</strong><span>官方示例（含预测）</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/r01-01.webp" target="_blank" rel="noopener" aria-label="放大：VisGel官方视觉到触觉演示截帧：原图包含真实输入／目标与模型生成结果，须按图内标签区分；预测列不作为真实触觉数据。"><img src="figures/r01-01.webp" alt="VisGel官方视觉到触觉演示截帧：原图包含真实输入／目标与模型生成结果，须按图内标签区分；预测列不作为真实触觉数据。" width="400" height="228" loading="lazy" decoding="async"></a>
<figcaption>VisGel官方视觉到触觉演示截帧：原图包含真实输入／目标与模型生成结果，须按图内标签区分；预测列不作为真实触觉数据。<span class="figure-links"><a href="https://github.com/YunzhuLi/VisGel/blob/master/README.md" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://github.com/YunzhuLi/VisGel/blob/master/imgs/vision2touch_0.gif" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="R02"></a>

### R02 · Calandra抓取数据 / The Feeling of Success

**分类：**机器人真机触觉　**参考年份：**2017　**证据：**B（已核实官方下载入口／脚本，归档未验证；计入主统计）

**数据／项目链接：**[原项目](https://sites.google.com/view/the-feeling-of-success/)；[可用汇编入口](https://huggingface.co/datasets/alanz-mit/FoundationTactile)；[Sparsh下载说明](https://github.com/facebookresearch/sparsh)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 多物体抓取试验；不同论文/预处理版的trial与双指image计数不同，本文不合并成一个未经核对的总数。 |
| 传感器、模态与时序 | 机器人夹爪双GelSight图像、视觉、抓取成功/失败标签。 |
| 动作与状态 | 主要监督是抓取结果；不能默认每例具有长时程完整动作。 |
| 对触觉世界模型的用途 | 抓取稳定性、接触状态、触觉编码器及成功预测。 |
| 数据许可 | 原始数据许可本次未确认 |
| 限制、证据缺口与去重 | Calandra17、后续抓取版本及FoTa/Sparsh重打包按一个来源家族计，避免版本膨胀。FoTa源码表确认收录该来源。 |

<div class="dataset-figures">
<figure class="dataset-figure" id="fig-r02-01">
<div class="figure-type"><strong>R02 · 图1</strong><span>采集装置</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/r02-01.webp" target="_blank" rel="noopener" aria-label="放大：The Feeling of Success论文图1：Sawyer机械臂与双GelSight平行夹爪。"><img src="figures/r02-01.webp" alt="The Feeling of Success论文图1：Sawyer机械臂与双GelSight平行夹爪。" width="683" height="1024" loading="lazy" decoding="async"></a>
<figcaption>The Feeling of Success论文图1：Sawyer机械臂与双GelSight平行夹爪。<span class="figure-links"><a href="https://arxiv.org/html/1710.05512v2" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://arxiv.org/html/1710.05512v2/fig/sawyer_gelsight.jpg" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
<figure class="dataset-figure wide" id="fig-r02-pdf-fig2">
<div class="figure-type"><strong>R02 · 图2</strong><span>论文真实触觉样例</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/r02-pdf-fig2.webp" target="_blank" rel="noopener" aria-label="放大：The Feeling of Success论文图2（PDF第2页裁图）：不同物体在GelSight上产生的原始接触图像。只裁去页边和无关正文，图像内容未改写。"><img src="figures/r02-pdf-fig2.webp" alt="The Feeling of Success论文图2（PDF第2页裁图）：不同物体在GelSight上产生的原始接触图像。只裁去页边和无关正文，图像内容未改写。" width="977" height="346" loading="lazy" decoding="async"></a>
<figcaption>The Feeling of Success论文图2（PDF第2页裁图）：不同物体在GelSight上产生的原始接触图像。只裁去页边和无关正文，图像内容未改写。<span class="figure-links"><a href="https://arxiv.org/abs/1710.05512" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://arxiv.org/pdf/1710.05512" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="R03"></a>

### R03 · ObjectFolder-Real

**分类：**机器人真机触觉　**参考年份：**2023　**证据：**B（已核实官方下载入口／脚本，归档未验证；计入主统计）

**数据／项目链接：**[官方数据与结构](https://objectfolder.stanford.edu/objectfolder-real-download)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 100个真实家居物体；每物体约30–50个接触位置；多分辨率mesh、视觉、声音和触觉。 |
| 传感器、模态与时序 | GelSight系光学触觉视频、手上相机和第三视角接触视频；另有敲击声及敲击接触力。 |
| 动作与状态 | 接触位置/物体几何提供条件；非通用机器人操作action轨迹。 |
| 对触觉世界模型的用途 | 几何—材质—触觉对齐、局部形变过程和多模态预训练。 |
| 数据许可 | 本次未确认数据专用许可 |
| 限制、证据缺口与去重 | 与ObjectFolder 1.0/2.0的合成数据区别；声学敲击实验的力曲线不能自动当成触觉按压实验的力真值。官方明确触觉读数是视频，不能误写成只有静态图。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-r03-01">
<div class="figure-type"><strong>R03 · 图1</strong><span>数据概览</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/r03-01.webp" target="_blank" rel="noopener" aria-label="放大：ObjectFolder官方ObjectFolder-Real物体集合图。该库另有视觉、声音和真实触觉视频；这里展示采集对象，不是三种模态的同步样例。"><img src="figures/r03-01.webp" alt="ObjectFolder官方ObjectFolder-Real物体集合图。该库另有视觉、声音和真实触觉视频；这里展示采集对象，不是三种模态的同步样例。" width="2400" height="1349" loading="lazy" decoding="async"></a>
<figcaption>ObjectFolder官方ObjectFolder-Real物体集合图。该库另有视觉、声音和真实触觉视频；这里展示采集对象，不是三种模态的同步样例。<span class="figure-links"><a href="https://objectfolder.stanford.edu/" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://objectfolder.stanford.edu/assets/img/objectfolder/ObjectFolder-Real.jpg" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="R04"></a>

### R04 · Eagle Shoal / Visual-Tactile Dataset

**分类：**机器人真机触觉　**参考年份：**2019　**证据：**B（已核实官方下载入口／脚本，归档未验证；计入主统计）

**数据／项目链接：**[数据仓库/许可](https://github.com/tsinghua-rll/Visual-Tactile_Dataset)；[论文](https://doi.org/10.1177/1729881418821571)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | Intel Eagle Shoal机器人手采集的多模态抓取数据；本次未重新统计总样本数。 |
| 传感器、模态与时序 | 视觉、非光学触觉读数及抓取相关数据；官方仓库说明数据文件格式。 |
| 动作与状态 | 机器人抓取上下文；是否具备目标控制命令应以文件字段为准。 |
| 对触觉世界模型的用途 | 数值式触觉与视觉融合、抓取识别。 |
| 数据许可 | CDLA-Permissive 1.0 |
| 限制、证据缺口与去重 | 不是GelSight图像库，不能直接套用只处理光学图像的加载器。 |


<a id="R05"></a>

### R05 · PoseIt

**分类：**机器人真机触觉　**参考年份：**2022　**证据：**B（已核实官方下载入口／脚本，归档未验证；计入主统计）

**数据／项目链接：**[官方数据说明/Drive](https://github.com/Robo-Touch/PoseIt)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 1,840次抓取数据点、26物体、16种持握姿态。 |
| 传感器、模态与时序 | UR5e+WSG50，GelSight和WSG-DSA触觉、RGB-D、六维力/力矩、关节位置/速度和夹爪力；抓取—变姿态—晃动检验。 |
| 动作与状态 | 有机器人状态和稳定性相关标签；控制命令须再核对。 |
| 对触觉世界模型的用途 | 持握稳定性、姿态变化与滑移风险。 |
| 数据许可 | 数据CC BY-SA 4.0；代码MIT |
| 限制、证据缺口与去重 | 16×26不是总trial数；不要把单次抓取样本直接视为长任务轨迹。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-r05-01">
<div class="figure-type"><strong>R05 · 图1</strong><span>数据模态</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/r05-01.webp" target="_blank" rel="noopener" aria-label="放大：PoseIt官方仓库的多模态观测图：机器人抓取中结合视觉、光学／阵列触觉与力学信息。"><img src="figures/r05-01.webp" alt="PoseIt官方仓库的多模态观测图：机器人抓取中结合视觉、光学／阵列触觉与力学信息。" width="1715" height="926" loading="lazy" decoding="async"></a>
<figcaption>PoseIt官方仓库的多模态观测图：机器人抓取中结合视觉、光学／阵列触觉与力学信息。<span class="figure-links"><a href="https://github.com/Robo-Touch/PoseIt/blob/main/README.md" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://github.com/CMURoboTouch/PoseIt/blob/main/figures/data_modalities-1-1.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
<figure class="dataset-figure" id="fig-r05-02">
<div class="figure-type"><strong>R05 · 图2</strong><span>采集对象</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/r05-02.webp" target="_blank" rel="noopener" aria-label="放大：PoseIt官方物体集合；用于比较不同材质、几何与持握稳定性。"><img src="figures/r05-02.webp" alt="PoseIt官方物体集合；用于比较不同材质、几何与持握稳定性。" width="481" height="324" loading="lazy" decoding="async"></a>
<figcaption>PoseIt官方物体集合；用于比较不同材质、几何与持握稳定性。<span class="figure-links"><a href="https://github.com/Robo-Touch/PoseIt/blob/main/README.md" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://github.com/CMURoboTouch/PoseIt/blob/main/figures/object_set.PNG" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="R06"></a>

### R06 · YCB-Sight（真实部分）

**分类：**机器人真机触觉　**参考年份：**2021　**证据：**B（已核实官方下载入口／脚本，归档未验证；计入主统计）

**数据／项目链接：**[官方数据/逐物体下载](https://github.com/Robo-Touch/YCB-Sight)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 官方列30个仿真物体、6个真实物体；主计数只纳入真实部分。 |
| 传感器、模态与时序 | GelSight触觉、Azure Kinect RGB-D、物体模型和接触几何。 |
| 动作与状态 | 几何与位姿条件，非通用操作动作数据。 |
| 对触觉世界模型的用途 | 视觉—触觉几何对齐、位姿与sim-to-real研究。 |
| 数据许可 | 数据CC BY-SA 4.0；代码MIT |
| 限制、证据缺口与去重 | 与YCB-Slide是不同采集项目；真实与仿真文件必须分开，不把全部30物体都写成真机。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-r06-01">
<div class="figure-type"><strong>R06 · 图1</strong><span>真实数据样例</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/r06-01.webp" target="_blank" rel="noopener" aria-label="放大：YCB-Sight作者仓库明确标为real的演示截帧；与同仓库的仿真展示分开。"><img src="figures/r06-01.webp" alt="YCB-Sight作者仓库明确标为real的演示截帧；与同仓库的仿真展示分开。" width="600" height="203" loading="lazy" decoding="async"></a>
<figcaption>YCB-Sight作者仓库明确标为real的演示截帧；与同仓库的仿真展示分开。<span class="figure-links"><a href="https://github.com/Robo-Touch/YCB-Sight" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://github.com/Robo-Touch/YCB-Sight/blob/main/media/YCBSight-real.gif" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="R07"></a>

### R07 · RH20T（触觉配置）

**分类：**机器人真机触觉　**参考年份：**2023　**证据：**A（已核实实际数据文件目录；计入主统计）

**数据／项目链接：**[官方项目/下载](https://rh20t.github.io/)；[配置/API](https://github.com/rh20t/rh20t_api)；[公开转换包](https://huggingface.co/datasets/MJJJJ1064/FTP-1-Dataset/tree/main/RH20TCfg7Tactile)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | RH20T全库规模不能当作触觉子集规模；本报告仅将有触觉的配置计一次。 |
| 传感器、模态与时序 | Cfg7的uSkin阵列触觉与腕部F/T、视觉和机器人状态；Cfg5/6主要是关节力矩/腕力。 |
| 动作与状态 | 机器人操作时序、状态与相应动作信息；应按配置读取。 |
| 对触觉世界模型的用途 | 阵列触觉机器人后训练、接触丰富操作；与人手压力编码器适配。 |
| 数据许可 | 原始条款以官方数据申请/下载说明为准；本次未确认独立数据许可 |
| 限制、证据缺口与去重 | FTP公开包中RH20TCfg7Tactile有归档；Cfg5/6不是额外两个触觉数据集。不能把所有RH20T视频/人类示范都声称带触觉。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-r07-pdf-fig5">
<div class="figure-type"><strong>R07 · 图1</strong><span>采集平台概览</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/r07-pdf-fig5.webp" target="_blank" rel="noopener" aria-label="放大：RH20T论文图5（PDF第5页裁图）：多视角视觉、机器人与力反馈采集平台。此图说明整套采集方式，不能用它推断所有配置都带指尖触觉；本报告只统计Cfg7触觉部分。"><img src="figures/r07-pdf-fig5.webp" alt="RH20T论文图5（PDF第5页裁图）：多视角视觉、机器人与力反馈采集平台。此图说明整套采集方式，不能用它推断所有配置都带指尖触觉；本报告只统计Cfg7触觉部分。" width="600" height="528" loading="lazy" decoding="async"></a>
<figcaption>RH20T论文图5（PDF第5页裁图）：多视角视觉、机器人与力反馈采集平台。此图说明整套采集方式，不能用它推断所有配置都带指尖触觉；本报告只统计Cfg7触觉部分。<span class="figure-links"><a href="https://rh20t.github.io/" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://rh20t.github.io/static/RH20T_paper_compressed.pdf" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="R08"></a>

### R08 · Reactive Diffusion Policy（RDP）数据

**分类：**机器人真机触觉　**参考年份：**2025　**证据：**A（已核实实际数据文件目录；计入主统计）

**数据／项目链接：**[官方数据](https://huggingface.co/datasets/WendiChen/reactive_diffusion_policy_dataset)；[代码/字段](https://github.com/xiaoxiaoxh/reactive_diffusion_policy)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 公开full/mini和触觉表征采集子集；任务含peeling、wiping、lifting。 |
| 传感器、模态与时序 | GelSight Mini/MCTac图像与marker位移、外部/腕部RGB、TCP姿态/速度、夹爪力/宽度、腕部wrench和时间戳。 |
| 动作与状态 | 公开Zarr示例明确有action、target、状态；不同任务/重采样频率须保持一致。 |
| 对触觉世界模型的用途 | 优先：动作条件触觉预测、快慢时序建模、接触反应控制。 |
| 数据许可 | MIT |
| 限制、证据缺口与去重 | 单臂/双臂、raw/processed、mini/full都按一个来源家族；PCA采集时长不是操作示范总时长。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-r08-01">
<div class="figure-type"><strong>R08 · 图1</strong><span>采集装置</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/r08-01.webp" target="_blank" rel="noopener" aria-label="放大：RDP论文图2：TactAR遥操作采集系统及触觉／力反馈。用于理解真实机器人状态、动作与触觉流的采集关系。"><img src="figures/r08-01.webp" alt="RDP论文图2：TactAR遥操作采集系统及触觉／力反馈。用于理解真实机器人状态、动作与触觉流的采集关系。" width="2043" height="600" loading="lazy" decoding="async"></a>
<figcaption>RDP论文图2：TactAR遥操作采集系统及触觉／力反馈。用于理解真实机器人状态、动作与触觉流的采集关系。<span class="figure-links"><a href="https://arxiv.org/html/2503.02881v3" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://arxiv.org/html/2503.02881v3/system_layout.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
<figure class="dataset-figure wide" id="fig-r08-02">
<div class="figure-type"><strong>R08 · 图2</strong><span>触觉样例</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/r08-02.webp" target="_blank" rel="noopener" aria-label="放大：RDP论文图3：不同接触模式下GelSight Mini的marker形变；形变场是触觉图像中的运动观测，不直接等同于标定力值。"><img src="figures/r08-02.webp" alt="RDP论文图3：不同接触模式下GelSight Mini的marker形变；形变场是触觉图像中的运动观测，不直接等同于标定力值。" width="1115" height="346" loading="lazy" decoding="async"></a>
<figcaption>RDP论文图3：不同接触模式下GelSight Mini的marker形变；形变场是触觉图像中的运动观测，不直接等同于标定力值。<span class="figure-links"><a href="https://arxiv.org/html/2503.02881v3" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://arxiv.org/html/2503.02881v3/marker_motion.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
<figure class="dataset-figure" id="fig-r08-03">
<div class="figure-type"><strong>R08 · 图3</strong><span>任务样例</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/r08-03.webp" target="_blank" rel="noopener" aria-label="放大：RDP论文图7：剥离、擦拭与双臂搬运三类接触丰富任务。"><img src="figures/r08-03.webp" alt="RDP论文图7：剥离、擦拭与双臂搬运三类接触丰富任务。" width="985" height="747" loading="lazy" decoding="async"></a>
<figcaption>RDP论文图7：剥离、擦拭与双臂搬运三类接触丰富任务。<span class="figure-links"><a href="https://arxiv.org/html/2503.02881v3" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://arxiv.org/html/2503.02881v3/task.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="R09"></a>

### R09 · UniT数据

**分类：**机器人真机触觉　**参考年份：**2024　**证据：**A（已核实实际数据文件目录；计入主统计）

**数据／项目链接：**[作者仓库/原始Drive](https://github.com/ZhengtongXu/UniT)；[公开转换包](https://huggingface.co/datasets/MJJJJ1064/FTP-1-Dataset/tree/main/Unit)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 原作者README：挂鸡腿200、薯片抓取180、六角扳手插入170示范；另有表征和手内姿态子集。 |
| 传感器、模态与时序 | 带markers的GelSight Mini、视觉和真实机器人操作数据。 |
| 动作与状态 | 有操作策略学习轨迹；不是只有单物体触觉表征图。 |
| 对触觉世界模型的用途 | 小规模动作条件动态基准，插装/易损物体操作。 |
| 数据许可 | 原始数据许可本次未确认 |
| 限制、证据缺口与去重 | FTP Unit/Unit_Bimanual不再额外计数。原表征“单物体20分钟内”等描述不能套到全部策略数据。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-r09-01">
<div class="figure-type"><strong>R09 · 图1</strong><span>数据可视化</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/r09-01.webp" target="_blank" rel="noopener" aria-label="放大：UniT官方Rerun数据可视化截帧，用于展示多路视觉、触觉与运动数据的共同时间轴。"><img src="figures/r09-01.webp" alt="UniT官方Rerun数据可视化截帧，用于展示多路视觉、触觉与运动数据的共同时间轴。" width="640" height="360" loading="lazy" decoding="async"></a>
<figcaption>UniT官方Rerun数据可视化截帧，用于展示多路视觉、触觉与运动数据的共同时间轴。<span class="figure-links"><a href="https://github.com/ZhengtongXu/UniT/blob/main/README.md" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://github.com/ZhengtongXu/UniT/blob/main/teasers/rerun_example.gif" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="R10"></a>

### R10 · VLA-Touch数据

**分类：**机器人真机触觉　**参考年份：**2025　**证据：**A（已核实实际数据文件目录；计入主统计）

**数据／项目链接：**[官方数据](https://huggingface.co/datasets/AllenBi21/VLA-Touch)；[原作者仓库](https://github.com/jxbi1010/VLA-Touch)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 官方提供原始Drive及HF processed数据；本次未重新计算轨迹总数。 |
| 传感器、模态与时序 | Franka机器人视觉、GelSight Mini触觉/marker信息和控制轨迹。 |
| 动作与状态 | 用于触觉反馈策略的机器人动作与观测。 |
| 对触觉世界模型的用途 | 机器人动作修正、接触阶段条件建模。 |
| 数据许可 | 代码MIT；数据专用许可本次未确认 |
| 限制、证据缺口与去重 | 不要与Tactile-VLA混名。FTP中VLA_touch是重打包；其时间戳/物理FPS应单独审计。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-r10-01">
<div class="figure-type"><strong>R10 · 图1</strong><span>触觉样例与文本</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/r10-01.webp" target="_blank" rel="noopener" aria-label="放大：VLA-Touch论文图3：原始触觉图、生成的语言描述及规划响应。原图同时含模型输出，文字描述不是物理传感器真值。"><img src="figures/r10-01.webp" alt="VLA-Touch论文图3：原始触觉图、生成的语言描述及规划响应。原图同时含模型输出，文字描述不是物理传感器真值。" width="2400" height="918" loading="lazy" decoding="async"></a>
<figcaption>VLA-Touch论文图3：原始触觉图、生成的语言描述及规划响应。原图同时含模型输出，文字描述不是物理传感器真值。<span class="figure-links"><a href="https://arxiv.org/html/2507.17294v2" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://arxiv.org/html/2507.17294v2/4_planning.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="R11"></a>

### R11 · OmniViTac（OmniVTA数据）

**分类：**机器人真机触觉　**参考年份：**2026　**证据：**A（已核实实际数据文件目录；计入主统计）

**数据／项目链接：**[官方数据](https://huggingface.co/datasets/tars-robotics/OmniVitac)；[论文](https://arxiv.org/abs/2603.19201)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 公开目录有多个操作任务，未从目录推断全论文训练小时数。 |
| 传感器、模态与时序 | 多类光学触觉传感器、机器人视觉与操作轨迹；论文框架支持跨传感器触觉策略。 |
| 动作与状态 | 机器人操作数据；具体每任务字段按公开文件核对。 |
| 对触觉世界模型的用途 | 跨传感器接触建模、策略与世界模型联合训练候选。 |
| 数据许可 | CC BY-NC 4.0 |
| 限制、证据缺口与去重 | OmniVTA、OmniVTLA和OmniViTac三个名字有区别；数据已发布不代表全部模型代码也已完整发布。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-r11-01">
<div class="figure-type"><strong>R11 · 图1</strong><span>采集装置</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/r11-01.webp" target="_blank" rel="noopener" aria-label="放大：OmniViTac论文图2：机器人／TacUMI采集平台、可更换触觉末端与数据概览。图示包含不同本体，不能把所有轨迹一概视为机器人自主执行。"><img src="figures/r11-01.webp" alt="OmniViTac论文图2：机器人／TacUMI采集平台、可更换触觉末端与数据概览。图示包含不同本体，不能把所有轨迹一概视为机器人自主执行。" width="2400" height="1109" loading="lazy" decoding="async"></a>
<figcaption>OmniViTac论文图2：机器人／TacUMI采集平台、可更换触觉末端与数据概览。图示包含不同本体，不能把所有轨迹一概视为机器人自主执行。<span class="figure-links"><a href="https://arxiv.org/html/2603.19201v3" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://arxiv.org/html/2603.19201v3/fig/dataset_teaser_0630.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
<figure class="dataset-figure wide" id="fig-r11-02">
<div class="figure-type"><strong>R11 · 图2</strong><span>数据样例</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/r11-02.webp" target="_blank" rel="noopener" aria-label="放大：OmniViTac论文图3：装配、切割、调整、剥离、抓取和擦拭六类视觉—触觉交互样例。"><img src="figures/r11-02.webp" alt="OmniViTac论文图3：装配、切割、调整、剥离、抓取和擦拭六类视觉—触觉交互样例。" width="2048" height="969" loading="lazy" decoding="async"></a>
<figcaption>OmniViTac论文图3：装配、切割、调整、剥离、抓取和擦拭六类视觉—触觉交互样例。<span class="figure-links"><a href="https://arxiv.org/html/2603.19201v3" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://arxiv.org/html/2603.19201v3/OmniVTA-6pattern.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="R12"></a>

### R12 · HapTile

**分类：**机器人真机触觉　**参考年份：**2026　**证据：**A（已核实实际数据文件目录；计入主统计）

**数据／项目链接：**[数据](https://huggingface.co/datasets/HapTile2026/HapTile)；[字段定义](https://haptile-dataset.github.io/)；[下载可视化工具](https://github.com/HapTile-dataset/Dataset_quickstart)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | HF有按任务打包的ZIP；涵盖折叠、插入、移动、按压、放置、移除、堆叠、旋转和擦拭。 |
| 传感器、模态与时序 | UR5e+Robotiq 2F-85双指光学触觉，外部/腕部视觉、语言指令、marker运动、机器人状态。 |
| 动作与状态 | HDF5 frames/control为6维delta pose+夹爪控制；逐示范freq.txt与时间戳。 |
| 对触觉世界模型的用途 | 优先：有明确动作字段的接触动态/策略模型训练。 |
| 数据许可 | CC BY 4.0 |
| 限制、证据缺口与去重 | HDF5中的frames/videos是组而非普通目录；频率读取元数据，不能把任务ZIP个数当成示范个数。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-r12-collection">
<div class="figure-type"><strong>R12 · 图1</strong><span>官方采集视频截帧</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/r12-collection.webp" target="_blank" rel="noopener" aria-label="放大：HapTile官方datacollection_process视频第4秒截帧：多次机器人搬放瓶子的采集过程。画面展示不同演示的动作差异。"><img src="figures/r12-collection.webp" alt="HapTile官方datacollection_process视频第4秒截帧：多次机器人搬放瓶子的采集过程。画面展示不同演示的动作差异。" width="1920" height="1080" loading="lazy" decoding="async"></a>
<figcaption>HapTile官方datacollection_process视频第4秒截帧：多次机器人搬放瓶子的采集过程。画面展示不同演示的动作差异。<span class="figure-links"><a href="https://haptile-dataset.github.io/" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://haptile-dataset.github.io/static/videos/datacollection_process.mov" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
<figure class="dataset-figure" id="fig-r12-markers">
<div class="figure-type"><strong>R12 · 图2</strong><span>真实触觉与派生特征</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/r12-markers.webp" target="_blank" rel="noopener" aria-label="放大：HapTile官方marker_tracking视频第2秒截帧：场景视觉、左右指原始触觉与marker追踪并列。右侧箭头是计算的运动特征，不是额外力传感器真值。"><img src="figures/r12-markers.webp" alt="HapTile官方marker_tracking视频第2秒截帧：场景视觉、左右指原始触觉与marker追踪并列。右侧箭头是计算的运动特征，不是额外力传感器真值。" width="1250" height="950" loading="lazy" decoding="async"></a>
<figcaption>HapTile官方marker_tracking视频第2秒截帧：场景视觉、左右指原始触觉与marker追踪并列。右侧箭头是计算的运动特征，不是额外力传感器真值。<span class="figure-links"><a href="https://haptile-dataset.github.io/" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://haptile-dataset.github.io/static/videos/marker_tracking.mov" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="R13"></a>

### R13 · LET-Dex Dataset

**分类：**机器人真机触觉　**参考年份：**2025　**证据：**A（已核实实际数据文件目录；计入主统计）

**数据／项目链接：**[官方数据](https://huggingface.co/datasets/LejuRobotics/LET-Dex-Dataset)；[转换工具](https://github.com/LejuRobotics/kuavo_data_challenge)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 官方首批真机灵巧手力触觉数据；本次以可见文件为发布证据，不把整个LET平台总量纳入。 |
| 传感器、模态与时序 | Kuavo 4 Pro/轮式平台、Linker Hand L6；5指6×12触觉阵列、六维力/力矩、RGB-D、关节及任务标注。 |
| 动作与状态 | ROSbag原始流和转换工具；机器人控制/状态须按topic区分。 |
| 对触觉世界模型的用途 | 优先：分布式指尖触觉、灵巧手动作后训练。 |
| 数据许可 | CC BY-NC-SA 4.0 |
| 限制、证据缺口与去重 | LET全库和LET-Dex触觉子库不能重复计数；数据卡部分旧下载别名与现仓库名不同，优先使用本条HF地址。 |

<div class="dataset-figures">
<figure class="dataset-figure" id="fig-r13-01">
<div class="figure-type"><strong>R13 · 图1</strong><span>采集硬件</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/r13-01.webp" target="_blank" rel="noopener" aria-label="放大：LET-Dex官方数据卡中的Linker Hand灵巧手硬件。触觉阵列与六维力传感器应按各自topic读取。"><img src="figures/r13-01.webp" alt="LET-Dex官方数据卡中的Linker Hand灵巧手硬件。触觉阵列与六维力传感器应按各自topic读取。" width="508" height="483" loading="lazy" decoding="async"></a>
<figcaption>LET-Dex官方数据卡中的Linker Hand灵巧手硬件。触觉阵列与六维力传感器应按各自topic读取。<span class="figure-links"><a href="https://huggingface.co/datasets/LejuRobotics/LET-Dex-Dataset/raw/main/README.md" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://huggingface.co/datasets/LejuRobotics/LET-Dex-Dataset/resolve/main/docs/images/linker_hand.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
<figure class="dataset-figure wide" id="fig-r13-02">
<div class="figure-type"><strong>R13 · 图2</strong><span>数据可视化</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/r13-02.webp" target="_blank" rel="noopener" aria-label="放大：LET-Dex官方数据可视化示例；用来了解机器人、相机和触觉流的布局，不代表所有录制均包含完整模态。"><img src="figures/r13-02.webp" alt="LET-Dex官方数据可视化示例；用来了解机器人、相机和触觉流的布局，不代表所有录制均包含完整模态。" width="1872" height="924" loading="lazy" decoding="async"></a>
<figcaption>LET-Dex官方数据可视化示例；用来了解机器人、相机和触觉流的布局，不代表所有录制均包含完整模态。<span class="figure-links"><a href="https://huggingface.co/datasets/LejuRobotics/LET-Dex-Dataset/raw/main/README.md" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://huggingface.co/datasets/LejuRobotics/LET-Dex-Dataset/resolve/main/docs/images/Visualize+Datasets.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="R14"></a>

### R14 · Humanoid Everyday（触觉部分）

**分类：**机器人真机触觉　**参考年份：**2025　**证据：**A（已核实实际数据文件目录；计入主统计）

**数据／项目链接：**[官方数据](https://huggingface.co/datasets/USC-PSI-Lab/humanoid-everyday)；[论文](https://arxiv.org/abs/2510.08807)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 作者数据卡：260+任务、7大类，30Hz多模态；仅触觉可用部分应进入触觉训练。 |
| 传感器、模态与时序 | 人形机器人遥操作；RGB、depth、LiDAR、触觉与IMU；FTP引用Unitree Dex3相关信号。 |
| 动作与状态 | 机器人操作序列，提供LeRobot及原始下载表。 |
| 对触觉世界模型的用途 | 人形/灵巧手本体多样性、触觉与全身操作关联。 |
| 数据许可 | Apache 2.0 |
| 限制、证据缺口与去重 | 不能假设每条、每个平台都含同一触觉字段；数据卡使用过USC-GVL旧命名，现发布方为USC-PSI-Lab。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-r14-01">
<div class="figure-type"><strong>R14 · 图1</strong><span>采集装置</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/r14-01.webp" target="_blank" rel="noopener" aria-label="放大：Humanoid Everyday论文的数据采集设置。整库含多种模态，本报告仅将实际含触觉的部分作为触觉训练候选。"><img src="figures/r14-01.webp" alt="Humanoid Everyday论文的数据采集设置。整库含多种模态，本报告仅将实际含触觉的部分作为触觉训练候选。" width="2400" height="1732" loading="lazy" decoding="async"></a>
<figcaption>Humanoid Everyday论文的数据采集设置。整库含多种模态，本报告仅将实际含触觉的部分作为触觉训练候选。<span class="figure-links"><a href="https://arxiv.org/html/2510.08807v3" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://arxiv.org/html/2510.08807v3/images/datacollection-1.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="R15"></a>

### R15 · 白虎-VTouch / OpenLoong visuo-tactile

**分类：**机器人真机触觉　**参考年份：**2026　**证据：**A（已核实实际数据文件目录；计入主统计）

**数据／项目链接：**[官方数据](https://www.modelscope.cn/datasets/OpenLoong-Community/visuo-tactile)；[官方可读数据卡](https://www.modelscope.cn/datasets/OpenLoong-Community/visuo-tactile.md)；[可见归档](https://huggingface.co/datasets/MJJJJ1064/FTP-1-Dataset/tree/main/VisuoTactile_D-WHEEL)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 官方ModelScope条目标示约16TB；含青龙、D-WHEEL及手持PIKA平台，100+原子技能（不是已核实任务数）。 |
| 传感器、模态与时序 | 多路真实视触觉、头/腕RGB-D、关节位姿与HDF5操作序列。 |
| 动作与状态 | 机器人与手持工具子集分别有运动字段，需按平台处理。 |
| 对触觉世界模型的用途 | 大规模视触觉操作候选；可用于跨本体动作条件模型。 |
| 数据许可 | CC BY-NC-SA 4.0 |
| 限制、证据缺口与去重 | 这里按一个跨本体来源家族计入R；PIKA部分不再计入U。FTP的VisuoTactile_D-WHEEL/QINGLOONG也是该家族，不能再加两个；16TB不是小时数。 |

<div class="dataset-figures">
<figure class="dataset-figure" id="fig-r15-01">
<div class="figure-type"><strong>R15 · 图1</strong><span>场景分布</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/r15-01.webp" target="_blank" rel="noopener" aria-label="放大：白虎-VTouch官方数据卡的采集场景分布图；这是场景统计，不是逐条轨迹质量或完整触觉覆盖的证明。"><img src="figures/r15-01.webp" alt="白虎-VTouch官方数据卡的采集场景分布图；这是场景统计，不是逐条轨迹质量或完整触觉覆盖的证明。" width="2096" height="1880" loading="lazy" decoding="async"></a>
<figcaption>白虎-VTouch官方数据卡的采集场景分布图；这是场景统计，不是逐条轨迹质量或完整触觉覆盖的证明。<span class="figure-links"><a href="https://www.modelscope.cn/datasets/OpenLoong-Community/visuo-tactile.md" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://openloong-apps-dev-public.obs.cn-east-3.myhuaweicloud.com/videos/visuo_tactile_readme/scenario.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
<figure class="dataset-figure" id="fig-r15-02">
<div class="figure-type"><strong>R15 · 图2</strong><span>触觉样例</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/r15-02.webp" target="_blank" rel="noopener" aria-label="放大：VTouch官方左手左指光学触觉视频的截帧；这是传感器内部接触图像，不能当成外部场景RGB。"><img src="figures/r15-02.webp" alt="VTouch官方左手左指光学触觉视频的截帧；这是传感器内部接触图像，不能当成外部场景RGB。" width="320" height="320" loading="lazy" decoding="async"></a>
<figcaption>VTouch官方左手左指光学触觉视频的截帧；这是传感器内部接触图像，不能当成外部场景RGB。<span class="figure-links"><a href="https://www.modelscope.cn/datasets/OpenLoong-Community/visuo-tactile.md" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://openloong-apps-dev-public.obs.cn-east-3.myhuaweicloud.com/videos/visuo_tactile_readme/hand_left_left.gif" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="R16"></a>

### R16 · Sharpa North-FTP-1

**分类：**机器人真机触觉　**参考年份：**2026　**证据：**A（已核实实际数据文件目录；计入主统计）

**数据／项目链接：**[公开数据](https://huggingface.co/datasets/MJJJJ1064/FTP-1-Dataset/tree/main/sharpa)；[论文及来源表](https://arxiv.org/abs/2606.13102)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | FTP-1论文报告约4,000条长时程灵巧操作示范；公开包有sharpa分卷。 |
| 传感器、模态与时序 | Sharpa North灵巧手、DTC视触觉及机器人视觉/状态/动作。 |
| 动作与状态 | 灵巧手操作轨迹；具体控制坐标与触觉时间轴应读取解包schema。 |
| 对触觉世界模型的用途 | 光学指尖触觉灵巧手的动作条件建模和长任务学习。 |
| 数据许可 | 汇编平台标MIT；独立原始数据授权仍需核对 |
| 限制、证据缺口与去重 | 这是FTP作者新增采集来源，计一次；不额外计FTP汇编。4,000是论文报告，不是本次解包后的完整轨迹统计。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-r16-01">
<div class="figure-type"><strong>R16 · 图1</strong><span>Sharpa平台与触觉样例</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/r16-01.webp" target="_blank" rel="noopener" aria-label="放大：FTP-1论文图5的左侧Sharpa North面板：真实灵巧手平台、DTC触觉外观及任务展示。裁去了其他本体和仿真面板；图中的评测任务不自动等于公开包的训练轨迹。"><img src="figures/r16-01.webp" alt="FTP-1论文图5的左侧Sharpa North面板：真实灵巧手平台、DTC触觉外观及任务展示。裁去了其他本体和仿真面板；图中的评测任务不自动等于公开包的训练轨迹。" width="668" height="1178" loading="lazy" decoding="async"></a>
<figcaption>FTP-1论文图5的左侧Sharpa North面板：真实灵巧手平台、DTC触觉外观及任务展示。裁去了其他本体和仿真面板；图中的评测任务不自动等于公开包的训练轨迹。<span class="figure-links"><a href="https://arxiv.org/html/2606.13102v2" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://arxiv.org/html/2606.13102v2/hardware_settings_0519_seen.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="R17"></a>

### R17 · Tactile MNIST（Real）

**分类：**机器人真机触觉　**参考年份：**2025　**证据：**A（已核实实际数据文件目录；计入主统计）

**数据／项目链接：**[真实序列数据](https://huggingface.co/datasets/TimSchneider42/tactile-mnist-touch-real-seq-t256-320x240)；[官方工具](https://github.com/TimSchneider42/tactile-mnist)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | Franka在3D打印MNIST数字上采集；真实单帧/序列和不同分辨率是同一来源的不同表示。 |
| 传感器、模态与时序 | 真实光学触觉图像/序列、数字形状分类标签；套件还含独立仿真任务。 |
| 动作与状态 | 适合主动感知/形状识别；不等于多任务操作指令数据。 |
| 对触觉世界模型的用途 | 受控几何感知基准、主动触摸与接触覆盖。 |
| 数据许可 | CC BY 2.0（真实序列数据卡）；代码MIT |
| 限制、证据缺口与去重 | 仅Real计数；synthetic、ABC mesh、Starstruck和缩放图像版不增加真实数据集数量。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-r17-01">
<div class="figure-type"><strong>R17 · 图1</strong><span>真实采集装置</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/r17-01.webp" target="_blank" rel="noopener" aria-label="放大：Tactile MNIST官方标为Real Interactions的采集图：机器人探测3D打印数字，下方展示触觉观测。未将同项目的模拟环境或CycleGAN生成图冒充真实录制。"><img src="figures/r17-01.webp" alt="Tactile MNIST官方标为Real Interactions的采集图：机器人探测3D打印数字，下方展示触觉观测。未将同项目的模拟环境或CycleGAN生成图冒充真实录制。" width="1280" height="1425" loading="lazy" decoding="async"></a>
<figcaption>Tactile MNIST官方标为Real Interactions的采集图：机器人探测3D打印数字，下方展示触觉观测。未将同项目的模拟环境或CycleGAN生成图冒充真实录制。<span class="figure-links"><a href="https://timschneider42.github.io/tactile-mnist/" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://timschneider42.github.io/tactile-mnist/img/dataset/data_collection.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="R18"></a>

### R18 · ARQ-CRISP Slip Detection Dataset

**分类：**机器人真机触觉　**参考年份：**2021　**证据：**B（已核实官方下载入口／脚本，归档未验证；计入主统计）

**数据／项目链接：**[数据及协议](https://github.com/ARQ-CRISP/slip_detection_dataset_2021)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 3物体×3摆放姿态×10次，合计90次抓取。 |
| 传感器、模态与时序 | 真实自主pick-and-place中的触觉与滑移标签，官方HDF5按物体组织。 |
| 动作与状态 | 抓取过程时序；公开读取脚本可分离各试验。 |
| 对触觉世界模型的用途 | 滑移事件预测、接触状态切换和稳定性评估。 |
| 数据许可 | 本次未确认数据专用许可 |
| 限制、证据缺口与去重 | 物体多样性较小，不适合作为通用操作世界模型唯一来源。 |


<a id="R19"></a>

### R19 · iCub Hybrid Touch Texture Dataset

**分类：**机器人真机触觉　**参考年份：**2019　**证据：**B（已核实官方下载入口／脚本，归档未验证；计入主统计）

**数据／项目链接：**[官方数据](https://github.com/clear-nus/TactileLearning)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 23种材质，提供touch和slide试验。 |
| 传感器、模态与时序 | iCub前臂触觉皮肤、15DoF机械臂编码器、skin_events力相关信号。 |
| 动作与状态 | 机器人状态与探索过程；主要面向材质识别。 |
| 对触觉世界模型的用途 | 非光学触觉动态、静态接触与滑动联合建模。 |
| 数据许可 | 本次未确认数据专用许可 |
| 限制、证据缺口与去重 | 与后续加工后的iCub材质数据需按原始采集来源去重。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-r19-01">
<div class="figure-type"><strong>R19 · 图1</strong><span>采集对象</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/r19-01.webp" target="_blank" rel="noopener" aria-label="放大：iCub Hybrid Touch官方材料与编号图，覆盖23种纹理；编号对应触碰／滑动试验目录。"><img src="figures/r19-01.webp" alt="iCub Hybrid Touch官方材料与编号图，覆盖23种纹理；编号对应触碰／滑动试验目录。" width="1504" height="332" loading="lazy" decoding="async"></a>
<figcaption>iCub Hybrid Touch官方材料与编号图，覆盖23种纹理；编号对应触碰／滑动试验目录。<span class="figure-links"><a href="https://github.com/clear-nus/TactileLearning" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://raw.githubusercontent.com/crslab/TactileLearning/master/materials_with_id.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="R20"></a>

### R20 · Doro/Jaco OptoForce Object Classification

**分类：**机器人真机触觉　**参考年份：**2021　**证据：**B（已核实官方下载入口／脚本，归档未验证；计入主统计）

**数据／项目链接：**[原作者数据仓库](https://github.com/philipmaus/Tactile_Object_Classification)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 作者仓库Test目录提供real-life dataset；本次未确认总试验数。 |
| 传感器、模态与时序 | Doro平台Jaco机械臂、3个OptoForce传感器的物体触觉分类数据。 |
| 动作与状态 | 触觉物体识别；不默认具有长序列控制命令。 |
| 对触觉世界模型的用途 | 少通道/力式触觉的跨传感器表征补充。 |
| 数据许可 | 数据许可未明确；作者说明允许使用/修改代码不能替代数据许可 |
| 限制、证据缺口与去重 | 规模与标注粒度需读取manual.py和数据；优先级低于有完整动作的操作库。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-r20-01">
<div class="figure-type"><strong>R20 · 图1</strong><span>采集装置</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/r20-01.webp" target="_blank" rel="noopener" aria-label="放大：Doro/Jaco官方仓库采集装置：Jaco机械臂和3个OptoForce接触传感器。"><img src="figures/r20-01.webp" alt="Doro/Jaco官方仓库采集装置：Jaco机械臂和3个OptoForce接触传感器。" width="540" height="365" loading="lazy" decoding="async"></a>
<figcaption>Doro/Jaco官方仓库采集装置：Jaco机械臂和3个OptoForce接触传感器。<span class="figure-links"><a href="https://github.com/philipmaus/Tactile_Object_Classification/blob/main/README.md" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://github.com/philipmaus/Tactile_Object_Classification/blob/main/doro_setup.PNG" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="R21"></a>

### R21 · BioTac_slide_20_50

**分类：**机器人真机触觉　**参考年份：**2021　**证据：**B（已核实官方下载入口／脚本，归档未验证；计入主统计）

**数据／项目链接：**[官方数据](https://github.com/dexrob/BioTac_slide_20_50)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 20种材料，每类50条，合计1,000条滑动试验；预处理单例400×44。 |
| 传感器、模态与时序 | BioTac原始传感通道，机械臂笛卡尔位置/速度/wrench、关节位置/速度/力矩CSV；并有预切分训练数据。 |
| 动作与状态 | 包含机器人状态与控制实验上下文；不应把状态/力矩直接等同目标action。 |
| 对触觉世界模型的用途 | 传统多物理触觉、滑动材质建模、跨传感器预训练。 |
| 数据许可 | 本次未确认数据专用许可 |
| 限制、证据缺口与去重 | BioTac原始44字段不都是同一种力学量；与其compiled_data和后续论文预处理包合并为一个来源。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-r21-01">
<div class="figure-type"><strong>R21 · 图1</strong><span>采集对象</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/r21-01.webp" target="_blank" rel="noopener" aria-label="放大：BioTac_slide_20_50官方材料集合图；数据由机器人在这些材料上执行滑动接触获得。"><img src="figures/r21-01.webp" alt="BioTac_slide_20_50官方材料集合图；数据由机器人在这些材料上执行滑动接触获得。" width="1156" height="1136" loading="lazy" decoding="async"></a>
<figcaption>BioTac_slide_20_50官方材料集合图；数据由机器人在这些材料上执行滑动接触获得。<span class="figure-links"><a href="https://github.com/dexrob/BioTac_slide_20_50/blob/master/README.md" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://github.com/dexrob/BioTac_slide_20_50/blob/master/matx_collage.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="R22"></a>

### R22 · Active Clothing Material Perception（Yuan18）

**分类：**机器人真机触觉　**参考年份：**2018　**证据：**B（已核实官方下载入口／脚本，归档未验证；计入主统计）

**数据／项目链接：**[原始数据归档](http://data.csail.mit.edu/active_clothing/Data_ICRA18.tar)；[公开汇编](https://huggingface.co/datasets/alanz-mit/FoundationTactile)；[下游数据说明](https://github.com/facebookresearch/sparsh)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | Sparsh官方说明其使用版有4,467段短片、每段10–25帧、20类衣物材质。 |
| 传感器、模态与时序 | 机器人GelSight抓取衣物的触觉短视频、材料/属性相关标签。 |
| 动作与状态 | 短程抓取过程；并非通用任务动作库。 |
| 对触觉世界模型的用途 | 柔性物体与材质表征，接触形变辨识。 |
| 数据许可 | 本次未确认数据专用许可 |
| 限制、证据缺口与去重 | 本条合并Yuan18/Clothing处理版本；不把未核实的GelFabric、ViTac Cloth一概当成其别名。FoTa可作为已发布数据的补充入口。 |


<a id="R23"></a>

### R23 · SSVTP

**分类：**机器人真机触觉　**参考年份：**2023　**证据：**A（已核实实际数据文件目录；计入主统计）

**数据／项目链接：**[公开数据（TVL内ssvtp）](https://huggingface.co/datasets/mlfu7/Touch-Vision-Language-Dataset)；[来源与采集协议](https://arxiv.org/abs/2402.13232)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 4,587组image-touch pairs（TVL论文明确说明其上游SSVTP部分）。 |
| 传感器、模态与时序 | UR5机器人、DIGIT触觉与台面RGB；机器人先拍视觉再到对应位置接触。 |
| 动作与状态 | 主要是单次触碰的视觉—触觉对，不是同步动态操作序列。 |
| 对触觉世界模型的用途 | 视觉与局部触觉表征对齐。 |
| 数据许可 | 原始数据专用许可本次未确认 |
| 限制、证据缺口与去重 | 图像与触觉并非同时拍摄；TVL包内ssvtp目录是该来源，单独计源后不把TVL全包另计。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-r23-01">
<div class="figure-type"><strong>R23 · 图1</strong><span>数据来源对照</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/r23-01.webp" target="_blank" rel="noopener" aria-label="放大：TVL论文图3：上游SSVTP的人工语言标注样例与新增HCT的GPT-4V标注样例，后者还展示标注错误。两种采集来源已在正文分别计数。"><img src="figures/r23-01.webp" alt="TVL论文图3：上游SSVTP的人工语言标注样例与新增HCT的GPT-4V标注样例，后者还展示标注错误。两种采集来源已在正文分别计数。" width="2033" height="1066" loading="lazy" decoding="async"></a>
<figcaption>TVL论文图3：上游SSVTP的人工语言标注样例与新增HCT的GPT-4V标注样例，后者还展示标注错误。两种采集来源已在正文分别计数。<span class="figure-links"><a href="https://arxiv.org/html/2402.13232" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://arxiv.org/html/2402.13232v1/dataset_figure_new.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>



## 6. 人操持工具／UMI触觉（5个）


<a id="U01"></a>

### U01 · FreeTacMan

**分类：**人操持工具／UMI　**参考年份：**2025　**证据：**A（已核实实际数据文件目录；计入主统计）

**数据／项目链接：**[官方数据](https://huggingface.co/datasets/OpenDriveLab/FreeTacMan)；[官方代码](https://github.com/OpenDriveLab/FreeTacMan)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 3M+视触觉图像对、10K+轨迹、50任务。 |
| 传感器、模态与时序 | 人操持的双指视触觉采集装置；腕视角与双指触觉视频，TCP位姿/四元数/夹爪距离和时间戳。 |
| 动作与状态 | 工具轨迹与夹爪开合；可映射到机器人，但不是机器人本体实际执行数据。 |
| 对触觉世界模型的用途 | 优先：规模较大的动作/运动条件触觉动态预训练。 |
| 数据许可 | 数据MIT；代码仓库Apache 2.0 |
| 限制、证据缺口与去重 | 每路、每帧、pair的计数不可直接换算小时；FTP内的同名包不再计数。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-u01-01">
<div class="figure-type"><strong>U01 · 图1</strong><span>采集与样例</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/u01-01.webp" target="_blank" rel="noopener" aria-label="放大：FreeTacMan官方系统演示截帧：人操持触觉夹爪的多模态采集与任务示例。该图包含系统／部署展示，不将全部画面当成原始训练样本。"><img src="figures/u01-01.webp" alt="FreeTacMan官方系统演示截帧：人操持触觉夹爪的多模态采集与任务示例。该图包含系统／部署展示，不将全部画面当成原始训练样本。" width="960" height="538" loading="lazy" decoding="async"></a>
<figcaption>FreeTacMan官方系统演示截帧：人操持触觉夹爪的多模态采集与任务示例。该图包含系统／部署展示，不将全部画面当成原始训练样本。<span class="figure-links"><a href="https://github.com/OpenDriveLab/FreeTacMan" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://github.com/OpenDriveLab/FreeTacMan/blob/main/figure/FreeTacMan_teaser.gif" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="U02"></a>

### U02 · Touch in the Wild

**分类：**人操持工具／UMI　**参考年份：**2025　**证据：**A（已核实实际数据文件目录；计入主统计）

**数据／项目链接：**[官方数据](https://huggingface.co/datasets/binghaohuang-robot/touch_in_the_wild-dataset)；[代码/数据管线](https://github.com/XinyueZhuXY/touch_in_the_wild)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 官方发布示范与预训练Zarr包；本次未核实统一总时长。 |
| 传感器、模态与时序 | 人手持触觉夹爪、GoPro、数值式触觉阵列、SLAM工具位姿；有完整SLAM和仅视觉—触觉两种处理管线。 |
| 动作与状态 | 完整操作包有运动/夹爪信息；visuo-tactile-only预训练包没有相同动作监督。 |
| 对触觉世界模型的用途 | 优先：阵列触觉运动条件建模、自然场景操作泛化。 |
| 数据许可 | MIT |
| 限制、证据缺口与去重 | 不是自由人手全手压力录制；只含视觉触觉的预训练子集不能冒充动作条件样本。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-u02-01">
<div class="figure-type"><strong>U02 · 图1</strong><span>采集与样例</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/u02-01.webp" target="_blank" rel="noopener" aria-label="放大：Touch in the Wild论文图2：便携式夹爪、相机、双指阵列触觉与真实场景样例。"><img src="figures/u02-01.webp" alt="Touch in the Wild论文图2：便携式夹爪、相机、双指阵列触觉与真实场景样例。" width="1356" height="540" loading="lazy" decoding="async"></a>
<figcaption>Touch in the Wild论文图2：便携式夹爪、相机、双指阵列触觉与真实场景样例。<span class="figure-links"><a href="https://arxiv.org/html/2507.15062v2" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://arxiv.org/html/2507.15062v2/system.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="U03"></a>

### U03 · exUMI

**分类：**人操持工具／UMI　**参考年份：**2025　**证据：**A（已核实实际数据文件目录；计入主统计）

**数据／项目链接：**[官方数据](https://huggingface.co/datasets/silicx/exUMI)；[代码](https://github.com/silicx/exUMI)；[论文](https://arxiv.org/abs/2509.14688)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 论文表征采集约5小时、480K触觉帧；公开任务Zarr覆盖范围与这两个数字不可默认相同。 |
| 传感器、模态与时序 | 扩展式人操持工具、GoPro、视触觉传感器；camera0_rgb、EEF位置/轴角、夹爪宽度、tactile_combined。 |
| 动作与状态 | 工具运动与抓口轨迹，episode_ends保存序列边界。 |
| 对触觉世界模型的用途 | 动作/运动条件触觉表示和工具到机器人迁移。 |
| 数据许可 | 本次未确认数据专用许可 |
| 限制、证据缺口与去重 | 示例shape仅代表某文件；控制目标与观测位姿的时间差仍需审计。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-u03-01">
<div class="figure-type"><strong>U03 · 图1</strong><span>采集装置</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/u03-01.webp" target="_blank" rel="noopener" aria-label="放大：exUMI官方硬件系统图，展示工具位姿、夹爪开合和视触觉采集所用部件。"><img src="figures/u03-01.webp" alt="exUMI官方硬件系统图，展示工具位姿、夹爪开合和视触觉采集所用部件。" width="2384" height="840" loading="lazy" decoding="async"></a>
<figcaption>exUMI官方硬件系统图，展示工具位姿、夹爪开合和视触觉采集所用部件。<span class="figure-links"><a href="https://github.com/silicx/exUMI/blob/main/README.md" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://github.com/silicx/exUMI/blob/main/assets/exUMI-system.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="U04"></a>

### U04 · ViTaMIn

**分类：**人操持工具／UMI　**参考年份：**2025　**证据：**A（已核实实际数据文件目录；计入主统计）

**数据／项目链接：**[作者数据](https://huggingface.co/datasets/chuanyune/ViTaMIn_dataset)；[项目](https://chuanyune.github.io/ViTaMIn_page/)；[论文](https://arxiv.org/abs/2504.06156)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 作者HF提供8个文件条目；数据规模/时长本文不根据文件数推断。 |
| 传感器、模态与时序 | robot-free视触觉操控夹爪，视觉、触觉与工具运动信息。 |
| 动作与状态 | 工具演示轨迹用于策略学习。 |
| 对触觉世界模型的用途 | 接触丰富任务运动条件模型、UMI到机器人迁移。 |
| 数据许可 | MIT（作者HF数据卡） |
| 限制、证据缺口与去重 | 项目页Code coming soon不等于数据未公开：HF及FTP均有数据。ViTaMIn-B/后续版本需按新旧采集部分另审，本文不额外计数。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-u04-01">
<div class="figure-type"><strong>U04 · 图1</strong><span>采集装置</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/u04-01.webp" target="_blank" rel="noopener" aria-label="放大：ViTaMIn论文图1：手持装置、GoPro、双触觉传感器与同步相机。该图展示采集硬件，机器人部署平台是另一个阶段。"><img src="figures/u04-01.webp" alt="ViTaMIn论文图1：手持装置、GoPro、双触觉传感器与同步相机。该图展示采集硬件，机器人部署平台是另一个阶段。" width="1332" height="636" loading="lazy" decoding="async"></a>
<figcaption>ViTaMIn论文图1：手持装置、GoPro、双触觉传感器与同步相机。该图展示采集硬件，机器人部署平台是另一个阶段。<span class="figure-links"><a href="https://arxiv.org/html/2504.06156v2" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://arxiv.org/html/2504.06156v2/system-8.0.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="U05"></a>

### U05 · Daimon-Infinity

**分类：**人操持工具／UMI　**参考年份：**2026　**证据：**B（已核实官方下载入口／脚本，归档未验证；计入主统计）

**数据／项目链接：**[官方说明/许可](https://github.com/dmrobot-admin/Daimon-Infinity)；[数据入口](https://modelscope.cn/datasets/daimonrobotics/Daimon-Infinity)；[样例](https://modelscope.cn/datasets/daimonrobotics/Daimon-Infinity-Lite)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 作者README称首批发布1,000小时，另用约1,209小时已采集数据做统计；10,000小时是后续开放目标。 |
| 传感器、模态与时序 | 主要DM-DataClaw手持夹爪，少量DM-DataDex外骨骼；视觉、指尖触觉形变/剪切/深度、运动和语言，LeRobot式格式。 |
| 动作与状态 | 工具/外骨骼运动轨迹；按设备解释，不能统称自主机器人执行。 |
| 对触觉世界模型的用途 | 大规模接触动态预训练的重点候选，先抽样核对原始触觉编码。 |
| 数据许可 | CC BY-NC-SA 4.0 |
| 限制、证据缺口与去重 | 官方ModelScope页面存在但本次未取得完整文件manifest；1,000小时是作者发布声明，未经本次逐轨迹验证。Lite不再算一个数据集。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-u05-02">
<div class="figure-type"><strong>U05 · 图1</strong><span>数据样例</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/u05-02.webp" target="_blank" rel="noopener" aria-label="放大：Daimon-Infinity官方DataClaw多模态样例；发布小时数仍以正文所列的发布口径为准。"><img src="figures/u05-02.webp" alt="Daimon-Infinity官方DataClaw多模态样例；发布小时数仍以正文所列的发布口径为准。" width="1609" height="962" loading="lazy" decoding="async"></a>
<figcaption>Daimon-Infinity官方DataClaw多模态样例；发布小时数仍以正文所列的发布口径为准。<span class="figure-links"><a href="https://github.com/dmrobot-admin/Daimon-Infinity/blob/main/README.md" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://github.com/dmrobot-admin/Daimon-Infinity/blob/main/assets/images/DataClawSample.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>



## 7. 其他人类真实触觉（6个）


<a id="H01"></a>

### H01 · STAG

**分类：**其他人类实测（非ego或视角未确认）　**参考年份：**2019　**证据：**B（已核实官方下载入口／脚本，归档未验证；计入主统计）

**数据／项目链接：**[官方项目与下载](https://stag.csail.mit.edu/)；[数据轻量版](https://stag.csail.mit.edu/datasets/classification_lite.zip)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 548传感点、135,000帧、26物体；另有蒙眼、重量预测和手姿态子集。 |
| 传感器、模态与时序 | 真实人类全手压阻织物手套触觉。 |
| 动作与状态 | 没有已核实的ego视频与机器人动作组合。 |
| 对触觉世界模型的用途 | 全手压力空间结构、抓型/物体识别、压力编码器。 |
| 数据许可 | 本次未确认数据专用许可 |
| 限制、证据缺口与去重 | 不能因采集者是人就算ego视频数据；同站多个任务ZIP按一个来源家族计。 |

<div class="dataset-figures">
<figure class="dataset-figure" id="fig-h01-01">
<div class="figure-type"><strong>H01 · 图1</strong><span>采集与样例</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/h01-01.webp" target="_blank" rel="noopener" aria-label="放大：STAG官方概览：可伸缩触觉手套、物体抓握与压力读数。人类采集不自动意味着同步ego视频。"><img src="figures/h01-01.webp" alt="STAG官方概览：可伸缩触觉手套、物体抓握与压力读数。人类采集不自动意味着同步ego视频。" width="800" height="531" loading="lazy" decoding="async"></a>
<figcaption>STAG官方概览：可伸缩触觉手套、物体抓握与压力读数。人类采集不自动意味着同步ego视频。<span class="figure-links"><a href="https://stag.csail.mit.edu/" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://stag.csail.mit.edu/assets/teaser3_800.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
<figure class="dataset-figure" id="fig-h01-02">
<div class="figure-type"><strong>H01 · 图2</strong><span>手姿态样例</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/h01-02.webp" target="_blank" rel="noopener" aria-label="放大：STAG官方手姿态演示图，展示手部姿态与触觉分布之间的关系。"><img src="figures/h01-02.webp" alt="STAG官方手姿态演示图，展示手部姿态与触觉分布之间的关系。" width="919" height="777" loading="lazy" decoding="async"></a>
<figcaption>STAG官方手姿态演示图，展示手部姿态与触觉分布之间的关系。<span class="figure-links"><a href="https://stag.csail.mit.edu/" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://stag.csail.mit.edu/assets/demo_poses.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="H02"></a>

### H02 · PressureVisionDB

**分类：**其他人类实测（非ego或视角未确认）　**参考年份：**2022　**证据：**B（已核实官方下载入口／脚本，归档未验证；计入主统计）

**数据／项目链接：**[官方代码/下载器/许可](https://github.com/facebookresearch/PressureVision)；[论文](https://arxiv.org/abs/2203.10385)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 36人、4相机、16小时；全版15FPS约960GB，小版3FPS约140GB。 |
| 传感器、模态与时序 | 固定外部RGB与桌面压力传感器测得的压力图。 |
| 动作与状态 | 人手压力估计数据，非机器人控制示范。 |
| 对触觉世界模型的用途 | 皮肤外观—压力关系、压力图预测与校准基准。 |
| 数据许可 | MIT（明确覆盖数据与代码） |
| 限制、证据缺口与去重 | 不是ego；PressureVision++另有弱标签采集，不能将++所有样本当作压力传感器真值。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-h02-01">
<div class="figure-type"><strong>H02 · 图1</strong><span>采集与样例</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/h02-01.webp" target="_blank" rel="noopener" aria-label="放大：PressureVisionDB论文图3：固定多相机采集装置、压力平面与投影到图像的压力标注；这里是真实压力板监督，不是网络预测效果图。"><img src="figures/h02-01.webp" alt="PressureVisionDB论文图3：固定多相机采集装置、压力平面与投影到图像的压力标注；这里是真实压力板监督，不是网络预测效果图。" width="2047" height="791" loading="lazy" decoding="async"></a>
<figcaption>PressureVisionDB论文图3：固定多相机采集装置、压力平面与投影到图像的压力标注；这里是真实压力板监督，不是网络预测效果图。<span class="figure-links"><a href="https://arxiv.org/html/2203.10385v3" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://arxiv.org/html/2203.10385v3/DatasetMultifig.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="H03"></a>

### H03 · PX OmniSharing DB SampleData

**分类：**其他人类实测（非ego或视角未确认）　**参考年份：**2026　**证据：**A（已核实实际数据文件目录；计入主统计）

**数据／项目链接：**[官方样例数据](https://huggingface.co/datasets/paxini/Omnisharing_DB_SampleData)；[处理和重定向工具](https://github.com/px-DataCollection/px_omnisharing_dataprocess_kit)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 公开样例；任务表列9项各100，正文又称10任务，且目录有1,002个文件条目，不据此给出未经核实的episode总数。 |
| 传感器、模态与时序 | 人类数据手套：双手29维关节、7维handpose、每手3465长度触觉字段、多视角RGB/RGB-D、音频/文字、物体位姿；DF-2 HDF5。 |
| 动作与状态 | 有action/observation结构和重定向工具；action的时序语义需核查。 |
| 对触觉世界模型的用途 | 优先：双手运动与触觉联合建模、重定向管线原型。 |
| 数据许可 | CC BY-NC-SA 4.0（数据卡正文） |
| 限制、证据缺口与去重 | 当前可读卡只明确multi-view，没有明确证明公开样例全为头戴ego，因此保守列H，不加到严格ego数。3465是存储向量长度，不等于3465独立taxels；样例不等于商业全库。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-h03-01">
<div class="figure-type"><strong>H03 · 图1</strong><span>数据处理流程</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/h03-01.webp" target="_blank" rel="noopener" aria-label="放大：PX官方数据处理流程：原始HDF5、触觉／姿态解析、重定向及LeRobot转换。重定向后的动作标签与原始人手观测需区分。"><img src="figures/h03-01.webp" alt="PX官方数据处理流程：原始HDF5、触觉／姿态解析、重定向及LeRobot转换。重定向后的动作标签与原始人手观测需区分。" width="2400" height="597" loading="lazy" decoding="async"></a>
<figcaption>PX官方数据处理流程：原始HDF5、触觉／姿态解析、重定向及LeRobot转换。重定向后的动作标签与原始人手观测需区分。<span class="figure-links"><a href="https://github.com/px-DataCollection/px_omnisharing_dataprocess_kit" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://github.com/px-DataCollection/px_omnisharing_dataprocess_kit/blob/main/images/data_flow.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="H04"></a>

### H04 · FusionX Multimodal Sample Dataset V3

**分类：**其他人类实测（非ego或视角未确认）　**参考年份：**2026　**证据：**A（已核实实际数据文件目录；计入主统计）

**数据／项目链接：**[发布方数据](https://huggingface.co/datasets/touchtronix/FusionX-Multimodal-Sample-Data-V3)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 11项任务的公开样例。 |
| 传感器、模态与时序 | 双目灰度640×400、720p RGB、OAK IMU、同步触觉手套、calib.json与frames.parquet。 |
| 动作与状态 | 数据卡未确认连续可执行动作/完整手关节。 |
| 对触觉世界模型的用途 | 双目视觉与手套触觉融合、标定和时间对齐原型。 |
| 数据许可 | CC BY 4.0 |
| 限制、证据缺口与去重 | 卡片未明言相机安装为头戴ego，本条不放入严格E。另一个zhicao/egoforce录制库虽写ego，但来源重叠与授权未厘清，不能据其说明改变本条分类或直接重复加数。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-h04-rgb">
<div class="figure-type"><strong>H04 · 图1</strong><span>真实RGB样例</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/h04-rgb.webp" target="_blank" rel="noopener" aria-label="放大：FusionX V3公开firstaid1/rgb/000580.jpg原始RGB帧，对应本条下方压力曲线所用的frame_idx=580。当前分类仍按已核实的发布说明保守列为人类实测。"><img src="figures/h04-rgb.webp" alt="FusionX V3公开firstaid1/rgb/000580.jpg原始RGB帧，对应本条下方压力曲线所用的frame_idx=580。当前分类仍按已核实的发布说明保守列为人类实测。" width="1280" height="720" loading="lazy" decoding="async"></a>
<figcaption>FusionX V3公开firstaid1/rgb/000580.jpg原始RGB帧，对应本条下方压力曲线所用的frame_idx=580。当前分类仍按已核实的发布说明保守列为人类实测。<span class="figure-links"><a href="https://huggingface.co/datasets/touchtronix/FusionX-Multimodal-Sample-Data-V3" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://huggingface.co/datasets/touchtronix/FusionX-Multimodal-Sample-Data-V3/resolve/main/firstaid1/rgb/000580.jpg" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
<figure class="dataset-figure wide" id="fig-h04-sample">
<div class="figure-type"><strong>H04 · 图2</strong><span>本报告据实测数据绘制</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/h04-sample.webp" target="_blank" rel="noopener" aria-label="放大：本报告从FusionX V3的firstaid1/frames.parquet第580行绘制双手压力字段（frame_idx=580）。横轴按拇指、食指、中指、无名指、小指、手掌的存储顺序拼接，不是手掌几何坐标；_cal字段保留原数值，未换算成力。"><img src="figures/h04-sample.webp" alt="本报告从FusionX V3的firstaid1/frames.parquet第580行绘制双手压力字段（frame_idx=580）。横轴按拇指、食指、中指、无名指、小指、手掌的存储顺序拼接，不是手掌几何坐标；_cal字段保留原数值，未换算成力。" width="1938" height="738" loading="lazy" decoding="async"></a>
<figcaption>本报告从FusionX V3的firstaid1/frames.parquet第580行绘制双手压力字段（frame_idx=580）。横轴按拇指、食指、中指、无名指、小指、手掌的存储顺序拼接，不是手掌几何坐标；_cal字段保留原数值，未换算成力。<span class="figure-links"><a href="https://huggingface.co/datasets/touchtronix/FusionX-Multimodal-Sample-Data-V3/resolve/main/firstaid1/frames.parquet" target="_blank" rel="noopener">来源：公开数据文件</a><a href="https://huggingface.co/datasets/touchtronix/FusionX-Multimodal-Sample-Data-V3/resolve/main/firstaid1/frames.parquet" target="_blank" rel="noopener">查看原始数据</a></span></figcaption>
</figure>
</div>


<a id="H05"></a>

### H05 · Tachin Fixed Viewpoint Tactile Dataset

**分类：**其他人类实测（非ego或视角未确认）　**参考年份：**2026　**证据：**A（已核实实际数据文件目录；计入主统计）

**数据／项目链接：**[发布方数据/字段定义](https://huggingface.co/datasets/Tachintech/Fixed_Viewpoint_Tactile_Dataset)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 160 episodes、59,158帧，20FPS。 |
| 传感器、模态与时序 | 固定机位RGB/深度、20路触觉、20个动捕点位置与四元数；LeRobot v3。 |
| 动作与状态 | action[140]明确等于当前帧动捕位置60⊕四元数80。 |
| 对触觉世界模型的用途 | 人手空间触觉与运动关系、时序预测。 |
| 数据许可 | Apache 2.0 |
| 限制、证据缺口与去重 | 字段叫action不代表控制命令；没有单独的未来控制目标。robot_type字符串也不能证明是机器人采集，按实际动捕说明列人手非ego。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-h05-sample">
<div class="figure-type"><strong>H05 · 图1</strong><span>本报告据实测数据绘制</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/h05-sample.webp" target="_blank" rel="noopener" aria-label="放大：本报告依据file-0000.parquet第100行绘制（episode=0，frame=100）。按官方info.json恢复4路触觉的二维形状；各子图使用各自色标，没有推断未提供的手部空间布局或物理单位。"><img src="figures/h05-sample.webp" alt="本报告依据file-0000.parquet第100行绘制（episode=0，frame=100）。按官方info.json恢复4路触觉的二维形状；各子图使用各自色标，没有推断未提供的手部空间布局或物理单位。" width="1875" height="594" loading="lazy" decoding="async"></a>
<figcaption>本报告依据file-0000.parquet第100行绘制（episode=0，frame=100）。按官方info.json恢复4路触觉的二维形状；各子图使用各自色标，没有推断未提供的手部空间布局或物理单位。<span class="figure-links"><a href="https://huggingface.co/datasets/Tachintech/Fixed_Viewpoint_Tactile_Dataset/resolve/main/data/chunk-000/file-0000.parquet" target="_blank" rel="noopener">来源：公开数据文件</a><a href="https://huggingface.co/datasets/Tachintech/Fixed_Viewpoint_Tactile_Dataset/resolve/main/data/chunk-000/file-0000.parquet" target="_blank" rel="noopener">查看原始数据</a></span></figcaption>
</figure>
</div>


<a id="H06"></a>

### H06 · Tactile Gesture Language

**分类：**其他人类实测（非ego或视角未确认）　**参考年份：**2026　**证据：**A（已核实实际数据文件目录；计入主统计）

**数据／项目链接：**[发布方数据](https://huggingface.co/datasets/Tachintech/Tactile_Gesture_Language)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 10位参与者、10类接触手势。 |
| 传感器、模态与时序 | 平面32×32触觉阵列；抚摸、敲击、按压、抓挠等时序，按参与者/手势组织。 |
| 动作与状态 | 手势分类与触觉动态，没有ego视频/机器人动作声明。 |
| 对触觉世界模型的用途 | 触觉事件、接触类型和时间模式预训练。 |
| 数据许可 | GPL 3.0（数据卡） |
| 限制、证据缺口与去重 | 名称含Language不等于逐帧自然语言指令；可读数据卡主要定义手势类别。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-h06-sample">
<div class="figure-type"><strong>H06 · 图1</strong><span>本报告据实测数据绘制</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/h06-sample.webp" target="_blank" rel="noopener" aria-label="放大：本报告从stroking_man0.npy的首个试验绘制。通过HTTP Range读取文件开头，得到前127帧，展示帧0, 94, 110, 118, 126；这是32×32实测阵列响应，不是ego视频。保留存储数值，未将其解释为已标定力。"><img src="figures/h06-sample.webp" alt="本报告从stroking_man0.npy的首个试验绘制。通过HTTP Range读取文件开头，得到前127帧，展示帧0, 94, 110, 118, 126；这是32×32实测阵列响应，不是ego视频。保留存储数值，未将其解释为已标定力。" width="2093" height="477" loading="lazy" decoding="async"></a>
<figcaption>本报告从stroking_man0.npy的首个试验绘制。通过HTTP Range读取文件开头，得到前127帧，展示帧0, 94, 110, 118, 126；这是32×32实测阵列响应，不是ego视频。保留存储数值，未将其解释为已标定力。<span class="figure-links"><a href="https://huggingface.co/datasets/Tachintech/Tactile_Gesture_Language/resolve/main/dataset/man0/stroking_man0.npy" target="_blank" rel="noopener">来源：公开数据文件</a><a href="https://huggingface.co/datasets/Tachintech/Tactile_Gesture_Language/resolve/main/dataset/man0/stroking_man0.npy" target="_blank" rel="noopener">查看原始数据</a></span></figcaption>
</figure>
</div>



## 8. 实物探测、标定和实验台触觉（13个）


<a id="P01"></a>

### P01 · TVL-HCT（TVL新增人采部分）

**分类：**实物探测／标定／混合实验台　**参考年份：**2024　**证据：**A（已核实实际数据文件目录；计入主统计）

**数据／项目链接：**[官方数据及修复说明](https://huggingface.co/datasets/mlfu7/Touch-Vision-Language-Dataset)；[论文](https://arxiv.org/abs/2402.13232)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 原论文：39,154组接触图像对、169,292组无接触对；另复用SSVTP的4,587组。 |
| 传感器、模态与时序 | 人手持DIGIT与RGB采集装置，同步视觉—触觉，触觉形容词/文本。 |
| 动作与状态 | 没有机器人动作；多数接触语言标签由GPT-4V生成。 |
| 对触觉世界模型的用途 | 触觉—视觉—语言编码器、接触语义对齐。 |
| 数据许可 | 代码Apache 2.0；数据独立许可本次未确认 |
| 限制、证据缺口与去重 | 本条只为新增HCT采集计数，SSVTP已列R23，TVL全包不再加1。官方提示原版视觉/触觉文件夹交换问题并链接修正版；人采相机不直接等同头戴ego。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-p01-01">
<div class="figure-type"><strong>P01 · 图1</strong><span>采集装置</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/p01-01.webp" target="_blank" rel="noopener" aria-label="放大：TVL论文图2：DIGIT与webcam组合的手持装置，用于同步采集HCT视觉—触觉接触样本。"><img src="figures/p01-01.webp" alt="TVL论文图2：DIGIT与webcam组合的手持装置，用于同步采集HCT视觉—触觉接触样本。" width="2046" height="853" loading="lazy" decoding="async"></a>
<figcaption>TVL论文图2：DIGIT与webcam组合的手持装置，用于同步采集HCT视觉—触觉接触样本。<span class="figure-links"><a href="https://arxiv.org/html/2402.13232" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://arxiv.org/html/2402.13232v1/sensor_device.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
<figure class="dataset-figure" id="fig-p01-02">
<div class="figure-type"><strong>P01 · 图2</strong><span>数据与模型概览</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/p01-02.webp" target="_blank" rel="noopener" aria-label="放大：TVL论文图1展示视觉、触觉和语言对齐目标；图内模型响应属于任务展示，HCT大部分语言标签由模型生成。"><img src="figures/p01-02.webp" alt="TVL论文图1展示视觉、触觉和语言对齐目标；图内模型响应属于任务展示，HCT大部分语言标签由模型生成。" width="1457" height="892" loading="lazy" decoding="async"></a>
<figcaption>TVL论文图1展示视觉、触觉和语言对齐目标；图内模型响应属于任务展示，HCT大部分语言标签由模型生成。<span class="figure-links"><a href="https://arxiv.org/html/2402.13232" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://arxiv.org/html/2402.13232v1/splash_figure_alt.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="P02"></a>

### P02 · TacQuad（AnyTouch）

**分类：**实物探测／标定／混合实验台　**参考年份：**2025　**证据：**A（已核实实际数据文件目录；计入主统计）

**数据／项目链接：**[官方数据](https://huggingface.co/datasets/xxuan01/TacQuad)；[方法/加载器](https://github.com/GeWu-Lab/AnyTouch)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 精细时空对齐：25物体/17,524接触帧；粗空间对齐：99物体/55,082接触帧。 |
| 传感器、模态与时序 | GelSight Mini、DIGIT、DuraGel、Tac3D四种传感器，视觉与文本。 |
| 动作与状态 | 有对应接触位置/过程，非通用机器人动作序列。 |
| 对触觉世界模型的用途 | 优先：跨传感器对齐、传感器迁移和统一触觉编码。 |
| 数据许可 | MIT |
| 限制、证据缺口与去重 | 粗对齐子集不保证时间对齐；不要把四种传感器的同一接触视为4条独立行为。text_other.zip是其他数据的派生标注，不是新的原始采集。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-p02-01">
<div class="figure-type"><strong>P02 · 图1</strong><span>采集与样例</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/p02-01.webp" target="_blank" rel="noopener" aria-label="放大：TacQuad论文图1：GelSight Mini、DIGIT、DuraGel、Tac3D四类传感器及对齐样例。精细与粗对齐子集的同步保证不同。"><img src="figures/p02-01.webp" alt="TacQuad论文图1：GelSight Mini、DIGIT、DuraGel、Tac3D四类传感器及对齐样例。精细与粗对齐子集的同步保证不同。" width="2001" height="972" loading="lazy" decoding="async"></a>
<figcaption>TacQuad论文图1：GelSight Mini、DIGIT、DuraGel、Tac3D四类传感器及对齐样例。精细与粗对齐子集的同步保证不同。<span class="figure-links"><a href="https://arxiv.org/html/2502.12191v3" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://arxiv.org/html/2502.12191v3/DATASET.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="P03"></a>

### P03 · HPT / HTT Dataset

**分类：**实物探测／标定／混合实验台　**参考年份：**2026　**证据：**A（已核实实际数据文件目录；计入主统计）

**数据／项目链接：**[官方数据/manifest](https://huggingface.co/datasets/AllenBi21/HTT-dataset)；[代码](https://github.com/jxbi1010/HTT)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 卡片称约1.59M frames；pretrain 1,789 episodes、classification 599；static/sliding各401/405。 |
| 传感器、模态与时序 | GelSight Mini、9DTact图像，Xela uSkin 72维、TAC-02 66维阵列；力/滑移标签、同步配对。 |
| 动作与状态 | 静态探测与滑动试验，无通用操作控制动作声明。 |
| 对触觉世界模型的用途 | 优先：光学与数值阵列统一表示、6D力估计与滑移识别。 |
| 数据许可 | CC BY-NC 4.0 |
| 限制、证据缺口与去重 | 卡片的frame合计跨光学/阵列采样率，且force统计含sliding、slip也统计该sliding；1.59M不是独立接触事件或唯一同步时间点，必须去重。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-p03-01">
<div class="figure-type"><strong>P03 · 图1</strong><span>采集装置</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/p03-01.webp" target="_blank" rel="noopener" aria-label="放大：HPT／HTT论文图2：真实力／滑移数据采集和统计。图中不同频率、任务重用的帧量仍需按正文去重解释。"><img src="figures/p03-01.webp" alt="HPT／HTT论文图2：真实力／滑移数据采集和统计。图中不同频率、任务重用的帧量仍需按正文去重解释。" width="785" height="242" loading="lazy" decoding="async"></a>
<figcaption>HPT／HTT论文图2：真实力／滑移数据采集和统计。图中不同频率、任务重用的帧量仍需按正文去重解释。<span class="figure-links"><a href="https://arxiv.org/html/2606.29948v2" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://arxiv.org/html/2606.29948v2/dataset.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
<figure class="dataset-figure wide" id="fig-p03-02">
<div class="figure-type"><strong>P03 · 图2</strong><span>真实触觉样例</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/p03-02.webp" target="_blank" rel="noopener" aria-label="放大：HPT论文图6：GelSight Mini与9DTact上的物体分类和受控施力触觉图。"><img src="figures/p03-02.webp" alt="HPT论文图6：GelSight Mini与9DTact上的物体分类和受控施力触觉图。" width="1184" height="405" loading="lazy" decoding="async"></a>
<figcaption>HPT论文图6：GelSight Mini与9DTact上的物体分类和受控施力触觉图。<span class="figure-links"><a href="https://arxiv.org/html/2606.29948v2" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://arxiv.org/html/2606.29948v2/data_samples.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="P04"></a>

### P04 · TaF-Dataset

**分类：**实物探测／标定／混合实验台　**参考年份：**2026　**证据：**A（已核实实际数据文件目录；计入主统计）

**数据／项目链接：**[官方数据/字段/许可](https://huggingface.co/datasets/jiamig/taf-dataset)；[方法代码](https://github.com/mrHuangyz/TaF-VLA)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 数据卡：10,053,265同步帧、3,592 episodes、408 object/sensor sequences、6传感器配置。 |
| 传感器、模态与时序 | 240×320触觉图像、12×12压力矩阵、ATI六维F/T和时间戳；LeRobot v3。 |
| 动作与状态 | 卡片列出的核心字段没有外部场景RGB/robot action；不是自动可用的完整VLA轨迹。 |
| 对触觉世界模型的用途 | 优先：触觉与真实力学信号对齐，跨传感器触觉编码与物理辅助损失。 |
| 数据许可 | MIT |
| 限制、证据缺口与去重 | 408是物体/传感器序列组合，不是408个独立物体；observation.image是触觉传感器内部图，不是场景相机。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-p04-01">
<div class="figure-type"><strong>P04 · 图1</strong><span>采集装置</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/p04-01.webp" target="_blank" rel="noopener" aria-label="放大：TaF论文图2：自动触觉—力采集平台及其运动机构、触觉传感器和ATI力传感器。"><img src="figures/p04-01.webp" alt="TaF论文图2：自动触觉—力采集平台及其运动机构、触觉传感器和ATI力传感器。" width="2400" height="1167" loading="lazy" decoding="async"></a>
<figcaption>TaF论文图2：自动触觉—力采集平台及其运动机构、触觉传感器和ATI力传感器。<span class="figure-links"><a href="https://arxiv.org/html/2601.20321v2" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://arxiv.org/html/2601.20321v2/figures/fig2-hardware.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="P05"></a>

### P05 · Touch-Slide

**分类：**实物探测／标定／混合实验台　**参考年份：**2024　**证据：**A（已核实实际数据文件目录；计入主统计）

**数据／项目链接：**[官方数据](https://huggingface.co/datasets/facebook/touch-slide)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 9物体×5轨迹，约180K帧。 |
| 传感器、模态与时序 | 人手操持多个DIGIT在厨房玩具物体表面滑动的触觉图像。 |
| 动作与状态 | 官方明确无标签，主要做SSL。 |
| 对触觉世界模型的用途 | 触觉时序/表征预训练、传感器背景鲁棒性。 |
| 数据许可 | CC BY-NC 4.0 |
| 限制、证据缺口与去重 | 不是YCB-Slide的另一名称；是Sparsh新增独立采集，但Sparsh汇编不再重复计。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-p05-01">
<div class="figure-type"><strong>P05 · 图1</strong><span>采集对象</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/p05-01.webp" target="_blank" rel="noopener" aria-label="放大：Touch-Slide官方对象集合；主要用于滑动触觉自监督，物体展示不代表公开了机器人action。"><img src="figures/p05-01.webp" alt="Touch-Slide官方对象集合；主要用于滑动触觉自监督，物体展示不代表公开了机器人action。" width="2400" height="287" loading="lazy" decoding="async"></a>
<figcaption>Touch-Slide官方对象集合；主要用于滑动触觉自监督，物体展示不代表公开了机器人action。<span class="figure-links"><a href="https://huggingface.co/datasets/facebook/touch-slide/raw/main/README.md" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://huggingface.co/datasets/facebook/touch-slide/resolve/main/assets/touch_slide_objs.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
<figure class="dataset-figure" id="fig-p05-02">
<div class="figure-type"><strong>P05 · 图2</strong><span>真实触觉样例</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/p05-02.webp" target="_blank" rel="noopener" aria-label="放大：Touch-Slide官方bread-digit接触序列截帧，展示DIGIT内部的表面形变图像。"><img src="figures/p05-02.webp" alt="Touch-Slide官方bread-digit接触序列截帧，展示DIGIT内部的表面形变图像。" width="240" height="320" loading="lazy" decoding="async"></a>
<figcaption>Touch-Slide官方bread-digit接触序列截帧，展示DIGIT内部的表面形变图像。<span class="figure-links"><a href="https://huggingface.co/datasets/facebook/touch-slide/raw/main/README.md" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://huggingface.co/datasets/facebook/touch-slide/resolve/main/assets/bread-digit.gif" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="P06"></a>

### P06 · YCB-Slide（Real）

**分类：**实物探测／标定／混合实验台　**参考年份：**2022　**证据：**B（已核实官方下载入口／脚本，归档未验证；计入主统计）

**数据／项目链接：**[官方数据/下载](https://github.com/rpl-cmu/YCB-Slide)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 10个YCB物体、每物体5条，合计50次真实滑动交互。 |
| 传感器、模态与时序 | DIGIT触觉、RGB视频、OptiTrack传感器位姿、物体mesh；另有TACTO仿真部分。 |
| 动作与状态 | 真实传感器运动轨迹；不等同于机器人控制命令。 |
| 对触觉世界模型的用途 | 运动条件触觉预测、触觉定位/建图。 |
| 数据许可 | MIT |
| 限制、证据缺口与去重 | 真实数据为传感器在固定物体上移动；仿真heightmap/contact mask不是真实采集额外测量。只计Real来源一次。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-p06-01">
<div class="figure-type"><strong>P06 · 图1</strong><span>真实数据样例</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/p06-01.webp" target="_blank" rel="noopener" aria-label="放大：YCB-Slide官方dataset_real_full图：真实物体上的滑动轨迹／接触覆盖可视化。图中的物体模型渲染用于定位接触，不应误认成原始RGB或真实触觉图。"><img src="figures/p06-01.webp" alt="YCB-Slide官方dataset_real_full图：真实物体上的滑动轨迹／接触覆盖可视化。图中的物体模型渲染用于定位接触，不应误认成原始RGB或真实触觉图。" width="2400" height="1119" loading="lazy" decoding="async"></a>
<figcaption>YCB-Slide官方dataset_real_full图：真实物体上的滑动轨迹／接触覆盖可视化。图中的物体模型渲染用于定位接触，不应误认成原始RGB或真实触觉图。<span class="figure-links"><a href="https://github.com/rpl-cmu/YCB-Slide/blob/master/README.md" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://github.com/rpl-cmu/YCB-Slide/blob/master/media/dataset_real_full.jpg " target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="P07"></a>

### P07 · Sparsh / TacBench原创监督数据

**分类：**实物探测／标定／混合实验台　**参考年份：**2024　**证据：**A（已核实实际数据文件目录；计入主统计）

**数据／项目链接：**[DIGIT力/滑移](https://huggingface.co/datasets/facebook/digit-force-estimation)；[GelSight力/滑移](https://huggingface.co/datasets/facebook/gelsight-force-estimation)；[DIGIT姿态](https://huggingface.co/datasets/facebook/digit-pose-estimation)；[采集协议](https://github.com/facebookresearch/sparsh)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 原创DIGIT/GelSight力—滑移试验与DIGIT手内姿态试验，按同一发布家族计1。 |
| 传感器、模态与时序 | 3种探针形状、ATI nano17力真值、法向/剪切/滑移；三指DIGIT+物体SE(3)位姿、机器人手关节。 |
| 动作与状态 | 受控施力与人扰动物体的序列；姿态变化/力监督不等于可执行动作。 |
| 对触觉世界模型的用途 | 优先：力、滑移和姿态探针任务，验证编码器是否保留控制相关信息。 |
| 数据许可 | CC BY-NC 4.0 |
| 限制、证据缺口与去重 | TacBench还复用抓取/衣物数据，不把整个6项任务各算新数据；本条只计作者新增采集，两种sensor不是两个独立家族。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-p07-01">
<div class="figure-type"><strong>P07 · 图1</strong><span>采集装置</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/p07-01.webp" target="_blank" rel="noopener" aria-label="放大：Sparsh／TacBench论文图3：力估计、滑移和位姿等监督采集装置。任务数不等于新增独立数据来源数。"><img src="figures/p07-01.webp" alt="Sparsh／TacBench论文图3：力估计、滑移和位姿等监督采集装置。任务数不等于新增独立数据来源数。" width="1319" height="366" loading="lazy" decoding="async"></a>
<figcaption>Sparsh／TacBench论文图3：力估计、滑移和位姿等监督采集装置。任务数不等于新增独立数据来源数。<span class="figure-links"><a href="https://arxiv.org/html/2410.24090v1" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://arxiv.org/html/2410.24090v1/figB_data_benchmark.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="P08"></a>

### P08 · TactileTracking / NormalFlow Dataset

**分类：**实物探测／标定／混合实验台　**参考年份：**2024　**证据：**A（已核实实际数据文件目录；计入主统计）

**数据／项目链接：**[官方数据](https://huggingface.co/datasets/joehjhuang/TactileTracking)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 12物体、84次试验、每物体7次，平均每次10.2秒。 |
| 传感器、模态与时序 | 无marker GelSight Mini触觉视频、外部webcam、动捕逐帧6DoF传感器位姿、派生接触mask/梯度。 |
| 动作与状态 | 有真实位姿轨迹；无通用机器人控制动作。 |
| 对触觉世界模型的用途 | 局部触觉运动预测、接触跟踪与几何表示。 |
| 数据许可 | MIT |
| 限制、证据缺口与去重 | 目标是短程连续重叠接触；mask/梯度由触觉图像计算，不是第二套独立传感器真值。 |

<div class="dataset-figures">
<figure class="dataset-figure" id="fig-p08-01">
<div class="figure-type"><strong>P08 · 图1</strong><span>采集装置</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/p08-01.webp" target="_blank" rel="noopener" aria-label="放大：NormalFlow／TactileTracking官方采集设置：GelSight Mini触觉、外部视觉与动捕位姿。"><img src="figures/p08-01.webp" alt="NormalFlow／TactileTracking官方采集设置：GelSight Mini触觉、外部视觉与动捕位姿。" width="2400" height="1850" loading="lazy" decoding="async"></a>
<figcaption>NormalFlow／TactileTracking官方采集设置：GelSight Mini触觉、外部视觉与动捕位姿。<span class="figure-links"><a href="https://huggingface.co/datasets/joehjhuang/TactileTracking/raw/main/README.md" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://huggingface.co/datasets/joehjhuang/TactileTracking/resolve/main/assets/setup.jpg" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
<figure class="dataset-figure" id="fig-p08-02">
<div class="figure-type"><strong>P08 · 图2</strong><span>接触样例</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/p08-02.webp" target="_blank" rel="noopener" aria-label="放大：TactileTracking官方接触位置图；有助于理解物体表面局部接触和跟踪轨迹的覆盖范围。"><img src="figures/p08-02.webp" alt="TactileTracking官方接触位置图；有助于理解物体表面局部接触和跟踪轨迹的覆盖范围。" width="1206" height="950" loading="lazy" decoding="async"></a>
<figcaption>TactileTracking官方接触位置图；有助于理解物体表面局部接触和跟踪轨迹的覆盖范围。<span class="figure-links"><a href="https://huggingface.co/datasets/joehjhuang/TactileTracking/raw/main/README.md" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://huggingface.co/datasets/joehjhuang/TactileTracking/resolve/main/assets/contact_locations.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="P09"></a>

### P09 · GelSLAM Dataset

**分类：**实物探测／标定／混合实验台　**参考年份：**2025　**证据：**A（已核实实际数据文件目录；计入主统计）

**数据／项目链接：**[官方数据及局限](https://huggingface.co/datasets/joehjhuang/GelSLAM_dataset)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 长程跟踪20物体/140 episodes，平均约21秒；重建另有15物体扫描。 |
| 传感器、模态与时序 | GelSight Mini视频、跟踪部分的动捕6DoF轨迹、背景/标定；重建部分人手持物体与传感器扫描。 |
| 动作与状态 | 跟踪部分有位姿；重建部分不能默认有相同运动真值。 |
| 对触觉世界模型的用途 | 长程触觉预测、跨接触断开重初始化、触觉几何记忆。 |
| 数据许可 | MIT |
| 限制、证据缺口与去重 | 重建子集官方明确不提供GT mesh；跟踪和重建按一个发布家族计，NormalFlow是另一采集来源。 |

<div class="dataset-figures">
<figure class="dataset-figure" id="fig-p09-01">
<div class="figure-type"><strong>P09 · 图1</strong><span>采集装置</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/p09-01.webp" target="_blank" rel="noopener" aria-label="放大：GelSLAM官方跟踪数据采集装置，包含触觉与动捕；重建子集的真值覆盖与此不同。"><img src="figures/p09-01.webp" alt="GelSLAM官方跟踪数据采集装置，包含触觉与动捕；重建子集的真值覆盖与此不同。" width="1865" height="1141" loading="lazy" decoding="async"></a>
<figcaption>GelSLAM官方跟踪数据采集装置，包含触觉与动捕；重建子集的真值覆盖与此不同。<span class="figure-links"><a href="https://huggingface.co/datasets/joehjhuang/GelSLAM_dataset/resolve/main/README.md" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://huggingface.co/datasets/joehjhuang/GelSLAM_dataset/resolve/main/assets/tracking_setup.jpg" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
<figure class="dataset-figure wide" id="fig-p09-02">
<div class="figure-type"><strong>P09 · 图2</strong><span>接触样例</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/p09-02.webp" target="_blank" rel="noopener" aria-label="放大：GelSLAM官方跟踪物体与初始接触位置。图中物体展示不能视为重建子集提供了GT mesh。"><img src="figures/p09-02.webp" alt="GelSLAM官方跟踪物体与初始接触位置。图中物体展示不能视为重建子集提供了GT mesh。" width="2400" height="1220" loading="lazy" decoding="async"></a>
<figcaption>GelSLAM官方跟踪物体与初始接触位置。图中物体展示不能视为重建子集提供了GT mesh。<span class="figure-links"><a href="https://huggingface.co/datasets/joehjhuang/GelSLAM_dataset/resolve/main/README.md" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://huggingface.co/datasets/joehjhuang/GelSLAM_dataset/resolve/main/assets/tracking_objects.jpg" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="P10"></a>

### P10 · GelSight Sim2Real（真实A域）

**分类：**实物探测／标定／混合实验台　**参考年份：**2022　**证据：**B（已核实官方下载入口／脚本，归档未验证；计入主统计）

**数据／项目链接：**[官方数据/协议](https://github.com/RVSATHU/GelSight-Sim2Real)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 每物体真实/仿真各1,200触觉图，作者提供完整归档。 |
| 传感器、模态与时序 | 真实GelSight系图像和仿真图像，支持CycleGAN域转换及深度重建。 |
| 动作与状态 | 不是操作动作轨迹库。 |
| 对触觉世界模型的用途 | 传感器域适配、光照/背景鲁棒性、sim-to-real。 |
| 数据许可 | 本次未确认数据专用许可 |
| 限制、证据缺口与去重 | 归档A为真实、B为仿真；CycleGAN不要求样本逐一配对，不能把它称作精确同姿态真实—仿真真值对。 |


<a id="P11"></a>

### P11 · PhysiCLeAR（Octopi 1.0/1.5）

**分类：**实物探测／标定／混合实验台　**参考年份：**2024　**证据：**B（已核实官方下载入口／脚本，归档未验证；计入主统计）

**数据／项目链接：**[官方数据下载说明](https://github.com/clear-nus/octopi-1.5)；[数据归档](https://drive.google.com/file/d/1ckSzE4DxSiq4U34gWBIUGreImryLw94c/view)；[原项目](https://octopi-tactile-lvlm.github.io/)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 日常物体接触视频及物理属性/语言任务；本文不把抽帧数当原始接触数。 |
| 传感器、模态与时序 | 人手持GelSight Mini按压/旋转，硬度、粗糙度、凹凸等物理标签；后续含不同表面marker版本。 |
| 动作与状态 | 无通用机器人动作监督。 |
| 对触觉世界模型的用途 | 触觉语言语义、物理属性辨识与多物体比较。 |
| 数据许可 | 本次未确认数据专用许可 |
| 限制、证据缺口与去重 | 原Octopi README主要列模型权重；本次以Octopi1.5明确标为tactile datasets的Drive入口确认数据，合并版本不重复计数。 |

<div class="dataset-figures">
<figure class="dataset-figure" id="fig-p11-01">
<div class="figure-type"><strong>P11 · 图1</strong><span>采集对象</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/p11-01.webp" target="_blank" rel="noopener" aria-label="放大：PhysiCLeAR／Octopi官方物体集合，用于触觉物理属性和语言任务。"><img src="figures/p11-01.webp" alt="PhysiCLeAR／Octopi官方物体集合，用于触觉物理属性和语言任务。" width="826" height="552" loading="lazy" decoding="async"></a>
<figcaption>PhysiCLeAR／Octopi官方物体集合，用于触觉物理属性和语言任务。<span class="figure-links"><a href="https://octopi-tactile-lvlm.github.io/" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://octopi-tactile-lvlm.github.io/media/items.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
<figure class="dataset-figure" id="fig-p11-02">
<div class="figure-type"><strong>P11 · 图2</strong><span>真实触觉样例</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/p11-02.webp" target="_blank" rel="noopener" aria-label="放大：Octopi官方egg触觉图像：GelSight接触鸡蛋表面的局部形变。"><img src="figures/p11-02.webp" alt="Octopi官方egg触觉图像：GelSight接触鸡蛋表面的局部形变。" width="320" height="240" loading="lazy" decoding="async"></a>
<figcaption>Octopi官方egg触觉图像：GelSight接触鸡蛋表面的局部形变。<span class="figure-links"><a href="https://octopi-tactile-lvlm.github.io/" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://octopi-tactile-lvlm.github.io/media/tactile_images/egg.jpg" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="P12"></a>

### P12 · Touch-Ex

**分类：**实物探测／标定／混合实验台　**参考年份：**2026　**证据：**A（已核实实际数据文件目录；计入主统计）

**数据／项目链接：**[发布方数据及采集说明](https://huggingface.co/datasets/gemixin/touch-ex)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 135K+触觉帧、1,128接触、23家居物体；14训练物体、9未见测试物体。 |
| 传感器、模态与时序 | 人手持DIGIT，背面FSR402测施加力，滑动/旋转序列，区域/材质/硬度/文字标注。 |
| 动作与状态 | 没有机器人控制轨迹，也不含外部物体RGB。 |
| 对触觉世界模型的用途 | 区域级材质语义、触觉力等级、短序列预测。 |
| 数据许可 | CC BY 4.0 |
| 限制、证据缺口与去重 | 标题visuo-tactile指传感器内部图像；背面FSR提供按压力估计，不能当物体接触面全分布力或精确6D wrench。 |

<div class="dataset-figures">
<figure class="dataset-figure" id="fig-p12-01">
<div class="figure-type"><strong>P12 · 图1</strong><span>真实触觉样例</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/p12-01.webp" target="_blank" rel="noopener" aria-label="放大：Touch-Ex官方hammer_handle_sliding_fl3样例：锤柄局部滑动接触。图像来自DIGIT内部，不是外部物体相机。"><img src="figures/p12-01.webp" alt="Touch-Ex官方hammer_handle_sliding_fl3样例：锤柄局部滑动接触。图像来自DIGIT内部，不是外部物体相机。" width="240" height="320" loading="lazy" decoding="async"></a>
<figcaption>Touch-Ex官方hammer_handle_sliding_fl3样例：锤柄局部滑动接触。图像来自DIGIT内部，不是外部物体相机。<span class="figure-links"><a href="https://huggingface.co/datasets/gemixin/touch-ex/raw/main/README.md" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://huggingface.co/datasets/gemixin/touch-ex/resolve/main/images/examples/hammer_handle_sliding_fl3.jpg" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
<figure class="dataset-figure" id="fig-p12-02">
<div class="figure-type"><strong>P12 · 图2</strong><span>采集对象</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/p12-02.webp" target="_blank" rel="noopener" aria-label="放大：Touch-Ex官方训练集物体及区域标记；未见物体测试集合另列，正文保留训练／测试划分。"><img src="figures/p12-02.webp" alt="Touch-Ex官方训练集物体及区域标记；未见物体测试集合另列，正文保留训练／测试划分。" width="2400" height="1873" loading="lazy" decoding="async"></a>
<figcaption>Touch-Ex官方训练集物体及区域标记；未见物体测试集合另列，正文保留训练／测试划分。<span class="figure-links"><a href="https://huggingface.co/datasets/gemixin/touch-ex/raw/main/README.md" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://huggingface.co/datasets/gemixin/touch-ex/resolve/main/images/objects/labelled_objects.jpg" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="P13"></a>

### P13 · Tactile Deformation Response

**分类：**实物探测／标定／混合实验台　**参考年份：**2026　**证据：**A（已核实实际数据文件目录；计入主统计）

**数据／项目链接：**[发布方数据](https://huggingface.co/datasets/Tachintech/tactile-deformation-response)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 32×32阵列，每样本64时刻；任务为rigid/deformable分类；未报未经核对的独立物体数。 |
| 传感器、模态与时序 | 去基线、归一化触觉响应时序，NPZ+JSON metadata，官方提供train/validation/test。 |
| 动作与状态 | 无动作或场景视频声明。 |
| 对触觉世界模型的用途 | 数值触觉序列编码、刚柔响应辨识。 |
| 数据许可 | GPL 3.0 |
| 限制、证据缺口与去重 | 归一化响应不等于牛顿/帕斯卡物理量；stiffness为人工估计/实验参考，非标定刚度真值。64×32×32第一维是时间，不是64个传感器。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-p13-sample">
<div class="figure-type"><strong>P13 · 图1</strong><span>本报告据实测数据绘制</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/p13-sample.webp" target="_blank" rel="noopener" aria-label="放大：本报告依据公开NPZ绘制的真实样例：测试文件20260712_103545_027153_rigid_70中第0、16、32、48、63帧，所有子图共用色标。颜色是数据存储的去基线归一化响应，不是牛顿／帕斯卡，也不是材料刚度真值。"><img src="figures/p13-sample.webp" alt="本报告依据公开NPZ绘制的真实样例：测试文件20260712_103545_027153_rigid_70中第0、16、32、48、63帧，所有子图共用色标。颜色是数据存储的去基线归一化响应，不是牛顿／帕斯卡，也不是材料刚度真值。" width="2093" height="478" loading="lazy" decoding="async"></a>
<figcaption>本报告依据公开NPZ绘制的真实样例：测试文件20260712_103545_027153_rigid_70中第0、16、32、48、63帧，所有子图共用色标。颜色是数据存储的去基线归一化响应，不是牛顿／帕斯卡，也不是材料刚度真值。<span class="figure-links"><a href="https://huggingface.co/datasets/Tachintech/tactile-deformation-response/resolve/main/data/test/20260712_103545_027153_rigid_70.npz" target="_blank" rel="noopener">来源：公开数据文件</a><a href="https://huggingface.co/datasets/Tachintech/tactile-deformation-response/resolve/main/data/test/20260712_103545_027153_rigid_70.npz" target="_blank" rel="noopener">查看原始数据</a></span></figcaption>
</figure>
</div>



## 9. 条件开放（另列2个）

此处公开卡片可见不等于原始归档可匿名读取；不混入A+B主数。


<a id="C01"></a>

### C01 · TouchScale

**分类：**人类ego＋实测触觉　**参考年份：**2026　**证据：**C（条件开放，已见卡片／目录但数据需接受条款；不计入主统计）

**数据／项目链接：**[发布状态及数据](https://huggingface.co/datasets/2077AIDataFoundation/TouchScale)；[项目](https://touch-scale.github.io/)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 2026-10-09数据卡：当前100小时；完整500小时计划2026年11月前发布。卡片并列15,324 episodes、929任务、22场景，应进一步确认这些总量与100小时子集的对应关系。 |
| 传感器、模态与时序 | 头戴RGB-D、双腕RGB、双手各880taxels；30Hz视频；自然语言任务。 |
| 动作与状态 | 人类演示；不能因论文有机器人后训练就认为公开100小时全为机器人动作轨迹。 |
| 对触觉世界模型的用途 | 高优先级条件开放候选：扩大人类视觉—触觉预训练。 |
| 数据许可 | CC BY-NC 4.0；HF自动条款门控 |
| 限制、证据缺口与去重 | API gated=auto；公共README经blob/resolve路径可读，不代表payload无门控。仅核实卡片/文件目录，未接受条款或下载受限数据。论文/网页500小时不能写成当前开放500小时。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-c01-01">
<div class="figure-type"><strong>C01 · 图1</strong><span>采集与样例</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/c01-01.webp" target="_blank" rel="noopener" aria-label="放大：TouchScale官方完整采集图：头戴RGB-D、双腕相机、全手触觉与拧布过程。配图来自公开项目，不表示受门控原始数据已下载；发布规模仍采用2026-10-09核查口径。"><img src="figures/c01-01.webp" alt="TouchScale官方完整采集图：头戴RGB-D、双腕相机、全手触觉与拧布过程。配图来自公开项目，不表示受门控原始数据已下载；发布规模仍采用2026-10-09核查口径。" width="2400" height="1320" loading="lazy" decoding="async"></a>
<figcaption>TouchScale官方完整采集图：头戴RGB-D、双腕相机、全手触觉与拧布过程。配图来自公开项目，不表示受门控原始数据已下载；发布规模仍采用2026-10-09核查口径。<span class="figure-links"><a href="https://touch-scale.github.io/" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://touch-scale.github.io/assets/images/paper/data-collection.svg?v=public-release-1" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
<figure class="dataset-figure wide" id="fig-c01-02">
<div class="figure-type"><strong>C01 · 图2</strong><span>传感器布局</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/c01-02.webp" target="_blank" rel="noopener" aria-label="放大：TouchScale官方右手触点布局图，标出880taxels及手部区域索引；双手对应字段仍须分别读取。"><img src="figures/c01-02.webp" alt="TouchScale官方右手触点布局图，标出880taxels及手部区域索引；双手对应字段仍须分别读取。" width="2100" height="1240" loading="lazy" decoding="async"></a>
<figcaption>TouchScale官方右手触点布局图，标出880taxels及手部区域索引；双手对应字段仍须分别读取。<span class="figure-links"><a href="https://huggingface.co/datasets/2077AIDataFoundation/TouchScale/blob/main/README.md" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://huggingface.co/datasets/SingleBicycle/TouchScale-assets/resolve/main/glove_layout.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="C02"></a>

### C02 · ToucHD（AnyTouch 2，真实Force/Mani）

**分类：**实物探测／标定／混合实验台　**参考年份：**2026　**证据：**C（条件开放，已见卡片／目录但数据需接受条款；不计入主统计）

**数据／项目链接：**[Force](https://huggingface.co/datasets/BAAI/ToucHD-Force)；[Mani](https://huggingface.co/datasets/BAAI/ToucHD-Mani)；[项目](https://gewu-lab.github.io/AnyTouch2/)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 全套2,426,174 contact samples包含仿真；Force为722,436 touch-force pairs，Mani约585K；其余约1,119K为Sim。 |
| 传感器、模态与时序 | 多类光学触觉、力、动态操控序列；Force涉及71种压头及受控滑动。 |
| 动作与状态 | Force是受控物理过程；Mani的动作字段要逐文件确认。 |
| 对触觉世界模型的用途 | 高优先级：动态触觉/力对齐与跨传感器预训练。 |
| 数据许可 | MIT；HF自动条款门控，要求联系方式/单位/国家 |
| 限制、证据缺口与去重 | Force/Mani按一个数据集家族计；不能把包含Sim的2.43M全部当真实触觉。匿名raw为401，仅条件开放统计加1。 |

<div class="dataset-figures">
<figure class="dataset-figure" id="fig-c02-01">
<div class="figure-type"><strong>C02 · 图1</strong><span>真实数据样例</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/c02-01.webp" target="_blank" rel="noopener" aria-label="放大：ToucHD论文图6：真实操作部分的视觉—触觉时间序列，包括套笔帽与放置试管等。这里展示Mani真实部分，不把Sim数量算入真实触觉。"><img src="figures/c02-01.webp" alt="ToucHD论文图6：真实操作部分的视觉—触觉时间序列，包括套笔帽与放置试管等。这里展示Mani真实部分，不把Sim数量算入真实触觉。" width="1317" height="2028" loading="lazy" decoding="async"></a>
<figcaption>ToucHD论文图6：真实操作部分的视觉—触觉时间序列，包括套笔帽与放置试管等。这里展示Mani真实部分，不把Sim数量算入真实触觉。<span class="figure-links"><a href="https://arxiv.org/html/2602.09617v1" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://arxiv.org/html/2602.09617v1/mani-time.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
<figure class="dataset-figure wide" id="fig-c02-02">
<div class="figure-type"><strong>C02 · 图2</strong><span>数据层级</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/c02-02.webp" target="_blank" rel="noopener" aria-label="放大：ToucHD论文图1：动态触觉数据层级。原图包含真实与仿真数据；它描述整套框架，不是全部公开真实样本的数量证明。"><img src="figures/c02-02.webp" alt="ToucHD论文图1：动态触觉数据层级。原图包含真实与仿真数据；它描述整套框架，不是全部公开真实样本的数量证明。" width="2030" height="870" loading="lazy" decoding="async"></a>
<figcaption>ToucHD论文图1：动态触觉数据层级。原图包含真实与仿真数据；它描述整套框架，不是全部公开真实样本的数量证明。<span class="figure-links"><a href="https://arxiv.org/html/2602.09617v1" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://arxiv.org/html/2602.09617v1/data2_cropped.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>



## 10. 汇编与再处理包（5个，不新增独立采集数）

这些包能节省工程时间，但首先要建立与上游来源的对应关系；转换可能丢失时间戳、动作或高频信号。


<a id="A01"></a>

### A01 · Foundation Tactile / FoTa

**分类：**汇编／再处理包（不重复计数）　**参考年份：**2024　**证据：**A（已核实实际数据文件目录；不计入主统计）

**数据／项目链接：**[官方数据](https://huggingface.co/datasets/alanz-mit/FoundationTactile)；[论文/代码](https://github.com/alanzjl/t3)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 3M+图像、13种传感器、11任务。 |
| 传感器、模态与时序 | 多种光学触觉与任务标签，WebDataset tar格式。 |
| 动作与状态 | 主要表征数据；各来源动作和时序信息不一致。 |
| 对触觉世界模型的用途 | 统一加载/编码器基线。 |
| 数据许可 | 汇编卡MIT；原始来源许可仍须逐项保留 |
| 限制、证据缺口与去重 | 汇编VisGel、TVL、Touch and Go、Calandra、Yuan18、YCB-Sight、ObjectFolder等，并含作者新增采集。主统计不把整个包再加1；新增部分未单独审计为完整来源，故暂不加数。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-a01-01">
<div class="figure-type"><strong>A01 · 图1</strong><span>汇编数据概览</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/a01-01.webp" target="_blank" rel="noopener" aria-label="放大：FoTa论文图2：汇编来源、传感器与任务分布及触觉外观。图中的原始来源已经分别计数，FoTa不再新增一个采集来源。"><img src="figures/a01-01.webp" alt="FoTa论文图2：汇编来源、传感器与任务分布及触觉外观。图中的原始来源已经分别计数，FoTa不再新增一个采集来源。" width="2031" height="1194" loading="lazy" decoding="async"></a>
<figcaption>FoTa论文图2：汇编来源、传感器与任务分布及触觉外观。图中的原始来源已经分别计数，FoTa不再新增一个采集来源。<span class="figure-links"><a href="https://arxiv.org/html/2406.13640v3" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://arxiv.org/html/2406.13640v3/data_pie_plots.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="A02"></a>

### A02 · Sparsh预训练混合与TacBench

**分类：**汇编／再处理包（不重复计数）　**参考年份：**2024　**证据：**A（已核实实际数据文件目录；不计入主统计）

**数据／项目链接：**[官方来源清单/下载](https://github.com/facebookresearch/sparsh)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 多来源光学触觉预训练与6项下游任务。 |
| 传感器、模态与时序 | DIGIT/GelSight图像；力、滑移、姿态、抓取和衣物标签。 |
| 动作与状态 | 任务特定监督。 |
| 对触觉世界模型的用途 | 成熟自监督触觉编码器和评价基线。 |
| 数据许可 | 分来源：部分CC BY-NC 4.0；须保留上游条款 |
| 限制、证据缺口与去重 | Touch-Slide与TacBench原创子集已经单列；借用数据不新增计数。Sparsh代码许可不是所有数据的统一许可。 |


<a id="A03"></a>

### A03 · FTP-1 Dataset

**分类：**汇编／再处理包（不重复计数）　**参考年份：**2026　**证据：**A（已核实实际数据文件目录；不计入主统计）

**数据／项目链接：**[HF公开目录](https://huggingface.co/datasets/MJJJJ1064/FTP-1-Dataset)；[官方仓库](https://github.com/michaelyuancb/ftp1-policy)；[论文](https://arxiv.org/abs/2606.13102)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 论文约3,000小时、26来源；本次HF根目录仍有15个一级来源目录；数据卡声称18 archives。 |
| 传感器、模态与时序 | 图像/阵列/state混合，人类/灵巧手/夹爪/UMI。 |
| 动作与状态 | 统一转换后的操作数据。 |
| 对触觉世界模型的用途 | 多来源操作数据入口和格式参考。 |
| 数据许可 | ModelScope汇编标MIT；HF卡无统一许可字段；保留各上游许可 |
| 限制、证据缺口与去重 | 26论文来源≠已公开26个独立集；15目录≠15个独立来源（RDP/RH20T/Unit拆配置）。不把3,000小时当已开放小时；EgoTac/OpenTouch等在根目录未见同名归档。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-a03-01">
<div class="figure-type"><strong>A03 · 图1</strong><span>汇编数据概览</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/a03-01.webp" target="_blank" rel="noopener" aria-label="放大：FTP-1论文图4：论文使用的26个来源和不同触觉模态。此为论文全量概览，不代表当前可下载目录覆盖全部26个来源。"><img src="figures/a03-01.webp" alt="FTP-1论文图4：论文使用的26个来源和不同触觉模态。此为论文全量概览，不代表当前可下载目录覆盖全部26个来源。" width="2041" height="819" loading="lazy" decoding="async"></a>
<figcaption>FTP-1论文图4：论文使用的26个来源和不同触觉模态。此为论文全量概览，不代表当前可下载目录覆盖全部26个来源。<span class="figure-links"><a href="https://arxiv.org/html/2606.13102v2" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://arxiv.org/html/2606.13102v2/datasets_main_0520_v1.png" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="A04"></a>

### A04 · Touch100k

**分类：**汇编／再处理包（不重复计数）　**参考年份：**2024　**证据：**B（已核实官方下载入口／脚本，归档未验证；不计入主统计）

**数据／项目链接：**[官方项目/数据](https://cocacola-lab.github.io/Touch100k/)；[代码](https://github.com/cocacola-lab/TLV-Link)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 约100K触觉—语言—视觉配对/标注规模。 |
| 传感器、模态与时序 | 触觉、视觉、多粒度文字描述。 |
| 动作与状态 | 主要是语言/表征监督。 |
| 对触觉世界模型的用途 | 触觉语义对齐、指令理解补充。 |
| 数据许可 | 本次未确认各上游与派生文本许可 |
| 限制、证据缺口与去重 | 此类派生标注包不应直接作为100K条全新实测接触，也不再增加独立原始采集数；原始来源应逐项去重。 |

<div class="dataset-figures">
<figure class="dataset-figure wide" id="fig-a04-01">
<div class="figure-type"><strong>A04 · 图1</strong><span>汇编样例</span><small>点击图片放大</small></div>
<a class="figure-open" href="figures/a04-01.webp" target="_blank" rel="noopener" aria-label="放大：Touch100k官方数据展示图：视觉、触觉与不同粒度语言描述。派生语言／汇编不增加原始采集来源数。"><img src="figures/a04-01.webp" alt="Touch100k官方数据展示图：视觉、触觉与不同粒度语言描述。派生语言／汇编不增加原始采集来源数。" width="1800" height="900" loading="lazy" decoding="async"></a>
<figcaption>Touch100k官方数据展示图：视觉、触觉与不同粒度语言描述。派生语言／汇编不增加原始采集来源数。<span class="figure-links"><a href="https://github.com/cocacola-lab/TLV-Link/blob/main/README.md" target="_blank" rel="noopener">来源：作者项目／论文</a><a href="https://github.com/cocacola-lab/TLV-Link/blob/main/assets/data_present.svg" target="_blank" rel="noopener">原图／视频／PDF</a></span></figcaption>
</figure>
</div>


<a id="A05"></a>

### A05 · Tactile Video Pretrain（yxma）

**分类：**汇编／再处理包（不重复计数）　**参考年份：**2026　**证据：**A（已核实实际数据文件目录；不计入主统计）

**数据／项目链接：**[主库](https://huggingface.co/datasets/yxma/tactile-video-pretrain)；[NC库](https://huggingface.co/datasets/yxma/tactile-video-pretrain-nc)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 主卡报告47.65小时视频流；NC兄弟库含VTouch子集。 |
| 传感器、模态与时序 | FreeTacMan、Touch and Go、GelSLAM等重新编码视频，tactile_only/tactile_rgb配置。 |
| 动作与状态 | 训练方便但不保证保留全部原始动作/高频触觉字段。 |
| 对触觉世界模型的用途 | 快速搭建视频触觉预训练加载器。 |
| 数据许可 | 按源保留MIT/CC BY及NC-SA等原始条款 |
| 限制、证据缺口与去重 | 两配置共享视频，双指/多视角stream-hour不能当行为小时；主卡整体MIT不能覆盖Touch and Go的署名义务；不另加独立数据源。 |



## 11. 力觉扩展（2个，不混入主触觉数）

若目标是接触力学世界模型，腕部wrench同样值得利用；只是其空间分辨率和物理意义与指尖触觉不同。


<a id="F01"></a>

### F01 · REASSEMBLE

**分类：**力觉扩展（不计主触觉数量）　**参考年份：**2025　**证据：**A（已核实实际数据文件目录；不计入主统计）

**数据／项目链接：**[官方数据](https://researchdata.tuwien.ac.at/records/0ewrv-8cb44)；[字段/作者说明](https://github.com/TUWIEN-ASL/REASSEMBLE)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 4,551示范、4,035成功、781分钟；17物体，pick/insert/remove/place。 |
| 传感器、模态与时序 | 机器人RGB、多视角、事件相机、麦克风、六维F/T、本体状态。 |
| 动作与状态 | 机器人时序、分段、成败和语言标签。 |
| 对触觉世界模型的用途 | 接触丰富装配的力觉世界模型。 |
| 数据许可 | 代码MIT；数据以TUData页面条款为准 |
| 限制、证据缺口与去重 | 腕部六维力/力矩不等于指尖触觉阵列/触觉图像，故不进入主触觉计数；与FTP内重打包去重。 |


<a id="F02"></a>

### F02 · MotionTrans（FTP公开机器人部分）

**分类：**力觉扩展（不计主触觉数量）　**参考年份：**2025　**证据：**A（已核实实际数据文件目录；不计入主统计）

**数据／项目链接：**[项目](https://motiontrans.github.io/)；[公开包](https://huggingface.co/datasets/MJJJJ1064/FTP-1-Dataset/tree/main/MotionTrans)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 论文3,213条为人类/机器人混合示范；不能作为公开触觉轨迹数量。 |
| 传感器、模态与时序 | 跨本体动作；FTP收录InspireHand state类型力学信号。 |
| 动作与状态 | 有运动迁移轨迹；触觉/力字段物理含义需看Inspire接口。 |
| 对触觉世界模型的用途 | 动作空间迁移、力学状态辅助。 |
| 数据许可 | 原始数据许可本次未确认 |
| 限制、证据缺口与去重 | 未在本次证据中确认公开部分为可与压力阵列等同的直接触觉，保守列扩展力觉类，不混入R。 |



## 12. 待核实来源（7个，不计数）

“本次未确认”不等于“确定未公开”。保留这些链接是为了后续补核，尤其是动作条件预测非常相关的Tactile Grasp and Move、DexUMI和3D-ViTac。


<a id="X01"></a>

### X01 · DexUMI

**分类：**待核实（不计数）　**参考年份：**2025　**证据：**D（下载、来源或分类依据尚不足；不计入主统计）

**数据／项目链接：**[官方项目](https://dex-umi.github.io/)；[官方仓库](https://github.com/real-stanford/DexUMI)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 本次已读官方项目与仓库，未确认与论文匹配的公开完整原始触觉数据入口。 |
| 传感器、模态与时序 | 人手外骨骼、手指力/触觉与视觉转换。 |
| 动作与状态 | 系统能采集并重定向动作，但能力描述不等于数据已发布。 |
| 对触觉世界模型的用途 | 采集系统/人手到灵巧手迁移参考。 |
| 数据许可 | 代码MIT；数据许可/完整下载待核实 |
| 限制、证据缺口与去重 | FTP论文使用DexumiInspire/XHand不证明对应归档已发布；暂不进入公开数据主计数。 |


<a id="X02"></a>

### X02 · EgoTac-Open / Kitchen / Hotel及EgoTac-SC

**分类：**待核实（不计数）　**参考年份：**2026　**证据：**D（下载、来源或分类依据尚不足；不计入主统计）

**数据／项目链接：**[新发现的公开包](https://huggingface.co/datasets/wkzhang/EgoTac-SC)；[FTP原始来源依据](https://arxiv.org/abs/2606.13102)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | FTP论文使用AetherGlove三域；另找到wkzhang/EgoTac-SC，卡称857个Zarr录制、45ZIP、约458GB。 |
| 传感器、模态与时序 | SC卡只说明aether_v1归档，未充分公开传感器字段/视频视角/与FTP三域的对应关系。 |
| 动作与状态 | 未验证。 |
| 对触觉世界模型的用途 | 值得后续追溯的新线索。 |
| 数据许可 | EgoTac-SC卡Apache 2.0；三项FTP来源授权未确认 |
| 限制、证据缺口与去重 | 不能将SC自动等同三个FTP来源，也不能依据名称直接加入ego数。公开SC文件存在与“三域全量已经开源”是两个不同结论。 |


<a id="X03"></a>

### X03 · 3D-ViTac

**分类：**待核实（不计数）　**参考年份：**2024　**证据：**D（下载、来源或分类依据尚不足；不计入主统计）

**数据／项目链接：**[官方项目](https://binghao-huang.github.io/3D-ViTac/)；[训练代码](https://github.com/binghao-huang/3d-vitac-training-code)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 官方有硬件/训练代码和真实实验。 |
| 传感器、模态与时序 | 机器人3D视觉与数值触觉阵列。 |
| 动作与状态 | 方法需要动作示范，但本次未确认独立完整数据释放。 |
| 对触觉世界模型的用途 | 统一3D触觉表示的直接参考。 |
| 数据许可 | 代码开放；数据下载本次未确认 |
| 限制、证据缺口与去重 | Touch in the Wild使用相关触觉硬件不意味着3D-ViTac论文数据已经公开；不重复/推断计数。 |


<a id="X04"></a>

### X04 · Tactile Grasp and Move / Action-Conditioned Tactile Prediction

**分类：**待核实（不计数）　**参考年份：**2022　**证据：**D（下载、来源或分类依据尚不足；不计入主统计）

**数据／项目链接：**[作者仓库](https://github.com/WillMandil001/Tactile_Grasp_and_Move_Dataset)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 作者说明包含有意制造滑移的人遥操作机器人运动数据；本次未核实可下载payload。 |
| 传感器、模态与时序 | 双指触觉、机器人本体和任务空间运动、物体相对腕部位姿。 |
| 动作与状态 | 对动作条件预测很相关。 |
| 对触觉世界模型的用途 | 后续数据获取优先核查。 |
| 数据许可 | 待确认 |
| 限制、证据缺口与去重 | 仓库README真实存在，但本次未确认数据文件/下载归档，故不加主数量。 |


<a id="X05"></a>

### X05 · Touch-GS Dataset

**分类：**待核实（不计数）　**参考年份：**2024　**证据：**D（下载、来源或分类依据尚不足；不计入主统计）

**数据／项目链接：**[作者数据仓库](https://github.com/armlabstanford/touch-gs-data)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 官方数据仓库描述imgs、touches、poses等目录。 |
| 传感器、模态与时序 | DenseTact真实触觉图/深度、相机与几何。 |
| 动作与状态 | 主要重建视角/触点，不是连续操作控制。 |
| 对触觉世界模型的用途 | 几何世界表示候选。 |
| 数据许可 | 待确认 |
| 限制、证据缺口与去重 | 本次仅核实README字段描述，未确认实际数据文件目录；不把“Official Dataset”字样当下载完成。 |


<a id="X06"></a>

### X06 · HUST-Tactile

**分类：**待核实（不计数）　**参考年份：**2025　**证据：**D（下载、来源或分类依据尚不足；不计入主统计）

**数据／项目链接：**[作者仓库](https://github.com/HUSTOROP/Hust-Tac-Dataset)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 作者README：3,949样本，正常/凸点/凹坑/划痕4类。 |
| 传感器、模态与时序 | GelSight微缺陷接触图像。 |
| 动作与状态 | 无动作监督声明。 |
| 对触觉世界模型的用途 | 触觉精细纹理/表面异常补充。 |
| 数据许可 | 待确认 |
| 限制、证据缺口与去重 | 本次核实统计说明但没有完成payload入口核实，保守不计主数量。 |


<a id="X07"></a>

### X07 · zhicao/egoforce（FusionX录制）

**分类：**待核实（不计数）　**参考年份：**2026　**证据：**D（下载、来源或分类依据尚不足；不计入主统计）

**数据／项目链接：**[候选录制库](https://huggingface.co/datasets/zhicao/egoforce)

| 项目 | 核实信息 |
|---|---|
| 规模与发布口径 | 公开MCAP及可视化资源；本次未统计独立有效录制小时。 |
| 传感器、模态与时序 | 头/腕相机、FusionX双手压力与IMU；卡片列死通道、偏置、部分录制缺腕相机等问题。 |
| 动作与状态 | 有人手姿态/语义topic，覆盖度未审计。 |
| 对触觉世界模型的用途 | 进一步扩大ego来源的候选。 |
| 数据许可 | 未见数据专用许可 |
| 限制、证据缺口与去重 | 作者身份/来源授权、与FusionX V3样本是否重叠仍不清；不进入已确认独立来源数。不要与DFKI的EgoForce姿态模型数据混淆。 |



### 12.1 历史数据的检索缺口

本轮未完成PHAC／PHAC-2（Penn Haptic Adjective Corpus）、HaTT（Penn Haptic Texture Toolkit）、LMT Haptic Texture、21ADL，以及ViTac Cloth／GelFabric的原始数据入口、现有可访问性与独立来源关系复核。这些是重要补核线索，不能因为旧站失联就断言数据不存在，也不能仅据综述名称加入已确认总数。

PHAC／HaTT旧站候选分别为[PHAC](https://haptics.seas.upenn.edu/index.php/Research/PHAC)、[PHAC-2](https://haptics.seas.upenn.edu/index.php/Research/PHAC-2)、[HaTT](https://haptics.seas.upenn.edu/index.php/Research/HaTT)，本次访问超时，**这些是待验证历史路径，不是已确认可用下载链接**。其余线索不提供未经核实的下载地址。ViTac Cloth与Active Clothing不能仅因作者／传感器相近就认定同一采集库；GelFabric与Touch100k的派生关系也需继续追溯。

## 13. 有价值但不应冒充触觉真值的ego／接触数据

以下用于视觉、动作和几何预训练，不增加上面的实测触觉数量。

| 数据 | 官方链接 | 可以学什么 | 为什么不纳入真实触觉主统计 |
|---|---|---|---|
| Ego4D | [官网](https://ego4d-data.org/) | 大规模ego行为、时序、手物交互 | 公开模态不等于带同步触觉传感器读数 |
| Ego-Exo4D | [官网](https://ego-exo4d-data.org/) | ego／exo对齐、技能和身体运动 | 多视角／姿态监督不能替代压力真值 |
| EgoDex | [作者仓库](https://github.com/apple/ml-egodex) | 灵巧人手操作与手部运动 | 运动跟踪数据不自动包含触觉压力；本次仓库直连超时 |
| HOT3D | [官网及下载](https://facebookresearch.github.io/hot3d/) | 精确手／物体三维跟踪、头戴视角 | 手物接近和接触几何不是直接测得的力／形变 |
| ARCTIC | [官网](https://arctic.is.tue.mpg.de/) | 双手操作关节物体、运动与接触几何 | 几何接触标注不是触觉传感器读数 |
| GRAB | [官网](https://grab.is.tue.mpg.de/) | 全身／手物运动、几何接触 | 不是带真实触觉信号的ego录制 |
| ContactDB | [官网及下载](https://contactdb.cc.gatech.edu/) | 热成像恢复的物体表面接触区域 | 接触留下的热印迹不等于同步逐帧压力或力 |
| ContactPose | [官网及下载](https://contactpose.cc.gatech.edu/) | 抓型、手姿态、物体表面接触 | 接触区域适合几何先验，不能直接当动态压力目标 |

视频模型生成的压力图／触觉词语应保存为独立的弱监督字段，例如`pseudo_tactile`，并记录生成模型及置信度。不能与压力板、手套或光学传感器实测混在同一“GT”列中。EgoTactile裸手子集也应单独打标，因为其参考压力来自另一只手。

## 14. 对自建触觉世界模型的具体建议

### 14.1 先明确模型要预测什么

建议区分三种任务，数据要求由低到高：

| 模型任务 | 最小数据条件 | 适合的来源 | 尚缺的关键信息 |
|---|---|---|---|
| 触觉表征／跨模态对齐 | 触觉读数，可选视觉、语言、力标签 | TacQuad、HPT、TaF、TVL、ObjectFolder-Real、Touch and Go | 很多没有动作，不能直接作为可控制世界模型 |
| 运动条件触觉预测 | 连续触觉＋同一时间轴的手／工具／传感器运动 | OpenTouch、ActionSense、FreeTacMan、Touch in the Wild完整包、exUMI、YCB-Slide、NormalFlow | 观测到的运动是行为结果；需区分真实控制与重定向动作 |
| 机器人动作条件世界模型 | 连续视觉／触觉／状态＋有明确执行时间和坐标的控制命令 | RDP、HapTile优先；UniT、VLA-Touch、LET-Dex、VTouch、Sharpa进一步抽样核实 | 各源控制频率、命令语义、时延、缺失字段仍须统一 |

一个合理的训练目标是预测下一段视觉／触觉／状态：`p(v[t+1:t+k], tactile[t+1:t+k], state[t+1:t+k] | history, action[t:t+k-1])`。若没有真实action，可训练被动时序模型或以观测运动为条件的模型，但不能仅靠把位姿字段改名为action，就声称获得了控制响应模型。Tachin的140维action等于当前帧动捕，是这个区别的具体例子。

### 14.2 第一批接入顺序

| 优先级／路线 | 建议接入 | 选择理由 | 第一次抽样检查 |
|---|---|---|---|
| P0：先跑通机器人动作预测 | RDP＋HapTile | 官方字段明确包含动作／控制；保留连续接触过程 | action是否在下一帧生效、delta pose坐标、触觉／相机时延 |
| P0：人手压力与姿态 | OpenTouch＋ActionSense；OpenGraph做轻量管线样例 | 有ego压力，前两者提供人体运动；样例便于快速调试 | 完整三模态有效区间、手套标定、缺失手指与坏通道 |
| P1：扩大人手自然操作 | EgoTouch、EgoTactile；条件允许再接TouchScale | 多任务、多环境或跨人泛化 | EgoTouch上传覆盖与WiLoR置信度；EgoTactile裸手参考标签；TouchScale实际开放100小时 |
| P1：扩大操作分布 | FreeTacMan、Touch in the Wild、exUMI、ViTaMIn | 人操持工具保留触觉和运动，更容易对接夹爪动作空间 | 完整轨迹与仅预训练子包分开、工具坐标到机器人坐标映射 |
| P1：跨传感器物理表示 | TacQuad＋HPT＋TaF＋TacBench | 传感器对齐、压力／ATI力、滑移与姿态监督 | 时间同步、标定单位、真实力标签与派生标签区分 |
| P2：灵巧手与大库 | LET-Dex、VTouch、Sharpa、Humanoid Everyday；Daimon-Infinity | 多指、多本体或更大操作覆盖 | 先核实manifest和触觉topic覆盖，不以整库TB／任务数估算可用触觉小时 |

人手压力阵列与机器人GelSight／DIGIT图像的观测空间差异很大。更合理的起步方案是分别使用传感器编码器，再对齐接触位置、法向／剪切、滑移、物体局部几何等中间表示；传感器类型、标定和本体信息作为条件。跨模态迁移是否有效应通过固定机器人测试集验证，不能仅凭预训练规模推断。

### 14.3 数据接入时必须核实的字段

这部分是模型可训练性的检查，不是泛泛的质量清单。建议先从每个候选源抽取少量完整episode：

1. **序列和时钟：**保存原始时间戳、episode边界、相机曝光／触觉采样／动作发送时间；先测偏移，再决定重采样。30Hz处理输出不证明原始传感器都以30Hz采集。
2. **物理量和空间：**分别保存原始读数、零点、标定、单位、传感器在手指／工具上的变换、相机外参；压力、总法向力、wrench、marker位移和像素颜色各自建字段。
3. **控制语义：**区分command、target、measured state、estimated pose、retargeted action；保留absolute／delta、位置／速度／力控、关节／TCP及坐标系。
4. **缺失与估计：**给坏触点、丢帧、遮挡手姿态、未同步相机和伪标签显式mask。既不能把未观测压力当零压力，也不能把估计姿态当硬件真值。
5. **来源与切分：**维护`source_family / original_episode / release_version / sensor_id / subject / object / scene`，原始和FTP／FoTa副本共用来源标识。按参与者、物体或场景分割；不要相邻视频帧随机切分造成泄漏。
6. **接触平衡：**记录无接触、静态稳定接触、滑移、碰撞、释放等阶段的有效时长。仅重建大部分平坦背景的低误差，不能代表接触动力学已经学好。

可先用下面的中间格式，不要求所有库都填满：

```text
source_family, episode_id, timestamp, sensor_id, embodiment_id
vision[view], tactile_raw[sensor], tactile_calibrated[sensor], tactile_mask
state, command, command_timestamp, human_or_tool_pose, pose_confidence
camera_calibration, sensor_extrinsics, contact_labels, label_origin
```

### 14.4 评价与规模汇总

评价至少包含接触起止、压力／力误差（有标定真值时）、滑移事件、触觉几何变化、动作改变后的未来预测差异，以及跨物体／传感器泛化。光学触觉只看图像重建分数容易被背景主导，宜另报接触区域指标。长时rollout和策略控制收益应在有动作的真机子集单独测试。

本报告**不提供一个将所有库帧数／小时简单相加的“总训练规模”**。合理的汇总单位是去重后的有效episode和单次交互时间：多相机、多指、不同采样率不重复贡献行为小时；独立保留各传感器的观测量。比如HPT的1.59M混合统计、FTP的论文3,000小时与当前15个来源目录、TouchScale的100／500小时，以及Daimon的发布1,000／已采1,209／目标10,000小时，都不具备直接相加的条件。

## 15. 交付文件与结论边界

| 文件 | 内容 |
|---|---|
| [report.html](report.html) | 便于阅读、带目录和筛选索引的完整报告，可由浏览器打印 |
| [report.md](report.md) | 中文报告源文档，适合版本管理和继续修订 |
| [datasets.csv](datasets.csv) | UTF-8 BOM表格，含全部71条、链接、分类、规模、模态、动作、许可、证据与限制 |
| [datasets.json](datasets.json) | 同一条目库的机器可读版本 |
| [summary.json](summary.json) | 从条目库自动计算的统计，避免正文与总表手算不一致 |
| [source_index.csv](source_index.csv) | 请求URL、检查时间、状态、访问路径与本地缓存路径；含失败请求 |
| [hub_audit.json](hub_audit.json) | HF仓库SHA、门控状态、许可、文件目录及样例路径快照 |

**最终统计：人类ego 8个，机器人真机23个，UMI／人操持工具5个，其他真实触觉19个，共55个具有公开发布证据的独立来源家族；另有2个条件开放，共57个。** 如果只接受已经看到实际文件目录的主类条目，则为34个；如果只接受本次确认数据许可且无非商业限制的主类条目，则为23个。这两个23和34分别来自不同筛选条件，不能互相替代。

检索无法证明全网穷尽；历史站点失联、新库持续发布、同源版本关系和部分数据许可仍有缺口。数字应与2026-10-09日期、A／B／C证据口径及来源家族去重规则一起引用。后续补核应修改同一目录并重算统计，不把尚未确认的条目提前纳入。


## 16. 配图来源与覆盖（2026-10-16）

配图更新：2026-10-16。本版新增81张图片，覆盖56个目录条目，其中51个属于主统计来源，8个人类ego来源均已配图。4组数值触觉样例由本报告读取公开数据后绘制。数据规模与开放状态仍沿用2026-10-09核验口径。

每张图都在对应条目下给出原始链接。项目图、论文图、视频截帧与实测数据重绘使用不同图注；不能据一张介绍图推断公开数据的全量覆盖。

[配图来源CSV](figure_sources.csv) · [逐图元信息](figures/manifest.json) · [全部条目配图覆盖](figure_coverage.json)

本轮主统计条目中，以下来源暂未取得可核实且适合展示的图片，继续保留官方下载／项目入口：R04 Eagle Shoal / Visual-Tactile Dataset、R18 ARQ-CRISP Slip Detection Dataset、R22 Active Clothing Material Perception（Yuan18）、P10 GelSight Sim2Real（真实A域）。没有用其他数据集或生成图代替。
