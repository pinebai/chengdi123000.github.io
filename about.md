---
layout: page
title: About
permalink: /about/
---

# 关于`CFL3D`

​	[CFL3D](https://cfl3d.larc.nasa.gov/)是一款基于结构网格、基于单元中心的有限体积法、上风格式的求解器雷诺平均Navier-Stokes方程的CFD程序。可以通过多网格区域分解进行并行运算。支持逐点匹配、面匹配、重叠网格或嵌入连接性网格。还支持在时间精确求解瞬态问题和求解稳态问题时采用多重网格法和网格序列法加速求解。

​	CFL3D从1980s年代末期开始就被用于支持多个NASA项目。直到今天，它还被用于CFD的验证和确认，例如[NASA Langley湍流建模资源网站](https://turbmodels.larc.nasa.gov/)上记录的那样。

​	CFL3D的最新版是6.7，于2017年2月1日发布，童年2017年7月10日被NASA以apache 2.0开源协议的方式发布于github网站：https://github.com/nasa/cfl3d，成为一款开源软件。

# 关于`CFL3D研究`

​	CFL3D由于是用FORTRAN 77语言写与1980年代末期，面向的机器主要是UNIX大型机和CRAY, SGI等当时的工作站，所以对Linux和Windows的支持不是特别好，很难编译。同时其界面也非常原始，前后处理都很困难。为了方便大家了解和使用这款经典的气动CFD软件。chengdi及其朋友们决定建立一个关于CFL3D的网站来收集和整理相关资料，计划大约每周会更新一次。

​	本站建于2017年8月9日，基于github pages和Jekyll技术构建。