---
title: 'CWNA:MIMO技术：高吞吐量与甚高吞吐量'
date: 2022-02-21 14:37:08
tags: 'Wi-Fi'
---
多输入多输出
> 射频链
> 空间复用
> MIMO分集
> 空时分组编码
> 循环移位分集
> 发射波束成形
> 802.11ac显示波束成形

多用户-多输入多输出
> 多用户波束成形

信道
> 20MHz信道
> 40MHz信道
> 40MHz不兼容
> 80MHz与160MHz信道

保护间隔
256-QAM
802.11n/ac PPDU
> 非HT PPDU
> HT混合PPDU
> VHT PPDU

802.11n/ac MAC体系结构
> 聚合MAC服务数据单元
> 聚合MAC协议数据单元
> 块确认
> 电源管理机制
> 调制编码方案
> 802.11ac数据速率

HT/VHT保护机制
> HT保护模式（0~3）

Wi-Fi联盟认证项目

802.11n首先定义的HT和802.11ac定义的VHT技术致力于改进物理层和MAC子层，以提高数据速率。

### 多输入多输出（MIMO）
多输入多输出（MIMO）技术是802.11n和802.11ac修正案中物理层增强机制的核心所在。

MIMO可以利用多径提高系统性能。

**射频链**
2X3 MIMO系统，由3条射频链构成，包括2台发射机和3台接收机；
3X3 MIMO系统，由3条射频链构成，包括3台发射机和3台接收机；

802.11n最多支持4条射频链；
802.11ac接入点最多支持8条射频链；

**空间复用**
MIMO接口可以传输多个信号，每路独立的数据流称为空间流，不同射频链发送的空间流包含不同的数据。
利用空间不同路径发送多路数据流称为空间分集。它能够显著提高吞吐量。

3X3:2 MIMO系统，3台发射机3台接收机，仅传输2路唯一的数据流
3X3:3 MIMO系统，3台发射机3台接收机，可以传输3路唯一的数据流。

**发射波束成形**
多幅天线以协调一致的方式调整发射信号的相位和振幅，使得接收机接收到同相信号，提供更好的信号质量。


### 多用户-多输入多输出（MU-MIMO）
802.11ac技术出现之前，802.11接入点每次只能与一台设备进行通信，即接入点传输数据的目标被限制为一台客户设备。
802.11ac使用MU-MIMO技术的802.11ac接入点可以最多4台设备同时交换数据。仅支持下行MU

4X4:4:3:3
SU-MIMO模式的802.11ac接入点可以向一个SU-MIMO客户端传输最多4路空间流，但最多只能向3个MU-MIMO客户端传输3路空间流，每个客户端接收一路空间流。

**多用户波束成形**
这种技术效果有限。

