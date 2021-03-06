class: middle, center

# 大数据系统

## 云计算视频监控

陈一帅

[yschen@bjtu.edu.cn](mailto:yschen@bjtu.edu.cn)

.footnote[网络智能实验室]

北京交通大学电子信息工程学院

---

# 内容

- .red[背景]
- 解决方案
- 实例分析
- 系统和案例

---

# 背景

- 配备摄像头的物联网媒体流应用程序正在兴起
- 启用实时计算机视觉的机器学习应用
- 领域
  - 智能家居
  - 智能城市
  - 工业自动化

---

# 智能家居

- 安全监控
- 媒体播放，智能照明，室温控制
- 一体化家居监控系统
  - 门铃、婴儿监视器、网络摄像头和家用监视系统
  - 与按门铃的访客说话
  - 在手机上远程控制搭载摄像头的扫地机器人

???

# 案例: 智能家居

- 从配备摄像头的家居设备直播视频和音频

- 使用视数据流构建各种智能家居应用程序
  - 视频播放
  - 智能照明
  - 温度控制
  - 安全监控

---

# 智能城市

- 已安装大量摄像头
  - 交通信号灯、停车场、购物商场
- 监测应用
  - 入口传感器、运动检测器、视频监控
- 应用
  - 交通管理
  - 犯罪防范
  - 应急人员调度
  - 下一代实体零售店（“拿上就走”）

???

案例: 视频警报系统

- 许多城市在交通信号灯、停车场、购物商场等几乎所有公共场所安装了大量摄像头，全天候拍摄视频
- 安全而经济高效地提取、存储和分析此类海量视频数据，用于解决交通问题、防范犯罪和派遣应急人员等
- 逐帧分析

---

# 设备智能维护

- 预测性维护
- 收集各种带有时间编码的数据
  - 视频
  - 雷达和激光雷达信号
  - 温度曲线
  - 来自设备的深度数据
- 使用机器学习方法分析和预测
  - 预测垫圈或阀门的寿命并提前安排更换部件
  - 减少停机时间和生产线上的缺陷

???

---

# 内容

- 背景
- .red[解决方案]
- 实例分析
- 系统和案例

---

# 基于云的视频物联网系统

- 自动连接并管理成千上万个摄像头
- 支持视频流式传输，存储、重放和机器学习分析

???

- 在 Amazon Web Services (AWS) 云上构建物联网 (IoT) Camera Connector 环境和无服务器架构
- 配置摄像头，将摄像头输出流式传输至 Amazon Kinesis Video Streams

组成

.center[.width-100[![](./figures/video/aws-iot-camera.png)]]

IoT 密钥配置

用于与连接的摄像头关联的 AWS IoT 策略。
AWS Identity and Access Management (IAM) 角色，以便连接的摄像头流式传输到 Kinesis Video Streams。
用于存储配置密钥的 Amazon DynamoDB 表。配置完成后，应删除这些密钥。
AWS Lambda 函数，用于创建配置密钥和角色别名、验证堆栈以及配置摄像头。
Amazon API Gateway，用于通过 HTTPS 公开配置端点。
Amazon CloudWatch 警报，通过 Amazon Simple Notification Service (Amazon SNS) 主题公开摄像头流式传输状态，并更新相关摄像头的 IoT 事物影子。

---

# 视频流数据的特点

- 时间编码数据
  - 数据流（Streaming）
  - 音频、雷达和激光雷达信号也是时间编码数据
- 视频流
  - 采用时间序列记录，每帧都通过空间转换与上一个和下一个帧相关联

---

# 功能

- 视频提取、存储、播放
- 视频直播和点播
- 低延时实时视频通话
- 计算机视觉和视频分析功能
- 支持设备数和类型

---

# 视频提取、存储、播放

- 自动配置，弹性扩展，支持从数百万台设备中提取视频流
- 持久存储和加密视频数据，并为其创建索引，以供检索
  - 可逐帧从流中检索数据，以构建实时应用程序
  - 帧级对象检索、片段元数据提取和收集、连续片段合并
- 为用户提供易用的数据访问 API，支持播放

???

- 视频聊天和对待媒体流传输（WebRTC）

基于开源项目 WebRTC，可通过简单 API 在 Web 浏览器、移动应用程序和连接的设备之间进行实时媒体流式传输与交互。一般用途包括视频聊天和对等媒体流式传输

