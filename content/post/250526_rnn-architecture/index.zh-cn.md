---
title: 循环神经网络 RNN
description: mindmap
date: 2025-05-27
draft: false
# image: lib.jpg
mindmap: true
translationKey: "rnn-architecture"
categories:
    - D2L
tags: 
    - RNN
---


- 预测策略
   - 自回归模型：长度为$\tau$的时间跨度
   - 隐变量自回归模型：保留一些对过去观测的总结$h_t$，同时更新预测$\hat{x}_t$和总结$h_t$
- 预测
   - warm-up 预热
   - 单步预测
   - $k$步预测
- 文本预处理
   - token 词元
      - `<unk>` 未知词元
      - `<pad>` 填充词元
      - `<bos>` 序列开始词元
      - `<eos>` 序列结束词元
   - vacabulary 词表
   - corpus 语料
   - stop words 停用词
   - one-hot 编码
- 序列采样策略
   - 顺序分区
      - 每个 epoch 开始时，初始化隐状态
      - batch 
         - 1. 分离梯度
         - 2. 初始化第 1 个样本的隐状态：上个 batch 最后 1 个样本的隐状态
   - 随机采样
      - 每个 epoch 重新初始化隐状态
- 条件概率
   - 马尔可夫模型
   - $n$-gram 语法模型
   - 拉普拉斯平滑
- 梯度
   - 梯度裁剪
   - 时间反向传播 BPTT
      - 完全计算
      - 截断时间步
         - 侧重短期影响
         - 轻度正则化
      - 随机截断
         - 时间步数↑：梯度估计方差↑，抵消精度
- 衡量模型质量
   - perplexity 困惑度
      - $\exp\left(-\frac{1}{n} \sum_{t=1}^n \log P(x_t \mid x_{t-1}, \ldots, x_1)\right)$
      - 下一个词元的实际选择数的调和平均数
      - 最理想：完美估计， perplexity = 1
      - 最差：全部错误，perplexity = $+\infty$
      - baseline 基线：完全随机， perplexity = $|\mathcal{V}|$（唯一的词表大小）


