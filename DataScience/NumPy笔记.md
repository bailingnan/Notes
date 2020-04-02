# `NumPy`笔记
<!-- TOC -->

- [`NumPy`笔记](#numpy笔记)
  - [Numpy基础数据结构](#numpy基础数据结构)
    - [多维数组](#多维数组)
    - [`arange()`](#arange)
    - [`airspace()`](#airspace)
    - [`zeros_like()`/`ones_like()`](#zeros_likeones_like)
    - [`eye()`](#eye)
    - [`empty()`](#empty)
    - [数组创建函数](#数组创建函数)
  - [`NumPy`通用函数](#numpy通用函数)
    - [数组的运算](#数组的运算)
    - [数组形状](#数组形状)
      - [改变形状](#改变形状)
      - [数组摊平](#数组摊平)
    - [数组的复制](#数组的复制)
    - [数组类型转换：](#数组类型转换)
    - [数组堆叠:](#数组堆叠)
    - [数组拆分](#数组拆分)
    - [数组运算](#数组运算)
      - [基本运算](#基本运算)
      - [统计量](#统计量)
      - [其他](#其他)
    - [基本索引及切片](#基本索引及切片)
      - [布尔型索引](#布尔型索引)
      - [数组索引及切片的值更改、复制](#数组索引及切片的值更改复制)
      - [花式索引](#花式索引)
        - [使用索引数组进行索引](#使用索引数组进行索引)
        - [使用布尔数组进行索引](#使用布尔数组进行索引)
      - [元素级数组函数](#元素级数组函数)
      - [将条件逻辑表述为数组运算](#将条件逻辑表述为数组运算)
      - [用于布尔型数组的方法](#用于布尔型数组的方法)

<!-- /TOC -->
```python
import numpy as np
```
## Numpy基础数据结构
### 多维数组
```python
ar = np.array([1,2,3,4,5,6,7])
print(ar.ndim)     # 输出数组维度的个数（轴数），或者说“秩”，维度的数量也称rank
print(ar.shape)    # 数组的维度，对于n行m列的数组，shape为（n，m）
print(ar.size)     # 数组的元素总数，对于n行m列的数组，元素总数为n*m
print(ar.dtype)    # 数组中元素的类型，类似type()（注意了，type()是函数，.dtype是方法）
print(ar.itemsize) # 数组中每个元素的字节大小，int32l类型字节为4，float64的字节为8
print(ar.data)     # 包含实际数组元素的缓冲区，由于一般通过数组的索引获取元素，所以通常不需要使用这个属性。
```
### `arange()`
类似`range()`，在给定间隔内返回均匀间隔的值。
```python
print(np.arange(10))    # 返回0-9，整型
print(np.arange(10.0))  # 返回0.0-9.0，浮点型
print(np.arange(5,12))  # 返回5-11
print(np.arange(5.0,12,2))  # 返回5.0-12.0，步长为2
```
### `airspace()`
返回在间隔`[开始，停止]`上计算的`num`个均匀间隔的样本。
```python
np.linspace(start, stop, num=50, endpoint=True, retstep=False, dtype=None)
```
- `start`：起始值
- `stop`：结束值
- `num`：生成样本数，默认为`50`
- `endpoint`：如果为真，则停止是最后一个样本。否则，不包括在内。默认值为`True`。
- `retstep`：如果为真，返回（样本，步长），其中步长是样本之间的间距 → 输出为一个包含2个元素的元组，第一个元素为`array`，第二个为步长实际值
### `zeros_like()`/`ones_like()`
返回具有和给定数组相同形状和类型的零矩阵和全为`1`矩阵
```python
ar= np.array([list(range(5)),list(range(5,10))])
ar1 = np.zeros_like(ar)
ar2=np.ones_like(ar)
```
### `eye()`
单位矩阵
```python
np.eye(5)
```
### `empty()`
`empty`可以创建一个没有任何具体值的数组。要用这些方法创建多维数组，只需传入一个表示形状的元组即可
```python
np.empty((2, 3, 2))
array([[[ 0.,  0.],
        [ 0.,  0.],
        [ 0.,  0.]],
       [[ 0.,  0.],
        [ 0.,  0.],
        [ 0.,  0.]]])
```
>注意：认为np.empty会返回全0数组的想法是不安全的。很多情况下（如前所示），它返回的都是一些未初始化的垃圾值。
### 数组创建函数
列出了一些数组创建函数。由于`NumPy`关注的是数值计算，因此，如果没有特别指定，数据类型基本都是`float64`（浮点数）:
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200327170700.png)
## `NumPy`通用函数
### 数组的运算
数组很重要，因为它使你不用编写循环即可对数据执行批量运算。`NumPy`用户称其为矢量化（`vectorization`）。大小相等的数组之间的任何算术运算都会将运算应用到元素级：
```python
arr = np.array([[1., 2., 3.], [4., 5., 6.]])
arr
array([[ 1.,  2.,  3.],
       [ 4.,  5.,  6.]])
arr * arr 
array([[  1.,   4.,   9.],
       [ 16.,  25.,  36.]])
arr - arr
array([[ 0.,  0.,  0.],
       [ 0.,  0.,  0.]])
```
数组与标量的算术运算会将标量值传播到各个元素：
```python
1 / arr 
array([[ 1.    ,  0.5   ,  0.3333],
       [ 0.25  ,  0.2   ,  0.1667]])
arr ** 0.5 
array([[ 1.    ,  1.4142,  1.7321],
       [ 2.    ,  2.2361,  2.4495]])
```
大小相同的数组之间的比较会生成布尔值数组：
```python
arr2 = np.array([[0., 4., 1.], [7., 2., 12.]])
arr2 
array([[  0.,   4.,   1.],
       [  7.,   2.,  12.]])
arr2 > arr
array([[False,  True, False],
       [ True, False,  True]], dtype=bool)
```
### 数组形状
- `.T`/`.reshape()`/`.resize()`,都是生成新的数组
- 转置：
转置是重塑的一种特殊形式，它返回的是源数据的视图（不会进行任何复制操作）。数组不仅有`transpose`方法，还有一个特殊的`T`属性：
```python
arr = np.arange(15).reshape((3, 5))
arr 
array([[ 0,  1,  2,  3,  4],
       [ 5,  6,  7,  8,  9],
       [10, 11, 12, 13, 14]])
arr.T
array([[ 0,  5, 10],
       [ 1,  6, 11],
       [ 2,  7, 12],
       [ 3,  8, 13],
       [ 4,  9, 14]])
```
在进行矩阵计算时，经常需要用到该操作，比如利用`np.dot`计算矩阵内积：
```python
arr = np.random.randn(6, 3)
arr 
array([[-0.8608,  0.5601, -1.2659],
       [ 0.1198, -1.0635,  0.3329],
       [-2.3594, -0.1995, -1.542 ],
       [-0.9707, -1.307 ,  0.2863],
       [ 0.378 , -0.7539,  0.3313],
       [ 1.3497,  0.0699,  0.2467]])
np.dot(arr.T, arr)
array([[ 9.2291,  0.9394,  4.948 ],
       [ 0.9394,  3.7662, -1.3622],
       [ 4.948 , -1.3622,  4.3437]])
```
对于高维数组，`transpose`需要得到一个由轴编号组成的元组才能对这些轴进行转置（比较费脑子）：
```python
arr = np.arange(16).reshape((2, 2, 4))
arr
array([[[ 0,  1,  2,  3],
        [ 4,  5,  6,  7]],
       [[ 8,  9, 10, 11],
        [12, 13, 14, 15]]])
arr.transpose((1, 0, 2))
array([[[ 0,  1,  2,  3],
        [ 8,  9, 10, 11]],
       [[ 4,  5,  6,  7],
        [12, 13, 14, 15]]])
```
这里，第一个轴被换成了第二个，第二个轴被换成了第一个，最后一个轴不变。
简单的转置可以使用`.T`，它其实就是进行轴对换而已。`ndarray`还有一个`swapaxes`方法，它需要接受一对轴编号：
```python
arr 
array([[[ 0,  1,  2,  3],
        [ 4,  5,  6,  7]],
       [[ 8,  9, 10, 11],
        [12, 13, 14, 15]]])
arr.swapaxes(1, 2) 
array([[[ 0,  4],
        [ 1,  5],
        [ 2,  6],
        [ 3,  7]],
       [[ 8, 12],
        [ 9, 13],
        [10, 14],
        [11, 15]]])
```
`swapaxes`也是返回源数据的视图（不会进行任何复制操作）。
#### 改变形状
以下三个命令都返回一个修改后的数组，但不会更改原始数组：`reshape`,`ravel`,`T`
- `numpy.reshape(a, newshape, order='C')`：为数组提供新形状，而不更改其数据，所以元素数量需要一致
```python
ar3 = ar1.reshape(2,5)     # 用法1：直接将已有数组改变形状             
ar4 = np.zeros((4,6)).reshape(3,8)   # 用法2：生成数组后直接改变形状
ar5 = np.reshape(np.arange(12),(3,4))   # 用法3：参数内添加数组，目标形状
```
- `numpy.resize(a, new_shape)`：返回具有指定形状的新数组，如有必要可重复填充所需数量的元素,`resize` 方法将直接修改原数组本身的维度。
#### 数组摊平
- `a.ravel()` :返回的是 `view`，会影响原始矩阵。
- `a.flatten()`: 都是将多维数组降为一维，`flatten()` 返回一份新的数组，且对它所做的修改不会影响原始数组。
### 数组的复制
```python
ar1 = np.arange(10)
ar2 = ar1
```
python的赋值逻辑：指向内存中生成的一个值 → 这里`ar1`和`ar2`指向同一个值，所以`ar1`改变，`ar2`**一起改变**
```python
ar3 = ar1.copy()
```
- `copy()`:生成数组及其数据的完整拷贝,`ar3`改变`ar1`**不随之改变**
- `view()`:创建一个新数组对象来查看相同数据。改变其中一个变量的 `shape` 并不会对应改变另一个。但这两个数组是共享所有元素的，所以改变一个数组的某个元素同样会改变另一个数组的对应元素。
### 数组类型转换：
- `ar.astype()`:类型转换
```python
ar1 = np.arange(10,dtype=float)# 可以在参数位置设置数组类型
ar2 = ar1.astype(np.int32)# a.astype()：转换数组类型,数组类型用np.int32，而不是直接int32
```
如果某字符串数组表示的全是数字，也可以用`astype`将其转换为数值形式：
```python
numeric_strings = np.array(['1.25', '-9.6', '42'], dtype=np.string_)
numeric_strings.astype(float)
array([  1.25,  -9.6 ,  42.  ])
```
>注意：使用`numpy.string_`类型时，一定要小心，因为NumPy的字符串数据是大小固定的，发生截取时，不会发出警告。`pandas`提供了更多非数值数据的便利的处理方法。
>笔记：调用`astype`总会创建一个新的数组（一个数据的备份），即使新的`dtype`与旧的`dtype`相同。
### 数组堆叠:
- `np.stack((a,b),axis)`:形状必须相同
- `np.hstack((a,b))`:注意:((a,b))，这里形状必须一样,水平（按列顺序）堆叠数组
- `np.vstack((a,b))`:这里形状可以不一样,垂直（按行顺序）堆叠数组
```python
a = np.array([[1],[2],[3]]) 
b = np.array([['a'],['b'],['c']])  
ar2 = np.hstack((a,b))  
```
```python
a = np.array([[1],[2],[3]])   
b = np.array([['a'],['b'],['c'],['d']])   
ar2 = np.vstack((a,b)) 
```
- `np.column_stack(a,b,c)`:可以将每个元素作为一列，例如 `np.column_stack((a,b,c))` 就将向量 `a` 作为第一列、`b` 作为第二列、`c` 作为第三列
### 数组拆分
- `numpy.hsplit(ary, indices_or_sections)`：将数组水平（逐列）拆分为多个子数组 → 按列拆分,输出结果为列表，列表中元素为数组
```python
ar = np.arange(16).reshape(4,4)
ar1 = np.hsplit(ar,2)
```
- `numpy.vsplit(ary, indices_or_sections)`：:将数组垂直（行方向）拆分为多个子数组 → 按行拆
```python
ar2 = np.vsplit(ar,4)
```
### 数组运算
#### 基本运算
- `a.T`/`a.transpose()`：转置
- `a*b`:点乘
- `np.dot(a,b)/a.dot(b)`:叉乘
#### 统计量
`sum`、`mean`以及标准差`std`等聚合计算（`aggregation`，通常叫做约简（`reduction`））既可以当做数组的实例方法调用，也可以当做顶级`NumPy`函数使用。
```python
ar.mean() # 求平均值
np.mean(arr)
ar.max() # 求最大值
ar.min()  # 求最小值
ar.std()  # 求标准差
ar.var()  # 求方差
ar.sum()
np.sum(ar,axis = 0)  # 求和，np.sum() → axis为0，按列求和；axis为1，按行求和
np.sort(np.array([1,4,3,2,5,6]))  # 排序
```
下表列出了全部的基本数组统计方法。
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200327235323.png)
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200327235341.png)
#### 其他
- `np.clip(array,min,max))`:限制数组中最小值为`min`,最大值为`max`
- `np.append(array,element/array)`:将元素或者新数组的每一个元素添加至新数组
- `np.diff(array,n=num)`:求取该数组两个元素之间的差，可用于计算相对误差，差分数组比原来少n个元素
- `np.extract(condition, array)`:按条件选取元素
- `np.setdiff1d(a,b)`:返回数组中不在另一个数组中的独有元素。这等价于两个数组元素集合的差集。
```python
a = np.array([1, 2, 3, 4, 5, 6, 7, 8, 9])
b = np.array([3,4,7,6,7,8,11,12,14])
c = np.setdiff1d(a,b)
array([1, 2, 5, 9])
```
### 基本索引及切片
当有些维度没有指定索引时，空缺的维度被默认为取所有元素。
`NumPy`还允许使用 `dots (...)` 表示足够多的冒号来构建完整的索引元组。
比如，如果 `x` 是 5 维数组：
- `x[1,2,...]` 等于 `x[1,2,:,:,:]`
- `x[...,3]` 等于 `x[:,:,:,:,3]`
- `x[4,...,5,:]` 等于 `x[4,:,:,5,:]`
`flat` 是一个在数组所有元素中运算的迭代器，如下将逐元素地对数组进行操作。
```python
for element in b.flat:
    print(element)
```
```python
ar = np.arange(16).reshape(4,4)
print(ar, '数组轴数为%i' %ar.ndim)   # 4*4的数组
print(ar[2],  '数组轴数为%i' %ar[2].ndim)  # 切片为下一维度的一个元素，所以是一维数组
print(ar[2][1]) # 二次索引，得到一维数组中的一个值
print(ar[1:3],  '数组轴数为%i' %ar[1:3].ndim)  # 切片为两个一维数组组成的二维数组
print(ar[2,2])  # 切片数组中的第三行第三列 → 10
print(ar[:2,1:])  # 切片数组中的1,2行、2,3,4列 → 二维数组
```
你可以传入一个以逗号隔开的索引列表来选取单个元素。也就是说，下面两种方式是等价的：
```python
arr2d[0][2]
arr2d[0, 2]
```
#### 布尔型索引
```python
m = ar > 5
print(m)  # 这里m是一个判断矩阵
print(ar[m])  # 用m判断矩阵去筛选ar数组中>5的元素 → 重点！后面的pandas判断方式原理就来自此处
```
```python
names = np.array(['Bob', 'Joe', 'Will', 'Bob', 'Will', 'Joe', 'Joe'])
data = np.random.randn(7, 4)
names 
array(['Bob', 'Joe', 'Will', 'Bob', 'Will', 'Joe', 'Joe'],
      dtype='<U4')
data 
array([[ 0.0929,  0.2817,  0.769 ,  1.2464],
       [ 1.0072, -1.2962,  0.275 ,  0.2289],
       [ 1.3529,  0.8864, -2.0016, -0.3718],
       [ 1.669 , -0.4386, -0.5397,  0.477 ],
       [ 3.2489, -1.0212, -0.5771,  0.1241],
       [ 0.3026,  0.5238,  0.0009,  1.3438],
       [-0.7135, -0.8312, -2.3702, -1.8608]])
```
假设每个名字都对应`data`数组中的一行，而我们想要选出对应于名字`"Bob"`的所有行。跟算术运算一样，数组的比较运算（如`==`）也是矢量化的。因此，对`names`和字符串`"Bob"`的比较运算将会产生一个布尔型数组：
```python
names == 'Bob'
array([ True, False, False,  True, False, False, False], dtype=bool)
```
这个布尔型数组可用于数组索引：
```python
data[names == 'Bob']
array([[ 0.0929,  0.2817,  0.769 ,  1.2464],
       [ 1.669 , -0.4386, -0.5397,  0.477 ]])
```
布尔型数组的长度必须跟被索引的轴长度一致。此外，还可以将布尔型数组跟切片、整数（或整数序列，稍后将对此进行详细讲解）混合使用：
```python
data[names == 'Bob'] 
array([[ 0.0929,  0.2817,  0.769 ,  1.2464],
       [ 1.669 , -0.4386, -0.5397,  0.477 ]])
```
注意：如果布尔型数组的长度不对，布尔型选择就会出错，因此一定要小心。
下面的例子，我选取了`names == 'Bob'`的行，并索引了列：
```python
data[names == 'Bob', 2:] 
array([[ 0.769 ,  1.2464],
       [-0.5397,  0.477 ]])
data[names == 'Bob', 3]
array([ 1.2464,  0.477 ])
```
要选择除`"Bob"`以外的其他值，既可以使用不等于符号（`!=`），也可以通过`~`对条件进行否定：
```python
names != 'Bob'
array([False,  True,  True, False,  True,  True,  True], dtype=bool)
data[~(names == 'Bob')]

array([[ 1.0072, -1.2962,  0.275 ,  0.2289],
       [ 1.3529,  0.8864, -2.0016, -0.3718],
       [ 3.2489, -1.0212, -0.5771,  0.1241],
       [ 0.3026,  0.5238,  0.0009,  1.3438],
       [-0.7135, -0.8312, -2.3702, -1.8608]])
```
`~`操作符用来反转条件很好用：
```python
cond = names == 'Bob'
data[~cond]
array([[ 1.0072, -1.2962,  0.275 ,  0.2289],
       [ 1.3529,  0.8864, -2.0016, -0.3718],
       [ 3.2489, -1.0212, -0.5771,  0.1241],
       [ 0.3026,  0.5238,  0.0009,  1.3438],
       [-0.7135, -0.8312, -2.3702, -1.8608]])
```
选取这三个名字中的两个需要组合应用多个布尔条件，使用`&`（和）、`|`（或）之类的布尔算术运算符即可：
```python
mask = (names == 'Bob') | (names == 'Will')
mask
array([ True, False,  True,  True,  True, False, False], dtype=bool)
data[mask] 
array([[ 0.0929,  0.2817,  0.769 ,  1.2464],
       [ 1.3529,  0.8864, -2.0016, -0.3718],
       [ 1.669 , -0.4386, -0.5397,  0.477 ],
       [ 3.2489, -1.0212, -0.5771,  0.1241]])
```
通过布尔型索引选取数组中的数据，将总是创建数据的副本，即使返回一模一样的数组也是如此。
注意：`Python`关键字`and`和`or`在布尔型数组中无效。要使用`&`与`|`。
通过布尔型数组设置值是一种经常用到的手段。为了将`data`中的所有负值都设置为`0`，我们只需：
```python
data[data < 0] = 0
data 
array([[ 0.0929,  0.2817,  0.769 ,  1.2464],
       [ 1.0072,  0.    ,  0.275 ,  0.2289],
       [ 1.3529,  0.8864,  0.    ,  0.    ],
       [ 1.669 ,  0.    ,  0.    ,  0.477 ],
       [ 3.2489,  0.    ,  0.    ,  0.1241],
       [ 0.3026,  0.5238,  0.0009,  1.3438],
       [ 0.    ,  0.    ,  0.    ,  0.    ]])
```
通过一维布尔数组设置整行或列的值也很简单：
```python
data[names != 'Joe'] = 7
data 
array([[ 7.    ,  7.    ,  7.    ,  7.    ],
       [ 1.0072,  0.    ,  0.275 ,  0.2289],
       [ 7.    ,  7.    ,  7.    ,  7.    ],
       [ 7.    ,  7.    ,  7.    ,  7.    ],
       [ 7.    ,  7.    ,  7.    ,  7.    ],
       [ 0.3026,  0.5238,  0.0009,  1.3438],
       [ 0.    ,  0.    ,  0.    ,  0.    ]])
```
后面会看到，这类二维数据的操作也可以用`pandas`方便的来做。
#### 数组索引及切片的值更改、复制
```python
ar = np.arange(10)
print(ar)
ar[5] = 100
ar[7:9] = 200# 一个标量赋值给一个索引/切片时，会自动改变/传播原始数组
```
跟列表最重要的区别在于，由于`NumPy`的设计目的是处理大数据，数组切片是原始数组的视图。这意味着数据不会被复制，视图上的任何修改都会直接反映到源数组上。
作为例子，先创建一个`arr`的切片：
```python
arr_slice = arr[5:8]
arr_slice
array([12, 12, 12])
```
现在，当我修改`arr_slice`中的值，变动也会体现在原始数组`arr`中：
```python
arr_slice[1] = 12345
arr
array([    0,     1,     2,     3,     4,    12, 12345,    12,     8,   
  9])
```
切片`[ : ]`会给数组中的所有值赋值：
```python
arr_slice[:] = 64
arr
array([ 0,  1,  2,  3,  4, 64, 64, 64,  8,  9])
```
>注意：如果你想要得到的是`ndarray`切片的一份副本而非视图，就需要明确地进行复制操作，例如`arr[5:8].copy()`。
#### 花式索引
```python
arr = np.empty((8, 4))
for i in range(8):
    arr[i] = i
arr 
array([[ 0.,  0.,  0.,  0.],
       [ 1.,  1.,  1.,  1.],
       [ 2.,  2.,  2.,  2.],
       [ 3.,  3.,  3.,  3.],
       [ 4.,  4.,  4.,  4.],
       [ 5.,  5.,  5.,  5.],
       [ 6.,  6.,  6.,  6.],
       [ 7.,  7.,  7.,  7.]])
```
为了以特定顺序选取行子集，只需传入一个用于指定顺序的整数列表或`ndarray`即可：
```python
arr[[4, 3, 0, 6]] 
array([[ 4.,  4.,  4.,  4.],
       [ 3.,  3.,  3.,  3.],
       [ 0.,  0.,  0.,  0.],
       [ 6.,  6.,  6.,  6.]])
```
使用负数索引将会从末尾开始选取行：
```python
arr[[-3, -5, -7]] 
array([[ 5.,  5.,  5.,  5.],
       [ 3.,  3.,  3.,  3.],
       [ 1.,  1.,  1.,  1.]])
```
一次传入多个索引数组会有一点特别。它返回的是一个一维数组，其中的元素对应各个索引元组：
```python
arr = np.arange(32).reshape((8, 4))
arr 
array([[ 0,  1,  2,  3],
       [ 4,  5,  6,  7],
       [ 8,  9, 10, 11],
       [12, 13, 14, 15],
       [16, 17, 18, 19],
       [20, 21, 22, 23],
       [24, 25, 26, 27],
       [28, 29, 30, 31]])
arr[[1, 5, 7, 2], [0, 3, 1, 2]]
array([ 4, 23, 29, 10])
```
最终选出的是元素`(1,0)`、`(5,3)`、`(7,1)`和`(2,2)`。无论数组是多少维的，花式索引总是一维的。
这个花式索引的行为可能会跟某些用户的预期不一样（包括我在内），选取矩阵的行列子集应该是矩形区域的形式才对。下面是得到该结果的一个办法：
```python
arr[[1, 5, 7, 2]][:, [0, 3, 1, 2]] 
array([[ 4,  7,  5,  6],
       [20, 23, 21, 22],
       [28, 31, 29, 30],
       [ 8, 11,  9, 10]])
```
花式索引跟切片不一样，它总是将数据复制到新数组中。
##### 使用索引数组进行索引
```python
a = np.arange(12)**2                       # the first 12 square numbers
i = np.array( [ 1,1,3,8,5 ] )              # an array of indices
a[i]                                       # the elements of a at the positions i
array([ 1,  1,  9, 64, 25])
j = np.array( [ [ 3, 4], [ 9, 7 ] ] )      # a bidimensional array of indices
a[j]                                       # the same shape as j
array([[ 9, 16],
       [81, 49]])
```
当索引数组`a`是多维的时，单个索引数组指的是第一个维度`a`。以下示例通过使用调色板将标签图像转换为彩色图像来显示此行为。和上边的二维矩阵的例子相比本例传入的参数为一个二维矩阵，而上例为两个一维矩阵。
```python
palette = np.array( [ [0,0,0],                # black
                       [255,0,0],              # red
                       [0,255,0],              # green
                       [0,0,255],              # blue
                       [255,255,255] ] )       # white
image = np.array( [ [ 0, 1, 2, 0 ],           # each value corresponds to a color in the palette
                  [ 0, 3, 4, 0 ]  ] )
palette[image]                            # the (2,4,3) color image
array([[[  0,   0,   0],
        [255,   0,   0],
        [  0, 255,   0],
        [  0,   0,   0]],
       [[  0,   0,   0],
        [  0,   0, 255],
        [255, 255, 255],
        [  0,   0,   0]]])
```
我们还可以为多个维度提供索引。每个维度的索引数组必须具有相同的形状。
```python
a = np.arange(12).reshape(3,4)
a
array([[ 0,  1,  2,  3],
       [ 4,  5,  6,  7],
       [ 8,  9, 10, 11]])
i = np.array( [ [0,1],                        # indices for the first dim of a
             [1,2] ] )
j = np.array( [ [2,1],                        # indices for the second dim
                 [3,3] ] )
a[i,j]                                     # i and j must have equal shape
array([[ 2,  5],
       [ 7, 11]])
a[i,2]
array([[ 2,  6],
       [ 6, 10]])
a[:,j]                                     # i.e., a[ : , j]
array([[[ 2,  1],
        [ 3,  3]],
       [[ 6,  5],
        [ 7,  7]],
       [[10,  9],
        [11, 11]]])
```
当然，我们可以按顺序（比如列表）放入`i`，`j`然后使用列表进行索引。
```python
l = [i,j]
a[l]                                       # equivalent to a[i,j]
array([[ 2,  5],
       [ 7, 11]])
```
但是，我们不能通过放入`i`和`j`放入数组来实现这一点，因为这个数组将被解释为索引a的第一个维度。
```python
s = np.array( [i,j] )
a[s]                                       # not what we want
Traceback (most recent call last):
  File "<stdin>", line 1, in ?
IndexError: index (3) out of range (0<=index<=2) in dimension 0
a[tuple(s)]                                # same as a[i,j]
array([[ 2,  5],
       [ 7, 11]])
```
使用数组索引的另一个常见用法是搜索与时间相关的系列的最大值：
```python
time = np.linspace(20, 145, 5)                 # time scale
data = np.sin(np.arange(20)).reshape(5,4)      # 4 time-dependent series
time
array([  20.  ,   51.25,   82.5 ,  113.75,  145.  ])
data
array([[ 0.        ,  0.84147098,  0.90929743,  0.14112001],
       [-0.7568025 , -0.95892427, -0.2794155 ,  0.6569866 ],
       [ 0.98935825,  0.41211849, -0.54402111, -0.99999021],
       [-0.53657292,  0.42016704,  0.99060736,  0.65028784],
       [-0.28790332, -0.96139749, -0.75098725,  0.14987721]])
ind = data.argmax(axis=0)                  # index of the maxima for each series
ind
array([2, 0, 3, 1])
time_max = time[ind]                       # times corresponding to the maxima
data_max = data[ind, range(data.shape[1])] # => data[ind[0],0], data[ind[1],1]...
time_max
array([  82.5 ,   20.  ,  113.75,   51.25])
data_max
array([ 0.98935825,  0.84147098,  0.99060736,  0.6569866 ])
np.all(data_max == data.max(axis=0))
True
```
您还可以使用数组索引作为分配给的目标：
```python
a = np.arange(5)
a
array([0, 1, 2, 3, 4])
a[[1,3,4]] = 0
a
array([0, 0, 2, 0, 0])
```
但是，当索引列表包含重复时，分配会多次完成，留下最后一个值：
```python
a = np.arange(5)
a[[0,0,2]]=[1,2,3]
a
array([2, 1, 3, 3, 4])
```
这是合理的，但请注意是否要使用`Python`的 `+=`构造，因为它可能不会按预期执行：
```python
a = np.arange(5)
a[[0,0,2]]+=1
a
array([1, 1, 3, 3, 4])
```
即使`0`在索引列表中出现两次，第`0`个元素也只增加一次。这是因为`Python`要求`a + = 1`等同于`a = a + 1`。
##### 使用布尔数组进行索引
当我们使用（整数）索引数组索引数组时，我们提供了要选择的索引列表。使用布尔索引，方法是不同的; 我们明确地选择我们想要的数组中的哪些项目以及我们不需要的项目。
人们可以想到的最自然的布尔索引方法是使用与原始数组具有 相同形状的 布尔数组：
```python
a = np.arange(12).reshape(3,4)
b = a > 4
b                                          # b is a boolean with a's shape
array([[False, False, False, False],
       [False,  True,  True,  True],
       [ True,  True,  True,  True]])
a[b]                                       # 1d array with the selected elements
array([ 5,  6,  7,  8,  9, 10, 11])
```
此属性在分配中非常有用：
```python
a[b] = 0                                   # All elements of 'a' higher than 4 become 0
a
array([[0, 1, 2, 3],
       [4, 0, 0, 0],
       [0, 0, 0, 0]])
```
使用布尔值进行索引的第二种方法更类似于整数索引; 对于数组的每个维度，我们给出一个`1D`布尔数组，选择我们想要的切片：
```python
a = np.arange(12).reshape(3,4)
b1 = np.array([False,True,True])             # first dim selection
b2 = np.array([True,False,True,False])       # second dim selection
a[b1,:]                                   # selecting rows
array([[ 4,  5,  6,  7],
       [ 8,  9, 10, 11]])
a[b1]                                     # same thing
array([[ 4,  5,  6,  7],
       [ 8,  9, 10, 11]])
a[:,b2]                                   # selecting columns
array([[ 0,  2],
       [ 4,  6],
       [ 8, 10]])
a[b1,b2]                                  # a weird thing to do
array([ 4, 10])
```
请注意，`1D`布尔数组的长度必须与要切片的尺寸（或轴）的长度一致。
#### 元素级数组函数
通用函数（即`ufunc`）是一种对`ndarray`中的数据执行元素级运算的函数。你可以将其看做简单函数（接受一个或多个标量值，并产生一个或多个标量值）的矢量化包装器。
许多`ufunc`都是简单的元素级变体，如`sqrt`和`exp`：
```python
arr = np.arange(10)
arr
array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
np.sqrt(arr) 
array([ 0.    ,  1.    ,  1.4142,  1.7321,  2.    ,  2.2361,  2.4495,
        2.6458,  2.8284,  3.    ])
np.exp(arr) 
array([    1.    ,     2.7183,     7.3891,    20.0855,    54.5982,
         148.4132,   403.4288,  1096.6332,  2980.958 ,  8103.0839])
```
这些都是一元（`unary`）`ufunc`。另外一些（如`add`或`maximum`）接受2个数组（因此也叫二元（`binary`）`ufunc`），并返回一个结果数组：
```python
x = np.random.randn(8)
y = np.random.randn(8)
x 
array([-0.0119,  1.0048,  1.3272, -0.9193, -1.5491,  0.0222,  0.7584,
       -0.6605])
y 
array([ 0.8626, -0.01  ,  0.05  ,  0.6702,  0.853 , -0.9559, -0.0235,
       -2.3042])
np.maximum(x, y) 
array([ 0.8626,  1.0048,  1.3272,  0.6702,  0.853 ,  0.0222,  0.7584,   
       -0.6605])
```
这里，`numpy.maximum`计算了`x`和`y`中元素级别最大的元素。
虽然并不常见，但有些`ufunc`的确可以返回多个数组。`modf`就是一个例子，它是`Python`内置函数`divmod`的矢量化版本，它会返回浮点数数组的小数和整数部分：
```python
arr = np.random.randn(7) * 5
arr
array([-3.2623, -6.0915, -6.663 ,  5.3731,  3.6182,  3.45  ,  5.0077])
remainder, whole_part = np.modf(arr)
remainder
array([-0.2623, -0.0915, -0.663 ,  0.3731,
0.6182,  0.45  ,  0.0077])
whole_part
array([-3., -6., -6.,  5.,  3.,  3.,  5.])
```
`Ufuncs`可以接受一个`out`可选参数，这样就能在数组原地进行操作：
```python
arr
array([-3.2623, -6.0915, -6.663 ,  5.3731,  3.6182,  3.45  ,  5.0077])
np.sqrt(arr)
array([    nan,     nan,     nan,  2.318 ,  1.9022,  1.8574,  2.2378])
np.sqrt(arr, arr)
array([    nan,     nan,     nan,  2.318 ,  1.9022,  1.8574,  2.2378])
arr
array([    nan,     nan,     nan,  2.318 ,  1.9022,  1.8574,  2.2378])
```
下表分别列出了一些一元和二元`ufunc`:
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200327185117.png)
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200327185147.png)
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200327185203.png)
#### 将条件逻辑表述为数组运算
`numpy.where`函数是三元表达式`x if condition else y`的矢量化版本。假设我们有一个布尔数组和两个值数组：
```python
xarr = np.array([1.1, 1.2, 1.3, 1.4, 1.5])
yarr = np.array([2.1, 2.2, 2.3, 2.4, 2.5])
cond = np.array([True, False, True, True, False])
```
假设我们想要根据`cond`中的值选取`xarr`和`yarr`的值：当`cond`中的值为`True`时，选取`xarr`的值，否则从`yarr`中选取。列表推导式的写法应该如下所示：
```python
result = [(x if c else y)for x, y, c in zip(xarr, yarr, cond)]
result
[1.1000000000000001, 2.2000000000000002, 1.3, 1.3999999999999999, 2.5]
```
这有几个问题。第一，它对大数组的处理速度不是很快（因为所有工作都是由纯`Python`完成的）。第二，无法用于多维数组。若使用`np.where`，则可以将该功能写得非常简洁：
```python
result = np.where(cond, xarr, yarr)
result
array([ 1.1,  2.2,  1.3,  1.4,  2.5])
```
`np.where`的第二个和第三个参数不必是数组，它们都可以是标量值。在数据分析工作中，`where`通常用于根据另一个数组而产生一个新的数组。假设有一个由随机数据组成的矩阵，你希望将所有正值替换为`2`，将所有负值替换为`－2`。若利用`np.where`，则会非常简单：
```python
arr = np.random.randn(4, 4)
arr 
array([[-0.5031, -0.6223, -0.9212, -0.7262],
       [ 0.2229,  0.0513, -1.1577,  0.8167],
       [ 0.4336,  1.0107,  1.8249, -0.9975],
       [ 0.8506, -0.1316,  0.9124,  0.1882]])
arr > 0 
array([[False, False, False, False],
       [ True,  True, False,  True],
       [ True,  True,  True, False],
       [ True, False,  True,  True]], dtype=bool)
np.where(arr > 0, 2, -2)
Out[175]: 
array([[-2, -2, -2, -2],
       [ 2,  2, -2,  2],
       [ 2,  2,  2, -2],
       [ 2, -2,  2,  2]])
```
使用`np.where`，可以将标量和数组结合起来。例如，可用常数`2`替换`arr`中所有正的值：
```python
np.where(arr > 0, 2, arr) # set only positive values to 2 
array([[-0.5031, -0.6223, -0.9212, -0.7262],
       [ 2.    ,  2.    , -1.1577,  2.    ],
       [ 2.    ,  2.    ,  2.    , -0.9975],
       [ 2.    , -0.1316,  2.    ,  2.    ]])
```
传递给`where`的数组大小可以不相等，甚至可以是标量值。
#### 用于布尔型数组的方法
`sum`经常被用来对布尔型数组中的`True`值计数：
```python
arr = np.random.randn(100)
(arr > 0).sum() # Number of positive values
42
另外还有两个方法`any`和`all`，它们对布尔型数组非常有用。`any`用于测试数组中是否存在一个或多个`True`，而`all`则检查数组中所有值是否都是`True`：
```python
bools = np.array([False, False, True, False])
bools.any()
True
bools.all()
False
```
这两个方法也能用于非布尔型数组，所有非`0`元素将会被当做`True`。
#### 排序
跟`Python`内置的列表类型一样，`NumPy`数组也可以通过`sort`方法就地排序：
```python
arr = np.random.randn(6)
arr
array([ 0.6095, -0.4938,  1.24  , -0.1357,  1.43  , -0.8469])
arr.sort()
arr
array([-0.8469, -0.4938, -0.1357,  0.6095,  1.24  ,  1.43  ])
```
多维数组可以在任何一个轴向上进行排序，只需将轴编号传给`sort`即可：
```python
arr = np.random.randn(5, 3)
arr 
array([[ 0.6033,  1.2636, -0.2555],
       [-0.4457,  0.4684, -0.9616],
       [-1.8245,  0.6254,  1.0229],
       [ 1.1074,  0.0909, -0.3501],
       [ 0.218 , -0.8948, -1.7415]])
arr.sort(1)
arr 
array([[-0.2555,  0.6033,  1.2636],
       [-0.9616, -0.4457,  0.4684],
       [-1.8245,  0.6254,  1.0229],
       [-0.3501,  0.0909,  1.1074],
       [-1.7415, -0.8948,  0.218 ]])
```
顶级方法`np.sort`返回的是数组的已排序副本，而就地排序则会修改数组本身。计算数组分位数最简单的办法是对其进行排序，然后选取特定位置的值：
```python
large_arr = np.random.randn(1000)
large_arr.sort()
large_arr[int(0.05 * len(large_arr))] # 5% quantile
-1.5311513550102103
```
在`pandas`中还可以找到一些其他跟排序有关的数据操作（比如根据一列或多列对表格型数据进行排序）。
#### 唯一化以及其它的集合逻辑
`NumPy`提供了一些针对一维`ndarray`的基本集合运算。最常用的可能要数`np.unique`了，它用于找出数组中的唯一值并返回已排序的结果：
```python
numpy.unique(ar, return_index=False, return_inverse=False, return_counts=False, axis=None)
#返回：
unique_indices:return_index=True时，原始数组中唯一值首次出现的索引。
unique_inverse:return_inverse=True时，从唯一数组重建原始数组的索引。
unique_counts:return_counts=True时，每个唯一值在原始数组中出现的次数。
```
```python
names = np.array(['Bob', 'Joe', 'Will', 'Bob', 'Will', 'Joe', 'Joe'])
np.unique(names) 
array(['Bob', 'Joe', 'Will'],
      dtype='<U4')
ints = np.array([3, 3, 3, 2, 2, 1, 1, 4, 4])
np.unique(ints)
array([1, 2, 3, 4])
```
拿跟`np.unique`等价的纯`Python`代码来对比一下：
```python
sorted(set(names))
['Bob', 'Joe', 'Will']
```
另一个函数`np.in1d`用于测试一个数组中的值在另一个数组中的成员资格，返回一个布尔型数组：
```python
values = np.array([6, 0, 0, 3, 2, 5, 6])
np.in1d(values, [2, 3, 6])
array([ True, False, False,  True,  True, False,  True], dtype=bool)
```
`NumPy`中的集合函数见下表。
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200328001840.png)
#### 线性代数
线性代数（如矩阵乘法、矩阵分解、行列式以及其他方阵数学等）是任何数组库的重要组成部分。不像某些语言（如`MATLAB`），通过`*`对两个二维数组相乘得到的是一个元素级的积，而不是一个矩阵点积。因此，`NumPy`提供了一个用于矩阵乘法的`dot`函数（既是一个数组方法也是`NumPy`命名空间中的一个函数）：
```python
x = np.array([[1., 2., 3.], [4., 5., 6.]])
y = np.array([[6., 23.], [-1, 7], [8, 9]])
x 
array([[ 1.,  2.,  3.],
       [ 4.,  5.,  6.]])
y 
array([[  6.,  23.],
       [ -1.,   7.],
       [  8.,   9.]])
x.dot(y) 
array([[  28.,   64.],
       [  67.,  181.]])
```
`x.dot(y)`等价于`np.dot(x, y)`：
```python
np.dot(x, y) 
array([[  28.,   64.],
       [  67.,  181.]])
```
一个二维数组跟一个大小合适的一维数组的矩阵点积运算之后将会得到一个一维数组：
```python
np.dot(x, np.ones(3))
array([  6.,  15.])
```
`@`符（类似`Python 3.5`）也可以用作中缀运算符，进行矩阵乘法：
```python
x @ np.ones(3)
array([  6.,  15.])
```
`numpy.linalg`中有一组标准的矩阵分解运算以及诸如求逆和行列式之类的东西。它们跟`MATLAB`和`R`等语言所使用的是相同的行业标准线性代数库，如`BLAS`、`LAPACK`、`Intel MKL`（`Math Kernel Library`，可能有，取决于你的`NumPy`版本）等：
```python
from numpy.linalg import inv, qr
X = np.random.randn(5, 5)
mat = X.T.dot(X)
inv(mat) 
array([[  933.1189,   871.8258, -1417.6902, -1460.4005,  1782.1391],
       [  871.8258,   815.3929, -1325.9965, -1365.9242,  1666.9347],
       [-1417.6902, -1325.9965,  2158.4424,  2222.0191, -2711.6822],
       [-1460.4005, -1365.9242,  2222.0191,  2289.0575, -2793.422 ],
       [ 1782.1391,  1666.9347, -2711.6822, -2793.422 ,  3409.5128]])
mat.dot(inv(mat)) 
array([[ 1.,  0., -0., -0., -0.],
       [-0.,  1.,  0.,  0.,  0.],
       [ 0.,  0.,  1.,  0.,  0.],
       [-0.,  0.,  0.,  1., -0.],
       [-0.,  0.,  0.,  0.,  1.]])
q, r = qr(mat)
r 
array([[-1.6914,  4.38  ,  0.1757,  0.4075, -0.7838],
       [ 0.    , -2.6436,  0.1939, -3.072 , -1.0702],
       [ 0.    ,  0.    , -0.8138,  1.5414,  0.6155],
       [ 0.    ,  0.    ,  0.    , -2.6445, -2.1669],
       [ 0.    ,  0.    ,  0.    ,  0.    ,  0.0002]])
```
表达式`X.T.dot(X)`计算`X`和它的转置`X.T`的点积。
下表列出了一些最常用的线性代数函数。
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200328002434.png)
## `NumPy`随机数
### 随机数生成
- `np.random.normal(size=(4,4))`:#生成一个标准正态分布的4*4样本值
- `np.random.rand(d0, d1, ..., dn)`：生成一个`[0,1)`之间的随机浮点数或N维浮点数组 —— 均匀分布
- `np.random.randn(d0, d1, ..., dn)`：生成一个浮点数或N维浮点数组 —— 正态分布
- `np.random.randint(low, high=None, size=None, dtype='l')`：生成一个整数或N维整数数组,若`high`不为`None`时，取`[low,high)`之间随机整数，否则取值`[0,low)`之间随机整数，且`high`必须大于`low`,`dtype`参数：只能是`int`类型  
## 附录
### `ndarray`对象的内部机理
`NumPy`的`ndarray`提供了一种将同质数据块（可以是连续或跨越）解释为多维数组对象的方式。正如你之前所看到的那样，数据类型（`dtype`）决定了数据的解释方式，比如浮点数、整数、布尔值等。
`ndarray`如此强大的部分原因是所有数组对象都是数据块的一个跨度视图（`strided view`）。你可能想知道数组视图`arr[::2,::-1]`不复制任何数据的原因是什么。简单地说，`ndarray`不只是一块内存和一个`dtype`，它还有跨度信息，这使得数组能以各种步幅（`step size`）在内存中移动。更准确地讲，`ndarray`内部由以下内容组成：
1. 一个指向数据（内存或内存映射文件中的一块数据）的指针。
2. 数据类型或`dtype`，描述在数组中的固定大小值的格子。
3. 一个表示数组形状（`shape`）的元组。
4. 一个跨度元组（`stride`），其中的整数指的是为了前进到当前维度下一个元素需要“跨过”的字节数。
下图简单地说明了`ndarray`的内部结构。
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200328003441.png)
### 高级数组操作
#### 数组重塑
与`reshape`将一维数组转换为多维数组的运算过程相反的运算通常称为扁平化（`flattening`）或散开（`raveling`）：
```python
arr = np.arange(15).reshape((5, 3))
arr 
array([[ 0,  1,  2],
       [ 3,  4,  5],
       [ 6,  7,  8],
       [ 9, 10, 11],
       [12, 13, 14]])
arr.ravel()
array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12, 13, 14])
```
如果结果中的值与原始数组相同，`ravel`不会产生源数据的副本。`flatten`方法的行为类似于`ravel`，只不过它总是返回数据的副本：
```python
arr.flatten()
array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12, 13, 14])
```
#### 数组的合并和拆分
`numpy.concatenate`可以按指定轴将一个由数组组成的序列（如元组、列表等）连接到一起：
```python
arr1 = np.array([[1, 2, 3], [4, 5, 6]])
arr2 = np.array([[7, 8, 9], [10, 11, 12]])
np.concatenate([arr1, arr2], axis=0) 
array([[ 1,  2,  3],
       [ 4,  5,  6],
       [ 7,  8,  9],
       [10, 11, 12]])
np.concatenate([arr1, arr2], axis=1) 
array([[ 1,  2,  3,  7,  8,  9],
       [ 4,  5,  6, 10, 11, 12]])
```
对于常见的连接操作，`NumPy`提供了一些比较方便的方法（如`vstack`和`hstack`）。因此，上面的运算还可以表达为：
```python
np.vstack((arr1, arr2)) 
array([[ 1,  2,  3],
       [ 4,  5,  6],
       [ 7,  8,  9],
       [10, 11, 12]])
np.hstack((arr1, arr2))
array([[ 1,  2,  3,  7,  8,  9],
[ 4,  5,  6, 10, 11, 12]])
```
与此相反，`split`用于将一个数组沿指定轴拆分为多个数组,传入到`np.split`的值`[1,3]`指示在哪个索引处分割数组。：
```python
arr = np.random.randn(5, 2)
arr 
array([[-0.2047,  0.4789],
       [-0.5194, -0.5557],
       [ 1.9658,  1.3934],
       [ 0.0929,  0.2817],
       [ 0.769 ,  1.2464]])
first, second, third = np.split(arr, [1, 3])
first
array([[-0.2047,  0.4789]])
second 
array([[-0.5194, -0.5557],
       [ 1.9658,  1.3934]])
third 
array([[ 0.0929,  0.2817],
       [ 0.769 ,  1.2464]])
```
下表中列出了所有关于数组连接和拆分的函数，其中有些是专门为了方便常见的连接运算而提供的。
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200328005543.png)
#### 元素的重复操作：`tile`和`repeat`
对数组进行重复以产生更大数组的工具主要是`repeat`和`tile`这两个函数。`repeat`会将数组中的各个元素重复一定次数，从而产生一个更大的数组：
```python
arr = np.arange(3)
arr
array([0, 1, 2])
arr.repeat(3)
array([0, 0, 0, 1, 1, 1, 2, 2, 2])
```
>笔记：跟其他流行的数组编程语言（如`MATLAB`）不同，`NumPy`中很少需要对数组进行重复（`replicate`）。这主要是因为广播（`broadcasting`，我们将在下一节中讲解该技术）能更好地满足该需求。
默认情况下，如果传入的是一个整数，则各元素就都会重复那么多次。如果传入的是一组整数，则各元素就可以重复不同的次数：
```python
arr.repeat([2, 3, 4])
array([0, 0, 1, 1, 1, 2, 2, 2, 2])
```
对于多维数组，还可以让它们的元素沿指定轴重复：
```python
arr = np.random.randn(2, 2)
arr 
array([[-2.0016, -0.3718],
       [ 1.669 , -0.4386]])
       arr.repeat(2, axis=0)
array([[-2.0016, -0.3718],
       [-2.0016, -0.3718],
       [ 1.669 , -0.4386],
       [ 1.669 , -0.4386]])
```
注意，如果没有设置轴向，则数组会被扁平化，这可能不会是你想要的结果。同样，在对多维进行重复时，也可以传入一组整数，这样就会使各切片重复不同的次数：
```python
arr.repeat([2, 3], axis=0)
array([[-2.0016, -0.3718],
       [-2.0016, -0.3718],
       [ 1.669 , -0.4386],
       [ 1.669 , -0.4386],
       [ 1.669 , -0.4386]])
arr.repeat([2, 3], axis=1) 
array([[-2.0016, -2.0016, -0.3718, -0.3718, -0.3718],
       [ 1.669 ,  1.669 , -0.4386, -0.4386, -0.4386]])
```
`tile`的功能是沿指定轴向堆叠数组的副本。你可以形象地将其想象成“铺瓷砖”：
```python
arr
array([[-2.0016, -0.3718],
       [ 1.669 , -0.4386]])
np.tile(arr, 2)
array([[-2.0016, -0.3718, -2.0016, -0.3718],
       [ 1.669 , -0.4386,  1.669 , -0.4386]])
```
第二个参数是瓷砖的数量。对于标量，瓷砖是水平铺设的，而不是垂直铺设。它可以是一个表示“铺设”布局的元组：
```python
arr 
array([[-2.0016, -0.3718],
       [ 1.669 , -0.4386]])
np.tile(arr, (2, 1)) 
array([[-2.0016, -0.3718],
       [ 1.669 , -0.4386],
       [-2.0016, -0.3718],
       [ 1.669 , -0.4386]])
np.tile(arr, (3, 2))
array([[-2.0016, -0.3718, -2.0016, -0.3718],
       [ 1.669 , -0.4386,  1.669 , -0.4386],
       [-2.0016, -0.3718, -2.0016, -0.3718],
       [ 1.669 , -0.4386,  1.669 , -0.4386],
       [-2.0016, -0.3718, -2.0016, -0.3718],
       [ 1.669 , -0.4386,  1.669 , -0.4386]])
```
#### 花式索引的等价函数：`take`和`put`
获取和设置数组子集的一个办法是通过整数数组使用花式索引：
```python
arr = np.arange(10) * 100
inds = [7, 1, 2, 6]
arr[inds]
array([700, 100, 200, 600])
```
`ndarray`还有其它方法用于获取单个轴向上的选区：
```python
arr.take(inds)
array([700, 100, 200, 600])
arr.put(inds, 42)
arr
array([  0,  42,  42, 300, 400, 500,  42,  42,800, 900])
arr.put(inds, [40, 41, 42, 43])
arr
array([  0,  41,  42, 300, 400, 500,  43,  40, 800, 900])
```
要在其它轴上使用`take`，只需传入`axis`关键字即可：
```python
inds = [2, 0, 2, 1]
arr = np.random.randn(2, 4)
arr
array([[-0.5397,  0.477 ,  3.2489, -1.0212],
       [-0.5771,  0.1241,  0.3026,  0.5238]])
arr.take(inds, axis=1)
array([[ 3.2489, -0.5397,  3.2489,  0.477 ],
       [ 0.3026, -0.5771,  0.3026,  0.1241]])
```
`put`不接受`axis`参数，它只会在数组的扁平化版本（一维，`C`顺序）上进行索引。因此，在需要用其他轴向的索引设置元素时，最好还是使用花式索引。
### 广播
广播（`broadcasting`）指的是不同形状的数组之间的算术运算的执行方式。它是一种非常强大的功能，但也容易令人误解，即使是经验丰富的老手也是如此。将标量值跟数组合并时就会发生最简单的广播：
```python
arr = np.arange(5)
arr
array([0, 1, 2, 3, 4])
arr * 4
array([ 0,  4,  8, 12, 16])
```
这里我们说：在这个乘法运算中，标量值`4`被广播到了其他所有的元素上。
`Broadcast`（广播）的规则：
1. 让所有输入数组都向其中`shape`最长的数组看齐，`shape`中不足的部分都通过在前面加`1`补齐
2. 输出数组的`shape`是输入数组`shape`的各个轴上的最大值
3. 如果输入数组的某个轴和输出数组的对应轴的长度相同或者其长度为`1`时，这个数组能够用来计算，否则出错
4. 当输入数组的某个轴的长度为`1`时，沿着此轴运算时都用此轴上的第一组值
5. 两个`array`的`shape`长度与`shape`的每个对应值都相等的时候，那么结果就是对应元素逐元素运算，运算的结果`shape`不变。`shape`长度不相等时，先把短的`shape`前面一直补`1`，直到与长的`shape`长度相等时，此时，两个`array`的`shape`对应位置上的值 ：
- 相等;
- 其中一个为`1`;
满足其一才能进行广播。
譬如：
#可以广播
A      (4d array):  8 x 1 x 6 x 1
B      (3d array):      7 x 1 x 5
Result (4d array):  8 x 7 x 6 x 5
#不可以广播
A  (2d array):      2 x 1
B  (3d array):  8 x 4 x 3（倒数第二维不匹配）
看一个例子，我们可以通过减去列平均值的方式对数组的每一列进行距平化处理。这个问题解决起来非常简单：
```python
arr = np.random.randn(4, 3)
arr.mean(0)
array([-0.3928, -0.3824, -0.8768])
demeaned = arr - arr.mean(0)
demeaned
array([[ 0.3937,  1.7263,  0.1633],
       [-0.4384, -1.9878, -0.9839],
       [-0.468 ,  0.9426, -0.3891],
       [ 0.5126, -0.6811,  1.2097]])
demeaned.mean(0)
array([-0.,  0., -0.])
```
下图形象地展示了该过程。用广播的方式对行进行距平化处理会稍微麻烦一些。幸运的是，只要遵循一定的规则，低维度的值是可以被广播到数组的任意维度的（比如对二维数组各列减去行平均值）。
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200328010928.png)
于是就得到了：
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200328010949.png)
虽然我是一名经验丰富的`NumPy`老手，但经常还是得停下来画张图并想想广播的原则。再来看一下最后那个例子，假设你希望对各行减去那个平均值。由于`arr.mean(0)`的长度为`3`，所以它可以在`0`轴向上进行广播：因为`arr`的后缘维度是`3`，所以它们是兼容的。根据该原则，要在`1`轴向上做减法（即各行减去行平均值），较小的那个数组的形状必须是`(4,1)`：
```python
arr
array([[ 0.0009,  1.3438, -0.7135],
       [-0.8312, -2.3702, -1.8608],
       [-0.8608,  0.5601, -1.2659],
       [ 0.1198, -1.0635,  0.3329]])
row_means = arr.mean(1)
row_means.shape
(4,)
row_means.reshape((4, 1)) 
array([[ 0.2104],
       [-1.6874],
       [-0.5222],
       [-0.2036]])
demeaned = arr - row_means.reshape((4, 1))
demeaned.mean(1)
array([ 0., -0.,  0.,  0.])
```
下图说明了该运算的过程。
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200328011257.png)
下图展示了另外一种情况，这次是在一个三维数组上沿`0`轴向加上一个二维数组。
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200328011338.png)
对于三维的情况，在三维中的任何一维上广播其实也就是将数据重塑为兼容的形状而已。下图说明了要在三维数组各维度上广播的形状需求。
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200328011338.png)
于是就有了一个非常普遍的问题（尤其是在通用算法中），即专门为了广播而添加一个长度为`1`的新轴。虽然`reshape`是一个办法，但插入轴需要构造一个表示新形状的元组。这是一个很郁闷的过程。因此，`NumPy`数组提供了一种通过索引机制插入轴的特殊语法。下面这段代码通过特殊的`np.newaxis`属性以及“全”切片来插入新轴：
```python
arr = np.zeros((4, 4))
arr_3d = arr[:, np.newaxis, :]
arr_3d.shape
(4, 1, 4)
arr_1d = np.random.normal(size=3)
arr_1d[:, np.newaxis] 
array([[-2.3594],
       [-0.1995],
       [-1.542 ]])
arr_1d[np.newaxis, :]
array([[-2.3594, -0.1995, -1.542 ]])
```
因此，如果我们有一个三维数组，并希望对轴`2`进行距平化，那么只需要编写下面这样的代码就可以了：
```python
arr = np.random.randn(3, 4, 5)
depth_means = arr.mean(2)
depth_means 
array([[-0.4735,  0.3971, -0.0228,  0.2001],
       [-0.3521, -0.281 , -0.071 , -0.1586],
       [ 0.6245,  0.6047,  0.4396, -0.2846]])
depth_means.shape
(3, 4)
demeaned = arr - depth_means[:, :, np.newaxis]
demeaned.mean(2)
array([[ 0.,  0., -0., -0.],
       [ 0.,  0., -0.,  0.],
       [ 0.,  0., -0., -0.]])
```
有些读者可能会想，在对指定轴进行距平化时，有没有一种既通用又不牺牲性能的方法呢？实际上是有的，但需要一些索引方面的技巧：
```python
def demean_axis(arr, axis=0):
    means = arr.mean(axis)

    # This generalizes things like [:, :, np.newaxis] to N dimensions
    indexer = [slice(None)] * arr.ndim#slice(None)等价于:,
    indexer[axis] = np.newaxis
    return arr - means[indexer]
```
算术运算所遵循的广播原则同样也适用于通过索引机制设置数组值的操作。对于最简单的情况，我们可以这样做：
```python
arr = np.zeros((4, 3))
arr[:] = 5
arr 
array([[ 5.,  5.,  5.],
       [ 5.,  5.,  5.],
       [ 5.,  5.,  5.],
       [ 5.,  5.,  5.]])
```
但是，假设我们想要用一个一维数组来设置目标数组的各列，只要保证形状兼容就可以了：
```python
col = np.array([1.28, -0.42, 0.44, 1.6])
arr
array([[ 5.,  5.,  5.],
       [ 5.,  5.,  5.],
       [ 5.,  5.,  5.],
       [ 5.,  5.,  5.]])
arr[:] = col[:, np.newaxis]
arr 
array([[ 1.28,  1.28,  1.28],
       [-0.42, -0.42, -0.42],
       [ 0.44,  0.44,  0.44],
       [ 1.6 ,  1.6 ,  1.6 ]])
arr[:2] = [[-1.37], [0.509]]
arr
array([[-1.37 , -1.37 , -1.37 ],
       [ 0.509,  0.509,  0.509],
       [ 0.44 ,  0.44 ,  0.44 ],
       [ 1.6  ,  1.6  ,  1.6  ]])
```
### `ufunc`高级应用
虽然许多`NumPy`用户只会用到通用函数所提供的快速的元素级运算，但通用函数实际上还有一些高级用法能使我们丢开循环而编写出更为简洁的代码。
`ufunc`实例方法
`NumPy`的各个二元`ufunc`都有一些用于执行特定矢量化运算的特殊方法。下表汇总了这些方法。
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200328012803.png)
`reduce`接受一个数组参数，并通过一系列的二元运算对其值进行聚合（可指明轴向）。例如，我们可以用`np.add.reduce`对数组中各个元素进行求和：
```python
arr = np.arange(10)
np.add.reduce(arr)
45
arr.sum()
45
```
起始值取决于`ufunc`（对于`add`的情况，就是`0`）。如果
如果设置了轴号，约简运算就会沿该轴向执行。这就使你能用一种比较简洁的方式得到某些问题的答案。在下面这个例子中，我们用`np.logical_and`检查数组各行中的值是否是有序的：
```python
np.random.seed(12346)  # for reproducibility
arr = np.random.randn(5, 5)
arr[::2].sort(1) # sort a few rows
arr[:, :-1] < arr[:, 1:] 
array([[ True,  True,  True,  True],
       [False,  True, False, False],
       [ True,  True,  True,  True],
       [ True, False,  True,  True],
       [ True,  True,  True,  True]], dtype=bool)
np.logical_and.reduce(arr[:, :-1] < arr[:, 1:], axis=1)
array([ True, False,  True, False,  True], dtype=bool)
```
>注意，`logical_and.reduce`跟`all`方法是等价的。
`ccumulate`跟`reduce`的关系就像`cumsum`跟`sum`的关系那样。它产生一个跟原数组大小相同的中间“累计”值数组：
```python
arr = np.arange(15).reshape((3, 5))
np.add.accumulate(arr, axis=1) 
array([[ 0,  1,  3,  6, 10],
       [ 5, 11, 18, 26, 35],
       [10, 21, 33, 46, 60]])
```
`outer`用于计算两个数组的叉积：
```python
arr = np.arange(3).repeat([1, 2, 2])
arr
array([0, 1, 1, 2, 2])
np.multiply.outer(arr, np.arange(5))
array([[0, 0, 0, 0, 0],
       [0, 1, 2, 3, 4],
       [0, 1, 2, 3, 4],
       [0, 2, 4, 6, 8],
       [0, 2, 4, 6, 8]])
```
`outer`输出结果的维度是两个输入数据的维度之和：
```python
x, y = np.random.randn(3, 4), np.random.randn(5)
result = np.subtract.outer(x, y)
result.shape
(3, 4, 5)
```
最后一个方法`reduceat`用于计算“局部约简”，其实就是一个对数据各切片进行聚合的`groupby`运算。它接受一组用于指示如何对值进行拆分和聚合的“面元边界”：
```python
arr = np.arange(10)
np.add.reduceat(arr, [0, 5, 8])
array([10, 18, 17])
```
最终结果是在`arr[0:5]`、`arr[5:8]`以及`arr[8:]`上执行的约简。跟其他方法一样，这里也可以传入一个`axis`参数：
```python
arr = np.multiply.outer(np.arange(4), np.arange(5))
arr
array([[ 0,  0,  0,  0,  0],
       [ 0,  1,  2,  3,  4],
       [ 0,  2,  4,  6,  8],
       [ 0,  3,  6,  9, 12]])
np.add.reduceat(arr, [0, 2, 4], axis=1)
array([[ 0,  0,  0],
       [ 1,  5,  4],
       [ 2, 10,  8],
       [ 3, 15, 12]])
```
### 编写新的`ufunc`
有多种方法可以让你编写自己的`NumPy ufuncs`。
`numpy.frompyfunc`接受一个`Python`函数以及两个分别表示输入输出参数数量的参数。例如，下面是一个能够实现元素级加法的简单函数：
```python
def add_elements(x, y):
    return x + y
add_them = np.frompyfunc(add_elements, 2, 1)
add_them(np.arange(8), np.arange(8))
array([0, 2, 4, 6, 8, 10, 12, 14], dtype=object)
```
用`frompyfunc`创建的函数总是返回`Python`对象数组，这一点很不方便。幸运的是，还有另一个办法，即`numpy.vectorize`。虽然没有`frompyfunc`那么强大，但可以让你指定输出类型：
```python
add_them = np.vectorize(add_elements, otypes=[np.float64])
add_them(np.arange(8), np.arange(8))
array([  0.,   2.,   4.,   6.,   8.,  10.,  12.,  14.])
```
虽然这两个函数提供了一种创建`ufunc`型函数的手段，但它们非常慢，因为它们在计算每个元素时都要执行一次`Python`函数调用，这就会比`NumPy`自带的基于`C`的`ufunc`慢很多：
```python
arr = np.random.randn(10000)
%timeit add_them(arr, arr)
4.12 ms +- 182 us per loop (mean +- std. dev. of 7 runs, 100 loops each)
%timeit np.add(arr, arr)
6.89 us +- 504 ns per loop (mean +- std. dev. of 7 runs, 100000 loops each)
```
###  结构化和记录式数组
你可能已经注意到了，到目前为止我们所讨论的`ndarray`都是一种同质数据容器，也就是说，在它所表示的内存块中，各元素占用的字节数相同（具体根据`dtype`而定）。从表面上看，它似乎不能用于表示异质或表格型的数据。结构化数组是一种特殊的`ndarray`，其中的各个元素可以被看做`C`语言中的结构体（`struct`，这就是“结构化”的由来）或`SQL`表中带有多个命名字段的行：
```python
dtype = [('x', np.float64), ('y', np.int32)]
sarr = np.array([(1.5, 6), (np.pi, -2)], dtype=dtype)
sarr
array([( 1.5   ,  6), ( 3.1416, -2)],
      dtype=[('x', '<f8'), ('y', '<i4')])
```
定义结构化`dtype`（请参考`NumPy`的在线文档）的方式有很多。最典型的办法是元组列表，各元组的格式为(`field_name`,`field_data_type`)。这样，数组的元素就成了元组式的对象，该对象中各个元素可以像字典那样进行访问：
```python
sarr[0]
( 1.5, 6)
sarr[0]['y']
6
```
字段名保存在`dtype.names`属性中。在访问结构化数组的某个字段时，返回的是该数据的视图，所以不会发生数据复制：
```python
sarr['x']
array([ 1.5   ,  3.1416])
```
嵌套`dtype`和多维字段
在定义结构化`dtype`时，你可以再设置一个形状（可以是一个整数，也可以是一个元组）：
```python
dtype = [('x', np.int64, 3), ('y', np.int32)]
arr = np.zeros(4, dtype=dtype)
arr
array([([0, 0, 0], 0), ([0, 0, 0], 0), ([0, 0, 0], 0), ([0, 0, 0], 0)],
      dtype=[('x', '<i8', (3,)), ('y', '<i4')])
```
在这种情况下，各个记录的`x`字段所表示的是一个长度为`3`的数组：
```python
arr[0]['x']
array([0, 0, 0])
```
这样，访问`arr['x']`即可得到一个二维数组，而不是前面那个例子中的一维数组：
```python
arr['x']
array([[0, 0, 0],
       [0, 0, 0],
       [0, 0, 0],
       [0, 0, 0]])
```
这就使你能用单个数组的内存块存放复杂的嵌套结构。你还可以嵌套`dtype`，作出更复杂的结构。下面是一个简单的例子：
```python
dtype = [('x', [('a', 'f8'), ('b', 'f4')]), ('y', np.int32)]
data = np.array([((1, 2), 5), ((3, 4), 6)], dtype=dtype)
data['x']
array([( 1.,  2.), ( 3.,  4.)],
      dtype=[('a', '<f8'), ('b', '<f4')])
data['y']
array([5, 6], dtype=int32)
data['x']['a']
array([ 1.,  3.])
```
`pandas`的`DataFrame`并不直接支持该功能，但它的分层索引机制跟这个差不多。
跟`pandas`的`DataFrame`相比，`NumPy`的结构化数组是一种相对较低级的工具。它可以将单个内存块解释为带有任意复杂嵌套列的表格型结构。由于数组中的每个元素在内存中都被表示为固定的字节数，所以结构化数组能够提供非常快速高效的磁盘数据读写（包括内存映像）、网络传输等功能。
结构化数组的另一个常见用法是，将数据文件写成定长记录字节流，这是`C`和`C++`代码中常见的数据序列化手段（业界许多历史系统中都能找得到）。只要知道文件的格式（记录的大小、元素的顺序、字节数以及数据类型等），就可以用`np.fromfile`将数据读入内存。
### 更多有关排序的话题
跟`Python`内置的列表一样，`ndarray`的`sort`实例方法也是就地排序。也就是说，数组内容的重新排列是不会产生新数组的：
```python
arr = np.random.randn(6)
arr.sort()
arr
array([-1.082 ,  0.3759,  0.8014,  1.1397,  1.2888,  1.8413])
```
在对数组进行就地排序时要注意一点，如果目标数组只是一个视图，则原始数组将会被修改：
```python
arr = np.random.randn(3, 5)
arr 
array([[-0.3318, -1.4711,  0.8705, -0.0847, -1.1329],
       [-1.0111, -0.3436,  2.1714,  0.1234, -0.0189],
       [ 0.1773,  0.7424,  0.8548,  1.038 , -0.329 ]])
arr[:, 0].sort()  # Sort first column values in-place
arr
array([[-1.0111, -1.4711,  0.8705, -0.0847, -1.1329],
       [-0.3318, -0.3436,  2.1714,  0.1234, -0.0189],
       [ 0.1773,  0.7424,  0.8548,  1.038 , -0.329 ]])
```
相反，`numpy.sort`会为原数组创建一个已排序副本。另外，它所接受的参数（比如`kind`）跟`ndarray.sort`一样：
```python
arr = np.random.randn(5)
arr
array([-1.1181, -0.2415, -2.0051,  0.7379, -1.0614])
np.sort(arr)
array([-2.0051, -1.1181, -1.0614, -0.2415,  0.7379])
arr
array([-1.1181, -0.2415, -2.0051,  0.7379, -1.0614])
```
这两个排序方法都可以接受一个`axis`参数，以便沿指定轴向对各块数据进行单独排序：
```python
arr = np.random.randn(3, 5)
arr
array([[ 0.5955, -0.2682,  1.3389, -0.1872,  0.9111],
       [-0.3215,  1.0054, -0.5168,  1.1925, -0.1989],
       [ 0.3969, -1.7638,  0.6071, -0.2222, -0.2171]])
arr.sort(axis=1)
arr
array([[-0.2682, -0.1872,  0.5955,  0.9111,  1.3389],
       [-0.5168, -0.3215, -0.1989,  1.0054,  1.1925],
       [-1.7638, -0.2222, -0.2171,  0.3969,  0.6071]])
```
你可能注意到了，这两个排序方法都不可以被设置为降序。其实这也无所谓，因为数组切片会产生视图（也就是说，不会产生副本，也不需要任何其他的计算工作）。许多`Python`用户都很熟悉一个有关列表的小技巧：`values[::-1]`可以返回一个反序的列表。对`ndarray`也是如此：
```python
arr[:, ::-1]
array([[ 1.3389,  0.9111,  0.5955, -0.1872, -0.2682],
       [ 1.1925,  1.0054, -0.1989, -0.3215, -0.5168],
       [ 0.6071,  0.3969, -0.2171, -0.2222, -1.7638]])
```
#### 间接排序：`argsort`和`lexsort`
在数据分析工作中，常常需要根据一个或多个键对数据集进行排序。例如，一个有关学生信息的数据表可能需要以姓和名进行排序（先姓后名）。这就是间接排序的一个例子，如果你阅读过有关`pandas`的章节，那就已经见过不少高级例子了。给定一个或多个键，你就可以得到一个由整数组成的索引数组（我亲切地称之为索引器），其中的索引值说明了数据在新顺序下的位置。`argsort`和`numpy.lexsort`就是实现该功能的两个主要方法。下面是一个简单的例子：
```python
values = np.array([5, 0, 1, 3, 2])
indexer = values.argsort()
indexer
array([1, 2, 4, 3, 0])
values[indexer]
array([0, 1, 2, 3, 5])
```
一个更复杂的例子，下面这段代码根据数组的第一行对其进行排序：
```python
arr = np.random.randn(3, 5)
arr[0] = values
arr 
array([[ 5.    ,  0.    ,  1.    ,  3.    ,  2.    ],
       [-0.3636, -0.1378,  2.1777, -0.4728,  0.8356],
       [-0.2089,  0.2316,  0.728 , -1.3918,  1.9956]])
arr[:, arr[0].argsort()]
array([[ 0.    ,  1.    ,  2.    ,  3.    ,  5.    ],
       [-0.1378,  2.1777,  0.8356, -0.4728, -0.3636],
       [ 0.2316,  0.728 ,  1.9956, -1.3918, -0.2089]])
```
`lexsort`跟`argsort`差不多，只不过它可以一次性对多个键数组执行间接排序（字典序）。假设我们想对一些以姓和名标识的数据进行排序：
```python
first_name = np.array(['Bob', 'Jane', 'Steve', 'Bill', 'Barbara'])
last_name = np.array(['Jones', 'Arnold', 'Arnold', 'Jones', 'Walters'])
sorter = np.lexsort((first_name, last_name))
sorter
array([1, 2, 3, 0, 4])
zip(last_name[sorter], first_name[sorter])
<zip at 0x7fa203eda1c8>
```
刚开始使用`lexsort`的时候可能会比较容易头晕，这是因为键的应用顺序是从最后一个传入的算起的。不难看出，`last_name`是先于`first_name`被应用的。
>笔记：`Series`和`DataFrame`的`sort_index`以及`Series`的`order`方法就是通过这些函数的变体（它们还必须考虑缺失值）实现的。
#### 其他排序算法
稳定的（`stable`）排序算法会保持等价元素的相对位置。对于相对位置具有实际意义的那些间接排序而言，这一点非常重要：
```python
values = np.array(['2:first', '2:second', '1:first', '1:second','1:third'])
key = np.array([2, 2, 1, 1, 1])
indexer = key.argsort(kind='mergesort')
indexer
array([2, 3, 4, 0, 1])
values.take(indexer)
array(['1:first', '1:second', '1:third', '2:first', '2:second'],
      dtype='<U8')
```
`mergesort`（合并排序）是唯一的稳定排序，它保证有`O(nlogn)`的性能（空间复杂度），但是其平均性能比默认的`quicksort`（快速排序）要差。
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200328020416.png)
#### 部分排序数组
排序的目的之一可能是确定数组中最大或最小的元素。`NumPy`有两个优化方法，`numpy.partition`和`np.argpartition`，可以在第k个最小元素划分的数组：
```python
np.random.seed(12345)
arr = np.random.randn(20)
arr
array([-0.2047,  0.4789, -0.5194, -0.5557,  1.9658,  1.3934,  0.0929,
        0.2817,  0.769 ,  1.2464,  1.0072, -1.2962,  0.275 ,  0.2289,
        1.3529,  0.8864, -2.0016, -0.3718,  1.669 , -0.4386])
np.partition(arr, 3)
array([-2.0016, -1.2962, -0.5557, -0.5194, -0.3718, -0.4386, -0.2047,
        0.2817,  0.769 ,  0.4789,  1.0072,  0.0929,  0.275 ,  0.2289,
        1.3529,  0.8864,  1.3934,  1.9658,  1.669 ,  1.2464])
```
当你调用`partition(arr, 3)`，结果中的头三个元素是最小的三个，没有特定的顺序。`numpy.argpartition`与`numpy.argsort`相似，会返回索引，重排数据为等价的顺序：
```python
indices = np.argpartition(arr, 3)
indices
array([16, 11,  3,  2, 17, 19,  0,  7,  8,  1, 10,  6, 12, 13, 14, 15,  5,
        4, 18,  9])
arr.take(indices)
array([-2.0016, -1.2962, -0.5557, -0.5194, -0.3718, -0.4386, -0.2047,
        0.2817,  0.769 ,  0.4789,  1.0072,  0.0929,  0.275 ,  0.2289,
        1.3529,  0.8864,  1.3934,  1.9658,  1.669 ,  1.2464])
```
`searchsorted`是一个在有序数组上执行二分查找的数组方法，只要将值插入到它返回的那个位置就能维持数组的有序性：
```python
arr = np.array([0, 1, 7, 12, 15])
arr.searchsorted(9)
3
```
你可以传入一组值就能得到一组索引：
```python
arr.searchsorted([0, 8, 11, 16])
array([0, 3, 3, 5])
```
从上面的结果中可以看出，对于元素`0`，`searchsorted`会返回`0`。这是因为其默认行为是返回相等值组的左侧索引：
```python
arr = np.array([0, 0, 0, 1, 1, 1, 1])
arr.searchsorted([0, 1])
array([0, 3])
arr.searchsorted([0, 1], side='right')
array([3, 7])
```
再来看`searchsorted`的另一个用法，假设我们有一个数据数组（其中的值在`0`到`10000`之间），还有一个表示“面元边界”的数组，我们希望用它将数据数组拆分开：
```python
data = np.floor(np.random.uniform(0, 10000, size=50))
bins = np.array([0, 100, 1000, 5000, 10000])
data
array([ 9940.,  6768.,  7908.,  1709.,   268.,  8003., 9037.,   246.,
        4917.,  5262.,  5963.,   519.,  8950.,  7282.,  8183.,  5002.,
        8101.,   959.,  2189.,  2587.,  4681.,  4593.,  7095.,  1780.,
        5314.,  1677.,  7688.,  9281.,  6094.,  1501.,  4896.,  3773.,
        8486.,  9110.,  3838.,  3154.,  5683.,  1878.,  1258.,  6875.,
        7996.,  5735.,  9732.,  6340.,  8884.,  4954.,  3516.,  7142.,
        5039.,  2256.])
```
然后，为了得到各数据点所属区间的编号（其中1表示面元`[0,100)`），我们可以直接使用`searchsorted`：
```python
labels = bins.searchsorted(data)
labels 
array([4, 4, 4, 3, 2, 4, 4, 2, 3, 4, 4, 2, 4, 4, 4, 4, 4, 2, 3, 3, 3, 3, 4,
       3, 4, 3, 4, 4, 4, 3, 3, 3, 4, 4, 3, 3, 4, 3, 3, 4, 4, 4, 4, 4, 4, 3,
       3, 4, 4, 3])
```
通过`pandas`的`groupby`使用该结果即可非常轻松地对原数据集进行拆分：
```python
pd.Series(data).groupby(labels).mean()
2     498.000000
3    3064.277778
4    7389.035714
dtype: float64
```
### 用`Numba`编写快速`NumPy`函数
`Numba`是一个开源项目，它可以利用`CPUs`、`GPUs`或其它硬件为类似`NumPy`的数据创建快速函数。它使用了`LLVM`项目，将`Python`代码转换为机器代码。
为了介绍`Numba`，来考虑一个纯粹的`Python`函数，它使用`for`循环计算表达式`(x - y).mean()`：
```python
import numpy as np
def mean_distance(x, y):
    nx = len(x)
    result = 0.0
    count = 0
    for i in range(nx):
        result += x[i] - y[i]
        count += 1
    return result / count
```
`Numba`的版本要比它快过100倍。我们可以转换这个函数为编译的`Numba`函数，使用`numba.jit`函数：
```python
import numba as nb
numba_mean_distance = nb.jit(mean_distance)
```
也可以写成装饰器：
```python
@nb.njit
def mean_distance(x, y):
    nx = len(x)
    result = 0.0
    count = 0
    for i in range(nx):
        result += x[i] - y[i]
        count += 1
    return result / count
```
### 性能建议
使用`NumPy`的代码的性能一般都很不错，因为数组运算一般都比纯`Python`循环快得多。下面大致列出了一些需要注意的事项：
- 将`Python`循环和条件逻辑转换为数组运算和布尔数组运算。
- 尽量使用广播。
- 避免复制数据，尽量使用数组视图（即切片）。
- 利用`ufunc`及其各种方法。
在某些应用场景中，数组的内存布局可以对计算速度造成极大的影响。这是因为性能差别在一定程度上跟`CPU`的高速缓存（`cache`）体系有关。运算过程中访问连续内存块（例如，对以`C`顺序存储的数组的行求和）一般是最快的，因为内存子系统会将适当的内存块缓存到超高速的`L1`或`L2``CPU Cache`中。此外，`NumPy`的`C`语言基础代码（某些）对连续存储的情况进行了优化处理，这样就能避免一些跨越式的内存访问。

一个数组的内存布局是连续的，就是说元素是以它们在数组中出现的顺序（即`Fortran`型（列优先）或`C`型（行优先））存储在内存中的。默认情况下，`NumPy`数组是以`C`型连续的方式创建的。列优先的数组（比如`C`型连续数组的转置）也被称为`Fortran`型连续。通过`ndarray`的`flags`属性即可查看这些信息：
```python
arr_c = np.ones((1000, 1000), order='C')
arr_f = np.ones((1000, 1000), order='F')
arr_c.flags
  C_CONTIGUOUS : True
  F_CONTIGUOUS : False
  OWNDATA : True
  WRITEABLE : True
  ALIGNED : True
  UPDATEIFCOPY : False
arr_f.flags
  C_CONTIGUOUS : False
  F_CONTIGUOUS : True
  OWNDATA : True
  WRITEABLE : True
  ALIGNED : True
  UPDATEIFCOPY : False
arr_f.flags.f_contiguous
True
```
在这个例子中，对两个数组的行进行求和计算，理论上说，`arr_c`会比`arr_f`快，因为`arr_c`的行在内存中是连续的。
注意，在构造数组的视图时，其结果不一定是连续的：
```python
arr_c[:50].flags.contiguous
True
arr_c[:, :50].flags
  C_CONTIGUOUS : False
  F_CONTIGUOUS : False
  OWNDATA : False
  WRITEABLE : True
  ALIGNED : True
  UPDATEIFCOPY : False
```