- 使用 WebRTC，构建支持双向实时媒体流式传输的应用程序，在 Web 浏览器、移动应用程序和连接的设备之间进行双向实时媒体流式传输，以超低延迟在应用程序及连接的设备之间实现双向通信的视频聊天与对等数据分享

步骤

- 在 AWS 管理控制台中单击几次，创建一个 Kinesis 视频流
- 在设备上安装 Kinesis Video Streams 软件开发工具包，将媒体流式传输到 AWS 以便进行播放、存储和分析
- 可以按实际使用量付费。不存在预先承诺和最低费用。

---

# 存储和检索

- 持久存储
  - 使用对象（Object）存储作为底层数据存储
  - 可以持久可靠地存储
  - 可以设置和控制每个流的保留期，可以有限期，也可以无限期存储，可随时更改
- 检索
  - 可基于设备生成的时间戳或接收时间戳自动编制关于视频流数据索引
  - 可结合使用流级标签和时间戳轻松搜索和检索特定视频片段，以进行播放、分析和其他处理

???

- 使用 Amazon S3 作为底层数据存储，意味着数据可以持久可靠地存储
- 检索
  - 自动索引编制

---

# 视频直播和点播

- 使用 HTTP 直播流 (HLS) 播放直播和点播视频
- 使用完全托管的 HTTP 直播流 (HLS) 功能播放提取的视频
- 在任何浏览器或移动平台上进行实时视频直播，或存档视频点播

---

# 低延时实时视频通话

- 使用 WebRTC 支持低延迟双向媒体流式传输
- 功能
  - WebRTC 信令托管终端节点，允许程序之间安全连接，实现对等实时媒体流式传输
  - TURN 托管终端节点，在程序无法对等传输时通过云启用媒体中继
  - STUN 托管终端节点，以便应用程序在位于 NAT 或防火墙后时能够发现其公有 IP 地址
- 开发工具
  - 启用摄像头
  - Android、iOS 和 Web 应用程序客户端开发工具

---

# 计算机视觉和视频分析

- 集成 Apache MxNet、TensorFlow 和 OpenCV 等常见的 ML 框架
- 目标检测和识别
- 人脸识别

---

# 支持设备数和类型

- 支持设备数
  - 数百万台
- 支持设备类型
  - 智能手机
  - 监控摄像机
  - 雷达、激光雷达
  - 无人机、卫星、行车记录仪
  - 深度传感器等

???

- 提供各种软件开发工具包，使设备可以轻松而安全地将媒体流式传输到云，用于播放、存储、分析、机器学习和其他处理
- 构建实时、视觉且支持视频的应用程序
- 支持录制视频流的检索和播放
- 使用 HTTP 实时流 (HLS) 功能，将实时和录制媒体流式传输到浏览器或移动应用程序，实现直播和点播

问：什么是 Amazon Kinesis Video Streams？
利用 Amazon Kinesis Video Streams，您可以轻松而安全地将媒体从互联设备流式传输到 AWS，用于存储、分析、机器学习 (ML)、播放以及其他处理。Kinesis Video Streams 可以自动预置和弹性扩展从数百万台设备中提取媒体流所需的所有基础设施。它可以持久存储和加密流中的媒体数据并为其创建索引，还允许您通过易用的 API 访问您的媒体。借助 Kinesis Video Streams，您可以与 Amazon Rekognition Video、Amazon SageMaker 和用于机器学习框架的各种库（例如 Apache MxNet、TensorFlow 和 OpenCV）集成，从而快速构建计算机视觉和机器学习应用程序。对于直播和点播，Kinesis Video Streams 为 HTTP 直播流式处理 (HLS) 和基于 HTTP 的动态自适应流 (DASH) 提供完全托管的功能。Kinesis Video Streams 还通过完全托管的功能 WebRTC 支持超低延迟双向媒体流。

---

# 基于物联网的设备连接

- 可扩展
  - 轻松安全地将数十亿台设备连接到云
  - 支持数万亿条消息的路由、处理
- 云随时跟踪所有设备并与其通信
  - 即使这些设备未处于连接状态
- 利用各种云计算模块构建应用
  - 收集、处理和分析互联设备生成的数据并据之采取行动
- 全托管
  - 无需管理任何基础设施

---

# 设备连接方法

