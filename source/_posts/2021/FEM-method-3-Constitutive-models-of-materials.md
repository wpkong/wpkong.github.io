---
title: 'FEM method 3: Constitutive models of materials'
date: 2021-07-16 18:47:40
tags: 
  - FEM
  - Reading Notes
cover: https://i.loli.net/2021/07/17/2AvtuwfSUBjd6O8.png
katex: True
---

## Constitutive models

**构成模型** (*Constitutive models*)，即对某一材料的物理特性的数学描述 



## strain measure

形变梯度 $F$ 反映不够直观，因此采用一些例如应**变度量** (strain measures)或者不变量 (invariants) 等中间量来定量描述这种形变。通常这些中间量由 $F$ 计算得到，但相对全面地保留 $F$ 的特征。

### Green strain tensor

描述了形变的程度
$$
E = \frac12(F^TF-I) \quad E\in R^{3\times3}
$$
良好的性质：只有旋转、平移的情况下 $\phi(\vec X) = R \vec X + \vec t$ ，$R^TR = I$，所以 $E$ 为 $0$

小trick：对于任意的形变，形变矩阵可以通过**极分解**成旋转矩阵和形变矩阵的乘 $F=RS$，那么上式也可以变成：
$$
E = \frac12(S^2-I)
$$
其中 $R$ 因为有三个自由度（$x-y-z$ 方向上的旋转），那么 $S$ 就有 $6$ 个自由度。这里也可以理解为 $S$ 是对称矩阵，所以自由度为 $6$. 所以 Green strain tensor 摒弃了旋转带来的 $3$ 个自由度，只保留了形变的 $6$ 个自由度。

由于 $E(F)$ 是个二次函数，线性性不好，通过在 $F = I$ 处泰勒展开，可以得到它的**线性**逼近（small strain tensor / infinitesimal strain tensor）：
$$
\mathbf{E}(\mathbf{F}) \approx \underbrace{\mathbf{E}(\mathbf{I})}_{=\mathbf{0}}+\left.\frac{\partial \mathbf{E}}{\partial \mathbf{F}}\right|_{\mathbf{F}=\mathbf{I}}:(\mathbf{F}-\mathbf{I})
$$
上式中由 $E(\cdot)$ 的定义，$E(I)$ 为 $0$.

而针对右边的偏导项，由于对 $E$ 变分后可以得到左右两边如下的式子（左边是形式化定义的变分，右边是对公式(1)的变分）：
$$
\frac{\partial \mathbf{E}}{\partial \mathbf{F}}: \delta \mathbf{F}=\delta \mathbf{E}=\frac{1}{2}\left(\delta \mathbf{F}^{T} \mathbf{F}+\mathbf{F}^{T} \delta \mathbf{F}\right)
$$
用 $F - I$ 代换 $\frac{\partial \mathbf{E}}{\partial \mathbf{F}}: \delta \mathbf{F}$ （为了得到公式(3)的右边项）：
$$
\left.\frac{\partial \mathbf{E}}{\partial \mathbf{F}}\right|_{\mathbf{F}=\mathbf{I}}:(\mathbf{F}-\mathbf{I})=\frac{1}{2}\left[(\mathbf{F}-\mathbf{I})^{T} \mathbf{I}+\mathbf{I}^{T}(\mathbf{F}-\mathbf{I})\right]=\frac{1}{2}\left(\mathbf{F}+\mathbf{F}^{T}\right)-\mathbf{I}
$$
最终，对 $E(F)$ 可以得到如下的线性逼近 (*small strain tensor*)：
$$
\epsilon=\frac12(F+F^T) - I
$$
注意，上述只在小形变的情况下适合，大形变下就不对了。



## Linear elasticity

如之前描述，$E$ 可以用上述线性描述的模型叫做 *Linear elasticity* ，其 *strain energy density* 可以描述为
$$
\Psi(F) = \mu\epsilon:\epsilon + \frac \lambda 2 tr^2(\epsilon)
$$
其中 $\mu,\lambda$ 是 Lame coeddcients ，可以根据**杨氏模量** $k$ 和**泊松比** $\nu$ 计算

