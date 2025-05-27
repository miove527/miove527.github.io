---
title: Denovo design of protein structure and function with RFdiffusion
description: RFdiffusion
date:  2025-05-15
draft: true
# image: calc-graph.png
mindmap: false
translationKey: "rfdiffusion"
categories:
    - AI4S
    - Literature
tags : 
    - RFdiffusion
    - Protein
---

# 背景
## 深度学习方法在蛋白质设计中的应用
ref1-9

## 蛋白质从头设计
ref4, 12,13

# 扩散模型
ref10-11
text prompt: ref14-15
## DDPMs
denoising diffusion probabilistic models
去噪扩散概率模型
high diverse output
训练：对已被Gaussian噪声污染的数据去噪

rotationally equivariant

protein monomer design
ref 5,8,9

我们推论，通过微调RF作为生成扩散模型中的去噪网络，我们可以克服这两个问题：
由于起始点是随机噪声，每个去噪轨迹产生不同的解决方案，
并且由于结构是通过多次去噪迭代逐步建立起来的，因此需要很少甚至不需要起始的结构信息。

## RF 结构预测 vs RFdiffusion 去噪
- 实现：坐标coordinate→预测结构predicted structure
- 输入input
    - RF
        - 序列sequence
        - 模板template, initial coordinates：extra structural information
    - RFdiffusion
        - noised coordinates from the previous step
        - 可选：
            - partial sequence
            - fold information
            - fixed functional-motif coordinates

# RFDiffusion
- 微调RoseTTAFold

每个残基包含一个Cα坐标和N-C α - C刚性取向

mse loss

这种损失驱动去噪轨迹与每个时间步的数据分布相匹配，从而收敛于可设计的蛋白质骨架结构


m.s.e.损失与RF结构预测训练(帧对齐点误差或FAPE)中使用的损失不同，
因为与FAPE不同，m.s.e.损失对全局参考框架不具有不变性，因此促进了全局坐标框架在时间步之间的连续性。
#todo FAPE

initialize random residue frames and RFdiffusion makes a denoised prediction
初始化随机残基框架，然后RFdiffusion进行去噪预测。
选择加入噪声的性质和这个反向步长的大小，使得去噪过程与去噪过程的分布相匹配
# 附
## 低温电镜
cryogenic electron microscopy


## CD

你上传的图片来自RFdiffusion论文，展示了蛋白质设计的结构和实验验证结果。下面是对这张图（f部分）的详细解读：

---

### 左侧：结构比对

- **左侧两幅结构图**分别对应两个设计蛋白（300aa_3 和 300aa_8）。
- **颜色说明**：  
  - 白色到蓝色：设计模型（Design）  
  - 粉色到橙色：AlphaFold2预测结构（AF2）
- **意义**：结构的重叠显示设计蛋白与AlphaFold2预测结构高度一致，说明设计的蛋白质结构可靠。

---

### 右上：CD光谱（CD spectra）

- **横轴**：波长（Wavelength, nm）
- **纵轴**：摩尔椭圆率（MRE, deg cm² dmol⁻¹）
- **曲线**：蓝色为300aa_3，粉色为300aa_8
- **意义**：CD光谱反映蛋白质的二级结构。两条曲线都显示出典型的折叠蛋白特征，说明设计蛋白在实验中能正确折叠。

---

### 右下：CD热熔曲线（CD melts）

- **横轴**：温度（Temperature, °C）
- **纵轴**：在222 nm下的MRE
- **曲线**：同上
- **意义**：热熔曲线平坦，说明蛋白质在高温下依然保持结构，具有很高的热稳定性。

---

### 总结

- 结构比对显示设计与预测高度一致。
- CD光谱和热熔实验表明设计蛋白质能正确折叠且非常稳定。
- 这些结果共同验证了RFdiffusion设计蛋白的结构准确性和实验可行性。

如需解读其他图，欢迎继续提问！


---

circular dichroism spectra


---

在RFdiffusion论文中，**Hallucination**（幻觉设计）指的是通过深度学习模型“凭空”生成（设计）全新蛋白质结构的方法，而不是基于已知蛋白模板或序列进行改造。具体来说，模型从随机输入（如随机噪声或随机结构）出发，通过优化或生成过程，逐步“幻化”出一个全新的、具有合理三维结构的蛋白质。