{{< mind-md mymindmap >}}
- 神经网络
   - 无隐状态的神经网络
      - 隐藏层
         - 输入 $\mathbf{H} = \phi(\mathbf{X} \mathbf{W}_{xh} + \mathbf{b}_h)$
         - 输出 $\mathbf{O} = \mathbf{H} \mathbf{W}_{hq} + \mathbf{b}_q$
   - 有隐状态的<mark>循环神经网络</mark> RNN
      - 组成
         - 隐藏层：隐状态/隐藏变量 $\mathbf{H}_t$
            - $\mathbf{H}_t = \phi(\mathbf{X}_t \mathbf{W}_{xh} + \mathbf{H}_{t-1} \mathbf{W}_{hh}  + \mathbf{b}_h)$
            - 计算步骤
               - 拼接当前时间步 $t$ 的输入 $\mathbf{X}_t$ 和前一时间步 $t-1$ 的隐状态 $\mathbf{H}_{t-1}$
               - 带有激活函数 $\phi$ 的全连接层
               - 输出当前时间步 $t$ 的隐状态 $\mathbf{H}_t$
         - 输出层：仅使用<mark>隐状态</mark>
            - $\mathbf{O}_t = \mathbf{H}_t \mathbf{W}_{hq} + \mathbf{b}_q$
      - 隐藏层架构
         - 门控循环单元 GRU
            - 输入
               - 当前时间步的输入 $\mathbf{X}_t$
               - 上一个时间步的<mark>隐状态</mark> $\mathbf{H}_{t-1}$
                  - 候选隐状态 $\tilde{\mathbf{H}}_t$
                     - $\tilde{\mathbf{H}}_t = \tanh(\mathbf{X}_t \mathbf{W}_{xh} + \mathbf{R}_t \odot \mathbf{H}_{t-1} \mathbf{W}_{hh} + \mathbf{b}_h)$
                  - 最终隐状态 $\mathbf{H}_t$
                     - $\mathbf{H}_t = \mathbf{Z}_t \odot \mathbf{H}_{t-1} + (1 - \mathbf{Z}_t) \odot \tilde{\mathbf{H}}_t$
            - 重置门 reset gate
               - 是否忘记上一个时间步的隐状态
                  - 0：清零记忆，只看现在输入
                     - 类：MLP 多层感知机
                  - 1：完整保留记忆
                     - 类：普通RNN
               - $\mathbf{R}_t = \sigma(\mathbf{X}_t \mathbf{W}_{xr} + \mathbf{H}_{t-1} \mathbf{W}_{hr} + \mathbf{b}_r)$
            - 更新门 update gate
               - 决定当前时间步的隐状态组成
                  - 1：原来记忆（上一个时间步的隐状态）
                  - 0：新的记忆（新的候选隐状态）
               - $\mathbf{Z}_t = \sigma(\mathbf{X}_t \mathbf{W}_{xz} + \mathbf{H}_{t-1} \mathbf{W}_{hz} + \mathbf{b}_z)$
         - 长短期记忆网络 LSTM
            - 输入
               - 当前时间步的输入 $\mathbf{X}_t$
               - 上一个时间步的记忆元 $\mathbf{C}_{t-1}$
                  - $\mathbf{C}_t = \mathbf{F}_t \odot \mathbf{C}_{t-1} + \mathbf{I}_t \odot \tilde{\mathbf{C}}_t$
               - 上一个时间步的隐状态 $\mathbf{H}_{t-1}$
                  - <mark>隐状态</mark> $\mathbf{H}_t = \mathbf{O}_t \odot \tanh(\mathbf{C}_t)$
                  - 输出门 output gate
                     - 控制隐状态使用多少长期记忆
                     - $\mathbf{O}_t = \sigma(\mathbf{X}_t \mathbf{W}_{xo} + \mathbf{H}_{t-1} \mathbf{W}_{ho} + \mathbf{b}_o)$
            - 记忆元/细胞状态 memory cell：长期记忆
               - $\mathbf{C}_t = \mathbf{F}_t \odot \mathbf{C}_{t-1} + \mathbf{I}_t \odot \tilde{\mathbf{C}}_t$
               - 候选记忆元 candidate memory cell
                  - $\tilde{\mathbf{C}}_t = \tanh(\mathbf{X}_t \mathbf{W}_{xc} + \mathbf{H}_{t-1} \mathbf{W}_{hc} + \mathbf{b}_c)$
               - 输入门 input gate
                  - 决定向长期记忆里写入多少当前输入
                  - $\mathbf{I}_t = \sigma(\mathbf{X}_t \mathbf{W}_{xi} + \mathbf{H}_{t-1} \mathbf{W}_{hi} + \mathbf{b}_i)$
               - 遗忘门 forget gate
                  - 决定遗忘多少长期记忆
                  - $\mathbf{F}_t = \sigma(\mathbf{X}_t \mathbf{W}_{xf} + \mathbf{H}_{t-1} \mathbf{W}_{hf} + \mathbf{b}_f)$
      - 深度循环神经网络
         - 隐藏层（数量$L$）
            - 隐状态（$l^{th}$ 隐藏层）
               - $\mathbf{H}_t^{(l)}=\phi_l(\mathbf{H}_{t}^{(l-1)} \mathbf{W}_{xh}^{(l)} + \mathbf{H}_{t-1}^{(l)} \mathbf{W}_{hh}^{(l)} + \mathbf{b}_h^{(l)})$
         - 输出层：基于最后一层的隐状态
            - $\mathbf{O}_t = \mathbf{H}_t^{(L)} \mathbf{W}_{hq} + \mathbf{b}_q$
         - 深度门控循环神经网络
         - 深度长短期记忆神经网络
      - 双向循环神经网络 bidirectional RNN
         - 隐马尔可夫模型 HMM
            - 动态规划
               - 前向递归
               - 后向递归
         - 隐藏层
            - 隐状态 $\mathbf{H}_t \in \mathbb{R}^{n \times 2h}$
               - 前向隐状态 $\overrightarrow{\mathbf{H}}_t^{(f)} \in \mathbb{R}^{n \times h}$
                  - $\overrightarrow{\mathbf{H}}_t = \phi(\mathbf{X}_t \mathbf{W}_{xh}^{(f)} + \overrightarrow{\mathbf{H}}_{t-1} \mathbf{W}_{hh}^{(f)} + \mathbf{b}_h^{(f)})$
               - 后向隐状态 $\overleftarrow{\mathbf{H}}_t^{(b)} \in \mathbb{R}^{n \times h}$
                  - $\overleftarrow{\mathbf{H}}_t = \phi(\mathbf{X}_t \mathbf{W}_{xh}^{(b)} + \overleftarrow{\mathbf{H}}_{t+1} \mathbf{W}_{hh}^{(b)} + \mathbf{b}_h^{(b)})$
         - 输出层
            - $\mathbf{O}_t = \mathbf{H}_t \mathbf{W}_{hq} + \mathbf{b}_q$
         - 缺点
            - 计算速度慢、内存消耗大
            - 预测精度差：下文未知
{{< /mind-md >}}

