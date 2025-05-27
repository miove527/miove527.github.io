---
title: Robust deep learning–based protein sequence design using ProteinMPNN
description: ProteinMPNN
date:  2025-05-22
# image: calc-graph.png
# slug:  backprop-graph
categories:
    - AI4S
    - Literature
tags : 
    - ProteinMPNN
    - Protein
---

基础：
Encoder: message passing neural  network (MPNN)
3 encoder and 3 decoder layers and 128 hidden dimensions
- a random decoding order
    - encoded features + a partial sequence

输入：
protein  backbone features：
- distances between Ca-Ca atoms
- relative Ca-Ca-Ca frame orientations  and rotations
- backbone dihedral angles

数据集：
19.7k high  resolution single-chain structures from the PDB
分割：
CATH (7) protein  classification


改进：
1. additional input features
    - Table1, experiment 1
    - adding distances between N, Ca, C, O
    - a virtual Cb placed based on the other backbone atoms
2. introducing edge updates in the backbone encoder neural network
    - Table1, experiment 2
3. 1+2
    - Table1, experiment 3

主链几何结构对氨基酸种类的影响范围
- Figure S1A
- performance saturated at 32-48 neighbors

structure to sequence mapping problem
- model: locally connected graph neural networks
- local neighborhoods: determine sequence identities


test perplexity
- 准确预测与随机猜测


replaced the fixed N to C terminal decoding order with an order agnostic autoregressive model
不再局限于从N端到C端的顺序，可以以任意顺序生成每个氨基酸残基，每一步只依赖于已经生成的部分，而不依赖于特定的生成顺序。
- Table1, experiment 4
- modest improvement in sequence recovery


decoding skips the fixed regions but includes them in the sequence context for the remaining positions

equivariant to the order of the protein  chains
- kept the per chain relative positional encoding capped at ±32 residues
- 限制在一定范围内（如±32个残基），以突出局部邻域的重要性，减少远距离残基对的影响。
- added a binary feature
    - 相同链/不同链


tie together
- decoded at the same time
- 在对应位置上固定氨基酸种类（保证作用）
- 举例一个同源二聚体
    - 分别预测两条链的logits（未归一化概率）
    - 构建归一化概率
    - 采样，选择一种氨基酸同时给到两条链
- multi-state
    - predicting logits for each state and then averaging
    - 同理，概率平均使得symmetric counterparts之间可以共享信息


positive-negative sequence design：
在蛋白质序列设计中，模型不仅可以优化某些结构状态（正向设计，positive design），还可以显式地抑制或排斥某些不希望出现的结构状态（负向设计，negative design）。

sequence cluster
聚类的目的是去除冗余、保证数据多样性。在蛋白质序列设计或分析中，很多蛋白质序列彼此非常相似，如果直接全部用于训练或评估，模型可能会过拟合于这些重复或高度相似的序列，影响泛化能力。
设定30%序列一致性截断（identity cutoff），意味着只有当两条序列的相似度低于30%时，才会被分到不同的簇。
提高数据集的代表性和多样性，减少重复信息。
有助于更公平地评估模型的泛化能力，避免模型只记住了某些重复序列。
聚类和设定截断阈值，是为了去除冗余、提升数据多样性和模型泛化能力。


residue burial：残基埋藏程度的范围，也就是从完全埋藏在蛋白质内部（core）的残基，到暴露在蛋白质表面（surface）的残基


IDDT-Ca???

sampling temperature
- 控制生成模型输出多样性和确定性的重要参数
- 会对概率分布进行缩放：
    - temperature = 1：原始分布，不做调整。
    - temperature < 1：分布变“尖锐”，高概率的选项更容易被选中，生成结果更确定、重复性高。
    - temperature > 1：分布变“平坦”，低概率的选项被选中的机会增加，生成结果更随机、多样性更高。


目标：对于给定的三维结构，追求一个序列，该序列更有可能折叠成该结构
$\to$ 序列内蕴含了更多结构相关的信息，也就是more strongly encoding the structures
$\to$ 也可以表示为 the strength of the single-sequence to structure mapping

ProteinMPNN encodes the sequences, then decodes into 3D coordinates

- 噪声nosing: blurs fine local structural details, focusing more on the overall topological features
- larger amounts of noise
    - sequences more strongly map to the target structures

- sequence quality测量指标：the averaged log probability of the sequence given the structure
    - 在一定温度范围内，与native sequence recovery强相关


通过ProteinMPNN补救rescue之前一些方法的设计失败