- HTTP
- WebSockets
- MQTT
  - 轻型通信协议
  - 专门设计，容许间断式连接
  - 最大限度减少代码在设备上占用的空间
  - 最大限度降低网络带宽要求

---

# 安全

- 访问安全
  - 可以进行身份识别和接入管理，控制对视频流的访问权限
  - 可以创建仅允许特定用户和组执行特定操作的策略，例如将数据输入视频流或从视频流中检索数据
- 数据安全
  - 静态加密：自动对视频流数据进行加密，保护静态数据。先加密，然再存储。因此，流数据始终处于静态加密状态
  - 传输安全：使用传输层安全性 (TLS) 进行安全流式传输，该协议可在两个互相通信的应用程序之间提供隐私和数据完整性。

???

数据加密

- 传输中数据和静态数据自动加密
- 可对设备硬件生成的帧和片段进行加密
- 可使用 AWS Key Management Service (KMS)

访问控制

- 与 AWS Identity and Access Management (IAM) 集成，能够控制对视频流的访问权限

---

# 云端软件开发和部署

- 各种视频应用可以部署在云上，完全托管
- 云管理所有计算和存储基础设施，用户无需自建任何管理基础设施
- 可以自动预置和弹性扩展至数百万台设备，并在设备不在传输视频时缩减规模，无需预置服务器群
- 用户不必担心平台的配置、软件更新、故障或基础设施扩展问题

???

工作原理

获取、处理和存储媒体流用于播放、分析和机器学习。
Amazon Kinesis Video Streams 工作原理
构建支持超低延迟直播流式传输和双向实时通信的应用程序。
构建超低延迟直播流式处理应用程序

---

# 终端软件开发

- C++ 和 Java 语言软件开发工具包
- 可在设备上构建和配置
  - 从媒体源接收数据
  - 逐帧实时安全地将其传输到云
- 基于 Docker 的快捷部署
  - 提供 Ubuntu、MacOS 和 Raspberry Pi 的 Docker 映像
  - 一个简单的 Docker pull 命令，几分钟内即可开始流式视频传输

???

Amazon Kinesis Video Streams 提供用 C++ 和 Java 语言编写的软件开发工具包，可供您在互联设备上进行构建和配置。这些软件开发工具包负责管理以下事项：从设备的媒体源接收数据，并逐帧实时安全地将其传输到 Kinesis 视频流。软件开发工具包还可用作 GStreamer 插件，用于构建自定义媒体数据流。

您可以从源中构建软件开发工具包，也可以使用供 Ubuntu、MacOS 和 Raspberry Pi 设备使用的 Docker 映像，这样您只需部署一个简单的 Docker pull 命令，便可以在几分钟内开始流式传输视频。

要了解有关软件开发工具包的更多信息，请参阅文档。

---

# 按使用量付费

- 通过服务提取、存储和使用的数据量付费
- 没有预付费用，也没有最低费用，无需担心要为闲置的视频流付费

---

# 内容

- 背景
- 解决方案
- .red[实例分析]
- 系统和案例

---

# 实例分析

- 智能城市交通摄像头
- 视频监控和实时通话

---

# 例 1：智能城市交通摄像头

- 大都市里有 150 个安全摄像头，覆盖繁忙的交通路口
- 每个摄像头每天产生 260 MB 的视频数据
- 此数据被流式传输，在云中存储 2 周
- 在云中对五个摄像头数据运行行人计数算法，并生成视频片段摘要

---

# 流量计算

- 150 个摄像头每个每天生成 260MB 的视频数据，每天共生成 39000MB 的数据
- AWS 上运行的流量分析应用程序播放来自五个摄像头的数据，数据量为 5 \* 260 MB/天 = 1300 MB/天。同时使用相同的数据量生成视频摘要片段

---

# 月度费用计算

- 在美国东部地区使用 AWS 视频流的价格为
  - 提取每 GB 数据 0.0085 USD，使用每 GB 数据 0.0085 USD
- 费用
  - 视频流提取：30 天 \* (39000/1024)GB \* (0.0085 USD/GB) = 9.71 USD
  - 两个应用程序消耗数据：30 天 \* (1300/1024)GB \* 2 \*(0.0085 USD/GB) =0.65 USD
  - 存储数据费用：14 天 \* (39000/1024)GB \* (0.023 USD/GB-月)=12.26USD
  - 总计：22.62 USD

???

- 其它费用
  - 通过互联网访问数据时，将会产生标准数据传输费用

