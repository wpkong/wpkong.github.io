---
title: 'FEM method: Constitutive models of materials'
date: 2021-07-16 18:47:40
tags: FEM, Reading Notes
mathjax: True
katex: True
---

## strain measure

### Green strain tensor

描述了形变的程度
$$
E = \frac12(F^TF-I) \quad E\in R^{3\times3}
$$
良好的性质：只有旋转、平移的情况下$\phi(\vec X) = R \vec X + \vec t$，$R^TR = I$，所以$E$为`0`

小trick：对于任意的形变，形变矩阵可以通过极分解成旋转矩阵和形变矩阵的乘$F=RS$，那么上式也可以变成：
$$
E = \frac12(S^2-I)
$$
其中R因为有三个自由度（x-y-z方向上的旋转），那么S就有6个自由度

由于$E(F)$是个二次函数，线性性不好，通过在$I$处泰勒展开，可以得到它的**线性**逼近（small strain tensor / infinitesimal strain tensor）：
$$
\epsilon=\frac12(F+F^T) - I
$$
注意，上述只在小形变的情况下适合，大形变下就不对了。



### Linear elasticity

如之前描述，$E$可以用上述线性描述的模型叫做Linear elasticity，其strain energy density可以描述为
$$
\Psi(F) = \mu\epsilon:\epsilon + \frac \lambda 2 tr^2(\epsilon)
$$
其中$\mu,\lambda$是Lame coeddcients，可以根据**杨氏模量k**和**泊松比**$\nu$计算

- 杨氏模量*Young’s modulus*: 伸缩性的度量
- 泊松比*Poisson’s ratio*: 不可压缩性的度量

$$
\mu = \frac k {2(1+\nu)} \\
\lambda = \frac{k\nu}{(1+\nu)(1-2\nu)}
$$

因此：
$$
P(F) = \frac {\partial \Psi}{\partial F} = \mu(F+F^T-2I) + \lambda tr(F-I)I
$$
有两点需要注意

1. P是F的线性函数，计算成本很低
2. 由于$\epsilon$是在小型变的情况下定义的，所以只有在小型变时才适用线性弹性Linear elastic

