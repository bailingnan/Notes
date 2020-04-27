<!-- TOC -->

- [线性模型(《机器学习》)](#线性模型机器学习)
  - [线性回归](#线性回归)
  - [对数几率回归](#对数几率回归)
- [逻辑斯谛回归与最大熵模型](#逻辑斯谛回归与最大熵模型)
  - [逻辑斯谛回归模型](#逻辑斯谛回归模型)
    - [逻辑斯谛分布](#逻辑斯谛分布)
    - [二项逻辑斯谛回归模型](#二项逻辑斯谛回归模型)
    - [模型参数估计](#模型参数估计)
    - [多项逻辑斯谛回归](#多项逻辑斯谛回归)
- [决策树(《统计学习方法》)](#决策树统计学习方法)
  - [决策树模型与学习](#决策树模型与学习)
    - [决策树与 `if-then` 规则](#决策树与-if-then-规则)
    - [决策树与条件概率分布](#决策树与条件概率分布)
    - [决策树学习](#决策树学习)
  - [特征选择](#特征选择)
    - [信息增益](#信息增益)
    - [信息增益比](#信息增益比)
  - [决策树的生成](#决策树的生成)
    - [ID3 算法](#id3-算法)
    - [C4.5 的生成算法](#c45-的生成算法)
  - [决策树的剪枝](#决策树的剪枝)
  - [CART 算法](#cart-算法)
    - [CART 生成](#cart-生成)
      - [回归树的生成](#回归树的生成)
      - [分类树的生成](#分类树的生成)
    - [CART 剪枝](#cart-剪枝)
- [决策树(《机器学习》)](#决策树机器学习)
  - [基本流程](#基本流程)
  - [划分选择](#划分选择)
    - [信息増益(ID3)](#信息増益id3)
    - [增益率(ID4.5)](#增益率id45)
    - [基尼指数](#基尼指数)
  - [剪枝处理](#剪枝处理)
    - [预剪枝](#预剪枝)
    - [后剪枝](#后剪枝)
  - [连续与缺失值](#连续与缺失值)
    - [连续值处理](#连续值处理)
    - [缺失值处理](#缺失值处理)
  - [多变量决策树](#多变量决策树)
- [提升方法](#提升方法)
  - [提升方法 `AdaBoost` 算法](#提升方法-adaboost-算法)
    - [提升方法的基本思路](#提升方法的基本思路)
    - [`AdaBoost` 算法](#adaboost-算法)
  - [`AdaBoost` 算法的训练误差分析](#adaboost-算法的训练误差分析)
  - [`AdaBoost` 算法的解释](#adaboost-算法的解释)
    - [前向分步算法](#前向分步算法)
    - [前向分步算法与 `AdaBoost`](#前向分步算法与-adaboost)
  - [提升树](#提升树)
    - [提升树模型](#提升树模型)
    - [提升树算法](#提升树算法)
    - [梯度提升](#梯度提升)
- [集成学习(《机器学习》)](#集成学习机器学习)
  - [个体与集成](#个体与集成)
  - [`Boosting`](#boosting)
  - [`Bagging`与随机森林](#bagging与随机森林)
    - [`Bagging`](#bagging)
    - [随机森林](#随机森林)
  - [结合策略](#结合策略)
    - [平均法](#平均法)
      - [简单平均法（simple averaging)](#简单平均法simple-averaging)
      - [加权平均法（weighted averaging)](#加权平均法weighted-averaging)
    - [投票法](#投票法)
      - [绝对多数投票法（majority voting)](#绝对多数投票法majority-voting)
      - [相对多数投票法（plurality voting)](#相对多数投票法plurality-voting)
      - [加权投票法（weighted voting)](#加权投票法weighted-voting)
    - [学习法](#学习法)
  - [多样性](#多样性)
    - [误差—分岐分解](#误差分岐分解)
    - [多样性度量(差异性度量)](#多样性度量差异性度量)
    - [多样性増强](#多样性増强)
      - [数据样本扰动](#数据样本扰动)
      - [输入属性扰动](#输入属性扰动)
      - [输出表示扰动](#输出表示扰动)
      - [算法参数扰动](#算法参数扰动)
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
# 线性模型(《机器学习》)
给定由$d$个属性描述的示例$\boldsymbol{x}=\left(x_{1} ; x_{2} ; \ldots ; x_{d}\right)$,其中$x_{i}$是$x$在第$i$个属性上的取值，线性模型（inear modell）试图学得一个通过属性的线性组合来进行预测的函数，即:

$f(\boldsymbol{x})=w_{1} x_{1}+w_{2} x_{2}+\ldots+w_{d} x_{d}+b$

一般用向量形式写成:

$f(\boldsymbol{x})=\boldsymbol{w}^{\mathrm{T}} \boldsymbol{x}+b$

其中$\boldsymbol{w}=\left(w_{1} ; w_{2} ; \ldots ; w_{d}\right)$。$\boldsymbol{w}$和$b$学得之后，模型就得以确定。

线性模型形式简单、易于建模，但却蕴涵着机器学习中一些重要的基本思想许多功能更为强大的非线性模型（monlinear model）可在线性模型的基础上通过引入层级结构或高维映射而得。此外，由于$\boldsymbol{w}$直观表达了各属性在预测中的重要性，因此线性模型有很好的可解释性（comprehensibility）。
## 线性回归
给定数据集$D=\left\{\left(\boldsymbol{x}_{1}, y_{1}\right),\left(\boldsymbol{x}_{2}, y_{2}\right), \ldots,\left(\boldsymbol{x}_{m}, y_{m}\right)\right\}$，其中$\boldsymbol{x}_{i}=\left(x_{i 1}\right.$$\left.x_{i 2} ; \ldots ; x_{i d}\right)$，$y_{i} \in \mathbb{R}$。“线性回归”（linear regression）试图学得一个线性模型以尽可能准确地预测实值输出标记。

我们先考虑一种最简单的情形：输入属性的数目只有一个。为便于讨论，此时我们忽略关于属性的下标，即$D=\left\{\left(x_{i}, y_{i}\right)\right\}_{i=1}^{m}$，其中$x_{i} \in \mathbb{R}$。对离散属性若属性值间存在“序”（order）关系，可通过连续化将其转化为连续值，例如二值属性“身高”的取值“高”“矮”可转化为{1.0,0.0}，三值属性“高度”的取值“高”“中”“低”可转化为{1.0,0.5,0.0}；若属性值间不存在序关系，假定有$k$个属性值，则通常转化为$k$维向量，例如属性“瓜类”的取值“西瓜”“南瓜”“黄瓜”可转化为(0,0,1), (0,1,0), (1,0,0)。

若将无序属性连续化，则会不恰当地引入序关系，对后续处理如距离计算等造成误导。

线性回归试图学得

$f\left(x_{i}\right)=w x_{i}+b$，使得$f\left(x_{i}\right) \simeq y_{i}$。

如何确定$w$和$b$呢？显然，关键在于如何衡量$f(x)$与$y$之间的差别。均方误差是回归任务中最常用的性能度量，因此我们可试图让均方误差最小化，即

$\begin{aligned}\left(w^{*}, b^{*}\right) &=\underset{(w, b)}{\arg \min } \sum_{i=1}^{m}\left(f\left(x_{i}\right)-y_{i}\right)^{2} \\ &=\underset{(w, b)}{\arg \min } \sum_{i=1}^{m}\left(y_{i}-w x_{i}-b\right)^{2} \end{aligned}$

>$w^{*}$，$b^{*}$表示$w$和$b$的解

均方误差有非常好的几何意义，它对应了常用的欧几里得距离或简称“欧氏距离”（Euclidean distance）。基于均方误差最小化来进行模型求解的方法称为“最小二乘法”（least square method，在线性回归中，最小二乘法就是试图找到一条直线，使所有样本到直线上的欧氏距离之和最小。

求解$w$和$b$使$E_{(w, b)}=\sum_{i=1}^{m}\left(y_{i}-w x_{i}-b\right)^{2}$最小化的过程，称为线性回归模型的最小二乘“参数估计”（parameter estimation）。我们可将$E_{(w, b)}$分别对$w$和$b$求导，得到

$\frac{\partial E_{(w, b)}}{\partial w}=2\left(w \sum_{i=1}^{m} x_{i}^{2}-\sum_{i=1}^{m}\left(y_{i}-b\right) x_{i}\right)$

$\frac{\partial E_{(w, b)}}{\partial b}=2\left(m b-\sum_{i=1}^{m}\left(y_{i}-w x_{i}\right)\right)$

>这里$E_{(w, b)}$是关于$w$和$b$的凸函数，当它关于$w$和$b$的导数均为零时，得到$w$和$b$的最优解。

>对区间$[a, b]$上定义的函数$f$，若它对区间中任意两点$x_{1}, x_{2}$均有$f\left(\frac{x_{1}+x_{2}}{2}\right) \leqslant \frac{f\left(x_{1}\right)+f\left(x_{2}\right)}{2}$则称$f$为区间$[a, b]$上的凸函数。U 形曲线的函数如$f(x)=x^{2}$, 通常是凸函数

>对实数集上的函数，可通过求二阶导数来判别若二阶导数在区间上非负则称为凸函数；若二阶导数在区间上恒大于 0, 则称为严格凸函数


然后令上式为零可得到$w$和$b$最优解的闭式（Closed-form）解：

$w=\frac{\sum_{i=1}^{m} y_{i}\left(x_{i}-\bar{x}\right)}{\sum_{i=1}^{m} x_{i}^{2}-\frac{1}{m}\left(\sum_{i=1}^{m} x_{i}\right)^{2}}$

$b=\frac{1}{m} \sum_{i=1}^{m}\left(y_{i}-w x_{i}\right)$

其中$\bar{x}=\frac{1}{m} \sum_{i=1}^{m} x_{i}$ 为 $x$为$x$的均值。

更一般的情形是如本节开头的数据集$D$，样本由$d$个属性描述。此时我们试图学得:

$f\left(\boldsymbol{x}_{i}\right)=\boldsymbol{w}^{\mathrm{T}} \boldsymbol{x}_{i}+b$,使得$f\left(\boldsymbol{x}_{i}\right) \simeq y_{i}$

这称为“多元线性回归”（multivariate linear regression)

类似的，可利用最小二乘法来对$\boldsymbol{w}$和$b$进行估计。为便于讨论，我们把$\boldsymbol{w}$和$b$吸收入向量形式$\hat{\boldsymbol{w}}=(\boldsymbol{w} ; b)$，相应的，把数据集$D$表示为一个$m \times(d+1)$大小的矩阵$\mathbf{X}$，其中每行对应于一个示例，该行前$d$个元素对应于示例的$d$个属性值，最后一个元素恒置为 1, 即

$\mathbf{X}=\left(\begin{array}{ccccc}x_{11} & x_{12} & \dots & x_{1 d} & 1 \\ x_{21} & x_{22} & \dots & x_{2 d} & 1 \\ \vdots & \vdots & \ddots & \vdots & \vdots \\ x_{m 1} & x_{m 2} & \dots & x_{m d} & 1\end{array}\right)=\left(\begin{array}{cc}\boldsymbol{x}_{1}^{\mathrm{T}} & 1 \\ \boldsymbol{x}_{2}^{\mathrm{T}} & 1 \\ \vdots & \vdots \\ \boldsymbol{x}_{m}^{\mathrm{T}} & 1\end{array}\right)$

再把标记也写成向量形式$\boldsymbol{y}=\left(y_{1} ; y_{2} ; \ldots ; y_{m}\right)$,则有：

$\hat{\boldsymbol{w}}^{*}=\underset{\hat{\boldsymbol{w}}}{\arg \min }(\boldsymbol{y}-\mathbf{X} \hat{\boldsymbol{w}})^{\mathrm{T}}(\boldsymbol{y}-\mathbf{X} \hat{\boldsymbol{w}})$

令$E_{\hat{\boldsymbol{w}}}=(\boldsymbol{y}-\mathbf{X} \hat{\boldsymbol{w}})^{\mathrm{T}}(\boldsymbol{y}-\mathbf{X} \hat{\boldsymbol{w}})$，对$\hat{\boldsymbol{w}}$求导得到：

$\frac{\partial E_{\hat{w}}}{\partial \hat{w}}=2 \mathbf{X}^{\mathrm{T}}(\mathbf{X} \hat{\boldsymbol{w}}-\boldsymbol{y})$

令上式为零可得$\hat{\boldsymbol{w}}$最优解的闭式解，但由于涉及矩阵逆的计算，比单变量情形要复杂一些。下面我们做一个简单的讨论。

当$\mathbf{X}^{\mathrm{T}} \mathbf{X}$为满秩矩阵（full-rank matrix）或正定矩阵（positive definite ma trix）时，令式为零可得

$\hat{\boldsymbol{w}}^{*}=\left(\mathbf{X}^{\mathrm{T}} \mathbf{X}\right)^{-1} \mathbf{X}^{\mathrm{T}} \boldsymbol{y}$

其中$\left(\mathbf{X}^{\mathrm{T}} \mathbf{X}\right)^{-1}$是矩阵$\left(\mathbf{X}^{\mathrm{T}} \mathbf{X}\right)$的逆矩阵。令$\hat{\boldsymbol{x}}_{i}=\left(\boldsymbol{x}_{i}, 1\right)$，则最终学得的多元线性回归模型为：

$f\left(\hat{\boldsymbol{x}}_{i}\right)=\hat{\boldsymbol{x}}_{i}^{\mathrm{T}}\left(\mathbf{X}^{\mathrm{T}} \mathbf{X}\right)^{-1} \mathbf{X}^{\mathrm{T}} \boldsymbol{y}$

然而，现实任务中$\mathbf{X}^{\mathrm{T}} \mathbf{X}$往往不是满秩矩阵。例如在许多任务中我们会遇到大量的变量，其数目甚至超过样例数，导致 $\mathbf{X}$的列数多于行数，$\mathbf{X}^{\mathrm{T}} \mathbf{X}$显然不满秩。此时可解出多个$\hat{\boldsymbol{w}}$，它们都能使均方误差最小化。选择哪一个解作为输出，将由学习算法的归纳偏好决定，常见的做法是引入正则化（regularization）项。

线性模型虽简单，却有丰富的变化。例如对于样例$(\boldsymbol{x}, y), y \in \mathbb{R}$，当我们希望线性模型的预测值逼近真实标记$y$时，就得到了线性回归模型。为便于观察，我们把线性回归模型简写为

$y=\boldsymbol{w}^{\mathrm{T}} \boldsymbol{x}+b$

可否令模型预测值逼近$y$的衍生物呢？譬如说，假设我们认为示例所对应的输出标记是在指数尺度上变化，那就可将输出标记的对数作为线性模型逼近的目标，即

$\ln y=\boldsymbol{w}^{\mathrm{T}} \boldsymbol{x}+b$

这就是“对数线性回归”（log-linear regression），它实际上是在试图让$e^{\boldsymbol{w}^{\mathrm{T}} \boldsymbol{x}+b}$逼近$y$。上式在形式上仍是线性回归，但实质上已是在求取输入空间到输出空间的非线性函数映射，如图所示。这里的对数函数起到了将线性回归模型的预测值与真实标记联系起来的作用。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200427150204.png)

更一般地，考虑单调可微函数$g(\cdot)$，$g(\cdot)$连续且充分光滑，令：

$y=g^{-1}\left(\boldsymbol{w}^{\mathrm{T}} \boldsymbol{x}+b\right)$

这样得到的模型称为“广义线性模型”（generalized linear model），其中函数$g(\cdot)$称为“联系函数”（link function）。显然，对数线性回归是广义线性模型在$g(\cdot)=\ln (\cdot)$时的特例。

>广义线性模型的参数估计常通过加权最小二乘法或极大似然法进行。
## 对数几率回归
上一节讨论了如何使用线性模型进行回归学习，但若要做的是分类任务该怎么办？答案蕴涵在广义线性模型中：只需找一个单调可微函数将分类任务的真实标记 $y$与线性回归模型的预测值联系起来。

考虑二分类任务，其输出标记$y \in\{0,1\}$，而线性回归模型产生的预测值$z=\boldsymbol{w}^{\mathrm{T}} \boldsymbol{x}+b$是实值,于是,我们需将实值$z$转换为0/1值.最理想的是“单位阶跃函数”（unit- step function):

$y=\left\{\begin{aligned} 0, & z<0 \\ 0.5, & z=0 \\ 1, & z>0 \end{aligned}\right.$

若预测值 2 大于零就判为正例，小于零则判为反例，预测值为临界值零则可任意判别，如图所示。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200427150632.png)

但从图可看出，单位阶跃函数不连续，因此不能直接用作式中的$g^{-}(\cdot)$，于是我们希望找到能在一定程度上近似单位阶跃函数的“替代函数”（surrogate function），并希望它单调可微。对数几率函数（logistic function）正是这样一个常用的替代函数：

$y=\frac{1}{1+e^{-z}}$

从图可看出，对数几率函数是一种“Sigmoid 函数”，它将$z$值转化为一个接近 0 或 1 的$y$值，并且其输出值在 x=0 附近变化很陡。将对数几率函数作为$g^{-}(\cdot)$代入式，得到

$y=\frac{1}{1+e^{-\left(\boldsymbol{w}^{\mathrm{T}} \boldsymbol{x}+b\right)}}$

$\ln \frac{y}{1-y}=\boldsymbol{w}^{\mathrm{T}} \boldsymbol{x}+b$

若将$y$视为样本$\boldsymbol{x}$作为正例的可能性，则$1-y$是其反例可能性，两者的比值

$\frac{y}{1-y}$

称为“几率”（odls），反映了$\boldsymbol{x}$作为正例的相对可能性。对几率取对数则得到“对数几率”（log odds，亦称 logit):

$\ln \frac{y}{1-y}$

由此可看出，实际上是在用线性回归模型的预测结果去逼近真实标记的对数几率，因此，其对应的模型称为“对数几率回归”（logistic regression，亦称 logit regression）。特别需注意到，虽然它的名字是“回归”，但实际却是一种分类学习方法。这种方法有很多优点，例如它是直接对分类可能性进行建模，无需事先假设数据分布，这样就避免了假设分布不准确所带来的问题；它不是仅预测出“类别”，而是可得到近似概率预测，这对许多需利用概率辅助决策的任务很有用；此外，对率函数是任意阶可导的凸函数，有很好的数学性质，现有的许多数值优化算法都可直接用于求取最优解。

下面我们来看看如何确定$\boldsymbol{w}$和$b$。若将$y$视为类后验概率估计$p(y=1 | \boldsymbol{x})$，则式可重写为

$\ln \frac{p(y=1 | \boldsymbol{x})}{p(y=0 | \boldsymbol{x})}=\boldsymbol{w}^{\mathrm{T}} \boldsymbol{x}+b$

显然有:

$p(y=1 | \boldsymbol{x})=\frac{e^{\boldsymbol{w}^{\mathrm{T}} \boldsymbol{x}+b}}{1+e^{\boldsymbol{w}^{\mathrm{T}} \boldsymbol{x}+b}}$

$p(y=0 | \boldsymbol{x})=\frac{1}{1+e^{\boldsymbol{w}^{\mathrm{T}} \boldsymbol{x}+b}}$

于是，我们可通过“极大似然法”（maximum likelihood method）来估计$\boldsymbol{w}$和$b$。给定数据集$\left\{\left(\boldsymbol{x}_{i}, y_{i}\right)\right\}_{i=1}^{m}$, 对率回归模型最大化“对数似然”（oglikelihood):

$\ell(\boldsymbol{w}, b)=\sum_{i=1}^{m} \ln p\left(y_{i} | \boldsymbol{x}_{i} ; \boldsymbol{w}, b\right)$

即令每个样本属于其真实标记的概率越大越好。为便于讨论，令$\boldsymbol{\beta}=(\boldsymbol{w} ; b)$，$\hat{\boldsymbol{x}}=(\boldsymbol{x} ; 1)$,则$\boldsymbol{w}^{\mathrm{T}} \boldsymbol{x}+b$可简写为$\boldsymbol{\beta}^{\mathrm{T}} \hat{\boldsymbol{x}}$。再令$p_{1}(\hat{\boldsymbol{x}} ; \boldsymbol{\beta})=p(y=1 | \hat{\boldsymbol{x}} ; \boldsymbol{\beta})$,$p_{0}(\hat{\boldsymbol{x}} ; \boldsymbol{\beta})=p(y=0 | \hat{\boldsymbol{x}} ; \boldsymbol{\beta})=1-p_{1}(\hat{\boldsymbol{x}} ; \boldsymbol{\beta})$，则式中的似然项可重写为:

$p\left(y_{i} | \boldsymbol{x}_{i} ; \boldsymbol{w}, b\right)=y_{i} p_{1}\left(\hat{\boldsymbol{x}}_{i} ; \boldsymbol{\beta}\right)+\left(1-y_{i}\right) p_{0}\left(\hat{\boldsymbol{x}}_{i} ; \boldsymbol{\beta}\right)$

最大化式$\ell(\boldsymbol{w}, b)$等价于最小化:

$\ell(\boldsymbol{\beta})=\sum_{i=1}^{m}\left(-y_{i} \boldsymbol{\beta}^{\mathrm{T}} \hat{\boldsymbol{x}}_{i}+\ln \left(1+e^{\boldsymbol{\beta}^{\mathrm{T}} \hat{\boldsymbol{x}}_{i}}\right)\right)$

上式是关于$\boldsymbol{\beta}$的高阶可导连续凸函数，根据凸优化理论, 经典的数值优化算法如梯度下降法（gradient descent method）、牛顿法（Newton method）等都可求得其最优解，于是就得到

$\boldsymbol{\beta}^{*}=\underset{\boldsymbol{\beta}}{\arg \min } \ell(\boldsymbol{\beta})$

以牛顿法为例，其第$t+1$轮迭代解的更新公式为：

$\boldsymbol{\beta}^{t+1}=\boldsymbol{\beta}^{t}-\left(\frac{\partial^{2} \ell(\boldsymbol{\beta})}{\partial \boldsymbol{\beta} \partial \boldsymbol{\beta}^{T}}\right)^{-1} \frac{\partial \ell(\boldsymbol{\beta})}{\partial \boldsymbol{\beta}}$

其中关于$\boldsymbol{\beta}$的一阶、二阶导数分别为：

$\begin{aligned} \frac{\partial \ell(\boldsymbol{\beta})}{\partial \boldsymbol{\beta}} &=-\sum_{i=1}^{m} \hat{\boldsymbol{x}}_{i}\left(y_{i}-p_{1}\left(\hat{\boldsymbol{x}}_{i} ; \boldsymbol{\beta}\right)\right) \\ \frac{\partial^{2} \ell(\boldsymbol{\beta})}{\partial \boldsymbol{\beta} \partial \boldsymbol{\beta}^{\mathrm{T}}} &=\sum_{i=1}^{m} \hat{\boldsymbol{x}}_{i} \hat{\boldsymbol{x}}_{i}^{\mathrm{T}} p_{1}\left(\hat{\boldsymbol{x}}_{i} ; \boldsymbol{\beta}\right)\left(1-p_{1}\left(\hat{\boldsymbol{x}}_{i} ; \boldsymbol{\beta}\right)\right) \end{aligned}$

# 逻辑斯谛回归与最大熵模型
逻辑斯谛回归（logistic regression）是统计学习中的经典分类方法。最大熵是概率模型学习的一个准则，将其推广到分类问题得到最大熵模型（maximum entropy model）。逻辑斯谛回归模型与最大熵模型都属于对数线性模型。本章首先介绍逻辑斯谛回归模型，然后介绍最大熵模型，最后讲述逻辑斯谛回归与最大熵模型的学习算法，包括改进的迭代尺度算法和拟牛顿法。

## 逻辑斯谛回归模型
### 逻辑斯谛分布
设$X$是连续随机变量，$X$服从逻辑斯谛分布是指$X$具有下列分布函数和密度函数：

$F(x)=P(X \leqslant x)=\frac{1}{1+\mathrm{e}^{-(x-\mu) / \gamma}}$

$f(x)=F^{\prime}(x)=\frac{\mathrm{e}^{-(x-\mu) / \gamma}}{\gamma\left(1+\mathrm{e}^{-(x-\mu) / \gamma}\right)^{2}}$

式中,$\mu$为位置参数，$\gamma>0$为形状参数

逻辑斯谛分布的密度函数$f(x)$和分布函数$F(x)$的图形如图所示。分布函数属于逻辑斯谛函数，其图形是一条$S$形曲线（sigmoid curve）。该曲线以点$\left(\mu, \frac{1}{2}\right)$中心对称，即满足:

$F(-x+\mu)-\frac{1}{2}=-F(x+\mu)+\frac{1}{2}$

曲线在中心附近增长速度较快，在两端增长速度较慢。形状参数$\gamma$的值越小，曲线在中心附近增长得越快。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200427161248.png)
### 二项逻辑斯谛回归模型
项逻辑斯谛回归模型（binomial logistic regression model）是一种分类模型，由条件概率分布$P(Y | X)$表示，形式为参数化的逻辑斯谛分布。这里，随机变量$X$取值为实数，随机变量$Y$取值为 1 或 0。我们通过监督学习的方法来估计模型参数。

逻辑斯谛回归模型：

二项逻辑斯谛回归模型是如下的条件概率分布：

$P(Y=1 | x)=\frac{\exp (w \cdot x+b)}{1+\exp (w \cdot x+b)}$

$P(Y=0 | x)=\frac{1}{1+\exp (w \cdot x+b)}$

这里，$x \in \mathbf{R}^{n}$是输入，$Y \in\{0,1\}$是输出，$w \in \mathbf{R}^{n}$和$b \in \mathbf{R}$是参数，$w$称为权值向量，$b$称为偏置，$w \cdot x$为$w$和$x$的内积。

对于给定的输入实例$x$，按照上式可以求得$P(Y=1 | x)$和$P(Y=0 | x)$。逻辑斯谛回归比较两个条件概率值的大小，将实例$x$分到概率值较大的那一类。

有时为了方便，将权值向量和输入向量加以扩充，仍记作$w, x$，即$w=\left(w^{(1)}\right.$$\left.w^{(2)}, \cdots, w^{(n)}, b\right)^{\mathrm{T}}, x=\left(x^{(1)}, x^{(2)}, \cdots, x^{(n)}, 1\right)^{\mathrm{T}}$。这时，逻辑斯谛回归模型如下

$P(Y=1 | x)=\frac{\exp (w \cdot x)}{1+\exp (w \cdot x)}$

$P(Y=0 | x)=\frac{1}{1+\exp (w \cdot x)}$

现在考査逻辑斯谛回归模型的特点。一个事件的几率（ods）是指该事件发生的概率与该事件不发生的概率的比值。如果事件发生的概率是$p$，那么该事件的几率是该事件的对数几率（log odds）或 lgit 函数是:

$\operatorname{logit}(p)=\log \frac{p}{1-p}$

对逻辑斯谛回归而言，得

$\log \frac{P(Y=1 | x)}{1-P(Y=1 | x)}=w \cdot x$

这就是说，在逻辑斯谛回归模型中，输出$Y=1$的对数几率是输入$x$的线性函数。或者说，输出$Y=1$的对数几率是由输入$x$的线性函数表示的模型，即逻辑斯谛回归模型。

换一个角度看，考虑对输入$x$进行分类的线性函数$w \cdot x$，其值域为实数域。注意，这里$x \in \mathbf{R}^{n+1}$,$w \in \mathbf{R}^{n+1}$。通过逻辑斯谛回归模型定义式可以将线性函数$w \cdot x$转换为概率：

$P(Y=1 | x)=\frac{\exp (w \cdot x)}{1+\exp (w \cdot x)}$

这时，线性函数的值越接近正无穷，概率值就越接近 1; 线性函数的值越接近负无穷，概率值就越接近0。这样的模型就是逻辑斯谛回归模型。

### 模型参数估计
逻辑斯谛回归模型学习时，对于给定的训练数据集$T=\left\{\left(x_{1}, y_{1}\right),\left(x_{2}, y_{2}\right), \cdots\right.$，其中,$x_{i} \in \mathbf{R}^{n}$, $y_{i} \in\{0,1\}$，可以应用极大似然估计法估计模型参数，从而得到逻辑斯谛回归模型:

设:

$P(Y=1 | x)=\pi(x), \quad P(Y=0 | x)=1-\pi(x)$

似然函数为:

$\prod_{i=1}^{N}\left[\pi\left(x_{i}\right)\right]^{y_{i}}\left[1-\pi\left(x_{i}\right)\right]^{1-y_{i}}$

对数似然函数为:

$\begin{aligned} L(w) &=\sum_{i=1}^{N}\left[y_{i} \log \pi\left(x_{i}\right)+\left(1-y_{i}\right) \log \left(1-\pi\left(x_{i}\right)\right)\right] \\ &=\sum_{i=1}^{N}\left[y_{i} \log \frac{\pi\left(x_{i}\right)}{1-\pi\left(x_{i}\right)}+\log \left(1-\pi\left(x_{i}\right)\right)\right] \\ &=\sum_{i=1}^{N}\left[y_{i}\left(w \cdot x_{i}\right)-\log \left(1+\exp \left(w \cdot x_{i}\right)\right]\right.\end{aligned}$

对$L(w)$求极大值，得到$w$的估计值。

这样，问题就变成了以对数似然函数为目标函数的最优化问题。逻辑斯谛回归学习中通常采用的方法是梯度下降法及拟牛顿法。

假设$w$的极大似然估计值是$\hat{w}$，那么学到的逻辑斯谛回归模型为:

$P(Y=1 | x)=\frac{\exp (\hat{w} \cdot x)}{1+\exp (\hat{w} \cdot x)}$

$P(Y=0 | x)=\frac{1}{1+\exp (\hat{w} \cdot x)}$

### 多项逻辑斯谛回归
上面介绍的逻辑斯谛回归模型是二项分类模型，用于二类分类。可以将其推广为多项逻辑斯谛回归模型（multi-nominal logistic regression model），用于多类分类。假设离散型随机变量$Y$的取值集合是$\{1,2, \cdots, K\}$，那么多项逻辑斯谛回归模型是:

$P(Y=k | x)=\frac{\exp \left(w_{k} \cdot x\right)}{1+\sum_{k=1}^{K-1} \exp \left(w_{k} \cdot x\right)}, \quad k=1,2, \cdots, K-1$
$P(Y=K | x)=\frac{1}{1+\sum_{k=1}^{K-1} \exp \left(w_{k} \cdot x\right)}$

这里，$x \in \mathbf{R}^{n+1}, w_{k} \in \mathbf{R}^{n+1}$。

项逻辑斯谛回归的参数估计法也可以推广到多项逻辑斯谛回归。

# 决策树(《统计学习方法》)
决策树（decision tree）是一种基本的分类与回归方法。本章主要讨论用于分类的决策树。决策树模型呈树形结构，在分类问题中，表示基于特征对实例进行分类的过程。它可以认为是 `if-then` 规则的集合，也可以认为是定义在特征空间与类空间上的条件概率分布。其主要优点是模型具有可读性，分类速度快。学习时，利用训练数据，根据损失函数最小化的原则建立决策树模型。预测时，对新的数据，利用决策树模型进行分类。决策树学习通常包括 3 个步骤：特征选择、决策树的生成和决策树的修剪。这些决策树学习的思想主要来源于ID3 算法和C4.5 算法，以及CART 算法。
## 决策树模型与学习
分类决策树模型是一种描述对实例进行分类的树形结构。决策树由结点（node）和有向边（directededge）组成。结点有两种类型：内部结点（internal node）和叶结点（leaf node）。内部结点表示一个特征或属性，叶结点表示一个类。

用决策树分类，从根结点开始，对实例的某-特征进行测试，根据测试结果，将实例分配到其子结点；这时，每一个子结点对应着该特征的一个取值。如此递归地对实例进行测试并分配，直至达到叶结点。最后将实例分到叶结点的类中。

下图是一个决策树的示意图。图中圆和方框分别表示内部结点和叶结点。
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200420172427.png)

### 决策树与 `if-then` 规则
可以将决策树看成一个 `if-then` 规则的集合。将决策树转换成`if-then`规则的过程是这样的：由决策树的根结点到叶结点的每一条路径构建一条规则；路径上内部结点的特征对应着规则的条件，而叶结点的类对应着规则的结论。决策树的路径或其对应的 `if-then` 规则集合具有一个重要的性质：互斥并且完备。这就是说，每一个实例都被条路径或一条规则所覆盖，而且只被一条路径或一条规则所覆盖。这里所谓覆盖是指实例的特征与路径上的特征一致或实例满足规则的条件。
### 决策树与条件概率分布
决策树还表示给定特征条件下类的条件概率分布。这一条件概率分布定义在特征空间的一个划分（partition）上。将特征空间划分为互不相交的单元（cel) l 或区域（region），并在每个单元定义一个类的概率分布就构成了一个条件概率分布。决策树的一条路径对应于划分中的一个单元。决策树所表示的条件概率分布由各个单元给定条件下类的条件概率分布组成。假设$X$为表示特征的随机变量，$Y$为表示类的随机变量，那么这个条件概率分布可以表示为$P(Y | X)$。$X$取值于给定划分下单元的集合，$Y$取值于类的集合。各叶结点（单元）上的条件概率往往偏向某一个类，即属于某一类的概率较大。決策树分类时将该结点的实例强行分到条件概率大的那类去。

图 5.2 (a)示意地表示了特征空间的一个划分。图中的大正方形表示特征空间。这个大正方形被若干个小矩形分割，每个小矩形表示一个单元。特征空间划分上的单元构成了一个集合，$X$取值为单元的集合。为简单起见，假设只有两类：正类和负类，即$Y$取值为+1和-1。小矩形中的数字表示单元的类。图 5.2 (b)示意地表示特征空间划分确定时，特征（单元）给定条件下类的条件概率分布。图 5.2 (b)中条件概率分布对应于图 5.2 (a)的划分。当某个单元 $c$ 的条件概率满足$P(Y=+1 | X=c)>0.5$时，则认为这个单元属于正类，即落在这个单元的实例都被视为正例。图 5.2 (c)为对应于图 5.2 (b)中条件概率分布的决策树。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200420173905.png)

### 决策树学习
假设给定训练数据集

$D=\left\{\left(x_{1}, y_{1}\right),\left(x_{2}, y_{2}\right), \cdots,\left(x_{N}, y_{N}\right)\right\}$

其中，$x_{i}=\left(x_{i}^{(1)}, x_{i}^{(2)}, \cdots, x_{i}^{(n)}\right)^{\mathrm{T}}$为输入实例（特征向量）,$n$为特征个数，$y_{i} \in$$\{1,2, \cdots, K\}$为类标记，$i=1,2, \cdots, N$，$N$为样本容量。决策树学习的目标是根据给定的训练数据集构建一个决策树模型，使它能够对实例进行正确的分类。

决策树学习本质上是从训练数据集中归纳出一组分类规则。与训练数据集不相矛盾的决策树（即能对训练数据进行正确分类的决策树）可能有多个，也可能一个都没有。我们需要的是一个与训练数据矛盾较小的决策树，同时具有很好的泛化能力。从另一个角度看，决策树学习是由训练数据集估计条件概率模型。基于特征空间划分的类的条件概率模型有无穷多个。我们选择的条件概率模型应该不仅对训练数据有很好的拟合，而且对未知数据有很好的预测。

决策树学习用损失函数表示这一目标。如下所述，决策树学习的损失函数通常是正则化的极大似然函数。决策树学习的策略是以损失函数为目标函数的最小化。

当损失函数确定以后，学习问题就变为在损失函数意义下选择最优决策树的问题。因为从所有可能的决策树中选取最优决策树是 NP 完全问题，所以现实中决策树学习算法通常采用启发式方法，近似求解这一最优化问题。这样得到的决策树是次最优（sub-optimal）的。

决策树学习的算法通常是一个递归地选择最优特征，并根据该特征对训练数据进行分割，使得对各个子数据集有一个最好的分类的过程。这一过程对应着对特征空间的划分，也对应着决策树的构建。开始，构建根结点，将所有训练数据都放在根结点。选择一个最优特征，按照这一特征将训练数据集分割成子集，使得各个子集有一个在当前条件下最好的分类。如果这些子集已经能够被基本正确分类，那么构建叶结点，并将这些子集分到所对应的叶结点中去；如果还有子集不能被基本正确分类，那么就对这些子集选择新的最优特征，继续对其进行分割，构建相应的结点。如此递归地进行下去，直至所有训练数据子集被基本正确分类，或者没有合适的特征为止。最后每个子集都被分到叶结点上，即都有了明确的类。这就生成了一棵决策树。

以上方法生成的决策树可能对训练数据有很好的分类能力，但对未知的测试数据却未必有很好的分类能力，即可能发生过拟合现象。我们需要对已生成的树自下而上进行剪枝，将树变得更简单，从而使它具有更好的泛化能力。具体地，就是去掉过于细分的叶结点，使其回退到父结点，甚至更高的结点，然后将父结点或更高的结点改为新的叶结点。

如果特征数量很多，也可以在决策树学习开始的时候，对特征进行选择，只留下对训练数据有足够分类能力的特征。

可以看出，决策树学习算法包含特征选择、决策树的生成与决策树的剪枝过程。由于决策树表示一个条件概率分布，所以深浅不同的决策树对应着不同复杂度的概率模型。决策树的生成对应于模型的局部选择，决策树的剪枝对应于模型的全局选择。决策树的生成只考虑局部最优，相对地，决策树的剪枝则考虑全局最优。

## 特征选择
特征选择在于选取对训练数据具有分类能力的特征。这样可以提高决策树学习的效率。如果利用一个特征进行分类的结果与随机分类的结果没有很大差别，则称这个特征是没有分类能力的。经验上扔掉这样的特征对决策树学习的精度影响不大。通常特征选择的准则是信息增益或信息增益比。

### 信息增益
为了便于说明，先给出熵与条件熵的定义。

在信息论与概率统计中，熵（entropy）是表示随机变量不确定性的度量。设$X$是一个取有限个值的离散随机变量，其概率分布为

$P\left(X=x_{i}\right)=p_{i}, \quad i=1,2, \cdots, n$

则随机变量$X$的熵定义为:

$H(X)=-\sum_{i=1}^{n} p_{i} \log p_{i}$

若$p_{i}=0$,则定义$0 \log 0=0$。通常，上式中的对数以 2 为底或以 e为底（自然对数），这时熵的单位分别称作比特（bit）或纳特（nat）。由定义可知，熵只依赖于$X$的分布，而与$X$的取值无关，所以也可将$X$的熵记作$H(p)$，即

$H(p)=-\sum_{i=1}^{n} p_{i} \log p_{i}$

熵越大，随机变量的不确定性就越大。从定义可验证

$0 \leqslant H(p) \leqslant \log n$

当随机变量只取两个值，例如 1,0 时，即$X$的分布为:

$P(X=1)=p, \quad P(X=0)=1-p, \quad 0 \leqslant p \leqslant 1$

熵为

$H(p)=-p \log _{2} p-(1-p) \log _{2}(1-p)$

这时，熵$H(p)$随概率$p$变化的曲线如图所示（单位为比特）:

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200420183232.png)

当$p=0$或$p=1$时$H(p)=0$, 随机变量完全没有不确定性。当$p=0.5$时,$H(p)=1$, 熵取值最大，随机变量不确定性最大。

设有随机变量$(X, Y)$，其联合概率分布为：

$P\left(X=x_{i}, Y=y_{j}\right)=p_{i j}, \quad i=1,2, \cdots, n ; \quad j=1,2, \cdots, m$

条件熵$H(Y | X)$表示在已知随机变量$X$的条件下随机变量$Y$的不确定性。随机变量$X$给定的条件下随机变量$Y$的条件熵(conditional entropy) $H(Y | X)$，定义为$X$给定条件下$Y$的条件概率分布的熵对$X$的数学期望:

$H(Y | X)=\sum_{i=1}^{n} p_{i} H\left(Y | X=x_{i}\right)$

这里，$p_{i}=P\left(X=x_{i}\right), i=1,2, \cdots, n$。

当熵和条件熵中的概率由数据估计（特别是极大似然估计）得到时，所对应的熵与条件熵分别称为经验熵（empirical entropy）和经验条件熵（empirical conditional entropy）。此时，如果有 0 概率，令$0 \log 0=0$。

信息增益（information gain）表示得知特征$X$的信息而使得类$Y$的信息的不确定性减少的程度。

信息增益:特征$A$对训练数据集$D$的信息増益$g(D, A)$，定义为集合$D$的经验熵$H(D)$与特征$A$给定条件下$D$的经验条件熵$H(D | A)$之差，即

$g(D, A)=H(D)-H(D | A)$

一般地，熵$H(Y)$与条件熵$H(Y | X)$之差称为互信息（mutual information）。决策树学习中的信息增益等价于训练数据集中类与特征的互信息。

决策树学习应用信息增益准则选择特征。给定训练数据集$D$和特征$A$，经验熵$H(D)$表示对数据集$D$进行分类的不确定性。而经验条件熵（DlA）表示在特征$A$给定的条件下对数据集$D$进行分类的不确定性。那么它们的差，即信息增益，就表示由于特征$A$而使得对数据集$D$的分类的不确定性减少的程度。显然，对于数据集$D$而言，信息增益依赖于特征，不同的特征往往具有不同的信息增益。信息增益大的特征具有更强的分类能力。

根据信息增益准则的特征选择方法是：对训练数据集（或子集）$D$，计算其每个特征的信息增益，并比较它们的大小，选择信息增益最大的特征。

设训练数据集为$D$,$|D|$表示其样本容量，即样本个数。设有$K$个类$C_{k}$,$k=$$1,2, \cdots, K$,$\left|C_{k}\right|$为属于类$C_{k}$的样本个数，$\sum_{k=1}^{K}\left|C_{k}\right|=|D|$.设特征$A$有$n$个不同的取值$\left\{a_{1}, a_{2}, \cdots, a_{n}\right\}$，根据特征$A$的取值将$D$划分为$n$个子集$D_{1}, D_{2}, \cdots, D_{n}$,$\left|D_{i}\right|$为$D_{i}$的样本个数，$\sum_{i=1}^{n}\left|D_{i}\right|=|D|$。记子集$D_{i}$中属于类$C_{k}$的样本的集合为$D_{i k}$即$D_{i k}=D_{i} \cap C_{k}$，$| D_{i k |}$为$D_{i k}$，的样本个数。于是信息增益的算法如下去。

输入：训练数据集$D$和特征$A$;
输出：特征$A$对训练数据集$D$的信息增益$g(D, A)$。

1. 计算数据集$D$的经验熵$H(D)$
$H(D)=-\sum_{k=1}^{K} \frac{\left|C_{k}\right|}{|D|} \log _{2} \frac{\left|C_{k}\right|}{|D|}$

2. 计算特征$A$对数据集$D$的经验条件熵$H(D | A)$

$H(D | A)=\sum_{i=1}^{n} \frac{\left|D_{i}\right|}{|D|} H\left(D_{i}\right)=-\sum_{i=1}^{n} \frac{\left|D_{i}\right|}{|D|} \sum_{k=1}^{K} \frac{\left|D_{i k}\right|}{\left|D_{i}\right|} \log _{2} \frac{\left|D_{i k}\right|}{\left|D_{i}\right|}$

3. 计算信息增益

$g(D, A)=H(D)-H(D | A)$

### 信息增益比
以信息增益作为划分训练数据集的特征，存在偏向于选择取值较多的特征的问题。使用信息增益比（information gain ratio）可以对这一问题进行校正。这是特征选择的另一准则。

信息增益比：

特征$A$对训练数据集$D$的信息増益比$g_{R}(D, A)$定义为其信息增益$g(D, A)$与训练数据集$D$关于特征$A$的值的熵$H_{A}(D)$之比，即

$g_{R}(D, A)=\frac{g(D, A)}{H_{A}(D)}$

其中，$H_{A}(D)=-\sum_{i=1}^{n} \frac{\left|D_{i}\right|}{|D|} \log _{2} \frac{\left|D_{i}\right|}{|D|}$，$n$是特征$A$取值的个数。
## 决策树的生成

### ID3 算法
ID3 算法的核心是在决策树各个结点上应用信息增益准则选择特征，递归地构建决策树。具体方法是：从根结点（root node）开始，对结点计算所有可能的特征的信息增益，选择信息增益最大的特征作为结点的特征，由该特征的不同取值建立子结点；再对子结点递归地调用以上方法，构建决策树；直到所有特征的信息增益均很小或没有特征可以选择为止。最后得到一棵决策树。ID3 相当于用极大似然法进行概率模型的选择。

输入：训练数据集$D$，特征集$A$阈值$\varepsilon$;
输出：决策树$T$。
1. 若$D$中所有实例属于同一类$C_{k}$，则$T$为单结点树，并将类$C_{k}$作为该结点
2. 若$A=\varnothing$，则$T$为单结点树，并将$D$中实例数最大的类$C_{k}$作为该结点的类标记，返回$T$。
3. 否则，计算$A$中各特征对$D$的信息增益，选择信息增益最大的特征$A_{g}$.
4. 如果$A$的信息增益小于阈值$\varepsilon$，则置$T$为单结点树，并将$D$中实例数最大的类$C_{k}$作为该结点的类标记，返回$T$.
5. 否则，对$A_{g}$的每一可能值$\boldsymbol{a}_{\boldsymbol{i}}$，依$A_{g}=a_{i}$将$D$分割为若干非空子集$D$，将$D$分割为若干非空子集$D_{i}$,将$D_{i}$中实例数最大的类作为标记，构建子结点，由结点及其子结点构成树$T$,返回$T$。
6. 对第$i$个子结点，以$D_{i}$为训练集，以$A-\left\{A_{g}\right\}$为特征集，递归地调用步1~5，得到子树$T_{i}$，返回$T_{i}$。

ID3 算法只有树的生成，所以该算法生成的树容易产生过拟合。
### C4.5 的生成算法
C4.5 算法与 ID3 算法相似，C4.5 算法对 ID3 算法进行了改进。C4.5 在生成的过程中，用信息增益比来选择特征

输入：训练数据集$D$，特征集$A$阈值$\varepsilon$;
输出：决策树$T$。
1. 若$D$中所有实例属于同一类$C_{k}$，则$T$为单结点树，并将类$C_{k}$作为该结点
2. 若$A=\varnothing$，则$T$为单结点树，并将$D$中实例数最大的类$C_{k}$作为该结点的类标记，返回$T$。
3. 否则，计算$A$中各特征对$D$的信息增益比，选择信息增益最大比的特征$A_{g}$.
4. 如果$A$的信息增益小于阈值$\varepsilon$，则置$T$为单结点树，并将$D$中实例数最大的类$C_{k}$作为该结点的类标记，返回$T$.
5. 否则，对$A_{g}$的每一可能值$\boldsymbol{a}_{\boldsymbol{i}}$，依$A_{g}=a_{i}$将$D$分割为若干非空子集$D$，将$D$分割为若干非空子集$D_{i}$,将$D_{i}$中实例数最大的类作为标记，构建子结点，由结点及其子结点构成树$T$,返回$T$。
6. 对第$i$个子结点，以$D_{i}$为训练集，以$A-\left\{A_{g}\right\}$为特征集，递归地调用步1~5，得到子树$T_{i}$，返回$T_{i}$。
## 决策树的剪枝
决策树生成算法递归地产生决策树，直到不能继续下去为止。这样产生的树往往对训练数据的分类很准确，但对未知的测试数据的分类却没有那么准确，即出现过拟合现象。过拟合的原因在于学习时过多地考虑如何提高对训练数据的正确分类，从而构建出过于复杂的决策树。解決这个问题的办法是考虑决策树的复杂度，对已生成的决策树进行简化。

在决策树学习中将已生成的树进行简化的过程称为剪枝（pruning）。具体地，剪枝从已生成的树上裁掉一些子树或叶结点，并将其根结点或父结点作为新的叶结点，从而简化分类树模型。

本节介绍一种简单的决策树学习的剪枝算法。

决策树的剪枝往往通过极小化决策树整体的损失函数（loss function）或代价函数（cost function）来实现。设树$T$的叶结点个数为$|T|$，$t$是树$T$的叶结点，该叶结点有$N_{t}$个样本点，其中$k$类的样本点有$N_{t k}$个，$k=1,2, \cdots, K$，$H_{t}(T)$为叶结点$t$上的经验熵，$\alpha \geqslant 0$为参数，则决策树学习的损失函数可以定义为

$C_{\alpha}(T)=\sum_{t=1}^{|T|} N_{t} H_{t}(T)+\alpha|T|$

其中经验熵为

$H_{t}(T)=-\sum_{k} \frac{N_{t k}}{N_{t}} \log \frac{N_{t k}}{N_{t}}$

在损失函数中，将右端的第 1 项记作

$C(T)=\sum_{t=1}^{|T|} N_{t} H_{t}(T)=-\sum_{t=1}^{|T|} \sum_{k=1}^{K} N_{t k} \log \frac{N_{t k}}{N_{t}}$

这时有

$C_{\alpha}(T)=C(T)+\alpha|T|$

$C(T)$表示模型对训练数据的预测误差，即模型与训练数据的拟合程度，$|T|$表示模型复杂度，参数$\alpha \geqslant 0$控制两者之间的影响。较大的$\alpha$促使选择较简单的模型（树），较小的$\alpha$促使选择较复杂的模型（树）。$\alpha=0$意味着只考虑模型与训练数据的拟合程度，不考虑模型的复杂度。

剪枝，就是当$\alpha$确定时，选择损失函数最小的模型，即损失函数最小的子树。当值确定时，子树越大，往往与训练数据的拟合越好，但是模型的复杂度就越高：相反，子树越小，模型的复杂度就越低，但是往往与训练数据的拟合不好。损失函数正好表示了对两者的平衡。

可以看出，决策树生成只考虑了通过提高信息增益（或信息增益比）对训练数据进行更好的拟合。而决策树剪枝通过优化损失函数还考虑了减小模型复杂度。决策树生成学习局部的模型，而决策树剪枝学习整体的模型。

损失函数的极小化等价于正则化的极大似然估计。所以，利用损失函数最小原则进行剪枝就是用正则化的极大似然估计进行模型选择。

下图是决策树剪枝过程的示意图。下面介绍剪枝算法。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200420203048.png)

输入：生成算法产生的整个树$T$，参数$\alpha$
输出：修剪后的子树$T_{\alpha}$
1. 计算每个结点的经验熵。
2. 递归地从树的叶结点向上回缩。

设一组叶结点回缩到其父结点之前与之后的整体树分别为$T_{B}$与$T_{A}$，其对应的损失函数值分别是$C_{\alpha}\left(T_{B}\right)$和$C_{\alpha}\left(T_{A}\right)$，如果

$C_{\alpha}\left(T_{A}\right) \leqslant C_{\alpha}\left(T_{B}\right)$

则进行剪枝，即将父结点变为新的叶结点。

3. 返回2，直至不能继续为止，得到损失函数最小的子树$T_{\alpha}$。

注意，只需考虑两个树的损失函数的差，其计算可以在局部进行。所以，决策树的剪枝算法可以由一种动态规划的算法实现。
## CART 算法
分类与回归树是应用广泛的决策树学习方法。CART同样由特征选择、树的生成及剪枝组成，既可以用于分类也可以用于回归。以下将用于分类与回归的树统称为決策树。

CART 是在给定输入随机变量$X$条件下输出随机变量$Y$的条件概率分布的学习方法。CART假设决策树是二叉树，内部结点特征的取值为“是”和“否”，左分支是取值为“是”的分支，右分支是取值为“否”的分支。这样的决策树等价于递归地二分每个特征，将输入空间即特征空间划分为有限个单元，并在这些单元上确定预测的概率分布，也就是在输入给定的条件下输出的条件概率分布。

CART算法由以下两步组成：

1. 决策树生成：基于训练数据集生成决策树，生成的决策树要尽量大
2. 决策树剪枝：用验证数据集对已生成的树进行剪枝并选择最优子树，这时用损失函数最小作为剪枝的标准。

### CART 生成
决策树的生成就是递归地构建二又决策树的过程。对回归树用平方误差最小化准则，对分类树用基尼指数（Gini index）最小化准则，进行特征选择，生成二叉树。

#### 回归树的生成
假设$X$与$Y$分别为输入和输出变量，并且$Y$是连续变量，给定训练数据集:

$D=\left\{\left(x_{1}, y_{1}\right),\left(x_{2}, y_{2}\right), \cdots,\left(x_{N}, y_{N}\right)\right\}$

考虑如何生成回归树。

一棵回归树对应着输入空间（即特征空间）的一个划分以及在划分的单元上的输出值。假设已将输入空间划分为$M$个单元$R_{1}, R_{2}, \cdots, R_{M}$,并且在每个单元$R_{m}$上有一个固定的输出值$c_{m}$,于是回归树模型可表示为

$f(x)=\sum_{m=1}^{M} c_{m} I\left(x \in R_{m}\right)$

当输入空间的划分确定时，可以用平方误差$\sum_{x_{i} \in R_{m}}\left(y_{i}-f\left(x_{i}\right)\right)^{2}$来表示回归树对于训练数据的预测误差，用平方误差最小的准则求解每个单元上的最优输出值。易知，单元$R_{m}$上的$c_{m}$的最优值$\hat{c}_{m}$是$R_{m}$上的所有输入实例$\boldsymbol{x}_{i}$对应的输出$y_{i}$的均值，即

$\hat{c}_{m}=\operatorname{ave}\left(y_{i} | x_{i} \in R_{m}\right)$

问题是怎样对输入空间进行划分。这里采用启发式的方法，选择第$j$个变量$x^{(j)}$和它取的值$s$，作为切分变量（splitting variable）和切分点(splitting point),并定义两个区域：

$R_{1}(j, s)=\left\{x | x^{(j)} \leqslant s\right\} \quad$ 和 $\quad R_{2}(j, s)=\left\{x | x^{(j)}>s\right\}$

然后寻找最优切分变量$j$和最优切分点$s$。具体地，求解

$\min _{j, s}\left[\min _{c_{1}} \sum_{x_{i} \in R_{1}(j, s)}\left(y_{i}-c_{1}\right)^{2}+\min _{c_{2}} \sum_{x_{i} \in R_{2}(j, s)}\left(y_{i}-c_{2}\right)^{2}\right]$

对固定输入变量$j$可以找到最优切分点$s$。

$\hat{c}_{1}=\operatorname{ave}\left(y_{i} | x_{i} \in R_{1}(j, s)\right)$

$\hat{c}_{2}=\operatorname{ave}\left(y_{i} | x_{i} \in R_{2}(j, s)\right)$

遍历所有输入变量，找到最优的切分变量$j$,构成一个对$(j, s)$。依此将输入空间划分为两个区域。接着，对每个区域重复上述划分过程，直到满足停止条件为止。这样就生成一棵回归树。这样的回归树通常称为最小二乘回归树（least squares regressionTree），现将算法叙述如下：

最小二乘回归树生成算法：

输入：训练数据集$D$

输出：回归树$f(x)$

在训练数据集所在的输入空间中，递归地将每个区域划分为两个子区域并决定每子区域上的输出值，构建二叉决策树：

1. 选择最优切分变量$j$与切分点$s$，求解:

$\min _{j, s}\left[\min _{c_{1}} \sum_{x_{i} \in R_{1}(j, s)}\left(y_{i}-c_{1}\right)^{2}+\min _{c_{2}} \sum_{x_{i} \in R_{2}(j, s)}\left(y_{i}-c_{2}\right)^{2}\right]$

遍历变量$j$，对固定的切分变量$j$扫描切分点$s$，选择使上式达到最小值的对$(j, s)$。
3. 用选定的$(j, s)$划分区域并决定相应的输出值：

$R_{1}(j, s)=\left\{x | x^{(j)} \leqslant s\right\}, \quad R_{2}(j, s)=\left\{x | x^{(j)}>s\right\}$
$\hat{c}_{m}=\frac{1}{N_{m}} \sum_{x_{i} \in R_{m}(j, s)} y_{i}, \quad x \in R_{m}, \quad m=1,2$

3. 继续对两个子区域调用步骤1，2，直至满足停止条件。
4. 将输入空间划分为$M$个区域$R_{1}, R_{2}, \cdots, R_{M}$，生成决策树

$f(x)=\sum_{m=1}^{M} \hat{c}_{m} I\left(x \in R_{m}\right)$

#### 分类树的生成
分类树用基尼指数选择最优特征，同时决定该特征的最优二值切分点。

分类问题中，假设有$K$个类，样本点属于第$k$类的概率为$p_{k}$,则概率分布的基尼指数定义为:

$\operatorname{Gini}(p)=\sum_{k=1}^{K} p_{k}\left(1-p_{k}\right)=1-\sum_{k=1}^{K} p_{k}^{2}$

对于二类分类问题，若样本点属于第 1 个类的概率是$p$，则概率分布的基尼指数为

$\operatorname{Gini}(p)=2 p(1-p)$

对于给定的样本集合$D$，其基尼指数为:

$\operatorname{Gini}(D)=1-\sum_{k=1}^{K}\left(\frac{\left|C_{k}\right|}{|D|}\right)^{2}$

这里，$C_{k}$是$D$中属于第$k$类的样本子集，$K$是类的个数。

如果样本集合$D$根据特征$A$是否取某一可能值$a$被分割成$D_{1}$和$D_{2}$两部分，即

$D_{1}=\{(x, y) \in D | A(x)=a\}, \quad D_{2}=D-D_{1}$

则在特征$A$的条件下，集合$D$的基尼指数定义为:

$\operatorname{Gini}(D, A)=\frac{\left|D_{1}\right|}{|D|} \operatorname{Gini}\left(D_{1}\right)+\frac{\left|D_{2}\right|}{|D|} \operatorname{Gini}\left(D_{2}\right)$

基尼指数$\operatorname{Gini}(D)$表示集合$D$的不确定性，基尼指数$\operatorname{Gini}(D, A)$表示经$A=a$分割后集合$D$的不确定性。基尼指数值越大，样本集合的不确定性也就越大，这一点与熵相似。

下图显示二类分类问题中基尼指数$\operatorname{Gini}(p)$、熵(单位比特)之半$H(p) / 2$和分类误差率的关系。横坐标表示概率，纵坐标表示损失。可以看出基尼指数和熵之半的曲线很接近，都可以近似地代表分类误差率。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200420223302.png)

CART生成算法：

输入：训练数据集$D$，停止计算的条件

输出：CART 决策树

根据训练数据集，从根结点开始，递归地对每个结点进行以下操作，构建二叉决策树：

1. 设结点的训练数据集为$D$，计算现有特征对该数据集的基尼指数。此时，对每一个特征$A$，对其可能取的每个值$a$，根据样本点对$A=a$的测试为“是”或“否”将$D$分割成$D_{1}$和$D_{2}$两部分，计算$A=a$时的基尼指数。
2. 在所有可能的特征$A$以及它们所有可能的切分点$a$中，选择基尼指数最小的特征及其对应的切分点作为最优特征与最优切分点。依最优特征与最优切分点，从现结点生成两个子结点，将训练数据集依特征分配到两个子结点中去。
3. 对两个子结点递归地调用1,2，直至满足停止条件
4. 生成 CART 决策树。

算法停止计算的条件是结点中的样本个数小于预定阈值，或样本集的基尼指数小于预定阈值（样本基本属于同一类），或者没有更多特征。

### CART 剪枝
CART 剪枝算法从“完全生长”的决策树的底端剪去一些子树，使决策树变小（模型变简单），从而能够对未知数据有更准确的预测。CART 剪枝算法由两步组成：首先从生成算法产生的决策树$T_{0}$底端开始不断剪枝，直到$T_{0}$的根结点，形成一个子树序列$\left\{T_{0}, T_{1}, \cdots, T_{n}\right\}$；然后通过交叉验证法在独立的验证数据集上对子树序列进行测试，从中选择最优子树。

1. 剪枝，形成一个子树序列

在剪枝过程中，计算子树的损失函数

$C_{\alpha}(T)=C(T)+\alpha|T|$

其中，$T$为任意子树，$C(T)$为对训练数据的预测误差（如基尼指数），$|T|$为子树的叶结点个数，$\alpha \geqslant 0$为参数,$C_{\alpha}(T)$为参数是$\alpha$时的子树的整体损失。参数$\alpha$权衡训练数据的拟合程度与模型的复杂度。

对固定的$\alpha$，一定存在使损失函数$C_{\alpha}(T)$最小的子树，将其表示为$T_{\alpha}$。$T_{\alpha}$在损失函数$C_{\alpha}(T)$最小的意义下是最优的。容易验证这样的最优子树是唯一的。当$\alpha$大的时候，最优子树$T_{\alpha}$偏小；当$\alpha$小的时候，最优子树$T_{\alpha}$偏大。极端情况，当$\alpha=0$时，整体树是最优的。当$\alpha \rightarrow \infty$时，根结点组成的单结点树是最优的。

Breiman 等人证明：可以用递归的方法对树进行剪枝。将$\alpha$从小增大,$0=\alpha_{0}<$$\alpha_{1}<\cdots<\alpha_{n}<+\infty$,产生一系列的区间$\left[\alpha_{i}, \alpha_{i+1}\right), i=0,1, \cdots, n$，剪枝得到的子树序列对应着区间$\alpha \in\left[\alpha_{i}, \alpha_{i+1}\right), i=0,1, \cdots, n$的最优子树序列$\left\{T_{0}, T_{1}, \cdots, T_{n}\right\}$，序列中的子树是嵌套的。

具体地，从整体树$T_{0}$开始剪枝。对$T_{0}$的任意内部结点$t$，以$t$为单结点树的损失函数是：

$C_{\alpha}(t)=C(t)+\alpha$

以$t$为根结点的子树$T$的损失函数是:

$C_{\alpha}\left(T_{t}\right)=C\left(T_{t}\right)+\alpha\left|T_{t}\right|$

当$\alpha=0$及$\alpha$充分小时，有不等式

$C_{\alpha}\left(T_{t}\right)<C_{\alpha}(t)$

当$\alpha$增大时，在某一$\alpha$有

$C_{\alpha}\left(T_{t}\right)=C_{\alpha}(t)$

当$\alpha$再增大时，不等式反向。只要$\alpha=\frac{C(t)-C\left(T_{t}\right)}{\left|T_{t}\right|-1}$,$T_{t}$与$t$有相同的损失函数值，而$t$的结点少，因此$t$比$T_{t}$更可取，对$T_{t}$进行剪枝。

为此，对$T_{0}$中每一内部结点$t$，计算

$g(t)=\frac{C(t)-C\left(T_{t}\right)}{\left|T_{t}\right|-1}$

它表示剪枝后整体损失函数减少的程度。在$T_{0}$中剪去$g(t)$最小的$T_{t}$，将得到的子树作为$T_{1}$, 同时将最小的$g(t)$设为$\alpha_{1}$。$T_{1}$为区间$\left[\alpha_{1}, \alpha_{2}\right)$的最优子树。

2. 在剪枝得到的子树序列$T_{0}, T_{1}, \cdots, T_{n}$中通过交又验证选取最优子树$T_{\alpha}$
具体地，利用独立的验证数据集，测试子树序列$T_{0}, T_{1}, \cdots, T_{n}$中各棵子树的平方误差或基尼指数。平方误差或基尼指数最小的决策树被认为是最优的决策树。在子树序列中，每棵子树$T_{1}, T_{2}, \cdots, T_{n}$都对应于一个参数$\alpha_{1}, \alpha_{2}, \cdots, \alpha_{n}$。所以，当最优子树$T_{k}$确定时，对应的$\alpha_{k}$也确定了，即得到最优决策树$T_{\alpha}$。

CART 剪枝算法

输入：CART 算法生成的决策树$T_{0}$;
输出：最优决策树$T_{\alpha}$。

1. 设$k=0, T=T_{0}$。
2. 设 $\alpha=+\infty$。
3. 自下而上地对各内部结点$t$计算$C\left(T_{t}\right),\left|T_{t}\right|$以及：

$\begin{aligned} g(t) &=\frac{C(t)-C\left(T_{t}\right)}{\left|T_{t}\right|-1} \\ \alpha &=\min (\alpha, g(t)) \end{aligned}$

这里，$T_{t}$表示以$t$为根结点的子树，$C\left(T_{t}\right)$是对训练数据的预测误差，$\left|T_{t}\right|$是$T_{t}$的叶结点个数。
4. 对$g(t)=\alpha$的内部结点$t$进行剪枝，并对叶结点$t$以多数表决法决定其类，得到树$T$。
5. 设$k=k+1, \alpha_{k}=\alpha, T_{k}=T$。
6. 如果$T_{k}$不是由根结点及两个叶结点构成的树，则回到步骤2；否则令$T_{k}=T_{n}$。
7. 采用交叉验证法在子树序列$T_{0}, T_{1}, \cdots, T_{n}$中选取最优子树$T_{\alpha}$。
# 决策树(《机器学习》)
## 基本流程
一般的，一棵决策树包含一个根结点、若干个内部结点和若干个叶结点；叶结点对应于决策结果，其他每个结点则对应于一个属性测试；每个结点包含的样本集合根据属性测试的结果被划分到子结点中；根结点包含样本全集。从根结点到每个叶结点的路径对应了一个判定测试序列。决策树学习的目的是为了产生一棵泛化能力强，即处理未见示例能力强的决策树，其基本流程遵循简单且直观的“分而治之”（divide-and-conquer）策略。

决策树的构造是一个递归的过程，有三种情形会导致递归返回：
1. 当前结点包含的样本全属于同一类别，这时直接将该节点标记为叶节点，并设为相应的类别；
2. 当前属性集为空，或是所有样本在所有属性上取值相同，无法划分，这时将该节点标记为叶节点，并将其类别设为该节点所含样本最多的类别；
3. 当前结点包含的样本集合为空，不能划分，这时也将该节点标记为叶节点，并将其类别设为父节点中所含样本最多的类别。

注意(2),(3)这两种情形的处理实质不同：情形(2) 是在利用当前结点的**后验分布**，而情形(3) 则是把父结点的样本分布作为当前结点的**先验分布**。

算法的基本流程如下图所示：

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200417201247.png)

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200417201306.png)
## 划分选择
由算法可看出，决策树学习的关键是如何选择最优划分属性。一般而言，随着划分过程不断进行，我们希望决策树的分支结点所包含的样本尽可能属于同一类别，即结点的“纯度”（purity）越来越高。
### 信息増益(ID3)
“信息熵”（informationentropy）是度量样本集合纯度最常用的一种指标。假定当前样本集合$D$中第$k$类样本所占的比例为$p_{k}(k=1,2, \ldots,|\mathcal{Y}|)$，则$D$的信息熵定义为

$\operatorname{Ent}(D)=-\sum_{k=1}^{|\mathcal{Y}|} p_{k} \log _{2} p_{k}$

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200417203207.png)

$\operatorname{Ent}(D)$的值越小，则$D$的纯度越高。

计算信息熵时约定：若$p=0$，则$p \log _{2} p=0$。

$E n t(D)$的最小值为 0,最大值为$\log _{2}|\mathcal{Y}|$

假定离散属性$a$有$V$个可能的取值$\left\{a^{1}, a^{2}, \ldots, a^{V}\right\}$，若使用$a$来对样本集$D$进行划分，则会产生$V$个分支结点，其中第$\mathcal{v}$个分支结点包含了$D$中所有在属性$a$上取值为$a^{v}$的样本，记为$D^{v}$.我们可根据信息熵公式计算出$D^{v}$的信息熵，再考虑到不同的分支结点所包含的样本数不同，给分支结点赋予权重$\left|D^{v}\right| /|D|$，即样本数越多的分支结点的影响越大，于是可计算出用属性$a$对样本集$D$进行划分所获得的“信息增益”(information gain)：

$\operatorname{Gain}(D, a)=\operatorname{Ent}(D)-\sum_{v=1}^{V} \frac{\left|D^{v}\right|}{|D|} \operatorname{Ent}\left(D^{v}\right)$

一般而言，信息增益越大，则意味着使用属性$a$来进行划分所获得的“纯度提升”越大。因此，我们可用信息增益来进行决策树的划分属性选择，即选择属性:

$a_{*}=\underset{a \in A}{\arg \max } \operatorname{Gain}(D, a)$

著名的ID3决策树学习算法就是以信息增益为准则来选择划分属性。
### 增益率(ID4.5)
实际上，信息增益准则对可取值数目较多的属性有所偏好，为减少这种偏好可能带来的不利影响，著名的 C4.5 决策树算法不直接使用信息增益，而是使用“增益率”（gain ratio）来选择最优划分属性。增益率定义为:

Gain_ratio $(D, a)=\frac{\operatorname{Gain}(D, a)}{\operatorname{IV}(a)}$

其中：

$\mathrm{IV}(a)=-\sum_{v=1}^{V} \frac{\left|D^{v}\right|}{|D|} \log _{2} \frac{\left|D^{v}\right|}{|D|}$
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200418011946.png)
称为属性$a$的“固有值”(intrinsic value)。属性$a$的可能取值数目越多(即$V$越大),则$\mathrm{IV}(a)$的值通常会越大。

需注意的是，增益率准则对可取值数目较少的属性有所偏好，因此，C4.5算法并不是直接选择增益率最大的候选划分属性，而是使用了一个启发式：先从候选划分属性中找出信息增益高于平均水平的属性，再从中选择增益率最高的。

### 基尼指数
CART决策树使用“基尼指数”（Gini index）来选择划分属性。数据集 D 的纯度可用基尼值来度量：

$\begin{aligned} \operatorname{Gini}(D) &=\sum_{k=1}^{|\mathcal{Y}|} \sum_{k^{\prime} \neq k} p_{k} p_{k^{\prime}} \\ &=1-\sum_{k=1}^{|\mathcal{Y}|} p_{k}^{2} \end{aligned}$

直观来说，$\operatorname{Gini}(D)$反映了从数据集$D$中随机抽取两个样本，其类别标记不一致的概率。因此，$\operatorname{Gini}(D)$越小，则数据集$D$的纯度越高。

属性$a$的基尼指数定义为Gini_index $(D, a)=\sum_{v=1}^{V} \frac{\left|D^{v}\right|}{|D|} \operatorname{Gini}\left(D^{v}\right)$

于是，我们在候选属性集合$A$中,选择那个使得划分后基尼指数最小的属性作为最优划分属性，即$a_{*}=\underset{a \in A}{\arg \min }$ Gini_index $(D, a)$。

## 剪枝处理
剪枝(pruning)是决策树学习算法对付“过拟合”的主要手段。在决策树学习中，为了尽可能正确分类训练样本，结点划分过程将不断重复，有时会造成决策树分支过多，这时就可能因训练样本学得“太好”了，以致于把训练集自身的一些特点当作所有数据都具有的一般性质而导致过拟合。因此，可通过主动。去掉一些分支来降低过拟合的风险。

决策树剪枝的基本策略有“预剪枝”(prepruning)和“后剪枝”(post- pruning)。预剪枝是指在决策树生成过程中，对每个结点在划分前先进行估计，若当前结点的划分不能带来决策树泛化性能提升，则停止划分并将当前结点标记为叶结点；后剪枝则是先从训练集生成一棵完整的决策树，然后自底向上地对非叶结点进行考察，若将该结点对应的子树替换为叶结点能带来决策树泛化性能提升，则将该子树替换为叶结点。

### 预剪枝
预剪枝使得决策树的很多分支都没有“展开”，这不仅降低了过拟合的风险，还显著减少了决策树的训练时间开销和测试时间开销。但另一方面，有些分支的当前划分虽不能提升泛化性能、甚至可能导致泛化性能暂时下降，但在其基础上进行的后续划分却有可能导致性能显著提高；预剪枝基于“贪心”本质禁止这些分支展开，给预剪枝决策树带来了欠拟合的风险。
### 后剪枝
后剪枝决策树通常比预剪枝决策树保留了更多的分支。一般情形下，后剪枝决策树的欠拟合风险很小，泛化性能往往优于预剪枝决策树。但后剪枝过程是在生成完全决策树之后进行的，并且要自底向，上地对树中的所有非叶结点进行逐一考察，因此其训练时间开销比未剪枝决策树和预剪枝决策树都要大得多。
## 连续与缺失值
### 连续值处理
由于连续属性的可取值数目不再有限，因此，不能直接根据连续属性的可取值来对结点进行划分。此时，连续属性离散化技术可派上用场。最简单的策略是采用二分法（bi-partition）对连续属性进行处理，这正是 C4.5 决策树算法中采用的机制.
给定样本集$D$和连续属性$a$，假定$a$在$D$上出现了$n$个不同的取值，将这些值从小到大进行排序，记为$\left\{a^{1}, a^{2}, \ldots, a^{n}\right\}$。基于划分点$t$可将$D$分为子集$D_{t}^{-}$和$D_{t}^{+}$，其中$D_{t}^{-}$包含那些在属性$a$上取值不大于$t$的样本，而$D_{t}^{+}$则包含那些在属性$a$上取值大于$t$的样本，显然，对相邻的属性取值$a^{i}$与$a^{i+1}$来说，$t$在区间$\left[a^{i}, a^{i+1}\right)$中取任意值所产生的划分结果相同。因此，对连续属性$a$,我们可考察包含$n-1$个元素的候选划分点集合:

$T_{a}=\left\{\frac{a^{i}+a^{i+1}}{2} | 1 \leqslant i \leqslant n-1\right\}$

>可将划分点设为该属性在训练集中出现的不大于中位点的最大值，从而使得最终决策树使用的划分点都在训练集中出现过.

即把区间$\left[a^{i}, a^{i+1}\right)$的中位点$\frac{a^{i}+a^{i+1}}{2}$作为候选划分点。然后，我们就可像离散属性值一样来考察这些划分点，选取最优的划分点进行样本集合的划分。例如，

$\begin{aligned} \operatorname{Gain}(D, a) &=\max _{t \in T_{a}} \operatorname{Gain}(D, a, t) \\ &=\max _{t \in T_{a}} \operatorname{Ent}(D)-\sum_{\lambda \in\{-,+\}} \frac{\left|D_{t}^{\lambda}\right|}{|D|} \operatorname{Ent}\left(D_{t}^{\lambda}\right) \end{aligned}$
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200418012137.png)
其中$\operatorname{Gain}(D, a, t)$是样本集$D$基于划分点$t$二分后的信息增益。于是，我们就可选择使$\operatorname{Gain}(D, a, t)$最大化的划分点。

需注意的是，与离散属性不同，若当前结点划分属性为连续属性，该属性还可作为其后代结点的划分属性。

例如在父结点上使用了“密度≤0.381”，不会禁止在子结点上使用“密度≤0.294”
### 缺失值处理
现实任务中常会遇到不完整样本，即样本的某些属性值缺失。例如由于诊测成本、隐私保护等因素，患者的医疗数据在某些属性上的取值（如 HIV 测试结果）未知；尤其是在属性数目较多的情况下，往往会有大量样本出现缺失值。如果简单地放弃不完整样本，仅使用无缺失值的样本来进行学习，显然是对数表示算法的空间或时间会随着输入规模的增大而增大

现实任务中常会遇到不完整样本，即样本的某些属性值缺失。例如由于诊测成本、隐私保护等因素，患者的医疗数据在某些属性上的取值（如 HIV 测试结果）未知；尤其是在属性数目较多的情况下，往往会有大量样本出现缺失值。如果简单地放弃不完整样本，仅使用无缺失值的样本来进行学习，显然是对数据信息极大的浪费。

我们需解决两个问题：
1. 如何在属性值缺失的情况下进行划分属性选择？
2. 给定划分属性，若样本在该属性上的值缺失，如何对样本进行划分？

给定训练集$D$和属性$\boldsymbol{a}$,令$\tilde{D}$表示$D$中在属性$a$上没有缺失值的样本子集。对问题（1），显然我们仅可根据$\tilde{D}$来判断属性$a$的优劣。假定属性$a$有$V$个可取值$\left\{a^{1}, a^{2}, \ldots, a^{V}\right\}$，令$\tilde{D}^{v}$表示$\tilde{D}$中在属性$a$上取值为$a^{v}$的样本子集，$\tilde{D}_{k}$表示$\tilde{D}$中属于第$k$类$(k=1,2, \ldots,|\mathcal{Y}|)$的样本子集，则显然有$\tilde{D}=\bigcup_{k=1}^{|\mathcal{Y}|} \tilde{D}_{k}$，$\tilde{D}=\bigcup_{v=1}^{V} \tilde{D}^{v}$，假定我们为每个样本$\boldsymbol{x}$赋予一个权重$w_{x}$，并定义：

$\begin{aligned} \rho &=\frac{\sum_{\boldsymbol{x} \in \tilde{D}} w_{\boldsymbol{x}}}{\sum_{\boldsymbol{x} \in D} w_{\boldsymbol{x}}} \\ \tilde{p}_{k} &=\frac{\sum_{\boldsymbol{x} \in \tilde{D}_{k}} w_{\boldsymbol{x}}}{\sum_{\boldsymbol{x} \in \tilde{D}} w_{\boldsymbol{x}}} \quad(1 \leqslant k \leqslant|\mathcal{Y}|) \\ \tilde{r}_{v} &=\frac{\sum_{\boldsymbol{x} \in \tilde{D}^{v}} w_{\boldsymbol{x}}}{\sum_{\boldsymbol{x} \in \tilde{D}} w_{\boldsymbol{x}}} \quad(1 \leqslant v \leqslant V) \end{aligned}$
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200418012204.png)

直观地看，对属性$a, \rho$，表示无缺失值样本所占的比例，$\tilde{p}_{k}$表示无缺失值样本中第$k$类所占的比例，$\tilde{\boldsymbol{r}}_{\boldsymbol{v}}$则表示无缺失值样本中在属性$a$上取值$a^{v}$的样本所占的比例。显然，$\sum_{k=1}^{|\mathcal{Y}|} \tilde{p}_{k}=1, \sum_{v=1}^{V} \tilde{r}_{v}=1$。

基于。上述定义，我们可将信息增益的计算式推广为：

$\begin{aligned} \operatorname{Gain}(D, a) &=\rho \times \operatorname{Gain}(\tilde{D}, a) \\ &=\rho \times\left(\operatorname{Ent}(\tilde{D})-\sum_{v=1}^{V} \tilde{r}_{v} \operatorname{Ent}\left(\tilde{D}^{v}\right)\right) \end{aligned}$

$\operatorname{Ent}(\tilde{D})=-\sum_{k=1}^{|\mathcal{Y}|} \tilde{p}_{k} \log _{2} \tilde{p}_{k}$
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200418012224.png)
对问题（2），若样本$\boldsymbol{x}$在划分属性$a$上的取值已知，则将$x$划入与其取值对应的子结点，且样本权值在子结点中保持为$w_{x}$。若样本$x$在划分属性$a$上的取值未知，则将$\boldsymbol{x}$同时划入所有子结点，且样本权值在与属性值$a^{v}$对应的子结点中调整为$\tilde{\boldsymbol{r}}_{\boldsymbol{v}} \cdot \boldsymbol{w}_{\boldsymbol{x}}$，直观地看，这就是让同一个样本以不同的概率划入到不同的子结点中去。

C4.5 算法使用了上述解决方案。
## 多变量决策树
若我们把每个属性视为坐标空间中的一个坐标轴，则$d$个属性描述的样本就对应了$d$维空间中的-一个数据点；对样本分类则意味着在这个坐标空间中寻找不同类样本之间的分类边界。决策树所形成的分类边界有一个明显的特点：轴平行（axis- parallel），即它的分类边界由若干个与坐标轴平行的分段组成。
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200418011038.png)

显然，分类边界的每一段都是与坐标轴平行的。这样的分类边界使得学习结果有较好的可解释性，因为每一段划分都直接对应了某个属性取值。但在学习任务的真实分类边界比较复杂时，必须使用很多段划分才能获得较好的近似，此时的决策树会相当复杂，由于要进行大量的属性测试，预测时间开销会很大。
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200418011054.png)

若能使用斜的划分边界，如图中红色线段所示，则决策树模型将大为简化。“多变量决策树”（multivariate decision tree）就是能实现这样的“斜划分”甚至更复杂划分的决策树。这样的多变量决策树亦称“斜决策树”(oblique decision tree).以实现斜划分的多变量决策树为例，在此类决策树中，非叶结点不再是仅对某个属性，而是对属性的线性组合进行测试；换言之，每个非叶结点是一个形如$\sum_{i=1}^{d} w_{i} a_{i}=t$的线性分类器，其中$w_{i}$是属性$a_{i}$的权重，$w_{i}$和$t$可在该结点所含的样本集和属性集上学得。于是，与传统的“单变量决策树”（univariatedecisiontree）不同，在多变量决策树的学习过程中，不是为每个非叶结点寻找一个最优划分属性，而是试图建立一个合适的线性分类器。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200418011428.png)
# 提升方法
提升（`boosting`）方法是一种常用的统计学习方法，应用广泛且有效。在分类问题中，它通过改变训练样本的权重，学习多个分类器，并将这些分类器进行线性组合，提高分类的性能。

## 提升方法 `AdaBoost` 算法
### 提升方法的基本思路
提升方法基于这样一种思想：对于一个复杂任务来说，将多个专家的判断进行适当的综合所得出的判断，要比其中任何一个专家单独的判断好。实际上，就是“三个臭皮匠顶个诸葛亮”的道理。

历史上，Kearns 和 Valiant 首先提出了“强可学习”（strongly learnable）和“弱可学习”（weakly learnable）的概念。指出：在概率近似正确（probably approximately correct, PAC）学习的框架中，一个概念（一个类），如果存在一个多项式的学习算法能够学习它，并且正确率很高，那么就称这个概念是强可学习的；一个概念，如果存在个多项式的学习算法能够学习它，学习的正确率仅比随机猜测略好，那么就称这个概念是弱可学习的。非常有趣的是 Schapire 后来证明强可学习与弱可学习是等价的，也就是说，在 PAC 学习的框架下，一个概念是强可学习的充分必要条件是这个概念是弱可学习的。

这样一来，问题便成为，在学习中，如果已经发现了“弱学习算法”，那么能否将它提升（boost）为“强学习算法”。大家知道，发现弱学习算法通常要比发现强学习算法容易得多。那么如何具体实施提升，便成为开发提升方法时所要解决的问题。关于提升方法的研究很多，有很多算法被提出。最具代表性的是 `Adaboost` 算法(Adaboost algorithm)

对于分类问题而言，给定一个训练样本集，求比较粗糙的分类规则（弱分类器）要比求精确的分类规则（强分类器）容易得多。提升方法就是从弱学习算法出发，反复学习，得到一系列弱分类器（又称为基本分类器），然后组合这些弱分类器，构成一个强分类器。大多数的提升方法都是改变训练数据的概率分布（训练数据的权值分布），针对不同的训练数据分布调用弱学习算法学习一系列弱分类器。

这样，对提升方法来说，有两个问题需要回答：一是在每一轮如何改变训练数据的权值或概率分布；二是如何将弱分类器组合成一个强分类器。关于第 1 个问
题，`Adaboost` 的做法是，提高那些被前一轮弱分类器错误分类样本的权值，而降低那些被正确分类样本的权值。这样一来，那些没有得到正确分类的数据，由于其权值的加大而受到后一轮的弱分类器的更大关注。于是，分类问题被一系列的弱分类器“分而治之”。至于第 2 个问题，即弱分类器的组合，`Adaboost` 采取加权多数表决的方法。具体地，加大分类误差率小的弱分类器的权值，使其在表决中起较大的作用；减小分类误差率大的弱分类器的权值，使其在表决中起较小的作用。

### `AdaBoost` 算法
假设给定一个二类分类的训练数据集:

$T=\left\{\left(x_{1}, y_{1}\right),\left(x_{2}, y_{2}\right), \cdots,\left(x_{N}, y_{N}\right)\right\}$

其中，每个样本点由实例与标记组成。实例$x_{i} \in \mathcal{X} \subseteq \mathbf{R}^{n}$,标记$y_{i} \in \mathcal{Y}=\{-1,+1\}$,$\mathcal{X}$是实例空间，$\mathcal{Y}$是标记集合。`Adaboost` 利用以下算法，从训练数据中学习一系列弱分类器或基本分类器，并将这些弱分类器线性组合成为一个强分类器。

输入：训练数据集 T=$T=\left\{\left(x_{1}, y_{1}\right),\left(x_{2}, y_{2}\right), \cdots,\left(x_{N}, y_{N}\right)\right\}$，其中$x_{i} \in \mathcal{X} \subseteq \mathbf{R}^{n}$，$y_{i} \in$$\mathcal{Y}=\{-1,+1\}$，弱学习算法；
输出：最终分类器$G(x)$。
1. 初始化训练数据的权值分布

$D_{1}=\left(w_{11}, \cdots, w_{1 i}, \cdots, w_{1 N}\right), \quad w_{1 i}=\frac{1}{N}, \quad i=1,2, \cdots, N$

2. 对 $m=1,2, \cdots, M$

(a)使用具有权值分布$D_{m}$的训练数据集学习，得到基本分类器

$G_{m}(x): \mathcal{X} \rightarrow\{-1,+1\}$

(b)计算$G_{m}(x)$在训练数据集上的分类误差率

$e_{m}=\sum_{i=1}^{N} P\left(G_{m}\left(x_{i}\right) \neq y_{i}\right)=\sum_{i=1}^{N} w_{m i} I\left(G_{m}\left(x_{i}\right) \neq y_{i}\right)$

(c)计算$G_{m}(x)$的系数

$\alpha_{m}=\frac{1}{2} \log \frac{1-e_{m}}{e_{m}}$

这里的对数是自然对数。

(d)更新训练数据集的权值分布

$D_{m+1}=\left(w_{m+1,1}, \cdots, w_{m+1, i}, \cdots, w_{m+1, N}\right)$

$w_{m+1, i}=\frac{w_{m i}}{Z_{m}} \exp \left(-\alpha_{m} y_{i} G_{m}\left(x_{i}\right)\right), \quad i=1,2, \cdots, N$

这里，$Z_{m}$是规范化因子:

$Z_{m}=\sum_{i=1}^{N} w_{m i} \exp \left(-\alpha_{m} y_{i} G_{m}\left(x_{i}\right)\right)$

它使$D_{m+1}$成为一个概率分布。
3. 构建基本分类器的线性组合

$f(x)=\sum_{m=1}^{M} \alpha_{m} G_{m}(x)$

得到最终分类器:

$\begin{aligned} G(x) &=\operatorname{sign}(f(x)) \\ &=\operatorname{sign}\left(\sum_{m=1}^{M} \alpha_{m} G_{m}(x)\right) \end{aligned}$

对 `Adaboost` 算法作如下说明：

步骤(1) 假设训练数据集具有均匀的权值分布，即每个训练样本在基本分类器的学习中作用相同，这一假设保证第 1 步能够在原始数据上学习基本分类器$G_{m}(x)$。

步骤(2) `Adaboost` 反复学习基本分类器，在每一轮$m=1,2, \cdots, M$顺次地执行下列操作:
(a) 使用当前分布$D_{m}$加权的训练数据集，学习基本分类器$G_{m}(x)$

(b)计算基本分类器$G_{m}(x)$在加权训练数据集上的分类误差率

$\begin{aligned} e_{m} &=\sum_{i=1}^{N} P\left(G_{m}\left(x_{i}\right) \neq y_{i}\right) \\ &=\sum_{G_{m}\left(x_{i}\right) \neq y_{i}} w_{m i} \end{aligned}$

这里，$w_{m i}$表示第$m$轮中第$i$个实例的权值，$\sum_{i=1}^{N} w_{m i}=1$.这表明，$G_{m}(x)$在加权的训练数据集上的分类误差率是被$G_{m}(x)$误分类样本的权值之和，由此可以看出数据权值分布$D_{m}$与基本分类器$G_{m}(x)$的分类误差率的关系。

(c)计算基本分类器$G_{m}(x)$的系数$\alpha_{m}$。$\alpha_{m}$表示$G_{m}(x)$在最终分类器中的重要性。当$e_{m} \leqslant \frac{1}{2}$时，$\alpha_{m} \geqslant 0$, 并且$\alpha_{m}$随着$e_{m}$的减小而增大，所以分类误差率越小的基本分类器在最终分类器中的作用越大。

(d)更新训练数据的权值分布为下一轮作准备。

$w_{m+1, i}=\left\{\begin{array}{ll}\frac{w_{m i}}{Z_{m}} \mathrm{e}^{-\alpha_{m}}, & G_{m}\left(x_{i}\right)=y_{i} \\ \frac{w_{m i}}{Z_{m}} \mathrm{e}^{\alpha_{m}}, & G_{m}\left(x_{i}\right) \neq y_{i}\end{array}\right.$

由此可知，被基本分类器$G_{m}(x)$误分类样本的权值得以扩大，而被正确分类样本的权值却得以缩小。两相比较，知误分类样本的权值被放大$\mathrm{e}^{2 \alpha_{m}}=\frac{1-e_{m}}{e_{m}}$倍。因此，误分类样本在下一轮学习中起更大的作用。不改变所给的训练数据，而不断改变训练数据权值的分布，使得训练数据在基本分类器的学习中起不同的作用，这是`Adaboost` 的一个特点。

步骤(3) 线性组合$f(x)$实现$M$个基本分类器的加权表決。系数$\alpha_{m}$表示了基本分类器$G_{m}(x)$的重要性，这里，所有$\alpha_{m}$之和并不为 1。$f(x)$的符号决定实例$x$的类，$f(x)$的绝对值表示分类的确信度。利用基本分类器的线性组合构建最终分类器是 `Adaboost` 的另一特点。

## `AdaBoost` 算法的训练误差分析
`Adaboost` 最基本的性质是它能在学习过程中不断减少训练误差，即在训练数据集上的分类误差率。

可以在每一轮选取适当的$G_{m}$使得$Z_{m}$最小，从而使训练误差下降最快。

对二类分类问题，在此条件下 `AdaBoost` 的训练误差是以指数速率下降的。

注意，`Adaboost` 算法不需要知道下界。与一些早期的提升方法不同，`Adaboost` 具有适应性，即它能适应弱分类器各自的训练误差率。这也是它的名称（适应的提升）的由来，`Ada` 是 Adaptive 的简写。

## `AdaBoost` 算法的解释
`Adaboost` 算法还有另一个解释，即可以认为 `Adaboost` 算法是模型为加法模型损失函数为指数函数、学习算法为前向分步算法时的二类分类学习方法。

### 前向分步算法
考虑加法模型（additive model),

$f(x)=\sum_{m=1}^{M} \beta_{m} b\left(x ; \gamma_{m}\right)$

其中，$b\left(x ; \gamma_{m}\right)$为基函数，$\gamma_{m}$为基函数的参数，$\beta_{m}$为基函数的系数。显然，$f(x)=\sum_{m=1}^{M} \alpha_{m} G_{m}(x)$是一个加法模型.

在给定训练数据及损失函数$L(y, f(x))$的条件下，学习加法模型$f(x)$成为经验风险极小化即损失函数极小化问题

$\min _{\beta_{m}, \gamma_{m}} \sum_{i=1}^{N} L\left(y_{i}, \sum_{m=1}^{M} \beta_{m} b\left(x_{i} ; \gamma_{m}\right)\right)$

通常这是一个复杂的优化问题。前向分步算法（forward stagewise algorithm）求解这一优化问题的想法是：因为学习的是加法模型，如果能够从前向后，每一步只学习一个基函数及其系数，逐步逼近优化目标函数式$\min _{\beta_{m}, \gamma_{m}} \sum_{i=1}^{N} L\left(y_{i}, \sum_{m=1}^{M} \beta_{m} b\left(x_{i} ; \gamma_{m}\right)\right)$，那么就可以简化优化的复杂度。具体地，每步只需优化如下损失函数：

$\min _{\beta, \gamma} \sum_{i=1}^{N} L\left(y_{i}, \beta b\left(x_{i} ; \gamma\right)\right)$

给定训练数据集$T=\left\{\left(x_{1}, y_{1}\right),\left(x_{2}, y_{2}\right), \cdots,\left(x_{N}, y_{N}\right)\right\}$。损失函数$L(y, f(x))$和基函数的集合$\{b(x ; \gamma)\}$，学习加法模型$f(x)$的前向分步算法如下。

输入：训练数据集$T=\left\{\left(x_{1}, y_{1}\right),\left(x_{2}, y_{2}\right), \cdots,\left(x_{N}, y_{N}\right)\right\}$；损失函数$L(y, f(x))$；基函数集$\{b(x ; \gamma)\}$
输出：加法模型$f(x)$

1. 初始化$f_{0}(x)=0$
2. 对 $m=1,2, \cdots, M$

(a) $\left(\beta_{m}, \gamma_{m}\right)=\arg \min _{\beta, \gamma} \sum_{i=1}^{N} L\left(y_{i}, f_{m-1}\left(x_{i}\right)+\beta b\left(x_{i} ; \gamma\right)\right)$

得到参数 $\beta_{m}, \gamma_{m^{\circ}}$

(b)更新

$f_{m}(x)=f_{m-1}(x)+\beta_{m} b\left(x ; \gamma_{m}\right)$

3. 得到加法模型

$f(x)=f_{M}(x)=\sum_{m=1}^{M} \beta_{m} b\left(x ; \gamma_{m}\right)$

这样，前向分步算法将同时求解从$m=1$到$M$所有参数$\beta_{m}, \gamma_{m}$的优化问题简化为逐次求解各个$\beta_{m}, \gamma_{m}$的优化问题

### 前向分步算法与 `AdaBoost`
由前向分步算法可以推导出 `Adaboost`，用定理叙述这一关系:

`Adaboost` 算法是前向分步加法算法的特例。这时，模型是由基本分类器组成的加法模型，损失函数是指数函数。

## 提升树
提升树是以分类树或回归树为基本分类器的提升方法。提升树被认为是统计学习中性能最好的方法之一。

### 提升树模型
提升方法实际采用加法模型（即基函数的线性组合）与前向分步算法。以决策树为基函数的提升方法称为提升树（boosting tree）。对分类问题决策树是二叉分类树，对回归问题決策树是二又回归树。提升树模型可以表示为决策树的加法模型：

$f_{M}(x)=\sum_{m=1}^{M} T\left(x ; \Theta_{m}\right)$

其中，$T\left(x ; \Theta_{m}\right)$表示决策树，$\Theta_{m}$为决策树的参数，$M$为树的个数。
### 提升树算法
提升树算法釆用前向分步算法。首先确定初始提升树$f_{0}(x)=0$, 第$m$步的模型是：

$f_{m}(x)=f_{m-1}(x)+T\left(x ; \Theta_{m}\right)$

其中，$f_{m-1}(x)$为当前模型，通过经验风险极小化确定下一棵决策树的参数$\Theta_{m}$：

$\hat{\Theta}_{m}=\arg \min _{\Theta} \sum_{i=1}^{N} L\left(y_{i}, f_{m-1}\left(x_{i}\right)+T\left(x_{i} ; \Theta_{m}\right)\right)$

由于树的线性组合可以很好地拟合训练数据，即使数据中的输入与输出之间的关系很复杂也是如此，所以提升树是一个高功能的学习算法。

下面讨论针对不同问题的提升树学习算法，其主要区别在于使用的损失函数不同。包括用平方误差损失函数的回归问题，用指数损失函数的分类问题，以及用一般损失函数的一般决策问题。

对于二类分类问题，提升树算法只需将 `Adaboost` 算法中的基本分类器限制为二类分类树即可，可以说这时的提升树算法是 `AdaBoost` 算法的特殊情况，这里不再细述。下面叙述回归问题的提升树:

已知一个训练数据集$T=\left\{\left(x_{1}, y_{1}\right),\left(x_{2}, y_{2}\right), \cdots,\left(x_{N}, y_{N}\right)\right\}$, $x_{i} \in \mathcal{X} \subseteq \mathbf{R}^{n}$，$\mathcal{X}$配为输入空间，$y_{i} \in \mathcal{Y} \subseteq \mathbf{R}$, $\mathcal{Y}$为输出空间。如果将输入空间划分为$J$个互不相交的区域$R_{1}, R_{2}, \cdots, R_{J}$，并且在每个区域上确定输出的常量$\c_{j}$，那么树可表示为:

$T(x ; \Theta)=\sum_{j=1}^{J} c_{j} I\left(x \in R_{j}\right)$

其中，参数$\Theta=\left\{\left(R_{1}, c_{1}\right),\left(R_{2}, c_{2}\right), \cdots,\left(R_{J}, c_{J}\right)\right\}$表示树的区域划分和各区域上的常数。$J$是回归树的复杂度即叶结点个数。

回归问题提升树使用以下前向分步算法：

$f_{0}(x)=0$
$f_{m}(x)=f_{m-1}(x)+T\left(x ; \Theta_{m}\right), \quad m=1,2, \cdots, M$
$f_{M}(x)=\sum_{m=1}^{M} T\left(x ; \Theta_{m}\right)$

在前向分步算法的第$m$步，给定当前模型$f_{m-1}(x)$，需求解

$\hat{\Theta}_{m}=\arg \min _{\Theta} \sum_{i=1}^{N} L\left(y_{i}, f_{m-1}\left(x_{i}\right)+T\left(x_{i} ; \Theta_{m}\right)\right)$

得到$\hat{\Theta}_{m}$，即第$m$棵树的参数。

当采用平方误差损失函数时，

$L(y, f(x))=(y-f(x))^{2}$

其损失变为

$\begin{aligned} L\left(y, f_{m-1}(x)+T\left(x ; \Theta_{m}\right)\right) &=\left[y-f_{m-1}(x)-T\left(x ; \Theta_{m}\right)\right]^{2} \\ &=\left[r-T\left(x ; \Theta_{m}\right)\right]^{2} \end{aligned}$

这里，

$r=y-f_{m-1}(x)$

是当前模型拟合数据的残差（residual）。所以，对回归问题的提升树算法来说，只需简单地拟合当前模型的残差。这样，算法是相当简单的。现将回归问题的提升树算法叙述如下。

输入：训练数据集$T=\left\{\left(x_{1}, y_{1}\right),\left(x_{2}, y_{2}\right), \cdots,\left(x_{N}, y_{N}\right)\right\}, x_{i} \in \mathcal{X} \subseteq \mathbf{R}^{n}, y_{i} \in \mathcal{Y} \subseteq \mathbf{R}$
输出：提升树$f_{M}(x)$

1. 初始化$f_{0}(x)=0$
2. 对$m=1,2, \cdots, M$

(a) 计算残差:

$r_{m i}=y_{i}-f_{m-1}\left(x_{i}\right), \quad i=1,2, \cdots, N$

(b)拟合残差$r_{m i}$学习一个回归树，得到$T\left(x ; \Theta_{m}\right)$。

(c)更新 $f_{m}(x)=f_{m-1}(x)+T\left(x ; \Theta_{m}\right)$

### 梯度提升
提升树利用加法模型与前向分步算法实现学习的优化过程。当损失函数是平方损失和指数损失函数时，每一步优化是很简单的。但对一般损失函数而言，往往每一步优化并不那么容易。针对这一问题，Freidman 提出了梯度提升（gradient boosting）算法。这是利用最速下降法的近似方法，其关键是利用损失函数的负梯度在当前模型的值：

$-\left[\frac{\partial L\left(y, f\left(x_{i}\right)\right)}{\partial f\left(x_{i}\right)}\right]_{f(x)=f_{m-1}(x)}$

作为回归问题提升树算法中的残差的近似值，拟合一个回归树。

梯度提升算法：

输入：训练数据集$T=\left\{\left(x_{1}, y_{1}\right),\left(x_{2}, y_{2}\right), \cdots,\left(x_{N}, y_{N}\right)\right\}, x_{i} \in \mathcal{X} \subseteq \mathbf{R}^{n}, y_{i} \in \mathcal{Y} \subseteq \mathbf{R}$，损失函数$L(y, f(x))$

输出：回归树$\hat{f}(x)$。

1. 初始化

$f_{0}(x)=\arg \min _{c} \sum_{i=1}^{N} L\left(y_{i}, c\right)$

2. 对$m=1,2, \cdots, M$：

(a)对$i=1,2, \cdots, N$，计算:

$r_{m i}=-\left[\frac{\partial L\left(y_{i}, f\left(x_{i}\right)\right)}{\partial f\left(x_{i}\right)}\right]_{f(x)=f_{m-1}(x)}$

(b)对$r_{m i}$拟合一个回归树，得到第$m$棵树的叶结点区域$R_{m j}, j=1,2, \cdots, J$

(c)对$j=1,2, \cdots, J$，计算:

$c_{m j}=\arg \min _{c} \sum_{x_{i} \in R_{m j}} L\left(y_{i}, f_{m-1}\left(x_{i}\right)+c\right)$

(d)更新$f_{m}(x)=f_{m-1}(x)+\sum_{j=1}^{J} c_{m j} I\left(x \in R_{m j}\right)$

3. 得到回归树：
   
$\hat{f}(x)=f_{M}(x)=\sum_{m=1}^{M} \sum_{j=1}^{J} c_{m j} I\left(x \in R_{m j}\right)$

算法第 1 步初始化，估计使损失函数极小化的常数值，它是只有一个根结点的树。第 2 (a）步计算损失函数的负梯度在当前模型的值，将它作为残差的估计。对于平方损失函数，它就是通常所说的残差；对于一般损失函数，它就是残差的近似值。第 2 (b）步估计回归树叶结点区域，以拟合残差的近似值。第 2（）步利用线性搜索估计叶结点区域的值，使损失函数极小化。第 2 (d）步更新回归树。第 3 步得到输出的最终模型 J (x)
# 集成学习(《机器学习》)
## 个体与集成
集成学习（ensemble learning）通过构建并结合多个学习器来完成学习任务，有时也被称为多分类器系统（multi-classifier system）、基于委员会的学习（committee-based learning）等。

下图显示出集成学习的一般结构：先产生一组“个体学习器”（individual learner），再用某种策略将它们结合起来。个体学习器通常由一个现有的学习算法从训练数据产生，例如 `C4.5` 决策树算法、`BP` 神经网络算法等，此时集成中只包含同种类型的个体学习器，例如“决策树集成”中全是决策树，“神经网络集成”中全是神经网络，这样的集成是“同质”的（homogeneous）。同质集成中的个体学习器亦称“基学习器”（base learner）相应的学习算法称为“基学习算法”（base learning algorithm）。集成也可包含不同类型的个体学习器，例如同时包含决策树和神经网络，这样的集成是“异质”的（heterogenous）。异质集成中的个体学习器由不同的学习算法生成，这时就不再有基学习算法；相应的，个体学习器一般不称为基学习器，常称为“组件学习器”（component learner）或直接称为个体学习器。
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200426221137.png)

集成学习通过将多个学习器进行结合，常可获得比单一学习器显著优越的泛化性能。这对“弱学习器”（weak learner）尤为明显，因此集成学习的很多理论研究都是针对弱学习器进行的，而基学习器有时也被直接称为弱学习器。但需注意的是，虽然从理论上来说使用弱学习器集成足以获得好的性能，但在实践中出于种种考虑，例如希望使用较少的个体学习器，或是重用关于常见学习器的一些经验等，人们往往会使用比较强的学习器。

>弱学习器常指泛化性能略优于随机猜测的学习器；例如在二分类问题上精度略高于 50%的分类器。

要获得好的集成，个体学习器应“好而不同”，即个体学习器要有一定的“准确性”，即学习器不能太坏，并且要有“多样性”（diversity），即学习器间具有差异。

>个体学习器至少不差于弱学习器。

假设基分类器的错误率相互独立，则随着集成中个体分类器数目$T$的增大，集成的错误率将指数级下降，最终趋向于零。

然而我们必须注意到，上面的分析有一个关键假设：基学习器的误差相互独立。在现实任务中，个体学习器是为解决同一个问题训练出来的，它们显然不可能相互独立！事实上，个体学习器的“准确性”和“多样性”本身就存在冲突。一般的，准确性很高之后，要增加多样性就需牺牲准确性。事实上，如何产生并结合“好而不同”的个体学习器，恰是集成学习研究的核心。

根据个体学习器的生成方式，目前的集成学习方法大致可分为两大类，即个体学习器间存在强依赖关系、必须串行生成的序列化方法，以及个体学习器间不存在强依赖关系、可同时生成的并行化方法；前者的代表是 `Boosting`，后者的代表是 `Bagging` 和“随机森林”(Random Forest)

## `Boosting`
`Boosting` 是一族可将弱学习器提升为强学习器的算法。这族算法的工作机制类似：先从初始训练集训练出一个基学习器，再根据基学习器的表现对训练样本分布进行调整，使得先前基学习器做错的训练样本在后续受到更多关注，然后基于调整后的样本分布来训练下一个基学习器；如此重复进行，直至基学习器数目达到事先指定的值$T$，最终将这$T$个基学习器进行加权结合。

Boosting 族算法最著名的代表是`AdaBoost`,其描述如图所示，其中$y_{i} \in\{-1,+1\}$, $f$是真实函数。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200426222456.png)

`AdaBoost`算法有多种推导方式，比较容易理解的是基于“加性模型”（additive model），即基学习器的线性组合

$H(\boldsymbol{x})=\sum_{t=1}^{T} \alpha_{t} h_{t}(\boldsymbol{x})$

来最小化指数损失函数(exponential loss function):

$\ell_{\exp }(H | \mathcal{D})=\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H(\boldsymbol{x})}\right]$

若$H(\boldsymbol{x})$能令指数损失函数最小化，则考虑损失函数对$H(\boldsymbol{x})$的偏导：

$\frac{\partial \ell_{\exp }(H | \mathcal{D})}{\partial H(\boldsymbol{x})}=-e^{-H(\boldsymbol{x})} P(f(\boldsymbol{x})=1 | \boldsymbol{x})+e^{H(\boldsymbol{x})} P(f(\boldsymbol{x})=-1 | \boldsymbol{x})$

令上式为零可解得：

$H(\boldsymbol{x})=\frac{1}{2} \ln \frac{P(f(x)=1 | \boldsymbol{x})}{P(f(x)=-1 | \boldsymbol{x})}$

因此，有

$\begin{aligned} \operatorname{sign}(H(\boldsymbol{x})) &=\operatorname{sign}\left(\frac{1}{2} \ln \frac{P(f(x)=1 | \boldsymbol{x})}{P(f(x)=-1 | \boldsymbol{x})}\right) \\ &=\left\{\begin{array}{ll}1, & P(f(x)=1 | \boldsymbol{x})>P(f(x)=-1 | \boldsymbol{x}) \\ -1, & P(f(x)=1 | \boldsymbol{x})<P(f(x)=-1 | \boldsymbol{x})\end{array}\right.\\ &=\underset{y \in\{-1,1\}}{\arg \max } P(f(x)=y | \boldsymbol{x}) \end{aligned}$

这意味着$\operatorname{sign}(H(\boldsymbol{x}))$达到了贝叶斯最优错误率，换言之，若指数损失函数最小化,则分类错误率也将最小化;这说明指数损失函数是分类任务原本$0 / 1$损失函数的一致的（consistent）替代损失函数。由于这个替代函数有更好的数学性质,例如它是连续可微函数,因此我们用它替代$0 / 1$损失函数作为优化目标。

在 `Adaboost` 算法中，第一个基分类器$h_{1}$是通过直接将基学习算法用于初始数据分布而得；此后迭代地生成$h_{t}$和$\alpha_{t}$，当基分类器 $h_{t}$基于分布$\mathcal{D}_{t}$产生后，该基分类器的权重$\alpha_{t}$应使得$\alpha_{t} h_{t}$最小化指数损失函数。

`AdaBoost`算法在获得$H_{t-1}$之后样本分布将进行调整，使下一轮的基学习器$h_{t}$能纠正$H_{t-1}$的一些错误。理想的$h_{t}$能纠正$H_{t-1}$的全部错误。

理想的$h_{t}$将在分布$\mathcal{D}_{t}$下最小化分类误差。因此，弱分类器将基于分布$\mathcal{D}_{t}$来训练，且针对$\mathcal{D}_{t}$的分类误差应小于 0.5. 这在一定程度上类似“残差逼近”的思想。

`Boosting`算法要求基学习器能对特定的数据分布进行学习，这可通过“重赋权法”（re-weighting）实施，即在训练过程的每一轮中，根据样本分布为每个训练样本重新赋予一个权重。对无法接受带权样本的基学习算法，则可通过“重采样法”（re-sampling）来处理，即在每一轮学习中，根据样本分布对训练集重新进行采样，再用重采样而得的样本集对基学习器进行训练。一般而言，这两种做法没有显著的优劣差别。需注意的是，`Boosting` 算法在训练的每一轮都要检査当前生成的基学习器是否满足基本条件（例如图 8.3 的第 5 行，检查当前基分类器是否是比随机猜测好），一旦条件不满足，则当前基学习器即被抛弃，且学习过程停止。在此种情形下，初始设置的学习轮数也许还远未达到，可能导致最终集成中只包含很少的基学习器而性能不佳。若采用“重采样法”，则可获得“重启动”机会以避免训练过程过早停止，即在抛弃不满足条件的当前基学习器之后，可根据当前分布重新对训练样本进行采样，再基于新的采样结果重新训练出基学习器，从而使得学习过程可以持续到预设的$T$轮完成。

从偏差一方差分解的角度看，`Boosting` 主要关注降低偏差，因此 `Boosting` 能基于泛化性能相当弱的学习器构建出很强的集成。

## `Bagging`与随机森林
欲得到泛化性能强的集成，集成中的个体学习器应尽可能相互独立；虽然“独立”在现实任务中无法做到，但可以设法使基学习器尽可能具有较大的差异。给定一个训练数据集，一种可能的做法是对训练样本进行采样，产生出若干个不同的子集，再从每个数据子集中训练出一个基学习器。这样，由于训练数据不同，我们获得的基学习器可望具有比较大的差异。然而，为获得好的集成，我们同时还希望个体学习器不能太差。如果采样出的每个子集都完全不同，则每个基学习器只用到了一小部分训练数据，甚至不足以进行有效学习，这显然无法确保产生出比较好的基学习器。为解决这个问题，我们可考虑使用相互有交叠的采样子集。

### `Bagging`
`Bagging`是并行式集成学习方法最著名的代表。从名字即可看出，它直接基于自助采样法（bootstrap sampling）给定包含`m`个样本的数据集，我们先随机取出一个样本放入采样集中，再把该样本放回初始数据集，使得下次采样时该样本仍有可能被选中，这样，经过`m`次随机采样操作，我们得到含`m`个样本的采样集，初始训练集中有的样本在采样集里多次出现，有的则从未出现。初始训练集中约有 63.2% 的样本出现在采样集中。

照这样，我们可采样出$T$个含$m$个训练样本的采样集，然后基于每个采样集训练出一个基学习器，再将这些基学习器进行结合。这就是`Bagging`的基本流程。在对预测输出进行结合时，`Bagging`通常对分类任务使用简单投票法，对回归任务使用简单平均法。即每个基学习器使用相同权重的投票、平均。若分类预测时出现两个类收到同样票数的情形，则最简单的做法是随机选择一个，也可进一步考察学习器投票的置信度来确定最终胜者。`Bagging` 的算法描述如图所示。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200426231522.png)

假定基学习器的计算复杂度为$O(m)$, 则 `Bagging` 的复杂度大致为$T(O(m)+O(s))$,考虑到采样与投票/平均过程的复杂度$O(s)$很小，而$T$通常是一个不太大的常数，因此，训练一个 `Bagging` 集成与直接使用基学习算法训练一个学习器的复杂度同阶，这说明 `Bagging` 是一个很高效的集成学习算法。另外，与标准 `AdaBoost` 只适用于二分类任务不同，`Bagging` 能不经修改地用于多分类、回归等任务。

值得一提的是，自助米样过程还给 `Bagging` 带来了另一个优点：由于每个基学习器只使用了初始训练集中约 63.2%的样本，剩下约 36.8%的样本可用作验证集来对泛化性能进行“包外估计”(out- of-bag estimate).

事实上，包外样本还有许多其他用途。例如当基学习器是决策树时，可使用包外样本来辅助剪枝，或用于估计决策树中各结点的后验概率以辅助对零训练样本结点的处理；当基学习器是神经网络时，可使用包外样本来辅助早期停止以减小过拟合风险。

从偏差-方差分解的角度看，`Bagging` 主要关注降低方差，因此它在不剪枝决策树、神经网络等易受样本扰动的学习器上效用更为明显。

### 随机森林
随机森林(Random Forest，简称 RF)是 `Bagging` 的一个扩展变体。`RF` 在以决策树为基学习器构建 `Bagging` 集成的基础上，进一步在决策树的训练过程中引入了随机属性选择。具体来说，传统决策树在选择划分属性时是在当前结点的属性集合（假定有$d$个属性）中选择一个最优属性；而在`RF`中，对基决策树的每个结点，先从该结点的属性集合中随机选择一个包含$k$个属性的子集，然后再从这个子集中选择一个最优属性用于划分。这里的参数$k$控制了随机性的引入程度：若令$k=d$，则基决策树的构建与传统决策树相同；若令$k=1$, 则是随机选择一个属性用于划分；一般情况下，推荐值$k=\log _{2} d$。

随机森林简单、容易实现、计算开销小，令人惊奇的是，它在很多现实任务中展现出强大的性能，被誉为“代表集成学习技术水平的方法”。可以看出随机森林对 `Bagging` 只做了小改动，但是与 `Bagging` 中基学习器的“多样性”仅通过样本扰动（通过对初始训练集采样）而来不同，随机森林中基学习器的多样性不仅来自样本扰动，还来自属性扰动，这就使得最终集成的泛化性能可通过个体学习器之间差异度的增加而进一步提升。

随机森林的收敛性与`Bagging`相似。如图所示，随机森林的起始性能往往相对较差，特别是在集成中只包含一个基学习器时。这很容易理解，为通过引入属性扰动，随机森林中个体学习器的性能往往有所降低。然而，随着个体学习器数目的增加，随机森林通常会收敛到更低的泛化误差。值得一提的是，随机森林的训练效率常优于 `Bagging`，因为在个体决策树的构建过程中，`Bagging` 使用的是“确定型”决策树，在选择划分属性时要对结点的所有属性进行考察，而随机森林使用的“随机型”决策树则只需考察一个属性子集。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200426232358.png)

## 结合策略
学习器结合可能会从三个方面带来好处: 首先，从统计的方面来看，由于学习任务的假设空间往往很大，可能有多个假设在训练集上达到同等性能，此时若使用单学习器可能因误选而导致泛化性能不佳，结合多个学习器则会减小这一风险；第二，从计算的方面来看，学习算法往往会陷入局部极小，有的局部极小点所对应的泛化性能可能很糟糕，而通过多次运行之后进行结合，可降低陷入糟糕局部极小点的风险；第三，从表示的方面来看，某些学习任务的真实假设可能不在当前学习算法所考虑的假设空间中，此时若使用单学习器则肯定无效，而通过结合多个学习器，由于相应的假设空间有所扩大，有可能学得更好的近似。下图给出了一个直观示意图。
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200426233039.png)

假定集成包含$T$个基学习器$\left\{h_{1}, h_{2}, \ldots, h_{T}\right\}$，其中$h_{i}$在示例$\boldsymbol{x}$上的输出为$h_{i}(\boldsymbol{x})$。本节介绍几种对$h_{i}$进行结合的常见策略。

### 平均法
对数值型输出$h_{i}(\boldsymbol{x}) \in \mathbb{R}$，最常见的结合策略是使用平均法（averaging)
#### 简单平均法（simple averaging)
$H(\boldsymbol{x})=\frac{1}{T} \sum_{i=1}^{T} h_{i}(\boldsymbol{x})$
#### 加权平均法（weighted averaging)
$H(\boldsymbol{x})=\sum_{i=1}^{T} w_{i} h_{i}(\boldsymbol{x})$

其中$w_{i}$是个体学习器$h_{i}$的权重，通常要求$w_{i} \geqslant 0$,$\sum_{i=1}^{T} w_{i}=1$

>Breiman在研究 `Stacking` 回归时发现，必须使用非负权重才能确保集成性能优于单一最佳个体学习器，因此在集成学习中一般对学习器的权重施以非负约束

显然,简单平均法是加权平均法令$w_{i}=1 / T$的特例。它在集成学习中具有特别的意义，集成学习中的各种结合方法都可视为其特例或变体。事实上，加权平均法可认为是集成学习研究的基本出发点，对给定的基学习器，不同的集成学习方法可视为通过不同的方式来确定加权平均法中的基学习器权重。

>例如估计出个体学习器的误差，然后令权重大小与误差大小成反比

加权平均法的权重一般是从训练数据中学习而得，现实任务中的训练样本通常不充分或存在噪声，这将使得学出的权重不完全可靠。尤其是对规模比较大的集成来说，要学习的权重比较多，较容易导致过拟合。因此，实验和应用均显示出，加权平均法未必一定优于简单平均法。一般而言，在个体学习器性能相差较大时宜使用加权平均法，而在个体学习器性能相近时宜使用简单平均法。

### 投票法
对分类任务来说，学习器$h_{i}$将从类别标记集合$\left\{c_{1}, c_{2}, \ldots, c_{N}\right\}$中预测出个标记，最常见的结合策略是使用投票法（voting）。为便于讨论，我们将$h_{i}$在样本$\boldsymbol{x}$上的预测输出表示为一个$N$维向量$\left(h_{i}^{1}(\boldsymbol{x}) ; h_{i}^{2}(\boldsymbol{x}) ; \ldots ; h_{i}^{N}(\boldsymbol{x})\right)$，其中$h_{i}^{j}(\boldsymbol{x})$是$h_{i}$在类别标记$c_{j}$上的输出。
#### 绝对多数投票法（majority voting)
$H(\boldsymbol{x})=\left\{\begin{array}{ll}c_{j}, & \text { if } \sum_{i=1}^{T} h_{i}^{j}(\boldsymbol{x})>0.5 \sum_{k=1}^{N} \sum_{i=1}^{T} h_{i}^{k}(\boldsymbol{x}) \\ \text { reject, } & \text { otherwise }\end{array}\right.$
即若某标记得票过半数，则预测为该标记；否则拒绝预测
#### 相对多数投票法（plurality voting)
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200426235158.png)
即预测为得票最多的标记，若同时有多个标记获最高票，则从中随机选取.
#### 加权投票法（weighted voting)
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200426235236.png)
与加权平均法类似，$w_{i}$是$h_{i}$的权重，通常$w_{i} \geqslant 0, \sum_{i=1}^{T} w_{i}=1$

标准的绝对多数投票法提供了“拒绝预测”选项，这在可靠性要求较高的学习任务中是一个很好的机制。但若学习任务要求必须提供预测结果，则绝对多数投票法将退化为相对多数投票法。因此，在不允许拒绝预测的任务中，绝对多数、相对多数投票法统称为“多数投票法”。

上述三式没有限制个体学习器输出值的类型。在现实任务中，不同类型个体学习器可能产生不同类型的$h_{i}^{j}(\boldsymbol{x})$值，常见的有：

- 类标记：$h_{i}^{j}(\boldsymbol{x}) \in\{0,1\}$，若$h_{i}$将样本预测为类别$c_{j}$则取值为 1, 否则为 0. 使用类标记的投票亦称“硬投票”（hard voting)
- 类概率：$h_{i}^{j}(\boldsymbol{x}) \in[0,1]$，相当于对后验概率$P\left(c_{j} | \boldsymbol{x}\right)$的一个估计。使用类概率的投票亦称“软投票”（soft voting）

不同类型的$h_{i}^{j}(\boldsymbol{x})$值不能混用。对一些能在预测出类别标记的同时产生分类置信度的学习器，其分类置信度可转化为类概率使用。若此类值未进行规范化，例如支持向量机的分类间隔值，则必须使用一些技术如 Plat 缩放（Platt scaling)，等分回归（isotonic regression)等进行“校准”（calibration）后才能作为类概率使用。有趣的是虽然分类器估计出的类概率值一般都不太准确，但基于类概率进行结合却往往比直接基于类标记进行结合性能更好。需注意的是，若基学习器的类型不同，则其类概率值不能直接进行比较；例如异质集成中不同类型的个体学习器。在此种情形下，通常可将类概率输出转化为类标记输出（例如将类概率输出最大的$h_{i}^{j}(\boldsymbol{x})$设为 1, 其他设为 0) 然后再投票。

### 学习法
当训练数据很多时，一种更为强大的结合策略是使用“学习法”，即通过另一个学习器来进行结合。`Stacking`是学习法的典型代表。这里我们把个体学习器称为初级学习器，用于结合的学习器称为次级学习器或元学习器（meta-learner).

>`Stacking`本身是一种著名的集成学习方法，且有不少集成学习算法可视为其变体或特例。它也可看作一种特殊的结合策略。

`Stacking`先从初始数据集训练出初级学习器，然后“生成”一个新数据集用于训练次级学习器。在这个新数据集中，初级学习器的输出被当作样例输入特征，而初始样本的标记仍被当作样例标记。`Stacking`的算法描述如图所示，这里我们假定初级学习器使用不同学习算法产生，即初级集成是异质的。

>初级学习器也可是同质的

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200427000407.png)

在训练阶段，次级训练集是利用初级学习器产生的，若直接用初级学习器的训练集来产生次级训练集，则过拟合风险会比较大；因此，一般是通过使用交叉验证或留一法这样的方式，用训练初级学习器未使用的样本来产生次级学习器的训练样本。以$k$折交叉验证为例，初始训练集$D$被随机划分为$k$个大小相似的集合$D_{1}, D_{2}, \dots, D_{k}$,令$D_{j}$和$\bar{D}_{j}=D \backslash D_{j}$分别表示第$j$折的测试集和训练集。给定$T$个初级学习算法，初级学习器$h_{t}^{(j)}$通过在$\bar{D}_{j}$上使用第$t$个学习算法而得。对$D_{j}$中每个样本$\boldsymbol{x}_{i}$，令$z_{i t}=h_{t}^{(j)}\left(\boldsymbol{x}_{i}\right)$，则由$\boldsymbol{x}_{i}$所产生的次级训练样例的示例部分为$\boldsymbol{z}_{i}=\left(z_{i 1} ; z_{i 2} ; \ldots ; z_{i T}\right)$，标记部分为$y_{i}$。于是，在整个交叉验证过程结東后，从这个初级学习器产生的次级训练集是$D^{\prime}=\left\{\left(\boldsymbol{z}_{i}, y_{i}\right)\right\}_{i=1}^{m}$, 然后$D^{\prime}$将用于训练次级学习器。

次级学习器的输入属性表示和次级学习算法对 `Stacking` 集成的泛化性能有很大影响。有研究表明，将初级学习器的输出类概率作为次级学习器的输入属性，用多响应线性回归（Multi-response Linear Regression，简称 MLR）作为次级学习算法效果较好, 在 MLR 中使用不同的属性集更佳。

贝叶斯模型平均（Bayes Model Averaging，简称 BMA）基于后验概率来为不同模型赋予权重，可视为加权平均法的一种特殊实现。理论上来说，若数据生成模型恰在当前考虑的模型中，且数据噪声很少，则 BMA 不差于 `Stacking`；然而，在现实应用中无法确保数据生成模型一定在当前考虑的模型中，甚至可能难以用当前考虑的模型来进行近似，因此，`Stacking` 通常优于 BMA，因为其鲁棒性比 BMA 更好，而且 BMA 对模型近似误差非常敏感。

## 多样性
### 误差—分岐分解
令$E$为集成的泛化误差，$\bar{E}=\sum_{i=1}^{T} w_{i} E_{i}$表示个体学习器泛化误差的加权均值，$\bar{A}=\sum_{i=1}^{T} w_{i} A_{i}$表示个体学习器的加权分歧值，有：
$E=\bar{E}-\bar{A}$

式子明确提示出：个体学习器准确性越高、多样性越大，则集成越好。称为“误差一分歧分解”（eror- ambiguity decomposition)。

至此，读者可能很高兴：我们直接把$\bar{E}-\bar{A}$作为优化目标来求解，不就能得到最优的集成了？遗憾的是，在现实任务中很难直接对$\bar{E}-\bar{A}$进行优化，不仅由于它们是定义在整个样本空间上，还由于$\bar{A}$不是一个可直接操作的多样性度量，它仅在集成构造好之后オ能进行估计。此外需注意的是，上面的推导过程只适用于回归学习，难以直接推广到分类学习任务上去。
### 多样性度量(差异性度量)
顾名思义，多样性度量（diversity measure）是用于度量集成中个体分类器的多样性，即估算个体学习器的多样化程度。典型做法是考虑个体分类器的两两相似/不相似性。

给定数据集$D=\left\{\left(\boldsymbol{x}_{1}, y_{1}\right),\left(\boldsymbol{x}_{2}, y_{2}\right), \ldots,\left(\boldsymbol{x}_{m}, y_{m}\right)\right\}$，对二分类任务，$y_{i} \in$\{-1,+1\}, 分类器$h_{i}$与$h_{j}$的预测结果列联表（contingency table）为：

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200427122829.png)

其中，$a$表示$h_{i}$与$h_{j}$均预测为正类的样本数目；$b$、$c$、$d$含义由此类推$a+b+c+d=m$。基于这个列联表，下面给出一些常见的多样性度量。

- 不合度量（disagreement measure)
$d i s_{i j}=\frac{b+c}{m}$

$d i s_{i j}$的值域为[0,1]。值越大则多样性越大。

- 相关系数（correlation coefficient)
$\rho_{i j}=\frac{a d-b c}{\sqrt{(a+b)(a+c)(c+d)(b+d)}}$

$\rho_{i j}$的值域为[-1,1]。若$h_{i}与$h_{j}$无关，则值为 0; 若$h_{i}$与$h_{j}$正相关则值为正，否则为负。

- $Q$-统计量（$Q$-statistic)
$Q_{i j}=\frac{a d-b c}{a d+b c c}$

$Q_{i j}$与相关系数$\rho_{i j}$的符号相同，且$\left|Q_{i j}\right| \leqslant\left|\rho_{i j}\right|$

- $\kappa$-统计量
$\kappa=\frac{p_{1}-p_{2}}{1-p_{2}}$

其中，$p_{1}$是两个分类器取得一致的概率；$p_{2}$是两个分类器偶然达成一致的概率，它们可由数据集$D$估算：

$p_{1}=\frac{a+d}{m}$

$p_{2}=\frac{(a+b)(a+c)+(c+d)(b+d)}{m^{2}}$

若分类器$h_{i}$与$h_{j}$在$D$上完全一致，则$\kappa=1$; 若它们仅是偶然达成一致，则$\kappa=0$. $\kappa$通常为非负值，仅在$h_{i}$与$h_{j}$达成一致的概率甚至低于偶然性的情况下取负值。

以上介绍的都是“成对型”（pair wise）多样性度量，它们可以容易地通过 2 维图绘制出来。例如著名的“$\kappa$-误差图”，就是将每一对分类器作为图上的一个点，横坐标是这对分类器的$\kappa$值，纵坐标是它们的平均误差，图 8.10 给出了一个例子。显然，数据点云的位置越高，则个体分类器准确性越低；点云的位置越靠右，则个体学习器的多样性越小。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200427124341.png)

### 多样性増强
在集成学习中需有效地生成多样性大的个体学习器。与简单地直接用初始数据训练出个体学习器相比，如何增强多样性呢？一般思路是在学习过程中引入随机性，常见做法主要是对数据样本、输入属性、输出表示、算法参数进行扰动
#### 数据样本扰动
给定初始数据集，可从中产生出不同的数据子集，再利用不同的数据子集训练出不同的个体学习器。数据样本扰动通常是基于采样法，例如在 `Bagging` 中使用自助采样，在 `Adaboost` 中使用序列采样。此类做法简单高效，使用最广。对很多常见的基学习器，例如决策树、神经网络等，训练样本稍加变化就会导致学习器有显著变动，数据样本扰动法对这样的“不稳定基学习器”很有效；然而，有一些基学习器对数据样本的扰动不敏感，例如线性学习器、支持向量机、朴素贝叶斯、`k` 近邻学习器等，这样的基学习器称为稳定基学习器（stable base learner），对此类基学习器进行集成往往需使用输入属性扰动等其他机制.
#### 输入属性扰动
训练样本通常由一组属性描述，不同的“子空间”（subspace，即属性子集）提供了观察数据的不同视角。显然，从不同子空间训练出的个体学习器必然有所不同。著名的随机子空间（random subspace）算法就依赖于输入属性扰动，该算法从初始属性集中抽取出若干个属性子集，再基于每个属性子集训练一个基学习器，算法描述如图 8.11 所示。对包含大量冗余属性的数据，在子空间中训练个体学习器不仅能产生多样性大的个体，还会因属性数的减少而大幅节省时间开销，同时，由于冗余属性多，减少一些属性后训练出的个体学习器也不至于太差。若数据只包含少量属性，或者冗余属性很少，则不宜使用输入属性扰动法。

>子空间一般指从初始的高维属性空间投影产生的低维属性空间，描述低维空间的属性是通过初始属性投影变換而得，未必是初始属性。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200427124623.png)

#### 输出表示扰动
此类做法的基本思路是对输出表示进行操纵以增强多样性。可对训练样本的类标记稍作变动，如“翻转法”（Flipping Output)随机改变一些训练样本的标记；也可对输出表示进行转化，如“输出调制法”（Output Smearing)将分类输出转化为回归输出后构建个体学习器；还可将原任务拆解为多个可同时求解的子任务，如 BCOC 法利用纠错输出码将多分类任务拆解为一系列二分类任务来训练基学习器。
#### 算法参数扰动
基学习算法一般都有参数需进行设置，例如神经网络的隐层神经元数、初始连接权值等，通过随机设置不同的参数，往往可产生差别较大的个体学习器。例如“负相关法”（Negative Correlation)显式地通过正则化项来强制个体神经网络使用不同的参数。对参数较少的算法，可通过将其学习过程中某些环节用其他类似方式代替，从而达到扰动的目的，例如可将决策树使用的属性选择机制替换成其他的属性选择机制。值得指出的是，使用单学习器时通常需使用交叉验证等方法来确定参数值，这事实上已使用了不同参数训练出多个学习器，只不过最终仅选择其中一个学习器进行使用，而集成学习则相当于把这些学习器都利用起来；由此也可看出，集成学习技术的实际计算开销并不比使用单一学习器大很多。

不同的多样性增强机制可同时使用，例如随机森林中同时使用了数据样本扰动和输入属性扰动，有些方法甚至同时使用了更多机制。
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

给定数据集$D=\left\{\left(\boldsymbol{x}_{i}, y_{i}\right)\right\}_{i=1}^{m}, y_{i} \in\{0,1\}$，令$X_{i}, \boldsymbol{\mu}_{i}, \mathbf{\Sigma}_{i}$分别表示第$i \in\{0,1\}$类示例的集合、均值向量、协方差矩阵。若将数据投影到直线$\boldsymbol{w}$上，则两类样本的中心在直线上的投影分别为$\boldsymbol{w}^{\mathrm{T}} \boldsymbol{\mu}_{0}$和$\boldsymbol{w}^{\mathrm{T}} \boldsymbol{\mu}_{1}$，若将所有样本点都投影到直线_上，则两类样本的协方差分别为$\boldsymbol{w}^{\mathrm{T}} \boldsymbol{\Sigma}_{0} \boldsymbol{w}$和$\boldsymbol{w}^{\mathrm{T}} \boldsymbol{\Sigma}_{1} \boldsymbol{w}$。由于直线是维空间，因此$\boldsymbol{w}^{\mathrm{T}} \boldsymbol{\mu}_{0}, \boldsymbol{w}^{\mathrm{T}} \boldsymbol{\mu}_{1}, \boldsymbol{w}^{\mathrm{T}} \boldsymbol{\Sigma}_{0} \boldsymbol{w}$和$\boldsymbol{w}^{\mathrm{T}} \boldsymbol{\Sigma}_{1} \boldsymbol{w}$均为实数。

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