- 杨氏模量 *Young’s modulus*: 伸缩性的度量
- 泊松比 *Poisson’s ratio*: 不可压缩性的度量

$$
\mu = \frac k {2(1+\nu)} \\
\lambda = \frac{k\nu}{(1+\nu)(1-2\nu)}
$$

对 $\epsilon$ 做变分：
$$
\delta \boldsymbol{\epsilon}=\frac{1}{2}\left(\delta \mathbf{F}+\delta \mathbf{F}^{T}\right)=\operatorname{Sym}\{\delta \mathbf{F}\}
$$
其中符号 $Sym$ 是对 $\frac 1 2 A \cdot A^T$ 的记号

将上述变分带入公式(7)中（**冒号为按位乘再相加**）：
$$
\boldsymbol{\epsilon}: \delta \boldsymbol{\epsilon}=\boldsymbol{\epsilon}: \operatorname{Sym}\{\delta \mathbf{F}\}=\boldsymbol{\epsilon}: \delta \mathbf{F} \\ \operatorname{tr}(\delta \boldsymbol{\epsilon})=\mathbf{I}: \operatorname{Sym}\{\delta \mathbf{F}\}=\mathbf{I}: \delta \mathbf{F}
$$

$$
\delta \Psi=2 \mu \boldsymbol{\epsilon}: \delta \boldsymbol{\epsilon}+\lambda \operatorname{tr}(\boldsymbol{\epsilon}) \operatorname{tr}(\delta \boldsymbol{\epsilon})=\underbrace{[2 \mu \boldsymbol{\epsilon}+\lambda \operatorname{tr}(\boldsymbol{\epsilon}) \mathbf{I}]}_{=\partial \Psi / \partial \mathbf{F}}: \delta \mathbf{F}
$$

因此：
$$
P(F) = \frac {\partial \Psi}{\partial F} = \mu(F+F^T-2I) + \lambda tr(F-I)I
$$
有两点需要注意

1. $P$ 是 $F$ 的线性函数，计算成本很低，这也是叫做 *Linear Elasticity* 的原因
2. 由于 $\epsilon$ 是在小型变的情况下定义的，所以只有在小型变时才适用线性弹性 *Linear elastic*



## St. Venant-Kirchhoff model

上述中由于把 *Green Strain Tensor* $E$ 用 *small strain tensor* $\epsilon$ 近似，那么顺理成章地在 *Linear Elasitcity* 中替换回来。也即 *St. Venant-Kirchhoff material*：
$$
\Psi(F) = \mu E:E + \frac \lambda 2 tr^2(E)
$$
由公式(1)可以先算出 $E$ 的变分：
$$
\delta \mathbf{E}=\frac{1}{2}\left(\delta \mathbf{F}^{T} \mathbf{F}+\mathbf{F}^{T} \delta \mathbf{F}\right)=\operatorname{Sym}\left\{\mathbf{F}^{T} \delta \mathbf{F}\right\}
$$

$$
\mathbf{E}: \delta \mathbf{E}=\mathbf{E}:\left\{\mathbf{F}^{T} \delta \mathbf{F}\right\}=\{\mathbf{F} \mathbf{E}\}: \delta \mathbf{F} \\

\operatorname{tr}(\delta \mathbf{E})=\mathbf{I}:\left\{\mathbf{F}^{T} \delta \mathbf{F}\right\}=\mathbf{F}: \delta \mathbf{F}
$$


> 注意上述公式中有个等式 $S:(A^TB) = (AS):B$，其中三个矩阵是任意的，不要求对称。可以通过求和形式展开，可以得到两边展开形式为
> $$
> \sum_i\sum_j\sum_k S_{i,j}A_{k,j}B_{k,j} = \sum_i\sum_j\sum_k S_{k,j}A_{i,k}B_{i,j}
> $$