定价示例 2：使用带有 WebRTC 功能的 Kinesis Video Streams 的智能手机直播流应用程序

一个移动应用程序开发人员拥有一个智能手机应用程序，该应用程序具有 100 个用户，使用 Kinesis Video Streams 中的 WebRTC 功能进行直播媒体流传输。假设每个用户应用程序都连接到其自己的唯一信令渠道，并通过 50 个直播流会话进行实时流式传输，每月总共进行 2000 分钟。月度费用将根据以下方式进行计算：

月度费用

在美国东部地区，WebRTC 的价格为活动信令通道每月 0.03 USD，每一百万个信令消息 2.25 USD，每一千 TURN 流式传输分钟 0.12 USD。

每个用户应用程序都连接到其自己的唯一信令通道，一个月内总共有 100 个活动信令通道。每个用户每月实时流式传输 50 次，每个直播流会话发送 30 条信令消息，每个月总计 150,000 条消息。假设每个应用程序的流式传输持续时间的 80% 是直接对等的，而流式传输持续时间的 20% 是通过 TURN 中继的，总共有 40,000 TURN 流式传输分钟。

月度费用：

活动信令通道 = 100 \* (0.03 USD/月) = 3.0 USD
信令消息 = 100 个用户 \* 1500 信令消息/1,000,000 \* (2.25 USD/一百万信令消息) = 0.34 USD

TURN 流式传输分钟 = 100 个用户 \* 400 TURN 流式传输分钟 \* (0.12 USD/1000 TURN 流式传输分钟) = 4.8 USD

合计 = 8.14 USD

注意：当您在 Internet 上使用 TURN 流式传输将数据发送到 AWS 以外的目标时，将会产生标准 AWS 数据传输费用。

---

# 例 2：视频监控和实时通话

- 一个安全系统提供商拥有 1000 个摄像头
- 功能 1：视频监控
  - 摄像头部署运动检测算法
  - 在检测到运动时进行流式传输
  - 假设摄像头平均每天传输 20 分钟，每分钟 7.5 MB 数据
  - 视频在云中存储 1 周
  - 其中 100 个摄像头的用户将使用手机 HLS 功能回放存储的视频

---

# 例 2：视频监控和实时通话

- 功能 2：实时语音视频通话
  - 一个月内，每个摄像头会通过配套应用连接 100 次
  - 连接期间，将观看其实时视频流，并进行双向音频会话
  - 每个会话持续 2 分钟
  - 60％ 的连接是点对点的，40％ 需要 TURN 中继

---

# 流量计算：监控部分

- 视频流
- 每个 1Mbps 摄像头每天 20 分钟传输生成 150MB 数据
- 1000 个摄像头每天共生成 150000MB 的数据
- 100 个用户使用 HLS 播放流式视频，每天消耗 15,000 MB 的数据

---

# 流量计算：通话部分

- WebRTC 信令
- 每个摄像头都连接到自己的唯一信令通道，一月内总共 1000 个活动信令通道
- 每个流会话传递 30 条信令消息，合计 3,000,000 条信令消息
- 每个摄像头通过 TURN 使用 80 分钟的实时流传输，一个月内合计 80,000 TURN 流式传输分钟。

---

# 费用计算：价格

- 美国东部地区使用 AWS 视频流的价格
  - 视频提取，每 GB 数据 0.0085 USD
  - HLS 视频播放，每 GB 数据 0.0119 USD
- WebRTC 价格
  - 活动信令通道每月 0.03 USD
  - 每一百万个信令消息 2.25 USD
  - 每一千 TURN 流式传输分钟 0.12 USD

---

# 费用计算：监控部分

- 视频提取数据费用
  - 30 天 \* (150,000/1024) GB \* (0.0085 USD/GB) = 37.35 USD
- HLS 直播数据费用
  - 30 天 \* (15000/1024) GB \* (0.0119 USD/GB) = 5.23 USD
- 数据存储费用
  - 7 天 \* (150,000/1024) GB \* (0.023 USD/GB) = 23.58 USD
- 合计 66.17 USD

---

# 费用计算：通话部分

- 费用
  - 活动信令通道 1000 \* (0.03 USD/月) = 30.0 USD
  - 信令消息 1000 个摄像头 \* 3000 信令消息/1,000,000 \* (2.25 USD/一百万信令消息) = 6.75 USD
  - TURN 流式传输分钟 = 1000 个摄像头 \* 80 TURN 流式传输分钟 \* (0.12 USD/1000 TURN 流式传输分钟) = 9.6 USD
  - 合计 46.35 USD

