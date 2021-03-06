---
title: 'CWNA:标准、行业组织与通信基础知识概述'
date: 2022-01-06 14:34:03
tags: 'Wi-Fi'
---
1.无线局域网的发展历程
2.标准组织
> 美国通信联邦委员会
> 国际电信联盟无线电通信部门
> 电气和电子工程师协会
> 互联网工程任务组
> Wi-Fi联盟
> 国际标准化组织

3.核心层、分布层与接入层
4.通信基础知识
> 通信术语
> 载波信号
> 键控法

## 无线局域网的发展历程
19世纪，迈克尔·法拉第、詹姆斯·克拉克·麦克斯韦、海因里希·鲁道夫·赫兹、尼古拉·特斯拉、大卫、爱德华·休斯、
托马斯·爱迪生、古列尔莫·马可尼等许多发明家与科学家开始进行无线通信方面的实验。 

1970年，第一种无线网络ALOHAnet在美国夏威夷大学诞生，这种网络以无线方式在夏威夷群岛之间传输数据。一般认
为，ALOHAnet使用的技术奠定了802.3以太网CSMA/CD和802.11无线局域网CSMA/CA技术基础。 

20世纪90年代，低速无线网络产品开始商用，多数工作在900MHz频段。1991年，IEEE开始讨论无线局域网技术的标准
化问题。1997年，IEEE批准通过802.11原始标准。  

1999年，IEEE批准通过数据速率较高的802.11b修正案。支持高达11Mbps的速率，无线局域网开始大规模商用。  

802.11技术营销术语称为Wi-Fi，被全球数十亿人公认为无线局域网络的代名词。

## 标准组织
   各种标准组织共同对无线网络行业进行监管和规范。
监管机构：
   国际电信联盟无线电通信部门(ITU-R)以及美国联邦通信委员会(FCC)等监管机构负责制定规则，以约束用户使用无线电的行为。这
些机构管理通信频率，功率电平，传输方法，共同致力于规范日益壮大的无线用户。

标准组织：
> 电气和电子工程师协会(IEEE)负责确保网络设备兼容与共存的标准。
> 互联网工程任务组(IETF)负责制定互联网标准。
> Wi-Fi联盟(Wi-Fi Alliance)对无线网络设备进行认证测试，以确保他们符合无线局域网通信协议,实现设备兼容。
> 国际标准化组织(ISO)创建了开放系统互连模型。


监管机构负责授权频谱和非授权频谱的管理。非授权频谱需要申请牌照。授权频谱和非授权频谱受6个方面的监管。
> 频率
> 带宽
> 有意辐射器的最大功率
> 最大等效全向辐射功率
> 使用（室内与室外）
> 频谱共享规则

国际电信联盟无线电通信部门(ITU-R)承担全球频谱管理任务。致力于确保陆地、海洋与空中的通信活动不受干扰，通过
5大行政区维护一个全球性的频率分配数据库。5大行政区：
> 行政区A:美洲
> 行政区B:西欧
> 行政区C:东欧与北亚
> 行政区D:非洲
> 行政区E:亚洲与澳大拉西亚

除行政区外，还设3个由地理界定的无线电监管区:
> 监管区1:欧洲、中东与非洲
> 监管区2:美洲
> 监管区3:亚洲与大洋洲

除了上述组织，各国设置自己的监管机构。


OSI模型
> 第七层        应用层
> 第六层        表示层
> 第五层        会话层
> 第四层        传输层
> 第三层        网络层
> 第二层        数量链路层 : LLC子层和MAC子层
> 第一层        物理层


## 核心层、分布层与接入层
网络的核心层：由高速主干网络构成，高速主干网相当于网络中的“高速公路”，其核心目标是在重要的数据中心和
分布区域之间传输大量信息，如同高速公路连接城市与都会区一样。核心层既不负责流量路由，也不负责操作数据
包，它的任务就是高速交换。

网络的分布层：将流量路由或引导至较小的节点集群或领域。类似于中等速度在城市或都会区中分流交通流量省道。

网络的接入层：以较低速度将流量直接床输给最终用户或末端节点，类似于直接到达最终地址的地方道路。

记住，速度是个相对概念，核心层、分布层、接入层是相对的。


## 通信基础知识
通信术语
> 单工：一个仅能发送，另一个仅能接收数据。例如调频广播
> 半双工：两台设备都能发送接收数据，但同一时间只有一个设备可以传输。例如无线对讲机
> 双工：两台设备可以同时发送接收数据。例如电话
> 载波信号：一种高频信号，用于承载有效信号进行发射。
> 振幅：波的高度，力度或能量。
> 波长：两个连续波的相似点之间的距离。一般是波峰与波峰之间。
> 频率：波传播的速度。一秒内产生的波的数量。周期的倒数。
> 周期：传播一个波长距离的时间。
> 相位：同频波之间的位置

操作信号以表示多个数据的方法称为键控法(keying method)。
> 幅移键控(ASK)
> 频移键控(FSK)
> 相移键控(PSK)



## 网站资源
Wi-Fi联盟：https://www.wi-fi.org
CWNP网站： https://www.cwnp.com
Revolution Wi-Fi: http://www.revolutionwifi.net
Wirednot: https://wirednot.wordpress.com
商业与个人博客链接：https://gcatewifi.wordpress.com
WLPC网站：https://www.wlanpros.com



