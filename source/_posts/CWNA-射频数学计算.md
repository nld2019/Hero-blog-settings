---
title: 'CWNA:射频数学计算'
date: 2022-01-12 09:32:41
tags: 'Wi-Fi'
---

射频计算中包含很多对数运算，不过工程应用无需进行复杂的计算。

**10与3规则**
用于工程近似计算。
4条规则：
> 每3dB增益（相对值），意味着绝对功率(mW)加倍；
> 每3dB损耗（相对值），意味着绝对功率(mW)减半；
> 每10dB增益，意味着绝对功率乘以10；
> 每10dB损耗，意味着绝对功率除以10；

多数802.11无线接口可以解析从-30dBm(1mW 的千分之一)到低至-100dBm(1mW的百亿分之一)的接收信号
毫瓦分贝对照表

|毫瓦分贝(dBm)|毫瓦(mW)|功率电平|
|:----------:|:------:|:------:|
|+20dBm|200mW|十分之一瓦|
|+10dBm|100mW|百分之一瓦|
|0dBm|1mW|千分之一瓦|
|-10dBm|0.1mW|十分之一毫瓦|
|-20dBm|0.01mW|百分之一毫瓦|
|-30dBm|0.001mW|千分之一毫瓦|
