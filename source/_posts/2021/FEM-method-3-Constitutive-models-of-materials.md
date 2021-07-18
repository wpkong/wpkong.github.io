---
title: 'FEM method 3: Constitutive models of materials'
date: 2021-07-16 18:47:40
tags: 
  - FEM
  - Reading Notes
cover: https://i.loli.net/2021/07/17/2AvtuwfSUBjd6O8.png
katex: True
---

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



### Linear elasticity

如之前描述，$E$ 可以用上述线性描述的模型叫做 *Linear elasticity* ，其 *strain energy density* 可以描述为
$$
\Psi(F) = \mu\epsilon:\epsilon + \frac \lambda 2 tr^2(\epsilon)
$$
其中 $\mu,\lambda$ 是 Lame coeddcients ，可以根据**杨氏模量 k**和**泊松比 **$\nu$计算

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

