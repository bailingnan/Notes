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


# 奇异值分解(统计学习方法)
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

