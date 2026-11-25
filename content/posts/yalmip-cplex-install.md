---
title: "OSX 11.6 and Windows 10 安装 yalmip 及 cplex 记录"
date: 2018-03-09
categories: [技术]
tags: [MATLAB, 优化, CPLEX]
draft: false
---

cplex：
**1、** 在以下链接（IBM的教育商店）下载 IBM ILOG CPLEX Optimization Studio v12.8安装包。
[https://ibm.onthehub.com/WebStore/OfferingDetails.aspx?o=b46379bb-0463-e611-9420-b8ca3a5db7a1](https://ibm.onthehub.com/WebStore/OfferingDetails.aspx?o=b46379bb-0463-e611-9420-b8ca3a5db7a1)

**2、** 默认路径默认设置的情况下安装上述安装包到系统（windows下要额外安装Visual C++ 2015）。

**3、** 安装完成后将安装路径下的cplex文件夹下的matlab文件夹添加到matlab路径，即可实现matlab对cplex的调用。
（1、如果只需要实现matlab调用cplex，只需要将安装后的上述文件夹提取出来就可以了
**2、** cplex文件夹下的example文件夹下的src文件夹下有各种语言写的cplex实例，因为只用到matlab，所以对其他语言调用cplex还不了解
**3、** CPLEX Optimization是一个包含cplex及 OP Optimizer的IDE，目前只用到其中cplex的matlab工具包）

yalmip：
**1、** 在以下链接下载yalmip压缩包，放在电脑任意位置并解压。
[https://yalmip.github.io/](https://yalmip.github.io/)

**2、** 将上述解压后的文件夹添加到matlab路径，即可实验matlab对cplex的调用。

验证：
在matlab命令行中输入：doc cplex；doc yalmip可以检验是否安装成功。
在matlab命令行中输入：yalmiptest可以测试当前可以解决哪些优化问题。
