title: 线缆和端口形态总结
date: 2017-12-20 10:47:30
toc: true
tags: [cable]
categories: networks
keywords: [computer, networks, cable, 线缆, AOC, DAC, 一分四]
description: 设备上用的线缆总结
---

详见 [数据中心线缆基础架构](https://www.viavisolutions.com/zh-cn/literature/shu-ju-zhong-xin-xian-lan-ji-chu-jia-gou-jian-ti-zhong-wen-ying-yong-zhi-nan-zh-hans.pdf)。

![数据中心线缆](/images/networks/ethernet_interfaces.png)

## Serdes

* 10G NRZ
* 25G NRZ
* 50G NRZ...
* 50G PAM4
* 100G PAM4

## 端口形态
### 10G
### 25G
### 40G
### 100G

## 线缆
### CAT
#### CAT5
#### CAT6
#### CAT7


### AOC, DAC, ACC, 2 Transceivers with Structured Cabling

* Active Optical Cable，AOC，译为有源光缆。将 2 只光模块与光缆封装在一起，这种线缆都不可更换模块。
* Passive Direct Attach Cable，DAC, 这种线缆都不可更换端口，模块头和铜缆不能分离。重、短。
* Active Direct Attach Cable, 也有称 Active Copper Cables, ACC。与 DAC 相似，但是在 transceiver connectors 中加入微处理器和一些电路放大信号（也称“线性放大芯片”）。

详见 [4 ADVANTAGES OF DIRECT ATTACH CABLING (DAC)](https://blog.cablestogo.com/4-advantages-of-direct-attach-cabling-dac/)。

![DAC-and-AOC-Cable-Differences](https://blog.cablestogo.com/content/images/2017/05/DAC-and-AOC-Cable-Differences.png)。

## 光模块

Transceivers MAU GBIC SFP XENPAK X2 XFP SFP+ QSFP CFP

* GBIC，高速以太网路界面转换器（英文：Gigabit Interface Converter，简称GBIC）
* SFP，小封装热插拔收发器（SFP, Small form-factor pluggable transceiver），mini GBIC，1Gbps
* SFP+，10Gbps
* SFP28，25Gbps
* QSFP，Quad SFP，4x1Gbps
* QSFP+，4x10Gbps
* QSFP28，4x25Gbps
* QSFP56，4x50Gbps
* QSFPDD，4x2x25Gbps，或者4x2x50Gbps

### 连接器类型

https://en.wikipedia.org/wiki/Optical_fiber_connector

* LC
* MPO

### 其他

* Number of Lanes: 1, 2, 4, 8, 16
* Number of PMDs: KR, CR, SR, PSM, CWDM, LR, ER
* Number of FECs: KR, RS(528,514), RS(544, 514)

IEEE 命名：

[xxxBASEmRn](http://img.chuansong.me/mmbiz_png/Vg9ZNgPzWbIn0AMnUic04BB8o98uLVzPicbfmjnoWdPiaco8FD5icHeL28Lxaia9PHwX3Jzj2S3o4OPwKZptqyMD9LA/0?wx_fmt=gif)

[PMD](http://img.chuansong.me/mmbiz_png/Vg9ZNgPzWbIn0AMnUic04BB8o98uLVzPicSldaHfAJyuUiajfIV8iaYGHhcEibrkHVmOYoQMNyqkeR0KTYez2n570jA/0?wx_fmt=gif)
