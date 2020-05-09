<!-- TOC -->

- [奇异值分解(《统计学习方法》)](#奇异值分解统计学习方法)
  - [奇异值分解的定义与性质](#奇异值分解的定义与性质)
    - [定义与定理](#定义与定理)
    - [紧奇异值分解与截断奇异值分解](#紧奇异值分解与截断奇异值分解)
      - [紧奇异值分解](#紧奇异值分解)
      - [截断奇异值分解](#截断奇异值分解)
    - [几何解释](#几何解释)
    - [主要性质](#主要性质)
  - [奇异值分解的计算](#奇异值分解的计算)
  - [奇异值分解与矩阵近似](#奇异值分解与矩阵近似)
    - [弗罗贝尼乌斯范数](#弗罗贝尼乌斯范数)
    - [矩阵的最优近似](#矩阵的最优近似)
    - [矩阵的外积展开式](#矩阵的外积展开式)
- [主成分分析(《统计学习方法》)](#主成分分析统计学习方法)
  - [总体主成分分析](#总体主成分分析)
    - [基本想法](#基本想法)
    - [定义和导出](#定义和导出)
    - [主要性质](#主要性质-1)
    - [主成分的个数](#主成分的个数)
    - [规范化变量的总体主成分](#规范化变量的总体主成分)
  - [样本主成分分析](#样本主成分分析)
    - [样本主成分的定义和性质](#样本主成分的定义和性质)
    - [相关矩阵的特征值分解算法](#相关矩阵的特征值分解算法)
    - [数据矩阵的奇异值分解算法](#数据矩阵的奇异值分解算法)
- [主成分分析(《机器学习》)](#主成分分析机器学习)
- [线性判别分析(《机器学习》)](#线性判别分析机器学习)

<!-- /TOC -->
# 奇异值分解(《统计学习方法》)
## 奇异值分解的定义与性质
### 定义与定理
矩阵的奇异值分解是指，将一个非零的$m \times n$实矩阵$A, A \in \mathbf{R}^{m \times n}$,表示为以下三个实矩阵乘积形式的运算，即进行矩阵的因子分解：

$A=U \Sigma V^{\mathrm{T}}$

其中$U$是$m$阶正交矩阵(orthogonal matrix),$V$是$n$阶正交矩阵，$\sum$是由降序排列的非负的对角线元素组成的$m \times n$矩形对角矩阵(rectangular diagonal matrix),满足

$U U^{\mathrm{T}}=I$

$V V^{\mathrm{T}}=I$

$\Sigma=\operatorname{diag}\left(\sigma_{1}, \sigma_{2}, \cdots, \sigma_{p}\right)$

$\sigma_{1} \geqslant \sigma_{2} \geqslant \cdots \geqslant \sigma_{p} \geqslant 0$

$p=\min (m, n)$

$U \Sigma V^{\mathrm{T}}$称为矩阵$A$的奇异值分解(singular value decomposition, SVD),$\sigma_{i}$称为矩阵$A$的奇异值，$U$的列向量称为左奇异向量，$V$的列向量称为右奇异向量。
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200413025410.png)

奇异值分解基本定理:

若$A$为一$m \times n$实矩阵，$A \in \mathbf{R}^{m \times n}$，则$A$的奇异值分解存在

$A=U \Sigma V^{\mathrm{T}}$

其中$U$是$m$阶正交矩阵，$V$是$n$阶正交矩阵，$\Sigma$是$m \times n$矩形对角矩阵，其对角线元素非负，且按降序排列。

### 紧奇异值分解与截断奇异值分解

$A=U \Sigma V^{\mathrm{T}}$又称为矩阵的完全奇异值分解（full singular value decomposition）。实际常用的是奇异值分解的紧凑形式和截断形式。紧奇异值分解是与原始矩阵等秩的奇异值分解，截断奇异值分解是比原始矩阵低秩的奇异值分解。

#### 紧奇异值分解

设有$m \times n$实矩阵$A$,其秩为$\operatorname{rank}(A)=r, r \leqslant \min (m, n)$,则称$U_{r} \Sigma_{r} V_{r}^{\mathrm{T}}$为$A$的紧奇异值分解（compact singular value decomposition），即：

$A=U_{r} \Sigma_{r} V_{r}^{\mathrm{T}}$

其中$U_{r}$是$m \times r$矩阵，$V_{r}$是$n \times r$矩阵，$\Sigma_{r}$是$r$阶对角矩阵；矩阵$U_{r}$由完全奇异值分解中$U$的前$r$列，矩阵$V_{r}$由$V$的前$r$列，矩阵$\Sigma_{r}$由$\sum$的前$r$个对角线元素得到。紧奇异值分解的对角矩阵$\Sigma_{r}$的秩与原始矩阵$A$的秩相等。

#### 截断奇异值分解
在矩阵的奇异值分解中，只取最大的$k$个奇异值($k<r, r$为矩阵的秩)对应的
部分，就得到矩阵的截断奇异值分解。实际应用中提到矩阵的奇异值分解时，通常指截断奇异值分解。

设$A$为$m \times n$实矩阵，其秩$\operatorname{rank}(A)=r$，且$0<k<r$，则称$U_{k} \Sigma_{k} V_{k}^{\mathrm{T}}$为矩阵$A$的截断奇异值分解（truncated singular value decomposition)

$A \approx U_{k} \Sigma_{k} V_{k}^{\mathrm{T}}$

其中$U_{k}$是$m \times k$矩阵，$V_{k}$是$n \times k$矩阵，$\Sigma_{k}$是$k$阶对角矩阵；矩阵$U_{k}$由完全奇异值分解中$U$的前$k$列，矩阵$V_{k}$由$V$的前$k$列，矩阵$\Sigma_{k}$由$\Sigma$的前$k$个对角线元素得到。对角矩阵$\Sigma_{k}$的秩比原始矩阵$A$的秩低。

在实际应用中，常常需要对矩阵的数据进行压缩，将其近似表示，奇异值分解提供了一种方法。后面将要叙述，奇异值分解是在平方损失（弗罗贝尼乌斯范数）意义下对矩阵的最优近似。紧奇异值分解对应着无损压缩，截断奇异值分解对应着有损压缩。

### 几何解释
从线性变换的角度理解奇异值分解，$m \times n$矩阵$A$表示从$n$维空间$\mathbf{R}^{n}$到$m$维空间$\mathbf{R}^{m}$的一个线性变换，

$T: x \rightarrow A x$

$x \in \mathbf{R}^{n}, A x \in \mathbf{R}^{m}$，$x$和$A x$分别是各自空间的向量。线性变换可以分解为三个简单的变换：--个坐标系的旋转或反射变换、一个坐标轴的缩放变换、另一个坐标系的旋转或反射变换。奇异值定理保证这种分解--定存在。这就是奇异值分解的几何解释。

对矩阵$A$进行奇异值分解，得到$A=U \Sigma V^{\mathrm{T}}, V$和$U$都是正交矩阵，所以$V$的列向量$v_{1}, v_{2}, \cdots, v_{n}$构成$\mathbf{R}^{n}$空间的一组标准正交基，表示$\mathbf{R}^{n}$中的正交坐标系的旋转或反射变换；$U$的列向量$u_{1}, u_{2}, \cdots, u_{m}$构成$\mathbf{R}^{m}$空间的一组标准正交基，表示$\mathbf{R}^{m}$中的正交坐标系的旋转或反射变换；$\sum$的对角元素$\sigma_{1}, \sigma_{2}, \cdots, \sigma_{n}$是一组非负实数，表示$\mathbf{R}^{n}$中的原始正交坐标系坐标轴的$\sigma_{1}, \sigma_{2}, \cdots, \sigma_{n}$倍的缩放变换。

任意一个向量$x \in \mathbf{R}^{n}$，经过基于$A=U \Sigma V^{\mathrm{T}}$的线性变换，等价于经过坐标系的旋转或反射变换$V^{\mathrm{T}}$，坐标轴的缩放变换$\sum$，以及坐标系的旋转或反射变换$U$，得到向量$A x \in \mathbf{R}^{m}$。下图给出直观的几何解释（见文前彩图）。原始空间的标准正交基（红色与黄色），经过坐标系的旋转变换$V^{\mathrm{T}}$，坐标轴的缩放变换$\sum$(黑色$\sigma_{1}, \sigma_{2}$),坐标系的旋转变换$U$,得到和经过线性变换$A$等价的结果。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200418202937.png)

综上，矩阵的奇异值分解也可以看作是将其对应的线性变换分解为旋转变换、缩放变换及旋转变换的组合。根据定理, 这个变换的组合一定存在。

### 主要性质
1. 设矩阵$A$的奇异值分解为$A=U \Sigma V^{\mathrm{T}}$，则以下关系成立：

$A^{\mathrm{T}} A=\left(U \Sigma V^{\mathrm{T}}\right)^{\mathrm{T}}\left(U \Sigma V^{\mathrm{T}}\right)=V\left(\Sigma^{\mathrm{T}} \Sigma\right) V^{\mathrm{T}}$

$A A^{\mathrm{T}}=\left(U \Sigma V^{\mathrm{T}}\right)\left(U \Sigma V^{\mathrm{T}}\right)^{\mathrm{T}}=U\left(\Sigma \Sigma^{\mathrm{T}}\right) U^{\mathrm{T}}$

也就是说，矩阵$A^{\mathrm{T}} A$和$A A^{\mathrm{T}}$的特征分解存在，且可以由矩阵$A$的奇异值分解的矩阵表示。$V$的列向量是$A^{\mathrm{T}} A$的特征向量，$U$的列向量是$A A^{\mathrm{T}}$的特征向量，$\sum$的奇异值是$=A^{T} A$和$A A^{\mathrm{T}}$的特征值的平方根。

2. 在矩阵$A$的奇异值分解中，奇异值、左奇异向量和右奇异向量之间存在对应关系。

由$A=U \Sigma V^{\mathrm{T}}$易知

$A V=U \Sigma$

比较这一等式两端的第$j$列，得到：

$A v_{j}=\sigma_{j} u_{j}, \quad j=1,2, \cdots, n$

这是矩阵$A$的右奇异向量和奇异值、左奇异向量的关系。

类似地，由

$A^{\mathrm{T}} U=V \Sigma^{\mathrm{T}}$

得到：

$A^{\mathrm{T}} u_{j}=\sigma_{j} v_{j}, \quad j=1,2, \cdots, n$
$A^{\mathrm{T}} u_{j}=0, \quad j=n+1, n+2, \cdots, m$

这是矩阵$A$的左奇异向量和奇异值、右奇异向量的关系。

3. 矩阵$A$的奇异值分解中，奇异值$\sigma_{1}, \sigma_{2}, \cdots, \sigma_{n}$是唯一的，而矩阵$U$和$V$不是唯一的。

4. 矩阵$A$和$\sum$的秩相等，等于正奇异值$\sigma_{i}$的个数$r$（包含重复的奇异值）。
5. 矩阵$A$的$r$个右奇异向量$v_{1}, v_{2}, \cdots, v_{r}$构成$A^{\mathrm{T}}$的值域$R\left(A^{\mathrm{T}}\right)$的一组标准正交基。因为矩阵$A^{\mathrm{T}}$是从$\mathbf{R}^{m}$映射到$\mathbf{R}^{n}$的线性变换，则$A^{\mathrm{T}}$的值域$R\left(A^{\mathrm{T}}\right)$和$A^{\mathrm{T}}$的列空间是相同的，$v_{1}, v_{2}, \cdots, v_{r}$是$A^{\mathrm{T}}$的一组标准正交基，因而也是$R\left(A^{\mathrm{T}}\right)$的一组标准正交基。

矩阵$A$的$n-r$个右奇异向量$v_{r+1}, v_{r+2}, \cdots, v_{n}$构成$A$的零空间$N(A)$的一组标准正交基。

矩阵$A$的$r$个左奇异向量$u_{1}, u_{2}, \cdots, u_{r}$构成值域$R(A)$的一组标准正交基。

矩阵$A$的$m-r$个左奇异向量$u_{r+1}, u_{r+2}, \cdots, u_{m}$构成$A^{\mathrm{T}}$的零空间$N\left(A^{\mathrm{T}}\right)$的一组标准正交基。

## 奇异值分解的计算
奇异值分解基本定理证明的过程蕴含了奇异值分解的计算方法。矩阵$A$的奇异值分解可以通过求对称矩阵$A^{\mathrm{T}} A$的特征值和特征向量得到。$A^{\mathrm{T}} A$的特征向量构成正交矩阵$V$的列，$A^{\mathrm{T}} A$的特征值$\lambda_{j}$$\lambda_{j}$的平方根为奇异值$\sigma_{i}$，即

$\sigma_{j}=\sqrt{\lambda_{j}}, \quad j=1,2, \cdots, n$

对其由大到小排列作为对角线元素，构成对角矩阵$\sum$；求正奇异值对应的左奇异向量，再求扩充的$A^{\mathrm{T}}$的标准正交基，构成正交矩阵$U$的列，从而得到$A$的奇异值分解$A=U \Sigma V^{\mathrm{T}}$。

给定$m \times n$的矩阵$A$,可以按照上面的叙述写出矩阵奇异值分解的计算过程。
1. 首先求$A^{\mathrm{T}} A$的特征值和特征向量。

计算对称矩阵$W=A^{\mathrm{T}} A$。

求解特征方程

$(W-\lambda I) x=0$

得到特征值$\lambda_{i}$，并将特征值由大到小排列：

$\lambda_{1} \geqslant \lambda_{2} \geqslant \cdots \geqslant \lambda_{n} \geqslant 0$

将特征值$\lambda_{i}(i=1,2, \cdots, n)$代入特征方程求得对应的特征向量。

2. 求$n$阶正交矩阵$V$
将特征向量单位化，得到单位特征向量$v_{1}, v_{2}, \cdots, v_{n}$,构成$n$阶正交矩阵$V$。

$V=\left[\begin{array}{llll}v_{1} & v_{2} & \cdots & v_{n}\end{array}\right]$

3. 求$m \times n$对角矩阵$\sum$

计算$A$的奇异值：

$\sigma_{i}=\sqrt{\lambda_{i}}, \quad i=1,2, \cdots, n$

构造$m \times n$矩形对角矩阵$\Sigma$，主对角线元素是奇异值，其余元素是零：

$\Sigma=\operatorname{diag}\left(\sigma_{1}, \sigma_{2}, \cdots, \sigma_{n}\right)$

4. 求$m$阶正交矩阵$U$
对$A$的前$r$个正奇异值，令

$u_{j}=\frac{1}{\sigma_{j}} A v_{j}, \quad j=1,2, \cdots, r$

得到：

$U_{1}=\left[\begin{array}{llll}u_{1} & u_{2} & \cdots & u_{r}\end{array}\right]$

求$A^{\mathrm{T}}$的零空间的一组标准正交基$\left\{u_{r+1}, u_{r+2}, \cdots, u_{m}\right\}$，令

$U_{2}=\left[\begin{array}{llll}u_{r+1} & u_{r+2} & \cdots & u_{m}\end{array}\right]$

并令：

$U=\left[\begin{array}{ll}U_{1} & U_{2}\end{array}\right]$

5. 得到奇异值分解：

$A=U \Sigma V^{\mathrm{T}}$

## 奇异值分解与矩阵近似
### 弗罗贝尼乌斯范数
奇异值分解也是一-种矩阵近似的方法，这个近似是在弗罗贝尼乌斯范数（Frobenius norm）意义下的近似。矩阵的弗罗贝尼乌斯范数是向量的$L_{2}$范数的直接推广，对应着机器学习中的平方损失函数。

弗罗贝尼乌斯范数：

设矩阵$A \in \mathbf{R}^{m \times n}, A=\left[a_{i j}\right]_{m \times n}$，定义矩阵$A$的弗罗贝尼乌斯范数为：

$\|A\|_{F}=\left(\sum_{i=1}^{m} \sum_{j=1}^{n}\left(a_{i j}\right)^{2}\right)^{\frac{1}{2}}$

引理：

设矩阵$A \in \mathbf{R}^{m \times n}$，$A$的奇异值分解为$U \Sigma V^{\mathrm{T}}$，其中$\Sigma=\operatorname{diag}\left(\sigma_{1}\right.$$\left.\sigma_{2}, \cdots, \sigma_{n}\right)$，则

$\|A\|_{F}=\left(\sigma_{1}^{2}+\sigma_{2}^{2}+\cdots+\sigma_{n}^{2}\right)^{\frac{1}{2}}$

### 矩阵的最优近似
奇异值分解是在平方损失（弗罗贝尼乌斯范数）意义下对矩阵的最优近似，即数据压缩。

设矩阵$A \in \mathbf{R}^{m \times n}$，矩阵的秩$\operatorname{rank}(A)=r$，并设$\mathcal{M}$为$\mathbf{R}^{m \times n}$中所有秩不超过$k$的矩阵集合，$0<k<r$，则存在一个秩为$k$的矩阵$X \in \mathcal{M}$，使得：

$\|A-X\|_{F}=\min _{S \in \mathcal{M}}\|A-S\|_{F}$

称矩阵$X$为矩阵$A$在弗罗贝尼乌斯范数意义下的最优近似。

设矩阵$A \in \mathbf{R}^{m \times n}$，矩阵的秩$\operatorname{rank}(A)=r$，有奇异值分解$A=$$U \Sigma V^{\mathrm{T}}$，并设$\mathcal{M}$为$\mathbf{R}^{m \times n}$中所有秩不超过$k$的矩阵的集合，$0<k<r$，若秩为$k$的矩阵$X \in \mathcal{M}$满足：

$\|A-X\|_{F}=\min _{S \in \mathcal{M}}\|A-S\|_{F}$

则：

$\|A-X\|_{F}=\left(\sigma_{k+1}^{2}+\sigma_{k+2}^{2}+\cdots+\sigma_{n}^{2}\right)^{\frac{1}{2}}$

特别地，若$A^{\prime}=U \Sigma^{\prime} V^{\mathrm{T}}$，其中：

$\Sigma^{\prime}=\left[\begin{array}{cccccc}\sigma_{1} & & & & \\ & \ddots & & & 0 \\ & & \sigma_{k} & & & \\ & & & 0 & & \\ & 0 & & & \ddots & \\ & & & & & 0\end{array}\right]=\left[\begin{array}{cc}\Sigma_{k} & 0 \\ 0 & 0\end{array}\right]$

则

$\left\|A-A^{\prime}\right\|_{F}=\left(\sigma_{k+1}^{2}+\sigma_{k+2}^{2}+\cdots+\sigma_{n}^{2}\right)^{\frac{1}{2}}=\min _{S \in \mathcal{M}}\|A-S\|_{F}$

在秩不超过$k$的$m \times n$矩阵的集合中，存在矩阵$A$的弗罗贝尼乌斯范数意义下的最优近似矩阵$X$。$A^{\prime}=U \Sigma^{\prime} V^{\mathrm{T}}$是达到最优值的一一个矩阵。

前面定义了矩阵的紧奇异值分解与截断奇异值分解。事实上紧奇异值分解是在弗罗贝尼乌斯范数意义下的无损压缩，截断奇异值分解是有损压缩。截断奇异值分解得到的矩阵的秩为$k$，通常远小于原始矩阵的秩$r$，所以是由低秩矩阵实现了对原始矩阵的压缩。

### 矩阵的外积展开式
下面介绍利用外积展开式对矩阵$A$的近似。矩阵$A$的奇异值分解$U \Sigma V^{\mathrm{T}}$也可以由外积形式表示。事实上，若将$A$的奇异值分解看成矩阵$U \Sigma$和$V^{\mathrm{T}}$的乘积，将$U \Sigma$按列向量分块，将$V^{\mathrm{T}}$按行向量分块，即得

$U \Sigma=\left[\begin{array}{llll}\sigma_{1} u_{1} & \sigma_{2} u_{2} & \cdots & \sigma_{n} u_{n}\end{array}\right]$
$V^{\mathrm{T}}=\left[\begin{array}{c}v_{1}^{\mathrm{T}} \\ v_{2}^{\mathrm{T}} \\ \vdots \\ v_{n}^{\mathrm{T}}\end{array}\right]$
$A=\sigma_{1} u_{1} v_{1}^{\mathrm{T}}+\sigma_{2} u_{2} v_{2}^{\mathrm{T}}+\cdots+\sigma_{n} u_{n} v_{n}^{\mathrm{T}}$

称为矩阵$A$的外积展开式，其中$u_{k} v_{k}^{\mathrm{T}}$为$m \times n$矩阵，是列向量$u_{k}$和行向量$v_{k}^{\mathrm{T}}$的外积，其第$i$行第$j$列元素为$u_{k}$的第$i$个元素与$v_{k}^{\mathrm{T}}$的第$j$个元素的乘积。即

$u_{i} v_{j}^{\mathrm{T}}=\left[\begin{array}{c}u_{1 i} \\ u_{2 i} \\ \vdots \\ u_{m i}\end{array}\right]\left[\begin{array}{cccc}v_{1 j} & v_{2 j} & \cdots & v_{n j}\end{array}\right]=\left[\begin{array}{cccc}u_{1 i} v_{1 j} & u_{1 i} v_{2 j} & \cdots & u_{1 i} v_{n j} \\ u_{2 i} v_{1 j} & u_{2 i} v_{2 j} & \cdots & u_{2 i} v_{n j} \\ \vdots & \vdots & & \vdots \\ u_{m i} v_{1 j} & u_{m i} v_{2 j} & \cdots & u_{m i} v_{n j}\end{array}\right]$

$A$的外积展开式也可以写成下面的形式

$A=\sum_{k=1}^{n} A_{k}=\sum_{k=1}^{n} \sigma_{k} u_{k} v_{k}^{\mathrm{T}}$

其中$A_{k}=\sigma_{k} u_{k} v_{k}^{\mathrm{T}}$是$m \times n$矩阵。

由矩阵$A$的外积展开式知，若$A$的秩为$n$，则

$A=\sigma_{1} u_{1} v_{1}^{\mathrm{T}}+\sigma_{2} u_{2} v_{2}^{\mathrm{T}}+\cdots+\sigma_{n} u_{n} v_{n}^{\mathrm{T}}$

一般地，设矩阵

$A_{k}=\sigma_{1} u_{1} v_{1}^{\mathrm{T}}+\sigma_{2} u_{2} v_{2}^{\mathrm{T}}+\cdots+\sigma_{k} u_{k} v_{k}^{\mathrm{T}}$

则$A_{k}$的秩为$k$，并且$A_{k}$是秩为$k$的矩阵中在弗罗贝尼乌斯范数意义下$A$的最优近似矩阵。矩阵$A_{k}$就是$A$的截断奇异值分解。

由于通常奇异值$\sigma_{i}$递减很快，所以$k$取很小值时，$A_{k}$也可以对$A$有很好的近似。
# 主成分分析(《统计学习方法》)
主成分分析（principal component analysis, PCA）是一种常用的无监督学习方法，这一方法利用正交变换把由线性相关变量表示的观测数据转换为少数几个由线性无关变量表示的数据，线性无关的变量称为主成分。主成分的个数通常小于原始变量的个数，所以主成分分析属于降维方法。主成分分析主要用于发现数据中的基本结构，即数据中变量之间的关系，是数据分析的有力工具，也用于其他机器学习方法的前处理。

## 总体主成分分析
### 基本想法
统计分析中，数据的变量之间可能存在相关性，以致增加了分析的难度。于是，考虑由少数不相关的变量来代替相关的变量，用来表示数据，并且要求能够保留数据中的大部分信息。

主成分分析中，首先对给定数据进行规范化，使得数据每一变量的平均值为 0, 方差为 1。之后对数据进行正交变换，原来由线性相关变量表示的数据，通过正交变换变成由若千个线性无关的新变量表示的数据。新变量是可能的正交变换中变量的方差的和（信息保存）最大的，方差表示在新变量上信息的大小。将新变量依次称为第一主成分、第二主成分等。这就是主成分分析的基本思想。通过主成分分析，可以利用主成分近似地表示原始数据，这可理解为发现数据的“基本结构”；也可以把数据由少数主成分表示，这可理解为对数据降维。

下面给出主成分分析的直观解释。数据集合中的样本由实数空间（正交坐标系）中的点表示，空间的一个坐标轴表示一个变量，规范化处理后得到的数据分布在原点附近。对原坐标系中的数据进行主成分分析等价于进行坐标系旋转变换，将数据投影到新坐标系的坐标轴上；新坐标系的第一坐标轴、第二坐标轴等分别表示第一主成分、第二主成分等，数据在每- -轴上的坐标值的平方表示相应变量的方差；并且，这个坐标系是在所有可能的新的坐标系中，坐标轴上的方差的和最大的。

例如，数据由两个变量$x_{1}$和$x_{2}$表示，存在于二维空间中，每个点表示一个样本，如下图所示。对数据已做规范化处理，可以看出，这些数据分布在以原点为中心的左下至右上倾斜的椭圆之内。很明显在这个数据中的变量$x_{1}$和$x_{2}$是线性相关的，具体地，当知道其中一个变量$x_{1}$的取值时，对另一个变量$x_{2}$的预测不是完全随机的，反之亦然。

主成分分析对数据进行正交变换，具体地，对原坐标系进行旋转变换，并将数据在新坐标系表示，如图所示。数据在原坐标系由变量$x_{1}$ 和$x_{2}$表示，通过正交变换后，在新坐标系里，由变量$y_{1}$和$y_{2}$表示。主成分分析选择方差最大的方向（第一主成分）作为新坐标系的第一坐标轴，即$y_{1}$ 轴，在这里意味着选择椭圆的长轴作为新坐标系的第一坐标轴；之后选择与第一坐标轴正交，且方差次之的方向（第二主成分）作为新坐标系的第二坐标轴，即 $y_{2}$轴，在这里意味着选择椭圆的短轴作为新坐标系的第二坐标轴。在新坐标系里，数据中的变量$y_{1}$和$y_{2}$ 是线性无关的，当知道其中一个变量 $y_{1}$的取值时，对另一个变量$y_{2}$的预测是完全随机的；反之亦然。如果主成分分析只取第一主成分，即新坐标系的$y_{1}$轴，那么等价于将数据投影在椭圆长轴上，用这个主轴表示数据，将二维空间的数据压缩到一维空间中。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200418235554.png)

下面再看方差最大的解释。假设有两个变量$x_{1}$和$x_{2}$,三个样本点$A、B、C$,样本分布在由$x_{2}$和$x_{2}$轴组成的坐标系中，如图所示。对坐标系进行旋转变换，得到新的坐标轴$y_{1}$,表示新的变量$y_{1}$, 样本点$A, B, C$在$y_{1}$轴上投影，得到$y_{1}$轴的坐标值,$A^{\prime}, B^{\prime}, C^{\prime}$。坐标值的平方和$O A^{\prime 2}+O B^{\prime 2}+O C^{\prime 2}$表示样本在变量$y_{1}$比的方差和。主成分分析旨在选取正交变换中方差最大的变量，作为第一主成分，也就是旋转变换中坐标值的平方和最大的轴。注意到旋转变换中样本点到原点的距离的平方和$O A^{2}+O B^{2}+O C^{2}$保持不变，根据勾股定理，坐标值的平方和$O A^{\prime 2}+O B^{\prime 2}+O C^{\prime 2}$最大等价于样本点到$y_{1}$轴的距离的平方和$A A^{\prime 2}+B B^{\prime 2}+C C^{\prime 2}$最小，所以，等价地，主成分分析在旋转变换中选取离样本点的距离平方和最小的轴，作为第一主成分。第二主成分等的选取，在保证与已选坐标轴正交的条件下，类似地进行。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200419001304.png)

在数据总体（population）。上进行的主成分分析称为总体主成分分析，在有限样本进行的主成分分析称为样本主成分分析，前者是后者的基础。以下分别予以介绍。
### 定义和导出
假设 $x=\left(x_{1}, x_{2}, \dots, x_{m}\right)^{\mathrm{T}}$是$m$维随机变量，其均值向量是$\mu$:

$\boldsymbol{\mu}=E(\boldsymbol{x})=\left(\mu_{1}, \mu_{2}, \cdots, \mu_{m}\right)^{\mathrm{T}}$

协方差矩阵是$\sum$：

$\Sigma=\operatorname{cov}(\boldsymbol{x}, \boldsymbol{x})=E\left[(\boldsymbol{x}-\boldsymbol{\mu})(\boldsymbol{x}-\boldsymbol{\mu})^{\mathrm{T}}\right]$

考虑由$m$维随机变量$x$到$m$维随机变量$y=\left(y_{1}, y_{2}, \cdots, y_{m}\right)^{\mathrm{T}}$的线性变换：

$y_{i}=\alpha_{i}^{\mathrm{T}} \boldsymbol{x}=\alpha_{1 i} x_{1}+\alpha_{2 i} x_{2}+\cdots+\alpha_{m i} x_{m}$

由随机变量的性质可知，

$E\left(y_{i}\right)=\alpha_{i}^{\mathrm{T}} \mu, \quad i=1,2, \cdots, m$
$\operatorname{var}\left(y_{i}\right)=\alpha_{i}^{\mathrm{T}} \Sigma \alpha_{i}, \quad i=1,2, \cdots, m$
$\operatorname{cov}\left(y_{i}, y_{j}\right)=\alpha_{i}^{\mathrm{T}} \Sigma \alpha_{j}, \quad i=1,2, \cdots, m ; \quad j=1,2, \cdots, m$

下面给出总体主成分的定义。

给定一个如式$y_{i}=\alpha_{i}^{\mathrm{T}} \boldsymbol{x}=\alpha_{1 i} x_{1}+\alpha_{2 i} x_{2}+\cdots+\alpha_{m i} x_{m}$所示的线性变换，如果它们满足下列条件：

1. 系数向量$\alpha_{i}^{\mathrm{T}}$是单位向量，即$\alpha_{i}^{\mathrm{T}} \alpha_{i}=1, i=1,2, \cdots, m$
2. 变量$y_{i}$与$y_{j}$互不相关，即$\operatorname{cov}\left(y_{i}, y_{j}\right)=0(i \neq j)$
3. 变量$y_{1}$是$x$的所有线性变换中方差最大的；$y_{2}$是与$y_{1}$不相关的$x$的所有线性变换中方差最大的；一般地，$y_{i}$是与$y_{1}, y_{2}, \cdots, y_{i-1}(i=1,2, \cdots, m)$都不相关的$x$的所有线性变换中方差最大的；这时分别称$y_{1}, y_{2}, \cdots, y_{m}$为$x$的第一主成分、第二主成分、。... 第$m$主成分。

定义中的条件(1) 表明线性变换是正交变换，$\alpha_{1}, \alpha_{2}, \cdots, \alpha_{m}$是其一组标准正交基，

$\alpha_{i}^{\mathrm{T}} \alpha_{j}=\left\{\begin{array}{ll}1, & i=j \\ 0, & i \neq j\end{array}\right.$

条件(2)  (3) 给出了一个求主成分的方法：第一步，在$\boldsymbol{x}$的所有线性变换

$\alpha_{1}^{\mathrm{T}} \boldsymbol{x}=\sum_{i=1}^{m} \alpha_{i 1} x_{i}$

中，在$\alpha_{1}^{\mathrm{T}} \alpha_{1}=1$条件下，求方差最大的，得到$\boldsymbol{x}$的第一主成分；第二步，在与$\alpha_{1}^{\mathrm{T}} \boldsymbol{x}$不相关的$x$的所有线性变换

$\alpha_{2}^{\mathrm{T}} \boldsymbol{x}=\sum_{i=1}^{m} \alpha_{i 2} x_{i}$

中，在$\alpha_{2}^{\mathrm{T}} \alpha_{2}=1$条件下，求方差最大的，得到$x$的第二主成分；第$k$步，在与$\alpha_{1}^{\mathrm{T}} \boldsymbol{x}, \alpha_{2}^{\mathrm{T}} \boldsymbol{x}, \cdots, \alpha_{k-1}^{\mathrm{T}} \boldsymbol{x}$不相关的$\boldsymbol{x}$的所有线性变换

$\alpha_{k}^{\mathrm{T}} \boldsymbol{x}=\sum_{i=1}^{m} \alpha_{i k} x_{i}$

中，在$\alpha_{k}^{\mathrm{T}} \alpha_{k}=1$条件下，求方差最大的，得到$\mathcal{L}$的第$k$主成分；如此继续下去，直到得到$x$的第$m$主成分。

### 主要性质
设$x$是$m$维随机变量，$\sum$是$x$的协方差矩阵，$\sum$的特征值分别是$\lambda_{1} \geqslant \lambda_{2} \geqslant \cdots \geqslant \lambda_{m} \geqslant 0$,特征值对应的单位特征向量分别是$\alpha_{1}, \alpha_{2}, \cdots, \alpha_{m}$，则$x$的第$k$主成分是:

$y_{k}=\alpha_{k}^{\mathrm{T}} \boldsymbol{x}=\alpha_{1 k} x_{1}+\alpha_{2 k} x_{2}+\cdots+\alpha_{m k} x_{m}, \quad k=1,2, \cdots, m$

$x$的第$k$主成分的方差是:

$\operatorname{var}\left(y_{k}\right)=\alpha_{k}^{\mathrm{T}} \Sigma \alpha_{k}=\lambda_{k}, \quad k=1,2, \cdots, m$

即协方差矩阵$\sum$的第$k$个特征值。

若特征值有重根，对应的特征向量组成$m$维空间$\mathbf{R}^{m}$的一个子空间，子空间的维数等于重根数，在子空间任取一-个正交坐标系，这个坐标系的单位向量就可作为特征向量。这时坐标系的取法不唯一。

$m$维随机变量$y=\left(y_{1}, y_{2}, \cdots, y_{m}\right)^{\mathrm{T}}$的分量依次是$\boldsymbol{x}$的第一主成分到第$m$主成分的充要条件是：

1. $\boldsymbol{y}=A^{\mathrm{T}} \boldsymbol{x}$,$A$为正交矩阵

$A=\left[\begin{array}{cccc}\alpha_{11} & \alpha_{12} & \cdots & \alpha_{1 m} \\ \alpha_{21} & \alpha_{22} & \cdots & \alpha_{2 m} \\ \vdots & \vdots & & \vdots \\ \alpha_{m 1} & \alpha_{m 2} & \cdots & \alpha_{m m}\end{array}\right]$

2. $\boldsymbol{y}$的协方差矩阵为对角矩阵

$\operatorname{cov}(\boldsymbol{y})=\operatorname{diag}\left(\lambda_{1}, \lambda_{2}, \cdots, \lambda_{m}\right)$
$\lambda_{1} \geqslant \lambda_{2} \geqslant \cdots \geqslant \lambda_{m}$

其中$\lambda_{k}$是$\sum$的第$k$个特征值，$\alpha_{k}$是对应的单位特征向量，$k=1,2, \cdots, m$

下面叙述总体主成分的性质：

1. 总体主成分$y$的协方差矩阵是对角矩阵

$\operatorname{cov}(\boldsymbol{y})=\Lambda=\operatorname{diag}\left(\lambda_{1}, \lambda_{2}, \cdots, \lambda_{m}\right)$

2. 总体主成分$\boldsymbol{y}$的方差之和等于随机变量$\boldsymbol{x}$的方差之和，即

$\sum_{i=1}^{m} \lambda_{i}=\sum_{i=1}^{m} \sigma_{i i}$

其中$\sigma_{i i}$是随机变量$\mathcal{L}_{i}$的方差，即协方差矩阵$\sum$的对角元素。

3. 第$k$个主成分$y_{k}$与变量$\boldsymbol{x}_{\boldsymbol{i}}$的相关系数$\rho\left(y_{k}, x_{i}\right)$称为因子负荷量（factor loading），它表示第$k$个主成分$y_{k}$与变量$\mathcal{X} i$的相关关系。计算公式是

$\rho\left(y_{k}, x_{i}\right)=\frac{\sqrt{\lambda_{k}} \alpha_{i k}}{\sqrt{\sigma_{i i}}}, \quad k, i=1,2, \cdots, m$

4. 第$k$个主成分$y_{k}$与$m$个变量的因子负荷量满足

$\sum_{i=1}^{m} \sigma_{i i} \rho^{2}\left(y_{k}, x_{i}\right)=\lambda_{k}$

5. $m$个主成分与第$i$个变量$i$的因子负荷量满足

$\sum_{k=1}^{m} \rho^{2}\left(y_{k}, x_{i}\right)=1$

### 主成分的个数
主成分分析的主要目的是降维，所以一般选择$k(k \ll m)$个主成分（线性无关变量）来代替$m$个原有变量（线性相关变量），使问题得以简化，并能保留原有变量的大部分信息。这里所说的信息是指原有变量的方差。为此，先给出一个定理，说明选择$k$个主成分是最优选择。

对任意正整数$q$，$1 \leqslant q \leqslant m$，考虑正交线性变换

$\boldsymbol{y}=B^{\mathrm{T}} \boldsymbol{x}$

其中$y$是$q$维向量,$B^{\mathrm{T}}$是$q \times m$矩阵，令$\boldsymbol{y}$的协方差矩阵为:

$\Sigma_{y}=B^{\mathrm{T}} \Sigma B$

则$\Sigma_{y}$的迹$\operatorname{tr}\left(\Sigma_{\boldsymbol{y}}\right)$在$B=A_{q}$时取得最大值，其中矩阵$A_{q}$由正交矩阵$A$的前$q$列组成。

定理表明，当$x$的线性变换$y$在$B=A_{q}$时，其协方差矩阵$\Sigma_{\boldsymbol{y}}$的迹$\operatorname{tr}\left(\Sigma_{y}\right)$取得最大值，这就是说，当取$A$的前$q$列取$x$的前$q$个主成分时，能够最大限度地保留原有变量方差的信息。

考虑正交变换

$\boldsymbol{y}=B^{\mathrm{T}} \boldsymbol{x}$

这里$B^{\mathrm{T}}$是$p \times m$矩阵，$A$和$\Sigma_{y}$的定义与前相同，则$\operatorname{tr}\left(\Sigma_{y}\right)$在$B=A_{p}$时取得最小值，其中矩阵$A_{p}$由$A$的后$p$列组成。

该定理可以理解为，当舍弃$A$的后$p$列，即舍弃变量$x$的后$p$个主成分时，原有变量的方差的信息损失最少。

以上两个定理可以作为选择$k$个主成分的理论依据。具体选择$k$的方法，通常利用方差贡献率。

第$k$主成分$y_{k}$的方差贡献率定义为$y_{k}$的方差与所有方差之和的比，记作$\eta_{k}$:

$\eta_{k}=\frac{\lambda_{k}}{\sum_{i=1}^{m} \lambda_{i}}$

$k$个主成分$y_{1}, y_{2}, \cdots, y_{k}$的累计方差贡献率定义为$k$个方差之和与所有方差之和的比:

$\sum_{i=1}^{k} \eta_{i}=\frac{\sum_{i=1}^{k} \lambda_{i}}{\sum_{i=1}^{m} \lambda_{i}}$

通常取$k$使得累计方差贡献率达到规定的百分比以上，例如 70%~80%以上。累计方差贡献率反映了主成分保留信息的比例，但它不能反映对某个原有变量$\boldsymbol{x}_{i}$保留信息的比例，这时通常利用$k$个主成分$y_{1}, y_{2}, \cdots, y_{k}$对原有变量$\boldsymbol{x}_{i}$的贡献率。

### 规范化变量的总体主成分
在实际问题中，不同变量可能有不同的量纲，直接求主成分有时会产生不合理的结果。为了消除这个影响，常常对各个随机变量实施规范化，使其均值为 0, 方差为 1。

设$\boldsymbol{x}=\left(x_{1}, x_{2}, \cdots, x_{m}\right)^{\mathrm{T}}$为$m$维随机变量，$\boldsymbol{x}_{i}$为第$i$个随机变量，$i=$$1,2, \cdots, m$，令

$x_{i}^{*}=\frac{x_{i}-E\left(x_{i}\right)}{\sqrt{\operatorname{var}\left(x_{i}\right)}}, \quad i=1,2, \cdots, m$，令

$x_{i}^{*}=\frac{x_{i}-E\left(x_{i}\right)}{\sqrt{\operatorname{var}\left(x_{i}\right)}}, \quad i=1,2, \cdots, m$

其中$E\left(x_{i}\right), \operatorname{var}\left(x_{i}\right)$分别是随机变量$\boldsymbol{x}_{i}$的均值和方差，这时$\boldsymbol{x}_{i}^{*}$就是$\boldsymbol{x}_{i}$的规范化随机变量。

显然，规范化随机变量的协方差矩阵就是相关矩阵$R$。主成分分析通常在规范化随机变量的协方差矩阵即相关矩阵上进行。

对照总体主成分的性质可知，规范化随机变量的总体主成分有以下性质：

1. 规范化变量主成分的协方差矩阵是

$\Lambda^{*}=\operatorname{diag}\left(\lambda_{1}^{*}, \lambda_{2}^{*}, \cdots, \lambda_{m}^{*}\right)$

其中$\lambda_{1}^{*} \geqslant \lambda_{2}^{*} \geqslant \cdots \geqslant \lambda_{m}^{*} \geqslant 0$是相关矩阵$R$的特征值。

2. 协方差矩阵的特征值之和$m$

$\sum_{k=1}^{m} \lambda_{k}^{*}=m$

3. 规范化随机变量$\mathcal{L}_{i}^{*}$与主成分$y_{k}^{*}$的相关系数的平方和等于$\lambda_{k}^{*}$:

$\sum_{i=1}^{m} \rho^{2}\left(y_{k}^{*}, x_{i}^{*}\right)=\sum_{i=1}^{m} \lambda_{k}^{*} e_{i k}^{* 2}=\lambda_{k}^{*}, \quad k=1,2, \cdots, m$

4. 规范化随机变量$x_{i}^{*}$与所有主成分$y_{k}^{*}$的相关系数的平方和等于 1

$\sum_{k=1}^{m} \rho^{2}\left(y_{k}^{*}, x_{i}^{*}\right)=\sum_{k=1}^{m} \lambda_{k}^{*} e_{i k}^{* 2}=1, \quad i=1,2, \cdots, m$

## 样本主成分分析
在实际问题中，需要在观测数据上进行主成分分析，这就是样本主成分分析。有了总体主成分的概念，容易理解样本主成分的概念。样本主成分也和总体主成分具有相同的性质。

### 样本主成分的定义和性质

假设对$m$维随机变量$\boldsymbol{x}=\left(x_{1}, x_{2}, \cdots, x_{m}\right)^{\mathrm{T}}$进行$n$次独立观测,$\boldsymbol{x}_{1}, \boldsymbol{x}_{2}, \cdots, \boldsymbol{x}_{n}$表示观测样本，其中：$\boldsymbol{x}_{j}=\left(x_{1 j}, x_{2 j}, \cdots, x_{m j}\right)^{\mathrm{T}}$表示第$j$个观测样本，$\boldsymbol{x}_{i j}$表示第$j$个观测样本的第$i$个变量，$j=1,2, \cdots, n$。观测数据用样本矩阵$X$表示，记作

$X=\left[\begin{array}{llll}x_{1} & x_{2} & \cdots & x_{n}\end{array}\right]=\left[\begin{array}{cccc}x_{11} & x_{12} & \cdots & x_{1 n} \\ x_{21} & x_{22} & \cdots & x_{2 n} \\ \vdots & \vdots & & \vdots \\ x_{m 1} & x_{m 2} & \cdots & x_{m n}\end{array}\right]$

给定样本矩阵$X$，可以估计样本均值，以及样本协方差。样本均值向量$\bar{x}$为:

$\bar{x}=\frac{1}{n} \sum_{j=1}^{n} \boldsymbol{x}_{j}$

样本协方差矩阵$S$为:

$S=\left[s_{i j}\right]_{m \times m}$
$s_{i j}=\frac{1}{n-1} \sum_{k=1}^{n}\left(x_{i k}-\bar{x}_{i}\right)\left(x_{j k}-\bar{x}_{j}\right), \quad i, j=1,2, \cdots, m$

其中$\bar{x}_{i}=\frac{1}{n} \sum_{k=1}^{n} x_{i k}$为第$i$个变量的样本均值，$\bar{x}_{j}=\frac{1}{n} \sum_{k=1}^{n} x_{j k}$为第$j$个变量的样本均值。

样本相关矩阵$R$为：

$R=\left[r_{i j}\right]_{m \times m}, \quad r_{i j}=\frac{s_{i j}}{\sqrt{s_{i i} s_{j j}}}, \quad i, j=1,2, \cdots, m$

定义$m$维向量$\boldsymbol{x}=\left(x_{1}, x_{2}, \cdots, x_{m}\right)^{\mathrm{T}}$到$m$维向量$y=\left(y_{1}, y_{2}, \ldots, y_{n}\right)^{T}$的线性变换:

$\boldsymbol{y}=A^{\mathrm{T}} \boldsymbol{x}$

其中

$\begin{array}{l}A=\left[\begin{array}{llllll}a_{1} & a_{2} & \cdots & a_{m}\end{array}\right]=\left[\begin{array}{cccc}a_{11} & a_{12} & \cdots & a_{1 m} \\ a_{21} & a_{22} & \cdots & a_{2 m} \\ \vdots & \vdots & & \vdots \\ a_{m 1} & a_{m 2} & \cdots & a_{m m}\end{array}\right] \\ a_{i}=\left(a_{1 i}, a_{2 i}, \cdots, a_{m i}\right)^{\mathrm{T}}, & i=1,2, \cdots, m\end{array}$

考虑任意一个线性变换:

$\boldsymbol{y}_{i}=a_{i}^{\mathrm{T}} \boldsymbol{x}=a_{1 i} \boldsymbol{x}_{1}+a_{2 i} \boldsymbol{x}_{2}+\cdots+a_{m i} \boldsymbol{x}_{m}, \quad i=1,2, \cdots, m$

其中$y_{i}$是$m$维向量$y$的第$i$个变量，相应于容量为$n$的样本$\boldsymbol{x}_{1}, \boldsymbol{x}_{2}, \cdots, \boldsymbol{x}_{n}$,$y_{i}$的样本均值$\bar{y} i$为:

$\bar{y}_{i}=\frac{1}{n} \sum_{j=1}^{n} a_{i}^{\mathrm{T}} \boldsymbol{x}_{j}=a_{i}^{\mathrm{T}} \overline{\boldsymbol{x}}$

其中$\overline{\boldsymbol{x}}$是随机向量$\boldsymbol{x}$的样本均值:

$\overline{\boldsymbol{x}}=\frac{1}{n} \sum_{j=1}^{n} \boldsymbol{x}_{j}$

$y_{i}$的样本方差$\operatorname{var}\left(y_{i}\right)$为：

$\begin{aligned} \operatorname{var}\left(y_{i}\right) &=\frac{1}{n-1} \sum_{j=1}^{n}\left(a_{i}^{\mathrm{T}} \boldsymbol{x}_{j}-a_{i}^{\mathrm{T}} \overline{\boldsymbol{x}}\right)^{2} \\ &=a_{i}^{\mathrm{T}}\left[\frac{1}{n-1} \sum_{j=1}^{n}\left(\boldsymbol{x}_{j}-\overline{\boldsymbol{x}}\right)\left(\boldsymbol{x}_{j}-\overline{\boldsymbol{x}}\right)^{\mathrm{T}}\right] a_{i}=a_{i}^{\mathrm{T}} S a_{i} \end{aligned}$

对任意两个线性变换$y_{i}=\alpha_{i}^{\mathrm{T}} \boldsymbol{x}, y_{k}=\alpha_{k}^{\mathrm{T}} \boldsymbol{x}$，相应于容量为 $n$的样本$\boldsymbol{x}_{1}, \boldsymbol{x}_{2}, \cdots, \boldsymbol{x}_{n}$,$y_{k}$的样本协方差为

$\operatorname{cov}\left(y_{i}, y_{k}\right)=a_{i}^{\mathrm{T}} S a_{k}$

样本主成分：

给定样本矩阵$X$。样本第一主成分$y_{1}=a_{1}^{\mathrm{T}} \boldsymbol{x}$是在$a_{1}^{\mathrm{T}} a_{1}=1$是在$a_{1}^{\mathrm{T}} a_{1}=1$条件下，使得$a_{1}^{\mathrm{T}} \boldsymbol{x}_{j}(j=1,2, \cdots, n)$的样本方差$a_{1}^{\mathrm{T}} S a_{1}$最大的$\boldsymbol{x}$的线性变换；一般地，样本第$i$主成分$y_{i}=a_{i}^{\mathrm{T}} \boldsymbol{x}$是在$a_{i}^{\mathrm{T}} a_{i}=1$和$a_{i}^{\mathrm{T}} \boldsymbol{x}_{j}$与$a_{k}^{\mathrm{T}} \boldsymbol{x}_{j}(k<i, \quad j=1,2, \cdots, n)$的样本协方差$a_{k}^{\mathrm{T}} S a_{i}=0$条件下，使得$a_{i}^{\mathrm{T}} \boldsymbol{x}_{j}(j=1,2, \cdots, n)$的样本方差$a_{i}^{\mathrm{T}} S a_{i}$最大的$x$的线性变换。

样本主成分与总体主成分具有同样的性质。这从样本主成分的定义容易看出。只要以样本协方差矩阵$S$代替总体协方差矩阵$\sum$即可。总体主成分的定理对样本主成分依然成立。

在使用样本主成分时，-般假设样本数据是规范化的，即对样本矩阵作如下变换：

$x_{i j}^{*}=\frac{x_{i j}-\bar{x}_{i}}{\sqrt{s_{i i}}}, \quad i=1,2, \cdots, m ; \quad j=1,2, \cdots, n$

其中：

$\bar{x}_{i}=\frac{1}{n} \sum_{j=1}^{n} x_{i j}, \quad i=1,2, \cdots, m$
$s_{i i}=\frac{1}{n-1} \sum_{j=1}^{n}\left(x_{i j}-\bar{x}_{i}\right)^{2}, \quad i=1,2, \cdots, m$

为了方便，以下将规范化变量$x_{i j}^{*}$仍记作$\boldsymbol{x}_{i j}$，规范化的样本矩阵仍记作$X$。这时，样本协方差矩阵$S$就是样本相关矩阵$R$

样本协方差矩阵$S$是总体协方差矩阵$\Sigma$的无偏估计，样本相关矩阵$R$是总体相关矩阵的无偏估计$S$的特征值和特征向量是$\Sigma$的特征值和特征向量的极大似然估计。

### 相关矩阵的特征值分解算法

传统的主成分分析通过数据的协方差矩阵或相关矩阵的特征值分解进行，现在常用的方法是通过数据矩阵的奇异值分解进行。首先叙述数据的协方差矩阵或相关矩阵的特征值分解方法。

给定样本矩阵$X$，利用数据的样本协方差矩阵或者样本相关矩阵的特征值分解进行主成分分析。具体步骤如下：

1. 对观测数据按式

$x_{i j}^{*}=\frac{x_{i j}-\bar{x}_{i}}{\sqrt{s_{i i}}}, \quad i=1,2, \cdots, m ; \quad j=1,2, \cdots, n$

进行规范化处理，得到规范化数据矩阵，仍以$X$表示。
2. 依据规范化数据矩阵，计算样本相关矩阵$R$：

$R=\left[r_{i j}\right]_{m \times m}=\frac{1}{n-1} X X^{\mathrm{T}}$

其中：

$r_{i j}=\frac{1}{n-1} \sum_{l=1}^{n} x_{i l} x_{l j}, \quad i, j=1,2, \cdots, m$

3. 求样本相关矩阵$R$的$k$个特征值和对应的$k$个单位特征向量。

求解$R$的特征方程:

$|R-\lambda I|=0$

得$R$的$m$个特征值:

$\lambda_{1} \geqslant \lambda_{2} \geqslant \cdots \geqslant \lambda_{m}$

求方差贡献率$\sum_{i=1}^{k} \eta_{i}$达到预定值的主成分个数$k$:

求前$k$个特征值对应的单位特征向量:

$a_{i}=\left(a_{1 i}, a_{2 i}, \cdots, a_{m i}\right)^{\mathrm{T}}, \quad i=1,2, \cdots, k$

4. 求$k$个样本主成分

以$k$个单位特征向量为系数进行线性变换，求出$k$个样本主成分:

$y_{i}=a_{i}^{\mathrm{T}} \boldsymbol{x}, \quad i=1,2, \cdots, k$

5. 计算$k$个主成分$y_{j}$与原变量$\boldsymbol{x}_{i}$的相关系数$\rho\left(x_{i}, y_{j}\right)$，以及$k$个主成分对原变量$\boldsymbol{x}_{i}$的贡献率$\mathcal{v}_{i}$。

6. 计算$n$个样本的$k$个主成分值:

将规范化样本数据代入$k$个主成分式$y_{i}=a_{i}^{\mathrm{T}} \boldsymbol{x}, \quad i=1,2, \cdots, k$,得到$n$个样本的主成分值。
第$j$个样本$\boldsymbol{x}_{j}=\left(x_{1 j}, x_{2 j}, \cdots, x_{m j}\right)^{\mathrm{T}}$的第$i$主成分值是:

$y_{i j}=\left(a_{1 i}, a_{2 i}, \cdots, a_{m i}\right)\left(x_{1 j}, x_{2 j}, \cdots, x_{m j}\right)^{\mathrm{T}}=\sum_{l=1}^{m} a_{l i} x_{l j}$
$i=1,2, \cdots, m, \quad j=1,2, \cdots, n$

主成分分析得到的结果可以用于其他机器学习方法的输入。比如，将样本点投影到以主成分为坐标轴的空间中，然后应用聚类算法，就可以对样本点进行聚类。

### 数据矩阵的奇异值分解算法
给定样本矩阵$X$，利用数据矩阵奇异值分解进行主成分分析。具体过程如下。这里假设有$k$个主成分。

对于$mxn$实矩阵$A$，假设其秩为$r$,$0<k<r$，则可以将矩阵解$A$进行截断奇异值分解

$A \approx U_{k} \Sigma_{k} V_{k}^{\mathrm{T}}$

式中$U_{k}$是$m \times k$矩阵，$V_{k}$是$n \times k$矩阵，$\sum_{k}$是$k$阶对角矩阵;$U_{k}, V_{k}$分别由取$A$的完全奇异值分解的矩阵$U, V$的前$k$列，$\sum_{k}$由取$A$的完全奇异值分解的矩阵$\sum$的前$k$个对角线元素得到。

定义一个新的$n \times m$矩阵$X^{\prime}$：

$X^{\prime}=\frac{1}{\sqrt{n-1}} X^{\mathrm{T}}$

$X^{\prime}$的每一列均值为零。不难得知，

$\begin{aligned} X^{\prime \mathrm{T}} X^{\prime} &=\left(\frac{1}{\sqrt{n-1}} X^{\mathrm{T}}\right)^{\mathrm{T}}\left(\frac{1}{\sqrt{n-1}} X^{\mathrm{T}}\right) \\ &=\frac{1}{n-1} X X^{\mathrm{T}} \end{aligned}$

即$X^{\prime \mathrm{T}} X^{\prime}$等于$X$的协方差矩阵$S_{X}$。

主成分分析归结于求协方差矩阵$S_{X}$的特征值和对应的单位特征向量，所以问题转化为求矩阵$=X^{\prime} \mathbf{T} X^{\prime}$的特征值和对应的单位特征向量。

假设$X^{\prime}$的截断奇异值分解为$X^{\prime}=U \Sigma V^{\mathrm{T}}$，那么$V$的列向量就是$S_{X}=X^{\prime} \mathbf{T}_{X^{\prime}}$的单位特征向量。因此，$V$的列向量就是$X$的主成分。于是，求$X$主成分可以通过求$X^{\prime}$的奇异值分解来实现。具体算法如下。
主成分分析算法：

输入：$mxn$样本矩阵$X$，其每一-行元素的均值为零；
输出：$kxn$样本主成分矩阵$Y$。
参数：主成分个数$k$

1. 构造新的$n \times m$矩阵：

$X^{\prime}=\frac{1}{\sqrt{n-1}} X^{\mathrm{T}}$

$X^{\prime}$每一列的均值为零。

2. 对矩阵$X^{\prime}$进行截断奇异值分解，得到：

$X^{\prime}=U \Sigma V^{\mathrm{T}}$

有$k$个奇异值、奇异向量。矩阵$V$的前$k$列构成$k$个样本主成分。

3. 求$kxn$样本主成分矩阵

$Y=V^{\mathrm{T}} X$

# 主成分分析(《机器学习》)
主成分分析（Principal Component Analysis，简称 PCA）是最常用的一种降维方法。在介绍 PCA 之前，不妨先考虑这样-一个问题：对于正交属性空间中的样本点，如何用一个超平面（直线的高维推广）对所有样本进行恰当的表达？

容易想到，若存在这样的超平面，那么它大概应具有这样的性质：
1. 最近重构性：样本点到这个超平面的距离都足够近；
2. 最大可分性：样本点在这个超平面上的投影能尽可能分开。

有趣的是，基于最近重构性和最大可分性，能分别得到主成分分析的两种等价推导。我们先从最近重构性来推导。

假定数据样本进行了中心化，即$\sum_{i} \boldsymbol{x}_{i}=\mathbf{0}$; 再假定投影变换后得到的新坐标系为$\left\{\boldsymbol{w}_{1}, \boldsymbol{w}_{2}, \dots, \boldsymbol{w}_{d}\right\}$，其中$\boldsymbol{U}_{i}$是标准正交基向量，$\left\|\boldsymbol{w}_{i}\right\|_{2}=1$,$\boldsymbol{w}_{i}^{\mathrm{T}} \boldsymbol{w}_{j}=0$。若丢弃新坐标系中的部分坐标，即将维度降低到$d^{\prime}<d$，则样本点$\boldsymbol{x}_{i}$在低维坐标系中的投影是$\boldsymbol{z}_{i}=\left(z_{i 1} ; z_{i 2} ; \ldots ; z_{i d^{\prime}}\right)$，其中$z_{i j}=\boldsymbol{w}_{j}^{\mathrm{T}} \boldsymbol{x}_{i}$是$\boldsymbol{x}_{i}$在低维坐标系下第$j$维的坐标。若基于$z_{i}$来重构 $\boldsymbol{x}_{i}$，则会得到$\hat{\boldsymbol{x}}_{i}=\sum_{j=1}^{d^{\prime}} z_{i j} \boldsymbol{w}_{j}$.

考虑整个训练集，原样本点$\boldsymbol{x}_{i}$与基于投影重构的样本点$\hat{\boldsymbol{x}}_{i}$之间的距离为:

$\sum_{i=1}^{m}\left\|\sum_{j=1}^{d^{\prime}} z_{i j} \boldsymbol{w}_{j}-\boldsymbol{x}_{i}\right\|_{2}^{2}=\sum_{i=1}^{m} \boldsymbol{z}_{i}^{\mathrm{T}} \boldsymbol{z}_{i}-2 \sum_{i=1}^{m} \boldsymbol{z}_{i}^{\mathrm{T}} \mathbf{W}^{\mathrm{T}} \boldsymbol{x}_{i}+\mathrm{const}$
$\propto-\operatorname{tr}\left(\mathbf{W}^{\mathrm{T}}\left(\sum_{i=1}^{m} \boldsymbol{x}_{i} \boldsymbol{x}_{i}^{\mathrm{T}}\right) \mathbf{W}\right)$

根据最近重构性，上式应被最小化，考虑到$\boldsymbol{w}_{j}$是标准正交基，$\sum_{i} \boldsymbol{x}_{i} \boldsymbol{x}_{i}^{\mathrm{T}}$是协方差矩阵，有

$\min _{\mathbf{W}}-\operatorname{tr}\left(\mathbf{W}^{T} \mathbf{X} \mathbf{X}^{T} \mathbf{W}\right)$
s.t. $\mathbf{W}^{\mathrm{T}} \mathbf{W}=\mathbf{I}$

这就是主成分分析的优化目标。

从最大可分性出发，能得到主成分分析的另一种解释。我们知道，样本点$\boldsymbol{x}_{i}$在新空间中超平面上的投影是$\mathbf{W}^{\mathrm{T}} \boldsymbol{x}_{i}$，若所有样本点的投影能尽可能分开，则应该使投影后样本点的方差最大化，如图所示。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200420134858.png)

投影后样本点的方差是$\sum_{i} \mathbf{W}^{\mathrm{T}} \boldsymbol{x}_{i} \boldsymbol{x}_{i}^{\mathrm{T}} \mathbf{W}$，于是优化目标可写为:

$\max _{\mathbf{W}} \operatorname{tr}\left(\mathbf{W}^{\mathrm{T}} \mathbf{X} \mathbf{X}^{\mathrm{T}} \mathbf{W}\right)$
s.t. $\mathbf{W}^{\mathrm{T}} \mathbf{W}=\mathbf{I}$

使用拉格朗日乘子法可得:

$\mathbf{x} \mathbf{x}^{\mathrm{T}} \mathbf{W}=\lambda \mathbf{W}$

于是，只需对协方差矩阵$\mathbf{X X}^{\mathrm{T}}$进行特征值分解，将求得的特征值排序：$\lambda_{1} \geqslant \lambda_{2} \geqslant \ldots \geqslant \lambda_{d}$，再取前$d^{\prime}$个特征值对应的特征向量构成 $\mathbf{W}=\left(\boldsymbol{w}_{1}\right.$$\left.\boldsymbol{w}_{2}, \ldots, \boldsymbol{w}_{d^{\prime}}\right)$。这就是主成分分析的解。PCA 算法描述如图所示。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200420135225.png)

实践中常通过对$X$进行奇异值分解来代替协方差矩阵的特征值分解。

PCA也可看作是逐一选取方差最大方向，即先对协方差矩阵$\sum_{i} \boldsymbol{x}_{i} \boldsymbol{x}_{i}^{\mathrm{T}}$做特征值分解，取最大特征值对应的特征向量$\boldsymbol{w}_{1}$; 再对$\sum_{i} \boldsymbol{x}_{i} \boldsymbol{x}_{i}^{\mathrm{T}}-\lambda_{1} \boldsymbol{w}_{1} \boldsymbol{w}_{1}^{\mathrm{T}}$做特征值分解，取最大特征值对应的特征向量$w_{2}$...由$W$各分量正交及$\sum_{i=1}^{m} \boldsymbol{x}_{i} \boldsymbol{x}_{i}^{\mathrm{T}}=\sum_{j=1}^{d} \lambda_{j} \boldsymbol{w}_{j} \boldsymbol{w}_{j}^{\mathrm{T}}$可知，上述逐一选取方差最大方向的做法与直接选取最大$d^{\prime}$个特征值等价。

降维后低维空间的维数$d^{\prime}$通常是由用户事先指定，或通过在$d^{\prime}$值不同的低维空间中对$k$近邻分类器（或其他开销较小的学习器）进行交叉验证来选取较好的$d^{\prime}$值。对 PCA，还可从重构的角度设置一个重构阈值，例如 t= 95%，然后选取使下式成立的最小$d^{\prime}$值：

$\frac{\sum_{i=1}^{d^{\prime}} \lambda_{i}}{\sum_{i=1}^{d} \lambda_{i}} \geqslant t$

PCA 仅需保留$W$与样本的均值向量即可通过简单的向量减法和矩阵-向量乘法将新样本投影至低维空间中。显然，低维空间与原始高维空间必有不同，因为对应于最小的$d-d^{\prime}$个特征值的特征向量被舍弃了，这是降维导致的结果。但舍弃这部分信息往往是必要的：一方面，舍弃这部分信息之后能使样本的采样密度增大，这正是降维的重要动机；另一方面，当数据受到噪声影响时，最小的特征值所对应的特征向量往往与噪声有关，将它们舍弃能在一定程度上起到去噪的效果。

# 线性判别分析(《机器学习》)
线性判别分析（Linear Discriminant Analysis，简称 LDA）是-种经典的线性学习方法。

LDA 的思想非常朴素：给定训练样例集，设法将样例投影到一条直线上，使得同类样例的投影点尽可能接近、异类样例的投影点尽可能远离；在对新样本进行分类时，将其投影到同样的这条直线上，再根据投影点的位置来确定新样本的类别。下图给出了一个二维示意图。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200419200806.png)

给定数据集$D=\left\{\left(\boldsymbol{x}_{i}, y_{i}\right)\right\}_{i=1}^{m}, y_{i} \in\{0,1\}$，令$X_{i}, \boldsymbol{\mu}_{i}, \mathbf{\Sigma}_{i}$分别表示第$i \in\{0,1\}$类示例的集合、均值向量、协方差矩阵。若将数据投影到直线$\boldsymbol{w}$上，则两类样本的中心在直线上的投影分别为$\boldsymbol{w}^{\mathrm{T}} \boldsymbol{\mu}_{0}$和$\boldsymbol{w}^{\mathrm{T}} \boldsymbol{\mu}_{1}$，若将所有样本点都投影到直线$\boldsymbol{w}$上，则两类样本的协方差分别为$\boldsymbol{w}^{\mathrm{T}} \boldsymbol{\Sigma}_{0} \boldsymbol{w}$和$\boldsymbol{w}^{\mathrm{T}} \boldsymbol{\Sigma}_{1} \boldsymbol{w}$。由于直线是维空间，因此$\boldsymbol{w}^{\mathrm{T}} \boldsymbol{\mu}_{0}, \boldsymbol{w}^{\mathrm{T}} \boldsymbol{\mu}_{1}, \boldsymbol{w}^{\mathrm{T}} \boldsymbol{\Sigma}_{0} \boldsymbol{w}$和$\boldsymbol{w}^{\mathrm{T}} \boldsymbol{\Sigma}_{1} \boldsymbol{w}$均为实数。

欲使同类样例的投影点尽可能接近，可以让同类样例投影点的协方差尽可能小，即$\boldsymbol{w}^{\mathrm{T}} \boldsymbol{\Sigma}_{0} \boldsymbol{w}+\boldsymbol{w}^{\mathrm{T}} \boldsymbol{\Sigma}_{1} \boldsymbol{w}$尽可能小；而欲使异类样例的投影点尽可能远离，
可以让类中心之间的距离尽可能大，即$\left\|\boldsymbol{w}^{\mathrm{T}} \boldsymbol{\mu}_{0}-\boldsymbol{w}^{\mathrm{T}} \boldsymbol{\mu}_{1}\right\|_{2}^{2}$尽可能大。同时考虑二者，则可得到欲最大化的目标

$\begin{aligned} J &=\frac{\left\|\boldsymbol{w}^{\mathrm{T}} \boldsymbol{\mu}_{0}-\boldsymbol{w}^{\mathrm{T}} \boldsymbol{\mu}_{1}\right\|_{2}^{2}}{\boldsymbol{w}^{\mathrm{T}} \boldsymbol{\Sigma}_{0} \boldsymbol{w}+\boldsymbol{w}^{\mathrm{T}} \boldsymbol{\Sigma}_{1} \boldsymbol{w}} \\ &=\frac{\boldsymbol{w}^{\mathrm{T}}\left(\boldsymbol{\mu}_{0}-\boldsymbol{\mu}_{1}\right)\left(\boldsymbol{\mu}_{0}-\boldsymbol{\mu}_{1}\right)^{\mathrm{T}} \boldsymbol{w}}{\boldsymbol{w}^{\mathrm{T}}\left(\boldsymbol{\Sigma}_{0}+\boldsymbol{\Sigma}_{1}\right) \boldsymbol{w}} \end{aligned}$

定义“类内散度矩阵”(within-class scatter matrix)：

$\begin{aligned} \mathbf{S}_{w} &=\mathbf{\Sigma}_{0}+\mathbf{\Sigma}_{1} \\ &=\sum_{\boldsymbol{x} \in X_{0}}\left(\boldsymbol{x}-\boldsymbol{\mu}_{0}\right)\left(\boldsymbol{x}-\boldsymbol{\mu}_{0}\right)^{\mathrm{T}}+\sum_{\boldsymbol{x} \in X_{1}}\left(\boldsymbol{x}-\boldsymbol{\mu}_{1}\right)\left(\boldsymbol{x}-\boldsymbol{\mu}_{1}\right)^{\mathrm{T}} \end{aligned}$

以及“类间散度矩阵”（between class scatter matrix):

$\mathbf{S}_{b}=\left(\boldsymbol{\mu}_{0}-\boldsymbol{\mu}_{1}\right)\left(\boldsymbol{\mu}_{0}-\boldsymbol{\mu}_{1}\right)^{\mathrm{T}}$

则有：

$J=\frac{\boldsymbol{w}^{\mathrm{T}} \mathbf{S}_{b} \boldsymbol{w}}{\boldsymbol{w}^{\mathrm{T}} \mathbf{S}_{w} \boldsymbol{w}}$

这就是 LDA 欲最大化的目标，即$\mathbf{S}_{b}$和$\mathbf{S}_{w}$的‘“广义瑞利熵”(generalizedRayleigh quotient).

如何确定$w$呢？注意到分子和分母都是关于$w$的二次项，因此解与$w$的长度无关，只与其方向有关。不失一般性，令$\boldsymbol{w}^{\mathrm{T}} \mathbf{S}_{w} \boldsymbol{w}=1$, 则式等价于:

>若$w$是一个解，则对于任意常数$\alpha, \alpha \boldsymbol{w}$也是式的解。

$\begin{array}{cl}\min _{\boldsymbol{w}} & -\boldsymbol{w}^{\mathrm{T}} \mathbf{S}_{b} \boldsymbol{w} \\ \text { s.t. } & \boldsymbol{w}^{\mathrm{T}} \mathbf{S}_{w} \boldsymbol{w}=1\end{array}$

由拉格朗日乘子法，上式等价于

$\mathbf{S}_{b} \boldsymbol{w}=\lambda \mathbf{S}_{w} \boldsymbol{w}$

其中$\lambda$是拉格朗日乘子。注意到$\mathbf{S}_{b} \boldsymbol{w}$的方向恒为$\boldsymbol{\mu}_{0}-\boldsymbol{\mu}_{1}$,不妨令：

$\mathbf{S}_{b} \boldsymbol{w}=\lambda\left(\boldsymbol{\mu}_{0}-\boldsymbol{\mu}_{1}\right)$

得：

$\boldsymbol{w}=\mathbf{S}_{w}^{-1}\left(\boldsymbol{\mu}_{0}-\boldsymbol{\mu}_{1}\right)$

考虑到数值解的稳定性，在实践中通常是对$\mathbf{S}_{w}$进行奇异值分解，即$\mathbf{S}_{w}=$$\mathbf{U} \boldsymbol{\Sigma} \mathbf{V}^{\mathrm{T}}$这里$\mathbf{\Sigma}$是一个实对角矩阵，其对角线上的元素是$\mathbf{S}_{w}$的奇异值，然后再由$\mathbf{S}_{w}^{-1}=\mathbf{V} \boldsymbol{\Sigma}^{-1} \mathbf{U}^{\mathrm{T}}$得到$\mathbf{S}_{w}^{-1}$。

值得一提的是，LDA 可从贝叶斯决策理论的角度来阐释，并可证明，当两类数据同先验、满足高斯分布且协方差相等时，LDA 可达到最优分类。

可以将 LDA 推广到多分类任务中。. 假定存在$N$个类，且第$i$类示例数为$m_{i}$。我们先定义“全局散度矩阵”:

$\begin{aligned} \mathbf{S}_{t} &=\mathbf{S}_{b}+\mathbf{S}_{w} \\ &=\sum_{i=1}^{m}\left(\boldsymbol{x}_{i}-\boldsymbol{\mu}\right)\left(\boldsymbol{x}_{i}-\boldsymbol{\mu}\right)^{\mathrm{T}} \end{aligned}$

其中$μ$是所有示例的均值向量。将类内散度矩阵$\mathbf{S}_{w}$重定义为每个类别的散度矩阵之和，即

$\mathbf{S}_{w}=\sum_{i=1}^{N} \mathbf{S}_{w_{i}}$

其中

$\mathbf{S}_{w_{i}}=\sum_{\boldsymbol{x} \in X_{i}}\left(\boldsymbol{x}-\boldsymbol{\mu}_{i}\right)\left(\boldsymbol{x}-\boldsymbol{\mu}_{i}\right)^{\mathrm{T}}$

$\begin{aligned} \mathbf{S}_{b} &=\mathbf{S}_{t}-\mathbf{S}_{w} \\ &=\sum_{i=1}^{N} m_{i}\left(\boldsymbol{\mu}_{i}-\boldsymbol{\mu}\right)\left(\boldsymbol{\mu}_{i}-\boldsymbol{\mu}\right)^{\mathrm{T}} \end{aligned}$

显然，多分类 LDA 可以有多种实现方法：使用$\mathbf{S}_{b}, \mathbf{S}_{w}, \mathbf{S}_{t}$三者中的任何两个即可。常见的一种实现是采用优化目标

$\max _{\mathbf{W}} \frac{\operatorname{tr}\left(\mathbf{W}^{\mathrm{T}} \mathbf{S}_{b} \mathbf{W}\right)}{\operatorname{tr}\left(\mathbf{W}^{\mathrm{T}} \mathbf{S}_{w} \mathbf{W}\right)}$

其中$\mathbf{W} \in \mathbb{R}^{d \times(N-1)}$，$\operatorname{tr}(\cdot)$表示矩阵的迹(trace).可通过如下广义特征值问题求解：

$\mathbf{S}_{b} \mathbf{W}=\lambda \mathbf{S}_{w} \mathbf{W}$

$\mathbf{W}$的闭式解则是$\mathbf{S}_{w}^{-1} \mathbf{S}_{b}$的$N-1$个最大广义特征值所对应的特征向量组成的矩阵。

若将$W$视为一个投影矩阵，则多分类 LDA 将样本投影到$N- 1$维空间$N- 1$通常远小于数据原有的属性数。于是，可通过这个投影来减小样本点的维数，且投影过程中使用了类别信息，因此 LDA 也常被视为-一种经典的监督降维技术。