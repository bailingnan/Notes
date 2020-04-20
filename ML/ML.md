<!-- TOC -->

- [决策树(《统计学习方法》)](#决策树统计学习方法)
  - [决策树模型与学习](#决策树模型与学习)
    - [决策树与 `if-then` 规则](#决策树与-if-then-规则)
    - [决策树与条件概率分布](#决策树与条件概率分布)
    - [决策树学习](#决策树学习)
  - [特征选择](#特征选择)
    - [信息增益](#信息增益)
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

设训练数据集为$D$,$|D|$表示其样本容量，即样本个数。设有$K$个类$C_{k}$,$k=$$1,2, \cdots, K$,$\left|C_{k}\right|$为属于类$C_{k}$的样本个数，$\sum_{k=1}^{K}\left|C_{k}\right|=|D|$.设特征$A$有$n$个不同的取值
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





