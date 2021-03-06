---
title: 'CWNA:MIMO技术：高吞吐量与甚高吞吐量(三)'
date: 2022-02-22 16:33:35
tags: 'Wi-Fi'
---

### 802.11n/ac MAC体系结构
MIMO无线接口使用的所有物理层增强机制可以增加并提高吞吐量。
定义了两种帧聚合技术减少介质争用开销，并利用块确认机制减少固定的MAC开销。
定义了3种电源管理机制。

**聚合MAC服务数据单元（A-MSDU）**
每次传输802.11单播帧时都会产生一定数量的固定开销，这些固定的开销包括物理层头部，MAC帧头，MAC帧尾，帧间间隔以及确认帧。
每个帧必须争夺介质的使用权，介质争用开销同样不可避免。

为了减少开销，将多个帧聚合为一个帧进行传输。这样减少了固定的帧头帧尾开销，减少了介质争用开销。
第一种帧聚合：多个MSDU合并为一个A-MSDU, 外加 帧头和帧尾。

A-MSDU中的所有MSDU必须属于同一802.11e Qos访问类别，使用相同的接收地址，采用同一种加密机制。

**聚合MAC协议数据单元（A-MPDU）**
A-MPDU包括多个MPDU以及物理层头部。
构成A-MPDU的所有MPDU必须使用相同的接收机地址。与A-MSUD不同，每个MPDU的数据净荷是分别加密和解密的。
A-MPDU中的所有MPDU必须同属同一种802.11e Qos访问类别。

由于A-MPDU中的MPDU都有自己的MAC帧头和帧尾，因此A-MPDU的开销大于A-MSDU。 A-MPDU能减少ACK帧的开销。如果某个MPDU损坏，只需重传损坏帧。

A-MPDU采用块确认帧作为交付验证方法，而A-MSDU采用确认帧的交付验证方法。 A-MSDU损坏，必须全部重传。

A-MPDU中可以包括多个A-MSDU.

**块确认**
块确认帧用于确认A-MPDU的传输。
如果某个MPDU损坏，只需重传损坏帧。A-MPDU的效率高于A-MSDU，这就是802.11ac无线接口要求使用A-MPDU的主要原因。

**电源管理机制**
802.11n定义的第一种电源管理机制是空间复用节电（SMPS），允许仅保留一个无线接口，关闭其他无线接口。

定义的第二种电源管理机制是节电多轮询（PSMP），改进了自动节电传输（APSD）。

定义的第三种电源管理机制是VHT TXOP节电。如果发现其他客户端享有发送机会，将在对方传输期间通过VHT TXOP节电机制关闭自己的无线接口。

**调制编码方案**
802.11n通过调制编码方案（MCS）矩阵定义数据速率。
采用OFDM技术的非HT无线接口根据使用的调制和编码方法定义数据速率（6Mbps~54Mbps）
HT无线接口根据调制技术，编码技术，空间流数量，信道宽度，保护间隔等多种参数定义数据速率。调制编码方案是这些参数的不同组合。
802.11n为20MHz HT和40MHz HT信道定义了77种调制编码方案。20MHz HT信道必须支持的8种编码方案：

|MCS指数|调制方法|空间流|数据速率：800ns保护间隔|数据速率：400ns保护间隔|
|:---:|:---:|:---:|:---:|:---:|
|0|BPSK|1路空间流|6.5Mbps|7.2Mbps|
|1|QPSK|1路空间流|13.0Mbps|14.4Mbps|
|2|QPSK|1路空间流|19.5Mbps|21.7Mbps|
|3|16-QAM|1路空间流|26.0Mbps|28.9Mbps|
|4|16-QAM|1路空间流|39.0Mbps|43.3Mbps|
|5|64-QAM|1路空间流|52.0Mbps|57.8Mbps|
|6|64-QAM|1路空间流|58.5Mbps|65.0Mbps|
|7|64-QAM|1路空间流|65.0Mbps|72.2Mbps|

802.11ac将802.11n调制编码方案缩减为10种。

|MCS指数|调制方法|码率|20MHz数据速率|
|:---:|:---:|:---:|:---:|:---:|
|0|BPSK|1/2|7.2Mbps|
|1|QPSK|1/2|14.4Mbps|
|2|QPSK|3/4|21.7Mbps|
|3|16-QAM|1/2|28.9Mbps|
|4|16-QAM|3/4|43.3Mbps|
|5|64-QAM|2/3|57.8Mbps|
|6|64-QAM|3/4|65.0Mbps|
|7|64-QAM|5/6|72.2Mbps|
|8|256-QAM|3/4|86.7Mbps|
|9|256-QAM|5/6|96.3Mbps|

**802.11ac数据速率**
802.11ac实现较高速率的措施：
1.采用256-QAM
2.空间流乘数
3.信道宽度

802.11ac最大速率 6933Mbps= 866.7 X 8 =  433.3 X 2 X 8 = 96.3Mbps X 4 X 2 X 8
96.3Mbps是MCS9的基础速率。

### HT/VHT保护机制
HT 保护模式（0~3）
模式0：绿地（无保护）模式：
网络中只有HT无线接口，所有的HT客户端必须具备相同的操作功能。比如：基本服务集为20/40MHz基本服务集，则所有客户端必须支持20/40MHz通信。

模式1：非成员保护模式：
基本服务集种的所有终端必须是HT终端。如果HT接入点侦听到不属于基本服务集成员的非HT客户端或接入点，则触发保护机制。

模式2：20MHz保护模式：
基本服务集种的所有终端必须是关联到20/40MHz接入点的HT终端。如果仅支持20MHz通信的HT客户端关联到20/40MHz接入点，则必须启动保护机制。

模式3：非HT混合模式：
一个或多个非HT终端关联到HT接入点，将启用该模式。这也是最常见的保护模式。