然后带入 $\Psi$ 的变分：
$$
\delta \Psi=2 \mu \mathbf{E}: \delta \mathbf{E}+\lambda \operatorname{tr}(\mathbf{E}) \operatorname{tr}(\delta \mathbf{E})=\underbrace{\mathbf{F}[2 \mu \mathbf{E}+\lambda \operatorname{tr}(\mathbf{E}) \mathbf{I}]}_{=\partial \Psi / \partial \mathbf{F}}: \delta \mathbf{F} 
$$
因此
$$
\mathbf{P}(\mathbf{F})=\mathbf{F}[2 \mu \mathbf{E}+\lambda \operatorname{tr}(\mathbf{E}) \mathbf{I}]
$$
由此可以看到，$P$ 是 $F$ 的三阶多项式。而对比 *Linear elasticity* 中用 *small strain tensor* 代替 *Green strain tensor* 这种损失了旋转不变性的做法，由于 *Green strain tensor* 的旋转不变性， *St. Venant-Kirchhoff material* 也具有旋转不变性。

但是这种模型一些问题，比方说在压缩一个物体的过程中，外力施加下会产生一个抵抗压缩的恢复力，这个恢复力随着外力增大而增大。在施加压力极大的情况下，物体一旦坍缩成 $0$ 体积，导致 $F = 0 \longrightarrow P = 0$ （体积为0）。如果再加大压力，会使得物体体积反转，导致反向扩大。**这和现实情况显然不符**，仿真的时候有可能会出现这种情况。这是由于高度的非线性复杂度，但是过于简单的话会导致如 *Linear elasticity* 这种构成模型中使得旋转不变性消失的情况。

## Corotated linear elasticity

这种构成模型试图结合简单性和足够的非线性，以保证旋转不变性（解决上述两个构成模型的缺陷）。

首先使用极分解 $F = RS$ 构成一个新的应变度量 (*strain measure*)：
$$
\epsilon_c = S - I
$$
用这个应变度量替换公式(7)的 *small strain tensor*：
$$
\begin{aligned}
\Psi(F)  &= \mu \epsilon_c:\epsilon_c + \frac \lambda 2 tr^2(\epsilon_c) \\
&= \mu\|\mathbf{S}-\mathbf{I}\|_{F}^{2}+(\lambda / 2) \operatorname{tr}^{2}(\mathbf{S}-\mathbf{I})

\end{aligned}
$$

>  之前公式(15)中用到了一个等式 $S:(A^TB) = (AS):B$ ，这里可以代换一下，我们知道 $A_F^2 = A:A$，那么：
> $$
> \begin{aligned}
> 
> \|S-I\|_F^2 &= (S-I):(S-I) \\
> & = (S-I):(R^TR(S-I)) = (S-I):(R^T(F-R)) \\
> &= (R(S-I)):(F-R) = (F-R):(F-R) = \\
> &= \|F-R\|_F^2
> 
> \end{aligned}
> $$
> 

也可以得到：
$$
\Psi(\mathbf{F})=\mu\|\mathbf{F}-\mathbf{R}\|_{F}^{2}+(\lambda / 2) \operatorname{tr}^{2}\left(\mathbf{R}^{T} \mathbf{F}-\mathbf{I}\right) \\
$$
因此用 *1st Piola-Kirchhoff stress tensor*， 对 $\Psi(F)$ 对 $F$ 求偏导得到 $P$： 
$$
\begin{aligned}
\mathbf{P}(\mathbf{F}) &=\mathbf{R}\left[2 \mu \boldsymbol{\epsilon}_{\mathrm{c}}+\lambda \operatorname{tr}\left(\boldsymbol{\epsilon}_{\mathrm{c}}\right) \mathbf{I}\right]=\mathbf{R}[2 \mu(\mathbf{S}-\mathbf{I})+\lambda \operatorname{tr}(\mathbf{S}-\mathbf{I}) \mathbf{I}] \\
&=2 \mu(\mathbf{F}-\mathbf{R})+\lambda \operatorname{tr}\left(\mathbf{R}^{T} \mathbf{F}-\mathbf{I}\right) \mathbf{R}
\end{aligned}
$$
这种模型由于采用了极分解，并且R处处不一样，导致其复杂度较高。



## Isotropic materials and invariants