### 作用
- **创新性设计**：Hallucination 能够生成自然界中未见过的新型蛋白质结构，为蛋白质工程和功能设计提供了极大空间。
- **探索结构空间**：它帮助科学家探索蛋白质结构的可能性边界，发现新的折叠类型或功能模块。
- **验证模型能力**：Hallucination 结果的质量可以直接反映模型对蛋白质结构规律的学习和理解能力。

### 总结
Hallucination 在RFdiffusion中是指“从无到有”地设计新蛋白结构，是推动蛋白质设计创新和检验模型泛化能力的重要手段。


---

在RFdiffusion论文中，**relabeling** 指的是对蛋白质多链体系中各条链的标签（chain labels，例如A链、B链等）进行重新编号或重新分配。

具体来说，多链蛋白质具有对称性时，不同链之间本质上是等价的。比如一个四聚体的A、B、C、D链，交换它们的标签（比如A↔B）不会改变整体结构的对称性。这种“relabeling”操作就是指在分析或比较结构时，允许链的标签发生重新排列，以便更好地对齐和比较对称结构。

在你提到的句子中：

> with respect to rotation and relabelings of chains, symmetry is largely maintained in the denoising predictions

意思是：在考虑了旋转和链标签重新分配（relabeling）之后，RFdiffusion在去噪预测过程中基本保持了蛋白质的对称性。

**总结：**  
relabeling 在这里指的是多链蛋白中链标签的重新分配或重新编号，用于对称结构的比较和分析。


---

size-exclusion chromatography (SEC)


---

在这段论文中，**Gram-Schmidt过程**被用来为蛋白质的每个残基构建一个**3×3的旋转矩阵**，这个矩阵描述了该残基的局部坐标系。推导过程如下：

---

### 1. 背景

对于蛋白质的每个残基，通常会用三个原子的坐标（N, Cα, C）来定义一个局部坐标系。  
- \( z_N \)：N原子的坐标  
- \( z_{C\alpha} \)：Cα原子的坐标  
- \( z_C \)：C原子的坐标

---

### 2. Gram-Schmidt正交化步骤

#### 第一步：定义第一个基向量 \( r_1 \)
\[
r_1 = \frac{z_C - z_{C\alpha}}{||z_C - z_{C\alpha}||_2}
\]
- 这是Cα到C的单位向量。

#### 第二步：定义第二个基向量 \( r_2 \)
\[
r_2 = \frac{(z_N - z_{C\alpha}) - ((z_N - z_{C\alpha}) \cdot r_1) r_1}{||(z_N - z_{C\alpha}) - ((z_N - z_{C\alpha}) \cdot r_1) r_1||}
\]
- 先取Cα到N的向量，减去它在\( r_1 \)方向上的分量，使其与\( r_1 \)正交，再归一化。

#### 第三步：定义第三个基向量 \( r_3 \)
\[
r_3 = r_1 \times r_2
\]
- 用叉乘得到与前两个基向量都正交的第三个基向量。

---

### 3. 结果

这样就得到了一个以Cα为原点、三个正交单位向量为基的局部坐标系（旋转矩阵），可用于描述残基的空间取向。

---

### 总结

**Gram-Schmidt过程**在这里的作用是：  
- 从蛋白质主链的三个原子坐标出发，构造出一组正交的基向量，形成一个标准的局部坐标系（旋转矩阵），便于后续结构分析和建模。

---

这条公式描述了**蛋白质主链原子坐标的刚性变换**，常用于将标准参考构象变换到实际三维空间中的位置。

公式含义如下：

\[
[z_C, z_N, z_{C\alpha}] = r \begin{bmatrix} z_C^* \\ z_N^* \\ z_{C\alpha}^* \end{bmatrix} + z \vec{1}_3
\]

- \([z_C, z_N, z_{C\alpha}]\)：实际空间中C、N、Cα三个原子的坐标（通常是3×3矩阵，每列一个原子）。
- \(r\)：3×3旋转矩阵，表示空间中的旋转操作。
- \([z_C^*, z_N^*, z_{C\alpha}^*]\)：标准参考构象下C、N、Cα三个原子的坐标。
- \(z\)：平移向量，表示整体的空间平移。
- \(\vec{1}_3\)：长度为3的全1向量，保证平移作用于每个原子。

**总结**：  
该式表示：通过对标准参考构象进行旋转（\(r\)）和平移（\(z\)），可以得到实际蛋白质结构中主链原子的空间坐标。这是蛋白质结构建模和比对中的常用表达方式。