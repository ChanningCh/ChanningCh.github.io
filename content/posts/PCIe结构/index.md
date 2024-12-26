---
title: "PCIe结构"
date: 2024-11-21 09:34:53+08:00
description: 
tags: ["IC", "PCIe", "SerDes", "接口"]
comment: true
draft: false
---
# PCIe

Source: Kindle

# 📒 本书总结

- PCIE

  - PCIe层级架构

    ![Untitled](Untitled.png)
  - 复位

    [https://adaptivesupport.amd.com/s/question/0D52E00006hpaAbSAI/pci-express-wake-and-perst?language=en_US](https://adaptivesupport.amd.com/s/question/0D52E00006hpaAbSAI/pci-express-wake-and-perst?language=en_US)

    复位时序和最小复位时间

    ![image.png](image.png)

    ![企业微信截图_16953606518991.png](%25E4%25BC%2581%25E4%25B8%259A%25E5%25BE%25AE%25E4%25BF%25A1%25E6%2588%25AA%25E5%259B%25BE_16953606518991.png)
  - Training

    - 在系统复位后，会自动进行链路训练，以达成以下目标：位锁定（Bit Lock）、字符锁定（Symbol Lock，Gen1 & Gen2 Only）、块锁定（Block Lock，Gen3 Only）、确定链路宽度（Link Width）、通道位置翻转（Lane Reversal）、信号极性翻转（Polarity Inversion）、确定链路的数据率（Data Rate）和通道对齐（Lane-to-Lane De-skew）等功能。

      首先是**位锁定（Bit Lock）：**

      前面的文章中提到过，PCIe总线采用了一种嵌入式时钟的机制，即发送端只向接收端发送数据信号，并不发送时钟信号（时钟信号隐藏在数据信号中）。接收端可以通过CDR（Clock and Data Recovery）逻辑将时钟从数据流中恢复出来，然后再用恢复出来的时钟对数据信号进行采样。当然，时钟恢复需要一定的时间，才能保证时钟信号与数据信号的相位对应关系符合要求。一旦CDR完成了时钟的恢复，我们就说PCIe总线完成了**位锁定。**

      **字符锁定（Symbol Lock）：**

      完成了位锁定之后，只是能够准确地识别出数据流中的0和1，还是不知道发送的内容是个啥。对于Gen1&Gen2来说，采用的8b/10b编码，即传输的数据是以10bit为一个字符。LTSSM可以引导物理层相关逻辑通过识别COM（K28.5）等控制字符来确定每个字符的开始与结束为止，即**字符锁定**。

      **链路宽度（Link Width）：**

      由于PCIe允许将x1的PCIe卡插入x4、x8甚至是x16的PCIe插槽中。因此在链路训练与初始化过程中，相邻的两个PCIe设备需要相互通信来确定其支持的最大链路宽度。

      **注：**实际上PCIe Spec还允许采用动态带宽的机制，即允许链路宽度和数据率动态调整，以实现降低功耗等功能。

      **通道位置翻转（Lane Reversal）：**

      有的时候两个PCIe设备的通道排列位置可能不太一致，PCIe Spec允许对默认的通道排列位置重新排列，如下图所示。但是，从大部分的PCIe设备（PCIe卡和插槽等）都是按照统一的标准实现的，一般不会出现这种情况，因此这一功能是**可选的**。
  -
  -
- FAQ

  [https://blog.csdn.net/u012158332/article/details/108902966](https://blog.csdn.net/u012158332/article/details/108902966)

  - Reveral lane

    具体在Configuration阶段配置lane reversal,

    ![Untitled](Untitled%201.png)
  - Polarity lane

    具体在LTSSM的polling轮训阶段

Polling:

这个阶段会进行train, bit对齐等。Polling.Active正式开始发送, 也就是所有lane会发送1024个TS1 order, 正常的话upstream/downstream都会收到连续的order, 那么就会进入config阶段

## PCIe测试

需要考虑的因素

![1734344534379](image/PCIe结构/1734344534379.png)
