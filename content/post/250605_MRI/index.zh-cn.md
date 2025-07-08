---
title: MRI成像的物理基础及实验
description: 以实验原理为主，含公式推导（pdf）
date: 2025-05-10 00:37:00+0000
draft: false
# image: lib.jpg
mindmap: false
translationKey: "MRI"
tags: 
    - MRI
categories:
    - Experiments
    - Physics
---

{{< embed-pdf url="./MRI.pdf" >}}

# 核子自旋
两种运动形式 $\to$ 核磁矩 $\to$ 旋磁比、磁化强度矢量
- 轨道运动：轨道角动量
- 自旋运动：自旋角动量

$\to$ 核子本征角动量：二者矢量和
$\to$ 原子核本征角动量：核子本征角动量的矢量和
$\to$ 核自旋量子数

# 拉莫尔进动
原子核处于外磁场中，本征角动量和核磁矩按某一角频率/圆频率进动。

# 原子核塞曼分裂
在外磁场中原子核的能级分裂。
Bolzmann分布 $\to$ 能级分裂的能量差、不同能级的粒子数比

# 射频场
频率在射频范围的交变电磁场，产生振荡磁场。
- 硬脉冲
- 软脉冲

# 核磁共振
原子核吸收等于能级分裂能量差的射频场能量，发生跃迁。
$\to$ 磁化强度矢量改变

# 弛豫
粒子受到激发后，以非辐射的方式回到基态而达到 Bolzmann 平衡。
- 横向弛豫
- 纵向弛豫

# MR信号
- FID 自由感应衰减信号
- SE 自旋回波信号
- GRE 梯度回波信号（todo）
- EPI 回波平面成像信号（todo）
- Q-CPMG 序列
