---
title: 'CWNA:802.11ax技术:高效无线局域网'
date: 2022-03-01 09:30:57
tags: 'Wi-Fi'
---

高效无线局域网技术概述
多用户
正交频分多址
> 副载波
> 资源单元
> 触发帧
> 下行链路OFDMA
> 上行链路OFDMA

多用户-多输入多输出
基本服务集颜色
> 自适应空闲信道评估
> 双NAV计数器

目标唤醒时间
其他802.11ax物理层与MAC子层功能
> 1024-QAM
> 长符合时间与保护间隔
> 802.11ax PPDU格式
> 仅20MHz模式
> 多流量标识符聚合的MAC协议数据单元

802.11ax设计注意事项

### 高效无线局域网技术概述
高效无线局域网（HEW）是描述802.11ax技术的专业术语。
由于无线局域网工作原理决定的，总吞吐量通常最多只能达到标定802.11数据速率的50%。

导致802.11流量拥塞的因素：
遗留客户端的存在导致需要部署RTS/CTS保护机制，保护机制会降低效率。
小的数据帧导致MAC子层和介质争用开销过多。
由于高层应用协议的缘故，小帧必须按顺序传输，难以实施聚合。（例如:语音）

802.11n/ac无线接口尽管可以使用更高的数据速率和更宽的信道，但却可能导致802.11流量拥塞。
802.11ax致力于更好的流量管理，它的目标并非提高数据速率或增加信道宽度，而是实现更优质、更高效的802.11流量管理。

802.11n、802.11ac、802.11ax技术比较

||802.11n|802.11ac|802.11ax|
|:---:|:---:|:---:|:---:|
|频段|2.4GHz 和5GHz|仅5GHz|2.4GHz和5GHz|
|信道宽度|20MHz\40MHz|20/40/80/80+80/160MHz|20/40/80/80+80/160MHz|
|频率复用技术|OFDM|OFDM|OFDM和OFDMA|
|副载波间隔|312.5kHz|312.5kHz|78.125kHz|
|OFDM符号时间|3.2微秒|3.2微秒|12.8微秒|
|保护间隔|0.4或0.8微秒|0.4或0.8微秒|0.8、1.6或3.2微秒|
|总符号时间|3.6或4.0微秒|3.6或4.0微秒|13.6、14.4或16.0微秒|
|调制技术|BPSK/QPSK/16-QAM/64-QAM|BPSK/QPSK/16-QAM/64-QAM|BPSK/QPSK/16-QAM/64-QAM/1024-QAM|
|多用户-多输入多输出|不适用|下行传输|下行传输和上行传输|

802.11ax主要的优势在于OFDMA技术，将信道划分为若干较小的子信道。

### 多用户
OFDMA通过细分信道来实现多用户访问；
MU-MIMO通过使用不同的空间流来实现多用户访问；

### 正交频分多址（OFDMA）
OFDMA技术将信道细分为较小的频率分配单元————资源单元（RU）————从而能同时向多个用户发送小帧。例如：
将传统的20MHz信道划分为9个子信道，采用OFDMA技术的802.11ax接入点可以同时向9个802.11ax客户端发送小帧。
它能够更好的利用频率空间，已经在LTE蜂窝通信中得到验证。

802.11ax技术仍然采用802.11a/g/n/ac无线接口可以理解的OFDM技术，以基本速率传输管理帧和控制帧。

**副载波**
802.11n/ac 在20MHz信道包括64个副载波，副载波的宽度为312.5kHz
802.11ax在20MHz信道包括256个副载波，副载波宽度为78.125KHz

副载波包括3种：
数据副载波， 采用802.11ac相同的调制编码方案（MCS），增加两种调制编码方案和1024-QAM
导频副载波， 仅用于发射机和接收机之间的同步
未使用的副载波， 防止邻信道和子信道干扰

**资源单元**
OFDM传输数据个多个客户端时，每一个时段都需要使用整个20MHz信道与单一客户端进行传输，各客户端分时占用。

OFDMA包括256个副载波，242个副载波用于传输数据，可以将其细分为 106、 52、 26副载波资源单元，大致对应20MHz、8MHz、4MHz、2MHz
802.11ax接入点可以利用8MHz信道与一个802.11ax客户端通信，同时利用4MHz子信道与2个802.11ax客户端进行通信。既可以上行，也可以是下行。

**触发帧**
触发帧为802.11ax客户端分配资源单元。

|触发帧类型|各类触发帧|
|:---:|:---:|
|0|基本触发帧|
|1|波束成形报告轮询|
|2|多用户块确认请求|
|3|多用户请求发送|
|4|缓冲区状态报告轮询|
|5|带重试的组播多用户块确认请求|
|6|带宽查询报告轮询|
|7|NDP反馈报告轮询|
|8~15|保留|

触发帧的用户信息字段种每个特定资源单元有长度为7bit的唯一组合定义。

**下行链路OFDMA**
接入点取得发送机会后，首先发送MU-RTS触发帧，分配资源单元，为了让遗留客户端重置，使用20MHz的信道发送。
收到客户端发送的并行CTS响应后，接入点开始向支持OFDMA的客户端传输多用户DL-PPDU。

802.11ax接入点---------------------------------------------------------------------802.11ax客户端

-------------AIFS MU-RTS SIFS---------SIFS---DL-PPDU SIFS  BAR  SIFS--------------->
<------------------------------CTS------------------------------------块确认帧-------STA1
<------------------------------CTS------------------------------------块确认帧-------STA2
<------------------------------CTS------------------------------------块确认帧-------STA3
<------------------------------CTS------------------------------------块确认帧-------STA4

**上行连接OFDMA**
802.11ax接入点必须首先竞争介质的控制权并取得发送机会，然后才能协调802.11ax客户端发送的数据（上行传输）
上行传输非常复杂。需要三轮触发帧才能实现一次上行传输数据。

802.11ax接入点------------------------------------------------------------------------------802.11ax客户端

---AIFS BSRP SIFS-------SIFS--MU-RTS SIFS ---- SIFS 触发帧 SIFS-----------SIFS 多终端块确认帧--->
<------------------BSR--------------------CTS--------------------UL-PPDU---------------------STA1
<------------------BSR--------------------CTS--------------------UL-PPDU---------------------STA2
<------------------BSR--------------------CTS--------------------UL-PPDU---------------------STA3
<------------------BSR--------------------CTS--------------------UL-PPDU---------------------STA4
