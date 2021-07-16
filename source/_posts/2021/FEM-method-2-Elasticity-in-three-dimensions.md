---
title: 'FEM method 2: Elasticity in three dimensions'
date: 2021-07-16 18:42:48
tags: FEM, Reading Notes
mathjax: True
katex: True
---

$\phi$ (**deformation function**)

 $ \vec x = \vec \phi(\vec X) $ 描述位置X的形变

$F$ (**deformation gradient tensor**)

$F=\frac{\partial \vec \phi}{\partial \vec X} \in R^{3\times 3}$  可以记作$F_{ij} = \phi_{i,j}$

$\Psi$ (**energy density**)

$E[\phi] = \int_\Omega \Psi[\phi;\vec X]d\vec X$ 描述$\vec X$周围微小体积上的能量

**force density**(内部力)和**traction**(外部牵引力)

内部力密度(force density)和边界牵引力(traction)需要分开算，因为边界力通常比内部力更强，物理角度来说，内部微元受到的力来自各个方向，就算很大也基本会被抵消，但是边界受力就一般来自内部一侧，通常无法抵消。

如何计算force density和traction呢，使用stress tensor $P$就可以了，这里用的是**First Piola-Kirchhoff stress tensor**

$P$可以认为是内部点$\vec X \in \Omega/\partial\Omega$沿着$\vec N$的垂直方向切了一刀，切面的traction就是$P$

- 对于边界情况（traction）

$\vec \tau(\vec X) = -P \cdot\vec N$

$\vec N$是未变形的物体表面向外射出的单位法向量

- 对于内部情况（force density)

$\vec f(\vec X) = div_{\vec X} P(\vec X)$ 或者 $f_i = \sum_{j=1}^3 P_{ij,j} = \frac{\partial P_{i1}}{\partial X_1} + \frac{\partial P_{i2}}{\partial X_2} + \frac{\partial P_{i3}}{\partial X_3}$

对于**超弹性材料**（hyperelastic）来说，$P$仅仅是deformation gradient($F$)的函数：$P(F) = \frac{\Psi(F)}{\partial F}$

如上可以看到，借助First Piola-Kirchhoff stress tensor，可以计算出traction和force density. 通常会提供两种超材料性质的描述形式：1. $\Psi$对$F$的函数；2.$P$对$F$的函数。