???

- 一个安全系统提供商拥有 1000 个。每个用户在家中安装一个摄像头，在检测到运动时进行流式传输。假设摄像头平均每天播放 20 分钟，每分钟 7.5 MB 视频数据。此视频在 Amazon Kinesis Video Streams 中存储一周。假设只有 100 个用户使用配套智能手机应用上的 HLS 功能播放存储的视频。

我们还假设一个月内，每个用户利用配套应用连接到摄像头 100 次，以观看实时视频流并参加基于 WebRTC 功能的双向音频会话。每个实时流会话持续 2 分钟，并且 60％ 的媒体流是点对点的，40％ 是 TURN 中继的。Kinesis Video Streams 的月度费用将根据以下方式进行计算：

月度费用

美国东部地区使用 Video Streams 的价格为提取的每 GB 数据 0.0085 USD，通过 HLS 使用的每 GB 数据 0.0119 USD。在美国东部地区，WebRTC 的价格为活动信令通道每月 0.03 USD，每一百万个信令消息 2.25 USD，每一千 TURN 流式传输分钟 0.12 USD。

Video Streams：每个 1Mbps 的摄像头在每天 20 分钟的流式传输过程中可生成 150MB 的数据，1000 个摄像头每天共生成 150000MB 的数据。当 100 个用户使用 HLS 播放流式视频时，每天将消耗 15,000 MB 的数据。

WebRTC：每个摄像头都连接到其自己的唯一信令通道，一个月内总共有 1000 个活动信令通道。每个直流流会话传递 30 条信令消息，合计 3,000,000 条信令消息。每个摄像头通过 TURN 使用 80 分钟的实时流传输，一个月内合计 80,000 TURN 流式传输分钟。

月度总费用将根据以下方式进行计算：

对于视频流：

提取的数据 = 30 天 \* (150,000/1024) GB \* (0.0085 USD/GB) = 37.35 USD

通过 HLS 使用的数据 = 30 天 \* (15000/1024) GB \* (0.0119 USD/GB) = 5.23 USD

存储的数据 = 7 天 \* (150,000/1024) GB \* (0.023 USD/GB) = 23.58 USD

视频流合计 = 66.17 USD

对于 WebRTC：

活动信令通道 = 1000 \* (0.03 USD/月) = 30.0 USD

信令消息 = 1000 个摄像头 \* 3000 信令消息/1,000,000 \* (2.25 USD/一百万信令消息) = 6.75 USD

TURN 流式传输分钟 = 1000 个摄像头 \* 80 TURN 流式传输分钟 \* (0.12 USD/1000 TURN 流式传输分钟) = 9.6 USD

WebRTC 合计 = 46.35 USD

注意：当您通过 Internet 将数据流式传输到 AWS 以外的目标时，将会产生标准 AWS 数据传输费用。

---

# 内容

- 背景
- 解决方案
- 实例分析
- .red[系统和案例]

---

# 系统

- 国际
  - Amazon Kinese 视频流系统
  - 微软 Azure，谷歌云
- 国内
  - 阿里云视频监控
  - 腾讯云视频监控

???

阿里云视频监控

- 依托阿里云全球接入节点，和出色的视频技术，面向监控设备提供统一开放的视频流接入、处理和分发服务
- 把视频内容接入云端，进行存储、录制回看、全网分发
- 可与智能视觉、视频计算系统、机器学习平台、生态合作伙伴能力集成，快速构建利用计算机视觉和视频分析的应用程序和智能监控解决方案。

视频监控（Video Surveillance）

视频接入、设备管理
支持 RTMP、GB/T28181 标准协议摄像头、智能设备、视频监控平台接入与管理

稳定可靠的媒体处理
支持截图、收录、转码、混流等处理，实时、历史流按需播放，视频和结构化数据同步播放

云端存储、全网分发
云端高可靠持久化存储与生命周期管理，录制快速回看，视频管理，内容全网高质量分发

OpenAPI 快速业务对接
多协议、封装格式支持，全功能 API 便于视频智能和大数据分析应用灵活对接

---

# 案例：高速公路视频上云

