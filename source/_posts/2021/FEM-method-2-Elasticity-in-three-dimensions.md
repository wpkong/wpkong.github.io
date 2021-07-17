---
title: 'FEM method 2: Elasticity in three dimensions'
date: 2021-07-16 18:42:48
tags: 
  - FEM
  - Reading Notes
cover: https://i.loli.net/2021/07/17/2AvtuwfSUBjd6O8.png
katex: True
---

$\Omega$ 描述了物体的体空间

大写 $\vec X$ 描述了**形变前**(reference configutation)任意一点的位置。

小写 $\vec x$ 描述**形变后**任意一点的位置

$\phi$  (**Deformation function**)

 $ \vec x = \vec \phi(\vec X) $  描述位置X的形变

具体来说 $\vec \phi(\vec X) = (\phi_1(\vec X), \phi_2(\vec X), \phi_3(\vec X))^T$



$F$ (**Deformation gradient tensor**)

$F=\frac{\partial \vec \phi}{\partial \vec X} \in R^{3\times 3}$  可以记作 $F_{ij} = \phi_{i,j}$

从这个角度来看，$F$ 其实可以认为是 $\vec \phi$ 的 jacobian 矩阵。

另外 $F$ 在 $\Omega$ 上处处变化，因此是一个与空间相关的量，其显式表达为 $F(\vec X)$.



$E[\phi]$ (**Strain energy**, 应变能)

形变物体上累计的势能

这种描述暗示了能量完全由形变($\phi$)决定，也暗示了能量累计只与前后状态有关，与过程无关。这种只与状态有关的应变能的**独立性**是**超弹性材料**的性质之一（超弹性材料的弹力是**保守力**）。



$\Psi$ (**Energy density**)

描述 $\vec X$ 周围微小体积上的能量，也即微元能量。

根据能量密度，总应变能就可以视为全域上的积分 $E[\phi] = \int_\Omega \Psi[\phi;\vec X]d\vec X$ 

$\Psi[\phi;\vec X]$ 的参数与**形变函数**和**位置**有关，但是如果把形变函数在 $\vec X_*$ 处一阶泰勒展开：
$$
\begin{aligned}
\phi(\vec X) & \approx \phi(\vec X_*) + \frac{\partial \phi}{\partial \vec X} \Bigg|_{\vec X_*}
= \vec x_* + F(\vec X_*)(\vec X - \vec X_*) \\
& = \underbrace{F(\vec X_*)}_{F_*}\vec X + \underbrace{\vec x_* - F(\vec X_*)\vec X_*}_{\vec t}\\
& = F_*\vec X + \vec t
\end{aligned}
$$
由于 $\vec t$ 项描述的是形变方程 $\phi(\vec X)$ 的位移项，而位移对于势能没有贡献，所以这一项可以看做与能量密度 $\Psi$ 无关。所以 $\Psi$ 只和 $F$ 有关，因此可以记作 $\Psi(F(\vec X))$。 



**Force density**  (内部力) 和 **Traction** (外部牵引力)

内部力密度(force density)和边界牵引力(traction)需要分开算，因为边界力通常比内部力更强，物理角度来说，内部微元受到的力来自各个方向，就算很大也基本会被抵消，但是边界受力就一般来自内部一侧，通常无法抵消。

> 一旦使用离散表示，就无需分开考虑了，后续部分只是在连续表示下分析

定义上来说

- $\vec f(\vec X)$ 是指单位未形变**体积(volume)**上的受力大小，合力大小 $\vec{f}_{\text {aggregate }}(A)=\int_{A} \vec{f}(\vec{X}) d \vec{X}, \space A \subset \Omega $
- $\vec \tau(\vec X)$ 是指单位未形变**面积(area)**上的受力大小，合力大小为 $\vec{f}_{\text {aggregate }}(B)=\oint_{B} \vec{\tau}(\vec{X}) dS, \space B \subset \partial \Omega$

如何计算 force density 和 traction 呢，使用 **stress tensor** $P, P \in R^{3\times3}$ 就可以了，这里用的是 **First Piola-Kirchhoff stress tensor**

$P$ 可以认为是内部点 $\vec X \in \Omega/\partial\Omega$ 沿着 $\vec N$ 的垂直方向切了一刀，切面的 traction 就是 $P$

- 对于边界情况（traction）

$\vec \tau(\vec X) = -P \cdot\vec N$

$\vec N$ 是**未变形的**物体表面向外射出的单位法向量