- 边缘 + 中心云联动智能视频监控
- 快速落地全国视频监控云联网
  - 就近实现监控摄像头接入、截图、转码、云台控制、摄像头图片质量检测、分发，中心云大数据分析
  - 轻资产，启动快，免运维，成本比传统方案低 10 ～ 15%
- 安全、可靠、智能
  - 全链路安全解决方案，高可靠架构设计
  - 事件分析算法丰富，无缝对接智慧高速应用
- 全时全域共享
  - 全国高速公路检测设施联通和视频资源实时在线共享

???

- 快速实现全国高速视频监控云联网，提升服务能力和监管水平，满足人民群众高品质出行需求。
- 可视、可测、可控、可服务

---

# 案例：明厨亮灶

- 餐馆门店统一监控、食安分析
- 接入
  - 快速接入全国各地门店、统一管理
- 部署
  - 启动快、充分利旧，轻量级部署，运维成本低
- 智能
  - 云端算法、视频分析
- 分发
  - 随时随地播放实时流、历史流，查看截图

???

- 全国范围内高质量视频接入，保障数据安全。部署运维方便，轻资产，启动快。AI 食安分析，算法快速迭代演进支撑多样化场景需求。同时满足卫生监管部门和 C 端网友秒开观看。

产品能力

- 视频监控产品将您的监控设备、智能设备通过标准化协议接入阿里云视频监控接入节点，在云端进行监控流的收录存储、媒体处理，通过 CDN 进行加速分发，供全网用户直接流畅播放实时监控流和历史监控流。提供接口方便监控内容对接视频智能类产品或平台。

使用方式

视频监控产品可以接入的监控设备包括摄像头、网络存储平台（NVR）、视频监控管理平台和有摄像推拉流功能的智能设备。详细使用方法可查看快速入门和用户指南。

通过控制台直接管理和访问您的监控设备和监控流。
通过视频监控 API 把视频监控能力与您自己的应用和服务集成。

原理

- 采集
  - 摄像头、平台、智能设备采集监控流，通过 RTMP 协议推流或者 GB/T28181 国标协议拉流。如果是国标协议需先进行国标设备注册。
- 存储
  - 在云端进行监控流的录制存储、截图，生成点播回看视频和截图文件，便于随时查看历史视频和截图内容。
  - 云存储支持录制视频和截图的生命周期管理。
- 分发
  - 通过 CDN 进行全网分发，方便不同地域用户清晰流畅地播放实时监控流和历史流。
- 智能
  - 可便捷对接机器视觉类视频智能产品，进行视频内容的结构化分析和处理，通过大数据产品构建业务应用。

???

关联服务

如果您需要对监控流进行录制、截图保存，需要您同时开通 OSS 存储服务。
如果您需要把监控流录制成 MP4，FLV 格式的点播文件，请同时开通 MPS 媒体处理服务。

计费

- 后付费，按量计费
- 计费项
  - 基础计费：接入、播放的带宽或流量计费
  - 增值服务费：设备管理、录制、截图

???

- 。当您开通视频监控服务后，可默认根据使用量付费，所有计费项都支持按量付费。

按量计费
按量计费：即按实际使用量 \* 单价的方式计费，按计费周期统计实际用量并从账户余额中扣除实际消费金额。

费用说明
等，增值服务具体费用请咨询您的商务经理。

---

# 部署

- 云端配置
  - 在云端账户中创建资源，将摄像头配置为物联网 IoT 设备
- 本地配置
  - 通过配置应用，发现和配置本地网络上的摄像头
  - 通过证书，与云端账户安全连接

???

- 5 分钟左右的时间最多连接成千上万个摄像头并开始实施流式视频分析解决方案

步骤

- 注册 AWS 账户
- 启动快速入门部署，大约需要 5 分钟
- 下载 Config App（适用于 AWS IoT Camera Connector），在其中配置摄像头
- 验证摄像头输出的流式传输功能，并移除 DynamoDB 表中的预置键。

---

# 小结

- 背景
- 解决方案
- 实例分析
- 系统和案例

---

# 参考文献

- 亚马逊公司 Kinesis 视频流云计算平台：https://aws.amazon.com/cn/kinesis/video-streams
- 亚马逊公司 IoT 物联网云计算平台：https://aws.amazon.com/cn/iot-core
- 阿里云视频监控平台：https://cn.aliyun.com/product/vs

???

- https://help.aliyun.com/product/108765.html