- 对于内部情况（force density)

$\vec f(\vec X) = div_{\vec X} P(\vec X)$ 或者 $f_i = \sum_{j=1}^3 P_{ij,j} = \frac{\partial P_{i1}}{\partial X_1} + \frac{\partial P_{i2}}{\partial X_2} + \frac{\partial P_{i3}}{\partial X_3}$

>  对于**超弹性材料**（hyperelastic）来说，$P$仅仅是deformation gradient($F$)的函数：$P(F) = \frac{\Psi(F)}{\partial F}$

如上可以看到，借助First Piola-Kirchhoff stress tensor $P$，可以计算出traction和force density. 通常会提供两种超材料性质的描述形式：1. $\Psi$对$F$的函数；2.$P$对$F$的函数。



上述描述的证明如下：

对 $\Psi(F)$ 变分处理： 
$$
\delta \Psi(F) = \frac{\partial \Psi}{\partial \mathbf{F}}: \delta \mathbf{F}
$$
而势能为**“力 * 位移”**，注意，之前定义的force density $\vec f$ 是“**力的密度**”，而不是力本身，$\tau$ 同理。因此 $\vec f \times dV$才是力的大小。而对于一个点 $\vec x$ ，位移为 $\delta \vec \phi$ (变分定义)。基于此，应变能 $E$ 的变分可以看做是：
$$
\delta E=-\int_{\Omega} \vec{f}(\vec{X}) \cdot \delta \vec{\phi}(\vec{X}) d \vec{X}-\oint_{\partial \Omega} \vec{\tau}(\vec{X}) \cdot \delta \vec{\phi}(\vec{X}) d S
$$
另一方面，应变能又可以看做是能量密度的积分，把公式(1)带入后得到：
$$
\begin{aligned}
\delta E=\delta\left[\int_{\Omega} \Psi(\mathbf{F}) d \vec{X}\right]= \int_{\Omega} \delta[\Psi(\mathbf{F})] d \vec{X}
& =\int_{\Omega}\left[\frac{\partial \Psi}{\partial \mathbf{F}}: \delta \mathbf{F}\right] d \vec{X}\\
&=\int_{\Omega}[\mathbf{P}: \delta \mathbf{F}] d \vec{X} \\
&=\sum_{i, j=1}^{3} \int_{\Omega} P_{i j} \delta F_{i j} d \vec{X} \\ 
&= \sum_{i, j=1}^{3} \int_{\Omega} \underbrace{P_{i j}}_u \cdot \underbrace{\frac{\partial}{\partial X_{j}}\left[\delta \phi_{i}(\vec{X})\right]}_{v'} d \vec{X} \\
&= \sum_{i, j=1}^{3} \int_{\Omega} u \cdot v' d\vec X
\end{aligned}
$$


由公式 $(uv)' = u'v + v'u \Longrightarrow u'v =  - v'u + (uv)'$，带入后有：
$$
- \int_\Omega v'u\space d\vec X = -\int_{\Omega} \frac{\partial}{\partial X_{j}}\left[P_{i j}\right] \cdot \delta \phi_{i}(\vec{X}) d \vec{X}
$$

$$
\int_\Omega (uv)' d\vec X = (uv) cos \theta_j \Bigg|_\Omega = \oint_{\partial \Omega} u \cdot v\cdot N_j d\vec X
$$

其中公式(4)的 $cos$ 项是因为之前是对 $\vec X$ 的分量进行偏导，相当于做了一次投影，因此这里恢复到全域上时需要补偿回来。

综上，有
$$
\begin{gathered}
\delta E=\sum_{i, j=1}^{3}\left[-\int_{\Omega} \frac{\partial}{\partial X_{j}}\left[P_{i j}\right] \cdot \delta \phi_{i}(\vec{X}) d \vec{X}+\oint_{\partial \Omega} P_{i j} N_{j} \cdot \delta \phi_{i}(\vec{X}) d \vec{X}\right] \\
=-\int_{\Omega} \operatorname{div} \mathbf{P} \cdot \delta \vec{\phi}(\vec{X}) d \vec{X}+\oint_{\partial \Omega}(\mathbf{P} \cdot \vec{N}) \cdot \delta \vec{\phi}(\vec{X}) d \vec{X}
\end{gathered}
$$
和公式(2)联立后可以得到
$$
\vec{f}(\vec{X})=\operatorname{div} \mathbf{P} \\
\vec{\tau}(\vec{X})=-\mathbf{P} \vec{N}
$$
