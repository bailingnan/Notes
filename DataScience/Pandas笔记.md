# `Pandas`笔记
<!-- TOC -->

- [`Pandas`笔记](#pandas笔记)
  - [`Pandas`加速方案](#pandas加速方案)
  - [生成对象](#生成对象)
    - [数据结构](#数据结构)
      - [`Series`](#series)
        - [`Series`类似多维数组](#series类似多维数组)
        - [`Series` 类似字典](#series-类似字典)
        - [矢量操作与对齐 `Series` 标签](#矢量操作与对齐-series-标签)
        - [名称属性](#名称属性)
      - [`DataFrame`](#dataframe)
        - [用 `Series` 字典或字典生成 `DataFrame`](#用-series-字典或字典生成-dataframe)
        - [用多维数组字典、列表字典生成 `DataFrame`](#用多维数组字典列表字典生成-dataframe)
        - [用结构多维数组或记录多维数组生成 `DataFrame`](#用结构多维数组或记录多维数组生成-dataframe)
        - [用列表字典生成 `DataFrame`](#用列表字典生成-dataframe)
        - [用元组字典生成 `DataFrame`](#用元组字典生成-dataframe)
        - [用 `Series` 创建 `DataFrame`](#用-series-创建-dataframe)
        - [备选构建器](#备选构建器)
        - [提取、添加、删除列](#提取添加删除列)
        - [用方法链分配新列](#用方法链分配新列)
        - [索引 / 选择](#索引--选择)
        - [数据对齐和运算](#数据对齐和运算)
        - [转置](#转置)
        - [`DataFrame` 应用 `NumPy`函数](#dataframe-应用-numpy函数)
        - [`DataFrame` 列属性访问](#dataframe-列属性访问)
    - [数据类型](#数据类型)
    - [整数被强制转换为浮点数](#整数被强制转换为浮点数)
    - [默认值](#默认值)
    - [向上转型](#向上转型)
    - [`astype·](#astype)
    - [对象转换](#对象转换)
    - [各种坑](#各种坑)
  - [查看数据](#查看数据)
  - [二进制操作](#二进制操作)
    - [匹配/广播机制](#匹配广播机制)
    - [比较操作](#比较操作)
    - [布尔简化](#布尔简化)
    - [比较对象是否等效](#比较对象是否等效)
    - [比较 `array` 型对象](#比较-array-型对象)
    - [合并重叠数据集](#合并重叠数据集)
    - [`DataFrame` 通用合并方法](#dataframe-通用合并方法)
  - [描述性统计](#描述性统计)
    - [数据总结：`describe`](#数据总结describe)
    - [最大值与最小值对应的索引](#最大值与最小值对应的索引)
    - [值计数（直方图）与众数](#值计数直方图与众数)
    - [离散化与分位数](#离散化与分位数)
  - [函数应用](#函数应用)
    - [表级函数应用](#表级函数应用)
    - [行列级函数应用](#行列级函数应用)
    - [聚合 `API`](#聚合-api)
    - [多函数聚合](#多函数聚合)
    - [用字典实现聚合](#用字典实现聚合)
    - [多种数据类型（`Dtype`）](#多种数据类型dtype)
    - [自定义 `Describe`](#自定义-describe)
    - [`Transform API`](#transform-api)
    - [多函数 `Transform`](#多函数-transform)
    - [用字典执行 `transform` 操作](#用字典执行-transform-操作)
    - [元素级函数应用](#元素级函数应用)
  - [重置索引与更换标签](#重置索引与更换标签)
    - [重置索引，并与其它对象对齐](#重置索引并与其它对象对齐)
    - [用 `align` 对齐多个对象](#用-align-对齐多个对象)
    - [重置索引填充的限制](#重置索引填充的限制)
    - [去掉轴上的标签](#去掉轴上的标签)
    - [重命名或映射标签](#重命名或映射标签)
  - [迭代](#迭代)
    - [项目（`items`）](#项目items)
    - [`iterrows()`](#iterrows)
    - [`itertuples()`](#itertuples)
  - [`.dt` 访问器](#dt-访问器)
    - [`DatetimeIndex`](#datetimeindex)
    - [PeriodIndex](#periodindex)
    - [`period`](#period)
    - [`timedelta`](#timedelta)
  - [矢量化字符串方法](#矢量化字符串方法)
  - [排序](#排序)
    - [按索引排序](#按索引排序)
    - [按值排序](#按值排序)
    - [按索引与值排序](#按索引与值排序)
      - [创建 `MultiIndex`](#创建-multiindex)
      - [创建 `DataFrame`](#创建-dataframe)
    - [搜索排序](#搜索排序)
    - [最大值与最小值](#最大值与最小值)
    - [用多层索引的列排序](#用多层索引的列排序)
  - [选择](#选择)
    - [获取数据](#获取数据)
    - [按标签选择](#按标签选择)
    - [按位置选择](#按位置选择)
    - [布尔索引](#布尔索引)
    - [赋值](#赋值)
    - [基于 `dtype` 选择列](#基于-dtype-选择列)
  - [缺失值](#缺失值)
  - [运算](#运算)
    - [统计](#统计)
    - [`Apply` 函数](#apply-函数)
    - [直方图](#直方图)
    - [字符串方法](#字符串方法)
  - [合并（`Merge`）](#合并merge)
    - [结合（`Concat`）](#结合concat)
    - [连接（`join`）](#连接join)
    - [追加（`Append`）](#追加append)
  - [分组（`Grouping`）](#分组grouping)
    - [堆叠（`Stack`）](#堆叠stack)
  - [数据透视表（`Pivot Tables`）](#数据透视表pivot-tables)
  - [时间序列(`TimeSeries`)](#时间序列timeseries)
  - [类别型（`Categoricals`）](#类别型categoricals)
  - [可视化](#可视化)
  - [数据输入 / 输出](#数据输入--输出)
    - [`CSV`](#csv)
    - [`HDF5`](#hdf5)
  - [各种坑（`Gotchas`）](#各种坑gotchas)
  - [与`SQL`比较](#与sql比较)
    - [`SELECT`](#select)
    - [`WHERE`](#where)
    - [`GROUP BY`](#group-by)
    - [`JOIN`](#join)
    - [`LEFT OUTER JOIN`](#left-outer-join)
    - [`RIGHT JOIN`](#right-join)
    - [`FULL JOIN`](#full-join)
    - [`UNION`](#union)
    - [`Pandas`等同于某些`SQL`分析和聚合函数](#pandas等同于某些sql分析和聚合函数)
      - [带有偏移量的前N行](#带有偏移量的前n行)
      - [每组前`N`行](#每组前n行)
    - [更新（`UPDATE`）](#更新update)
    - [删除（`DELETE`）](#删除delete)

<!-- /TOC -->
```python
import numpy as np
import pandas as pd
```
## `Pandas`加速方案
   - `modin`:`import modin.pandas as mdpd`,用`mdpd`代替`pd`即可，加速`pandas`,加载数据和查询数据更快,统计方法`pandas`更快
   - `swifter`:`df.apply()`→`df.swifter.apply()`，加速`pandas`
   - `cupy`:加速`pandas`,1000万以上数据更快
   - `numexpr`/`bottleneck`:`Pandas` 可以加速特定类型的二进制数值与布尔操作。处理大型数据集时，这两个支持库特别有用，加速效果也非常明显。 `numexpr` 使用智能分块、缓存与多核技术。`bottleneck` 是一组专属 `cython` 例程，处理含 `nans` 值的数组时，特别快.
这两个支持库默认为启用状态，可用以下选项设置：
```python
pd.set_option('compute.use_bottleneck', True)
pd.set_option('compute.use_numexpr', True)
```
## 生成对象
### 数据结构
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200401150738.png)
`Pandas`所有数据结构的值都是可变的，但数据结构的大小并非都是可变的，比如，`Series` 的长度不可改变，但 `DataFrame` 里就可以插入列。
`Pandas` 里，绝大多数方法都不改变原始的输入数据，而是复制数据，生成新的对象。 一般来说，原始输入数据不变更稳妥。
`Pandas` 可以通过多个属性访问元数据：
- `shape`:输出对象的轴维度，与 `ndarray` 一致
- 轴标签:
  - `Series`: `Index` (仅有此轴)
  - `DataFrame`: `Index` (行) 与列
>注意： 为属性赋值是安全的！
`Pandas` 对象（`Index`， `Series`， `DataFrame`）相当于数组的容器，用于存储数据、执行计算。大部分类型的底层数组都是 `numpy.ndarray`。不过，`Pandas` 与第三方支持库一般都会扩展 `NumPy` 类型系统，添加自定义数组。
`.array` 属性用于提取 `Index` 或 `Series` 里的数据。
```python
index = pd.date_range('1/1/2000', periods=8)
s = pd.Series(np.random.randn(5), index=['a', 'b', 'c', 'd', 'e'])
df = pd.DataFrame(np.random.randn(8, 3), index=index,columns=['A', 'B', 'C'])
s.array
<PandasArray>
[ 0.4691122999071863, -0.2828633443286633, -1.5090585031735124,
 -1.1356323710171934,  1.2121120250208506]
Length: 5, dtype: float64
s.index.array
<PandasArray>
['a', 'b', 'c', 'd', 'e']
Length: 5, dtype: object
```

#### `Series`
`Series`是带标签的一维数组，可存储整数、浮点数、字符串、`Python`对象等类型的数据。轴标签统称为索引。调用 `pd.Series` 函数即可创建 `Series`：
```python
s = pd.Series(data, index=index)
```
上述代码中，`data` 支持以下数据类型：
- `Python`字典
- 多维数组
- 标量

`index`是轴标签列表。不同数据可分为以下几种情况：
- 多维数组

`data` 是多维数组时，`index` 长度必须与 `data` 长度一致。没有指定 `index` 参数时，创建数值型索引，即 `[0, ..., len(data) - 1]`。
```python
s = pd.Series(np.random.randn(5), index=['a', 'b', 'c', 'd', 'e'])
s 
a    0.469112
b   -0.282863
c   -1.509059
d   -1.135632
e    1.212112
dtype: float64
s.index
Index(['a', 'b', 'c', 'd', 'e'], dtype='object')
pd.Series(np.random.randn(5)) 
0   -0.173215
1    0.119209
2   -1.044236
3   -0.861849
4   -2.104569
dtype: float64
```
>注意：`Pandas` 的索引值可以重复。不支持重复索引值的操作会触发异常。其原因主要与性能有关，有很多计算实例，比如 `GroupBy` 操作就不用索引。
- 字典

`Series` 可以用字典实例化：
```python
d = {'b': 1, 'a': 0, 'c': 2}
pd.Series(d) 
b    1
a    0
c    2
dtype: int64
```
如果设置了 `index` 参数，则按索引标签提取 `data` 里对应的值。
```python
d = {'a': 0., 'b': 1., 'c': 2.}
pd.Series(d) 
a    0.0
b    1.0
c    2.0
dtype: float64
pd.Series(d, index=['b', 'c', 'd', 'a']) 
b    1.0
c    2.0
d    NaN
a    0.0
dtype: float64
```
>注意：`Pandas` 用 `NaN`（`Not a Number`）表示**缺失数据**。

- 标量值

`data` 是标量值时，必须提供索引。`Series` 按索引长度重复该标量值。
```python
pd.Series(5., index=['a', 'b', 'c', 'd', 'e']) 
a    5.0
b    5.0
c    5.0
d    5.0
e    5.0
dtype: float64
```
##### `Series`类似多维数组
`Series` 操作与 `ndarray` 类似，支持大多数 `NumPy` 函数，还支持索引切片。
```python
s = pd.Series(np.random.randn(5), index=['a', 'b', 'c', 'd', 'e'])
s 
a    0.469112
b   -0.282863
c   -1.509059
d   -1.135632
e    1.212112
dtype: float64
s[0]
0.4691122999071863
s[:3]
a    0.469112
b   -0.282863
c   -1.509059
dtype: float64
s[s > s.median()]
a    0.469112
e    1.212112
dtype: float64
s[[4, 3, 1]]
e    1.212112
d   -1.135632
b   -0.282863
dtype: float64
np.exp(s) 
a    1.598575
b    0.753623
c    0.221118
d    0.321219
e    3.360575
dtype: float64
```
和 `NumPy` 数组一样，`Series` 也支持 `dtype`。
```python
s.dtype
dtype('float64')
```
`Series` 的数据类型一般是 `NumPy` 数据类型。不过，`Pandas` 和第三方库在一些方面扩展了 `NumPy` 类型系统，即`扩展数据类型`。
`Series.array` 用于提取 `Series` 数组。
```python
s.array 
<PandasArray>
[ 0.4691122999071863, -0.2828633443286633, -1.5090585031735124,
 -1.1356323710171934,  1.2121120250208506]
Length: 5, dtype: float64
```
执行不用索引的操作时，如禁用自动对齐，访问数组非常有用。
`Series.array` 一般是扩展数组。简单说，扩展数组是把 `N` 个 `numpy.ndarray` 包在一起的打包器。`Pandas` 知道怎么把扩展数组存储到 `Series` 或 `DataFrame` 的列里。
Series 只是类似于多维数组，提取真正的多维数组，要用 `Series.to_numpy()`或`numpy.asarray()`。
```python
s.to_numpy()
array([ 0.4691, -0.2829, -1.5091, -1.1356,  1.2121])
np.asarray(s)
array([ 0.4691, -0.2829, -1.5091, -1.1356,  1.2121])
```
`Series` 是扩展数组 ，`Series.to_numpy()` 返回的是 `NumPy` 多维数组,其代价是需要复制、并强制转换数据的值。
 `Series.array` 则只返回 `ExtensionArray`，且不会复制数据。
##### `Series` 类似字典
`Series` 类似固定大小的字典，可以用索引标签提取值或设置值：
```python
s['a']
0.4691122999071863
s['e'] = 12.
s
a     0.469112
b    -0.282863
c    -1.509059
d    -1.135632
e    12.000000
dtype: float64
'e' in s
True
'f' in s
False
```
引用 `Series` 里没有的标签会触发异常：
```python
s['f']
KeyError: 'f'
```
`get` 方法可以提取 `Series` 里没有的标签，返回 `None` 或指定默认值：
```python
s.get('f')

s.get('f', np.nan)
nan
```
##### 矢量操作与对齐 `Series` 标签
`Series` 和 `NumPy` 数组一样，都不用循环每个值，而且 `Series` 支持大多数 `NumPy` 多维数组的方法。
```python
s + s 
a     0.938225
b    -0.565727
c    -3.018117
d    -2.271265
e    24.000000
dtype: float64
s * 2 
a     0.938225
b    -0.565727
c    -3.018117
d    -2.271265
e    24.000000
dtype: float64
np.exp(s) 
a         1.598575
b         0.753623
c         0.221118
d         0.321219
e    162754.791419
dtype: float64
```
`Series` 和多维数组的主要区别在于， `Series` 之间的操作会自动基于标签对齐数据。因此，不用顾及执行计算操作的 `Series` 是否有相同的标签。
```python
s[1:] + s[:-1] 
a         NaN
b   -0.565727
c   -3.018117
d   -2.271265
e         NaN
dtype: float64
```
操作未对齐索引的 `Series`， 其计算结果是所有涉及索引的并集。如果在 `Series` 里找不到标签，运算结果标记为 `NaN`，即缺失值。编写无需显式对齐数据的代码，给交互数据分析和研究提供了巨大的自由度和灵活性。`Pandas` 数据结构集成的数据对齐功能，是 `Pandas` 区别于大多数标签型数据处理工具的重要特性。
>注意：总之，让不同索引对象操作的默认结果生成索引并集，是为了避免信息丢失。就算缺失了数据，索引标签依然包含计算的重要信息。当然，也可以用`dropna`函数清除含有缺失值的标签。
##### 名称属性
`Series` 支持 `name` 属性：
```python
s = pd.Series(np.random.randn(5), name='something')
s 
0   -0.494929
1    1.071804
2    0.721555
3   -0.706771
4   -1.039575
Name: something, dtype: float64
s.name
'something'
```
一般情况下，`Series` 自动分配 `name`，特别是提取一维 `DataFrame` 切片时。
`pandas.Series.rename()` 方法用于重命名 `Series` 。
```python
s2 = s.rename("different")
s2.name
'different'
```
注意，`s` 与 `s2` 指向不同的对象。
#### `DataFrame`
`DataFrame` 是由多种类型的列构成的二维标签数据结构，类似于 Excel 、SQL 表，或 `Series` 对象构成的字典。`DataFrame` 是最常用的 `Pandas` 对象，与 `Series` 一样，`DataFrame` 支持多种类型的输入数据：
- 一维 `ndarray`、列表、字典、`Series` 字典
- 二维 `numpy.ndarray`
- 结构多维数组或记录多维数组
- `Series`
- `DataFrame`

除了数据，还可以有选择地传递 `index`（行标签）和 `columns`（列标签）参数。传递了索引或列，就可以确保生成的 `DataFrame` 里包含索引或列。`Series` 字典加上指定索引时，会丢弃与传递的索引不匹配的所有数据。
没有传递轴标签时，按常规依据输入数据进行构建。
##### 用 `Series` 字典或字典生成 `DataFrame`
生成的索引是每个 `Series` 索引的并集。先把嵌套字典转换为 `Series`。如果没有指定列，`DataFrame` 的列就是字典键的有序列表。
```python
d = {'one': pd.Series([1., 2., 3.], index=['a', 'b', 'c']),
'two': pd.Series([1., 2., 3., 4.], index=['a', 'b', 'c', 'd'])}
df = pd.DataFrame(d)
df 
   one  two
a  1.0  1.0
b  2.0  2.0
c  3.0  3.0
d  NaN  4.0
pd.DataFrame(d, index=['d', 'b', 'a']) 
   one  two
d  NaN  4.0
b  2.0  2.0
a  1.0  1.0
pd.DataFrame(d, index=['d', 'b', 'a'], columns=['two', 'three']) 
   two three
d  4.0   NaN
b  2.0   NaN
a  1.0   NaN
```
用 `Series` 字典对象生成 `DataFrame`:
```python
df2 = pd.DataFrame({'A': 1.,
                    'B': pd.Timestamp('20130102'),
                    'C': pd.Series(1, index=list(range(4)), dtype='float32'),
                    'D': np.array([3] * 4, dtype='int32'),
                    'E': pd.Categorical(["test", "train", "test", "train"]),
                    'F': 'foo'})
df2
     A          B    C  D      E    F
0  1.0 2013-01-02  1.0  3   test  foo
1  1.0 2013-01-02  1.0  3  train  foo
2  1.0 2013-01-02  1.0  3   test  foo
3  1.0 2013-01-02  1.0  3  train  foo
```
`DataFrame` 的列有不同数据类型。
```python
df2.dtypes
A           float64
B    datetime64[ns]
C           float32
D             int32
E          category
F            object
dtype: object
```
`index` 和 `columns` 属性分别用于访问行、列标签：

>注意:指定列与数据字典一起传递时，传递的列会覆盖字典的键。
```python
df.index
Index(['a', 'b', 'c', 'd'], dtype='object')
df.columns
Index(['one', 'two'], dtype='object')
```
##### 用多维数组字典、列表字典生成 `DataFrame`
多维数组的长度必须相同。如果传递了索引参数，`index` 的长度必须与数组一致。如果没有传递索引参数，生成的结果是 `range(n)`，`n` 为数组长度。
```python
d = {'one': [1., 2., 3., 4.],'two': [4., 3., 2., 1.]}
pd.DataFrame(d)
   one  two
0  1.0  4.0
1  2.0  3.0
2  3.0  2.0
3  4.0  1.0
pd.DataFrame(d, index=['a', 'b', 'c', 'd']) 
   one  two
a  1.0  4.0
b  2.0  3.0
c  3.0  2.0
d  4.0  1.0
```
用含日期时间索引与标签的 `NumPy` 数组生成 `DataFrame`：
```python
dates = pd.date_range('20130101', periods=6)
dates 
DatetimeIndex(['2013-01-01', '2013-01-02', '2013-01-03', '2013-01-04',
               '2013-01-05', '2013-01-06'],
              dtype='datetime64[ns]', freq='D')

df = pd.DataFrame(np.random.randn(6, 4), index=dates, columns=list('ABCD'))
df 
                   A         B         C         D
2013-01-01  0.469112 -0.282863 -1.509059 -1.135632
2013-01-02  1.212112 -0.173215  0.119209 -1.044236
2013-01-03 -0.861849 -2.104569 -0.494929  1.071804
2013-01-04  0.721555 -0.706771 -1.039575  0.271860
2013-01-05 -0.424972  0.567020  0.276232 -1.087401
2013-01-06 -0.673690  0.113648 -1.478427  0.524988
```
##### 用结构多维数组或记录多维数组生成 `DataFrame`
本例与数组字典的操作方式相同。
```python
data = np.zeros((2, ), dtype=[('A', 'i4'), ('B', 'f4'), ('C', 'a10')])
data[:] = [(1, 2., 'Hello'), (2, 3., "World")]
pd.DataFrame(data)
   A    B         C
0  1  2.0  b'Hello'
1  2  3.0  b'World'
pd.DataFrame(data, index=['first', 'second']) 
        A    B         C
first   1  2.0  b'Hello'
second  2  3.0  b'World'
pd.DataFrame(data, columns=['C', 'A', 'B']) 
          C  A    B
0  b'Hello'  1  2.0
1  b'World'  2  3.0
```
>注意:`DataFrame` 的运作方式与 `NumPy` 二维数组不同。
##### 用列表字典生成 `DataFrame`
```python
data2 = [{'a': 1, 'b': 2}, {'a': 5, 'b': 10, 'c': 20}]
pd.DataFrame(data2) 
   a   b     c
0  1   2   NaN
1  5  10  20.0
pd.DataFrame(data2, index=['first', 'second']) 
        a   b     c
first   1   2   NaN
second  5  10  20.0
pd.DataFrame(data2, columns=['a', 'b'])
Out[55]: 
   a   b
0  1   2
1  5  10
```
##### 用元组字典生成 `DataFrame`
元组字典可以自动创建多层索引 `DataFrame`。
```python
pd.DataFrame({('a', 'b'): {('A', 'B'): 1, ('A', 'C'): 2},
              ('a', 'a'): {('A', 'C'): 3, ('A', 'B'): 4},
              ('a', 'c'): {('A', 'B'): 5, ('A', 'C'): 6},
              ('b', 'a'): {('A', 'C'): 7, ('A', 'B'): 8},
              ('b', 'b'): {('A', 'D'): 9, ('A', 'B'): 10}})
       a              b      
       b    a    c    a     b
A B  1.0  4.0  5.0  8.0  10.0
  C  2.0  3.0  6.0  7.0   NaN
  D  NaN  NaN  NaN  NaN   9.0
```
##### 用 `Series` 创建 `DataFrame`
生成的 `DataFrame` 继承了输入的 `Series` 的索引，如果没有指定列名，默认列名是输入 `Series` 的名称。
`DataFrame` 里的缺失值用 `np.nan` 表示。`DataFrame` 构建器以 `numpy.MaskedArray` 为参数时 ，被屏蔽的条目为缺失数据。

##### 备选构建器
- `DataFrame.from_dict`

`DataFrame.from_dict` 接收字典组成的字典或数组序列字典，并生成 `DataFrame`。除了 `orient` 参数默认为 `columns`，本构建器的操作与 `DataFrame` 构建器类似。把 `orient` 参数设置为 `'index'`， 即可把字典的键作为行标签。
```python
pd.DataFrame.from_dict(dict([('A', [1, 2, 3]), ('B', [4, 5, 6])])) 
   A  B
0  1  4
1  2  5
2  3  6
```
`orient='index'` 时，键是行标签。本例还传递了列名：
```python
pd.DataFrame.from_dict(dict([('A', [1, 2, 3]), ('B', [4, 5, 6])]),
                        orient='index', columns=['one', 'two', 'three']) 
   one  two  three
A    1    2      3
B    4    5      6
```
- `DataFrame.from_records`

`DataFrame.from_records` 构建器支持元组列表或结构数据类型（`dtype`）的多维数组。本构建器与 `DataFrame` 构建器类似，只不过生成的 `DataFrame` 索引是结构数据类型指定的字段。例如：
```python
data
array([(1, 2., b'Hello'), (2, 3., b'World')],
      dtype=[('A', '<i4'), ('B', '<f4'), ('C', 'S10')])
pd.DataFrame.from_records(data, index='C')
          A    B
C               
b'Hello'  1  2.0
b'World'  2  3.0
```
##### 提取、添加、删除列
`DataFrame` 就像带索引的 `Series` 字典，提取、设置、删除列的操作与字典类似：
```python
df
   one  two
a  1.0  1.0
b  2.0  2.0
c  3.0  3.0
d  NaN  4.0
df['one'] 
a    1.0
b    2.0
c    3.0
d    NaN
Name: one, dtype: float64
df['three'] = df['one'] * df['two']
df['flag'] = df['one'] > 2
df 
   one  two  three   flag
a  1.0  1.0    1.0  False
b  2.0  2.0    4.0  False
c  3.0  3.0    9.0   True
d  NaN  4.0    NaN  False
```
删除（`del`、`pop`）列的方式也与字典类似：
```python
del df['two']
three = df.pop('three')
df 
   one   flag
a  1.0  False
b  2.0  False
c  3.0   True
d  NaN  False
```
标量值以广播的方式填充列：
```python
df['foo'] = 'bar'

In [69]: df
Out[69]: 
   one   flag  foo
a  1.0  False  bar
b  2.0  False  bar
c  3.0   True  bar
d  NaN  False  bar
```
插入与 `DataFrame` 索引不同的 `Series` 时，以 `DataFrame` 的索引为准：
```python
df['one_trunc'] = df['one'][:2]
df 
   one   flag  foo  one_trunc
a  1.0  False  bar        1.0
b  2.0  False  bar        2.0
c  3.0   True  bar        NaN
d  NaN  False  bar        NaN
```
可以插入原生多维数组，但长度必须与 `DataFrame` 索引长度一致。
默认在 `DataFrame` 尾部插入列。`insert` 函数可以指定插入列的位置：
```python
df.insert(1, 'bar', df['one'])
df 
   one  bar   flag  foo  one_trunc
a  1.0  1.0  False  bar        1.0
b  2.0  2.0  False  bar        2.0
c  3.0  3.0   True  bar        NaN
d  NaN  NaN  False  bar        NaN
```
##### 用方法链分配新列
`DataFrame` 提供了 `assign()` 方法，可以利用现有的列创建新列。
```python
iris = pd.read_csv('data/iris.data')
iris.head()
   SepalLength  SepalWidth  PetalLength  PetalWidth         Name
0          5.1         3.5          1.4         0.2  Iris-setosa
1          4.9         3.0          1.4         0.2  Iris-setosa
2          4.7         3.2          1.3         0.2  Iris-setosa
3          4.6         3.1          1.5         0.2  Iris-setosa
4          5.0         3.6          1.4         0.2  Iris-setosa
(iris.assign(sepal_ratio=iris['SepalWidth'] / iris['SepalLength']).head())
   SepalLength  SepalWidth  PetalLength  PetalWidth         Name  sepal_ratio
0          5.1         3.5          1.4         0.2  Iris-setosa     0.686275
1          4.9         3.0          1.4         0.2  Iris-setosa     0.612245
2          4.7         3.2          1.3         0.2  Iris-setosa     0.680851
3          4.6         3.1          1.5         0.2  Iris-setosa     0.673913
4          5.0         3.6          1.4         0.2  Iris-setosa     0.720000
```
上例中，插入了一个预计算的值。还可以传递带参数的函数，在 `assign` 的 `DataFrame` 上求值。
```python
iris.assign(sepal_ratio=lambda x: (x['SepalWidth'] / x['SepalLength'])).head() 
   SepalLength  SepalWidth  PetalLength  PetalWidth         Name  sepal_ratio
0          5.1         3.5          1.4         0.2  Iris-setosa     0.686275
1          4.9         3.0          1.4         0.2  Iris-setosa     0.612245
2          4.7         3.2          1.3         0.2  Iris-setosa     0.680851
3          4.6         3.1          1.5         0.2  Iris-setosa     0.673913
4          5.0         3.6          1.4         0.2  Iris-setosa     0.720000
```
`assign` 返回的都是数据副本，原 `DataFrame` 不变。
未引用 `DataFrame` 时，传递可调用的，不是实际要插入的值。这种方式常见于在操作链中调用 `assign` 的操作。例如，将 `DataFrame` 限制为花萼长度大于 5 的观察值，计算比例，再制图：
```python
(iris.query('SepalLength > 5')
      .assign(SepalRatio=lambda x: x.SepalWidth / x.SepalLength,
              PetalRatio=lambda x: x.PetalWidth / x.PetalLength)
                    .plot(kind='scatter', x='SepalRatio', y='PetalRatio'))
<matplotlib.axes._subplots.AxesSubplot at 0x7f66075a7978>
```
上例用 `assign` 把函数传递给 `DataFrame`， 并执行函数运算。这是要注意的是，该 `DataFrame` 是筛选了花萼长度大于 5 以后的数据。首先执行的是筛选操作，再计算比例。这个例子就是对没有事先筛选 `DataFrame` 进行的引用。
`assign` 函数签名就是 `**kwargs`。键是新字段的列名，值为是插入值（例如，`Series` 或 `NumPy` 数组），或把 `DataFrame` 当做调用参数的函数。返回结果是插入新值的 `DataFrame` 副本。
`Python` 可以保存 `**kwargs` 顺序。这种操作允许依赖赋值，`**kwargs` 后的表达式，可以引用同一个 `assign()` 函数里之前创建的列 。
```python
dfa = pd.DataFrame({"A": [1, 2, 3],
                     "B": [4, 5, 6]}) 
dfa.assign(C=lambda x: x['A'] + x['B'],
       D=lambda x: x['A'] + x['C']) 
   A  B  C   D
0  1  4  5   6
1  2  5  7   9
2  3  6  9  12
```
第二个表达式里，`x['C']` 引用刚创建的列，与 `dfa['A']` + `dfa['B']` 等效。

##### 索引 / 选择
索引基础用法如下：
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200401201357.png)
选择行返回 `Series`，索引是 `DataFrame` 的列：
```python
df 
   one  bar   flag  foo  one_trunc
a  1.0  1.0  False  bar        1.0
b  2.0  2.0  False  bar        2.0
c  3.0  3.0   True  bar        NaN
d  NaN  NaN  False  bar        NaN
df.loc['b'] 
one              2
bar              2
flag         False
foo            bar
one_trunc        2
Name: b, dtype: object
df.iloc[2] 
one             3
bar             3
flag         True
foo           bar
one_trunc     NaN
Name: c, dtype: object
```
##### 数据对齐和运算
`DataFrame` 对象可以自动对齐**列与索引（行标签）**的数据。与上文一样，生成的结果是列和行标签的并集。
```python
df = pd.DataFrame(np.random.randn(10, 4), columns=['A', 'B', 'C', 'D'])
df2 = pd.DataFrame(np.random.randn(7, 3), columns=['A', 'B', 'C'])
df + df2
          A         B         C   D
0  0.045691 -0.014138  1.380871 NaN
1 -0.955398 -1.501007  0.037181 NaN
2 -0.662690  1.534833 -0.859691 NaN
3 -2.452949  1.237274 -0.133712 NaN
4  1.414490  1.951676 -2.320422 NaN
5 -0.494922 -1.649727 -1.084601 NaN
6 -1.047551 -0.748572 -0.805479 NaN
7       NaN       NaN       NaN NaN
8       NaN       NaN       NaN NaN
9       NaN       NaN       NaN NaN
```
`DataFrame` 和 `Series` 之间执行操作时，默认操作是在 `DataFrame` 的列上对齐 `Series` 的索引，按行执行广播操作。例如：
```python
df - df.iloc[0] 
          A         B         C         D
0  0.000000  0.000000  0.000000  0.000000
1 -1.359261 -0.248717 -0.453372 -1.754659
2  0.253128  0.829678  0.010026 -1.991234
3 -1.311128  0.054325 -1.724913 -1.620544
4  0.573025  1.500742 -0.676070  1.367331
5 -1.741248  0.781993 -1.241620 -2.053136
6 -1.240774 -0.869551 -0.153282  0.000430
7 -0.743894  0.411013 -0.929563 -0.282386
8 -1.194921  1.320690  0.238224 -1.482644
9  2.293786  1.856228  0.773289 -1.446531
```
时间序列是特例，`DataFrame` 索引包含日期时，按列广播：
标量操作与其它数据结构一样：
```python
df * 5 + 2 
                   A         B         C
2000-01-01 -4.134126  5.849018 -4.406237
2000-01-02 -1.638535  1.393469  1.510587
2000-01-03  5.478873  3.708672  6.798628
2000-01-04 -3.551681 -1.099880  2.748742
2000-01-05 -1.661697  5.438692  2.882222
2000-01-06  4.016548  1.225246  3.508122
2000-01-07 -8.899303 -4.849247 -2.771039
2000-01-08  9.313480 -6.715805 -2.132955
1 / df 
                   A         B          C
2000-01-01 -0.815112  1.299033  -0.780489
2000-01-02 -1.374179 -8.243600 -10.216313
2000-01-03  1.437247  2.926250   1.041965
2000-01-04 -0.900628 -1.612966   6.677871
2000-01-05 -1.365487  1.454041   5.667510
2000-01-06  2.479485 -6.453662   3.315381
2000-01-07 -0.458745 -0.730007  -1.047990
2000-01-08  0.683669 -0.573671  -1.209788
df ** 4
                    A         B         C
2000-01-01   2.265327  0.351172  2.694833
2000-01-02   0.280431  0.000217  0.000092
2000-01-03   0.234355  0.013638  0.848376
2000-01-04   1.519910  0.147740  0.000503
2000-01-05   0.287640  0.223714  0.000969
2000-01-06   0.026458  0.000576  0.008277
2000-01-07  22.579530  3.521204  0.829033
2000-01-08   4.577374  9.233151  0.466834
```
支持布尔运算符：
```python
df1 = pd.DataFrame({'a': [1, 0, 1], 'b': [0, 1, 1]}, dtype=bool)
df2 = pd.DataFrame({'a': [0, 1, 1], 'b': [1, 1, 0]}, dtype=bool)
df1 & df2 
       a      b
0  False  False
1  False   True
2   True  False
df1 | df2
      a     b
0  True  True
1  True  True
2  True  True
df1 ^ df2
       a      b
0   True   True
1   True  False
2  False   True
-df1 
       a      b
0  False   True
1   True  False
2  False  False
```
##### 转置
类似于多维数组，`T` 属性（即 `transpose` 函数）可以转置 `DataFrame`：
```python
index = pd.date_range('1/1/2000', periods=8)
df = pd.DataFrame(np.random.randn(8, 3), index=index, columns=list('ABC'))
df
                   A         B         C
2000-01-01 -1.226825  0.769804 -1.281247
2000-01-02 -0.727707 -0.121306 -0.097883
2000-01-03  0.695775  0.341734  0.959726
2000-01-04 -1.110336 -0.619976  0.149748
2000-01-05 -0.732339  0.687738  0.176444
2000-01-06  0.403310 -0.154951  0.301624
2000-01-07 -2.179861 -1.369849 -0.954208
2000-01-08  1.462696 -1.743161 -0.826591
# only show the first 5 rows
df[:5].T 
   2000-01-01  2000-01-02  2000-01-03  2000-01-04  2000-01-05
A   -1.226825   -0.727707    0.695775   -1.110336   -0.732339
B    0.769804   -0.121306    0.341734   -0.619976    0.687738
C   -1.281247   -0.097883    0.959726    0.149748    0.176444
```
##### `DataFrame` 应用 `NumPy`函数
`Series` 与 `DataFrame` 可使用 `log`、`exp`、`sqrt` 等多种元素级 `NumPy` 通用函数（`ufunc`） ，假设 `DataFrame` 的数据都是数字：
```python
np.exp(df) 
                   A         B         C
2000-01-01  0.293222  2.159342  0.277691
2000-01-02  0.483015  0.885763  0.906755
2000-01-03  2.005262  1.407386  2.610980
2000-01-04  0.329448  0.537957  1.161542
2000-01-05  0.480783  1.989212  1.192968
2000-01-06  1.496770  0.856457  1.352053
2000-01-07  0.113057  0.254145  0.385117
2000-01-08  4.317584  0.174966  0.437538
np.asarray(df) 
array([[-1.2268,  0.7698, -1.2812],
       [-0.7277, -0.1213, -0.0979],
       [ 0.6958,  0.3417,  0.9597],
       [-1.1103, -0.62  ,  0.1497],
       [-0.7323,  0.6877,  0.1764],
       [ 0.4033, -0.155 ,  0.3016],
       [-2.1799, -1.3698, -0.9542],
       [ 1.4627, -1.7432, -0.8266]])
```
`DataFrame` 不是多维数组的替代品，它的索引语义和数据模型与多维数组都不同。
`Series` 应用 `__array_ufunc__`，支持 `NumPy` 通用函数。
通用函数应用于 `Series` 的底层数组。
```python
ser = pd.Series([1, 2, 3, 4])
np.exp(ser) 
0     2.718282
1     7.389056
2    20.085537
3    54.598150
dtype: float64
```
`Pandas` 可以自动对齐 `ufunc` 里的多个带标签输入数据。例如，两个标签排序不同的 `Series` 运算前，会先对齐标签。
```python
ser1 = pd.Series([1, 2, 3], index=['a', 'b', 'c'])
ser2 = pd.Series([1, 3, 5], index=['b', 'a', 'c'])
ser1 
a    1
b    2
c    3
dtype: int64
ser2 
b    1
a    3
c    5
dtype: int64
np.remainder(ser1, ser2) 
a    1
b    0
c    3
dtype: int64
```
一般来说，`Pandas` 提取两个索引的并集，不重叠的值用缺失值填充。
```python
ser3 = pd.Series([2, 4, 6], index=['b', 'c', 'd'])
ser3 
b    2
c    4
d    6
dtype: int64
np.remainder(ser1, ser3) 
a    NaN
b    0.0
c    3.0
d    NaN
dtype: float64
```
对 `Series` 和 `Index` 应用二进制 `ufunc` 时，优先执行 `Series`，并返回的结果也是 `Series` 。
```python
ser = pd.Series([1, 2, 3])
idx = pd.Index([4, 5, 6])
np.maximum(ser, idx) 
0    4
1    5
2    6
dtype: int64
```
`NumPy` 通用函数可以安全地应用于非多维数组支持的 `Series`，例如，`SparseArray`。如有可能，应用 `ufunc` 而不把基础数据转换为多维数组。

##### `DataFrame` 列属性访问
`DataFrame` 列标签是有效的 `Python` 变量名时，可以像属性一样访问该列：
```python
df = pd.DataFrame({'foo1': np.random.randn(5),'foo2': np.random.randn(5)})
df 
       foo1      foo2
0  1.171216 -0.858447
1  0.520260  0.306996
2 -1.197071 -0.028665
3 -1.066969  0.384316
4 -0.303421  1.574159
df.foo1
0    1.171216
1    0.520260
2   -1.197071
3   -1.066969
4   -0.303421
Name: foo1, dtype: float64
```
### 数据类型
大多数情况下，`Pandas` 使用 `NumPy` 数组、`Series` 或 `DataFrame` 里某列的数据类型。`NumPy` 支持 `float`、`int`、`bool`、`timedelta[ns]`、`datetime64[ns]`，注意，`NumPy` 不支持带时区信息的 `datetime`。
`Pandas` 与第三方支持库扩充了 `NumPy` 类型系统，本节只介绍 `Pandas` 的内部扩展。
下表列出了 `Pandas` 扩展类型:
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200403160958.png)
`Pandas` 用 `object` 存储字符串。
虽然， `object` 数据类型能够存储任何对象，但应尽量避免这种操作。
`DataFrame` 的 `dtypes` 属性用起来很方便，以 `Series` 形式返回每列的数据类型。
```python
dft = pd.DataFrame({'A': np.random.rand(3),
                    'B': 1,
                    'C': 'foo',
                    'D': pd.Timestamp('20010102'),
                    'E': pd.Series([1.0] * 3).astype('float32'),
                    'F': False,
                    'G': pd.Series([1] * 3, dtype='int8')})
dft
          A  B    C          D    E      F  G
0  0.035962  1  foo 2001-01-02  1.0  False  1
1  0.701379  1  foo 2001-01-02  1.0  False  1
2  0.281885  1  foo 2001-01-02  1.0  False  1
dft.dtypes
A           float64
B             int64
C            object
D    datetime64[ns]
E           float32
F              bool
G              int8
dtype: object
```
要查看 `Series` 的数据类型，用 `dtype` 属性。
```python
dft['A'].dtype
dtype('float64')
```
`Pandas` 对象单列中含多种类型的数据时，该列的数据类型为可适配于各类数据的数据类型，通常为 `object`。

### 整数被强制转换为浮点数
```python
pd.Series([1, 2, 3, 4, 5, 6.])
0    1.0
1    2.0
2    3.0
3    4.0
4    5.0
5    6.0
dtype: float64
'''
字符串数据决定了该 Series 的数据类型为object
'''
In [333]: pd.Series([1, 2, 3, 6., 'foo'])
Out[333]: 
0      1
1      2
2      3
3      6
4    foo
dtype: object
```
`DataFrame.dtypes.value_counts()` 用于统计 `DataFrame` 里不同数据类型的列数。
```python
dft.dtypes.value_counts()
float32           1
object            1
bool              1
int8              1
float64           1
datetime64[ns]    1
int64             1
dtype: int64
```
多种数值型数据类型可以在 `DataFrame` 里共存。如果只传递一种数据类型，不论是通过 `dtype` 关键字直接传递，还是通过 `ndarray` 或 `Series` 传递，都会保存至 `DataFrame` 操作。此外，不同数值型数据类型不会合并。示例如下：
```python
df1 = pd.DataFrame(np.random.randn(8, 1), columns=['A'], dtype='float32')
df1
          A
0  0.224364
1  1.890546
2  0.182879
3  0.787847
4 -0.188449
5  0.667715
6 -0.011736
7 -0.399073
df1.dtypes
A    float32
dtype: object
df2 = pd.DataFrame({'A': pd.Series(np.random.randn(8), dtype='float16'),
                    'B': pd.Series(np.random.randn(8)),
                    'C': pd.Series(np.array(np.random.randn(8),
                     dtype='uint8'))})
df2
          A         B    C
0  0.823242  0.256090    0
1  1.607422  1.426469    0
2 -0.333740 -0.416203  255
3 -0.063477  1.139976    0
4 -1.014648 -1.193477    0
5  0.678711  0.096706    0
6 -0.040863 -1.956850    1
7 -0.357422 -0.714337    0
df2.dtypes
A    float16
B    float64
C      uint8
dtype: object
```
### 默认值
整数的默认类型为 `int64`，浮点数的默认类型为 `float64`，这里的默认值与系统平台无关，不管是 `32` 位系统，还是 `64` 位系统都是一样的。下列代码返回的结果都是 `int64`：
```python
pd.DataFrame([1, 2], columns=['a']).dtypes
a    int64
dtype: object
pd.DataFrame({'a': [1, 2]}).dtypes
a    int64
dtype: object
pd.DataFrame({'a': 1}, index=list(range(2))).dtypes
a    int64
dtype: object
```
注意，`NumPy` 创建数组时，会根据系统选择类型。下列代码在 `32` 位系统上将返回 `int32`。
```python
frame = pd.DataFrame(np.array([1, 2]))
```
### 向上转型
与其它类型合并时，用的是向上转型，指的是从现有类型转换为另一种类型，如`int` 变为 `float`。
```python
df3 = df1.reindex_like(df2).fillna(value=0.0) + df2
df3
          A         B      C
0  1.047606  0.256090    0.0
1  3.497968  1.426469    0.0
2 -0.150862 -0.416203  255.0
3  0.724370  1.139976    0.0
4 -1.203098 -1.193477    0.0
5  1.346426  0.096706    0.0
6 -0.052599 -1.956850    1.0
7 -0.756495 -0.714337    0.0
df3.dtypes
A    float32
B    float64
C    float64
dtype: object
```
`DataFrame.to_numpy()` 返回多个数据类型里用得最多的数据类型，这里指的是，输出结果的数据类型，适用于所有同构 `NumPy` 数组的数据类型。此处强制执行向上转型。
```python
df3.to_numpy().dtype
dtype('float64')
```
### `astype·
`astype()` 方法显式地把一种数据类型转换为另一种，默认操作为复制数据，就算数据类型没有改变也会复制数据，`copy=False` 改变默认操作模式。此外，`astype` 无效时，会触发异常。

向上转型一般都遵循 `NumPy` 规则。操作中含有两种不同类型的数据时，返回更为通用的那种数据类型。
```python
df3
          A         B      C
0  1.047606  0.256090    0.0
1  3.497968  1.426469    0.0
2 -0.150862 -0.416203  255.0
3  0.724370  1.139976    0.0
4 -1.203098 -1.193477    0.0
5  1.346426  0.096706    0.0
6 -0.052599 -1.956850    1.0
7 -0.756495 -0.714337    0.0
df3.dtypes
A    float32
B    float64
C    float64
dtype: object
''' 
转换数据类型
'''
df3.astype('float32').dtypes
A    float32
B    float32
C    float32
dtype: object
```
用 `astype()` 把一列或多列转换为指定类型 。
```python
dft = pd.DataFrame({'a': [1, 2, 3], 'b': [4, 5, 6], 'c': [7, 8, 9]})
dft[['a', 'b']] = dft[['a', 'b']].astype(np.uint8)
dft
   a  b  c
0  1  4  7
1  2  5  8
2  3  6  9
dft.dtypes
a    uint8
b    uint8
c    int64
dtype: object
```
`astype()` 通过字典指定哪些列转换为哪些类型。
```python
dft1 = pd.DataFrame({'a': [1, 0, 1], 'b': [4, 5, 6], 'c': [7, 8, 9]})
dft1 = dft1.astype({'a': np.bool, 'c': np.float64})
dft1
       a  b    c
0   True  4  7.0
1  False  5  8.0
2   True  6  9.0
dft1.dtypes
a       bool
b      int64
c    float64
dtype: object
```
注意:用 `astype()` 与 `loc()` 为部分列转换指定类型时，会发生向上转型。

`loc()` 尝试分配当前的数据类型，而 `[]` 则会从右方获取数据类型并进行覆盖。因此，下列代码会产出意料之外的结果：
```python
dft = pd.DataFrame({'a': [1, 2, 3], 'b': [4, 5, 6], 'c': [7, 8, 9]})
dft.loc[:, ['a', 'b']].astype(np.uint8).dtypes
a    uint8
b    uint8
dtype: object
dft.loc[:, ['a', 'b']] = dft.loc[:, ['a', 'b']].astype(np.uint8)
dft.dtypes
a    int64
b    int64
c    int64
dtype: object
```
### 对象转换
`Pandas` 提供了多种函数可以把 `object` 从一种类型强制转为另一种类型。这是因为，数据有时存储的是正确类型，但在保存时却存成了 `object` 类型，此时，用 `DataFrame.infer_objects()` 与 `Series.infer_objects()` 方法即可把数据软转换为正确的类型。
```python
import datetime
df = pd.DataFrame([[1, 2],
                  ['a', 'b'],
                  [datetime.datetime(2016, 3, 2),
                  datetime.datetime(2016, 3, 2)]])
df = df.T
df
   0  1          2
0  1  a 2016-03-02
1  2  b 2016-03-02
df.dtypes
0            object
1            object
2    datetime64[ns]
dtype: object
```
因为数据被转置，所以把原始列的数据类型改成了 `object`，但使用 `infer_objects` 后就变正确了。
```python
df.infer_objects().dtypes
0             int64
1            object
2    datetime64[ns]
dtype: object
```
下列函数可以应用于一维数组与标量，执行硬转换，把对象转换为指定类型。
`to_numeric()`，转换为数值型:
```python
m = ['1.1', 2, 3]
pd.to_numeric(m)
array([1.1, 2. , 3. ])
```
`to_datetime()`，转换为 `datetime` 对象:
```python
import datetime
m = ['2016-07-09', datetime.datetime(2016, 3, 2)]
pd.to_datetime(m)
DatetimeIndex(['2016-07-09', '2016-03-02'], dtype='datetime64[ns]', freq=None)
```
`to_timedelta()`，转换为 `timedelta` 对象:
```python
m = ['5us', pd.Timedelta('1day')]
pd.to_timedelta(m)
TimedeltaIndex(['0 days 00:00:00.000005', '1 days 00:00:00'], dtype='timedelta64[ns]', freq=None)
```
如需强制转换，则要加入 `error` 参数，指定 `Pandas` 怎样处理不能转换为成预期类型或对象的数据。`errors` 参数的默认值为 `False`，指的是在转换过程中，遇到任何问题都触发错误。设置为 `errors='coerce'` 时，`pandas` 会忽略错误，强制把问题数据转换为 `pd.NaT`（`datetime` 与 `timedelta`），或 `np.nan`（数值型）。读取数据时，如果大部分要转换的数据是数值型或 `datetime`，这种操作非常有用，但偶尔也会有非制式数据混合在一起，可能会导致展示数据缺失：
```python
import datetime
m = ['apple', datetime.datetime(2016, 3, 2)]
pd.to_datetime(m, errors='coerce')
DatetimeIndex(['NaT', '2016-03-02'], dtype='datetime64[ns]', freq=None)
m = ['apple', 2, 3]
pd.to_numeric(m, errors='coerce')
array([nan,  2.,  3.])
m = ['apple', pd.Timedelta('1day')]
pd.to_timedelta(m, errors='coerce')
TimedeltaIndex([NaT, '1 days'], dtype='timedelta64[ns]', freq=None)
```
`error` 参数还有第三个选项，`error='ignore'`。转换数据时会忽略错误，直接输出问题数据：
```python
import datetime
m = ['apple', datetime.datetime(2016, 3, 2)]
pd.to_datetime(m, errors='ignore')
Index(['apple', 2016-03-02 00:00:00], dtype='object')
m = ['apple', 2, 3]
pd.to_numeric(m, errors='ignore')
array(['apple', 2, 3], dtype=object)
m = ['apple', pd.Timedelta('1day')]
pd.to_timedelta(m, errors='ignore')
array(['apple', Timedelta('1 days 00:00:00')], dtype=object)
```
执行转换操作时，`to_numeric()` 还有一个参数，`downcast`，即向下转型，可以把数值型转换为减少内存占用的数据类型：
```python
m = ['1', 2, 3]
pd.to_numeric(m, downcast='integer')   # smallest signed int dtype
array([1, 2, 3], dtype=int8)
pd.to_numeric(m, downcast='signed')    # same as 'integer'
array([1, 2, 3], dtype=int8)
pd.to_numeric(m, downcast='unsigned')  # smallest unsigned int dtype
array([1, 2, 3], dtype=uint8)
pd.to_numeric(m, downcast='float')     # smallest float dtype
array([1., 2., 3.], dtype=float32)
```
上述方法仅能应用于一维数组、列表或标量；不能直接用于 `DataFrame` 等多维对象。不过，用 `apply()`，可以快速为每列应用函数：
```python
import datetime
df = pd.DataFrame([['2016-07-09', datetime.datetime(2016, 3, 2)]] * 2, dtype='O')
df
            0                    1
0  2016-07-09  2016-03-02 00:00:00
1  2016-07-09  2016-03-02 00:00:00
df.apply(pd.to_datetime)
           0          1
0 2016-07-09 2016-03-02
1 2016-07-09 2016-03-02
df = pd.DataFrame([['1.1', 2, 3]] * 2, dtype='O')
df
     0  1  2
0  1.1  2  3
1  1.1  2  3
df.apply(pd.to_numeric)
     0  1  2
0  1.1  2  3
1  1.1  2  3
df = pd.DataFrame([['5us', pd.Timedelta('1day')]] * 2, dtype='O')
df
     0                1
0  5us  1 days 00:00:00
1  5us  1 days 00:00:00
df.apply(pd.to_timedelta)
                0      1
0 00:00:00.000005 1 days
1 00:00:00.000005 1 days
```
### 各种坑
对 `integer` 数据执行选择操作时，可以很轻而易举地把数据转换为 `floating` 。`Pandas` 会保存输入数据的数据类型，以防未引入 `nans` 的情况。
```python
dfi = df3.astype('int32')
dfi['E'] = 1
dfi 
   A  B    C  E
0  1  0    0  1
1  3  1    0  1
2  0  0  255  1
3  0  1    0  1
4 -1 -1    0  1
5  1  0    0  1
6  0 -1    1  1
7  0  0    0  1
dfi.dtypes
A    int32
B    int32
C    int32
E    int64
dtype: object
casted = dfi[dfi > 0]
casted
     A    B      C  E
0  1.0  NaN    NaN  1
1  3.0  1.0    NaN  1
2  NaN  NaN  255.0  1
3  NaN  1.0    NaN  1
4  NaN  NaN    NaN  1
5  1.0  NaN    NaN  1
6  NaN  NaN    1.0  1
7  NaN  NaN    NaN  1
casted.dtypes
A    float64
B    float64
C    float64
E      int64
dtype: object
```
浮点数类型未改变。
```python
dfa = df3.copy()
dfa['A'] = dfa['A'].astype('float32')
dfa.dtypes 
A    float32
B    float64
C    float64
dtype: object
casted = dfa[df2 > 0]
casted 
          A         B      C
0  1.047606  0.256090    NaN
1  3.497968  1.426469    NaN
2       NaN       NaN  255.0
3       NaN  1.139976    NaN
4       NaN       NaN    NaN
5  1.346426  0.096706    NaN
6       NaN       NaN    1.0
7       NaN       NaN    NaN
casted.dtypes 
A    float32
B    float64
C    float64
dtype: object
```
## 查看数据
`head()` 与 `tail()` 用于快速预览 `Series` 与 `DataFrame`，默认显示 `5` 条数据，也可以指定显示数据的数量。
```python
long_series = pd.Series(np.random.randn(1000))
long_series.head() 
0   -1.157892
1   -1.344312
2    0.844885
3    1.075770
4   -0.109050
dtype: float64
long_series.tail(3) 
997   -0.289388
998   -1.020544
999    0.589993
dtype: float64
```
```python
df =pd.DataFrame(np.random.randn(6, 4), index=dates, columns=list('ABCD'))
df 
                   A         B         C         D
2013-01-01  0.469112 -0.282863 -1.509059 -1.135632
2013-01-02  1.212112 -0.173215  0.119209 -1.044236
2013-01-03 -0.861849 -2.104569 -0.494929  1.071804
2013-01-04  0.721555 -0.706771 -1.039575  0.271860
2013-01-05 -0.424972  0.567020  0.276232 -1.087401
2013-01-06 -0.673690  0.113648 -1.478427  0.524988
df.head()
                   A         B         C         D
2013-01-01  0.469112 -0.282863 -1.509059 -1.135632
2013-01-02  1.212112 -0.173215  0.119209 -1.044236
2013-01-03 -0.861849 -2.104569 -0.494929  1.071804
2013-01-04  0.721555 -0.706771 -1.039575  0.271860
2013-01-05 -0.424972  0.567020  0.276232 -1.087401
df.tail(3)
                   A         B         C         D
2013-01-04  0.721555 -0.706771 -1.039575  0.271860
2013-01-05 -0.424972  0.567020  0.276232 -1.087401
2013-01-06 -0.673690  0.113648 -1.478427  0.524988
```
显示索引与列名：
```python
df.index
DatetimeIndex(['2013-01-01', '2013-01-02', '2013-01-03', '2013-01-04',
               '2013-01-05', '2013-01-06'],
              dtype='datetime64[ns]', freq='D')

df.columns
Index(['A', 'B', 'C', 'D'], dtype='object')
```
`DataFrame.to_numpy()` 输出底层数据的 `NumPy` 对象。注意，`DataFrame` 的列由多种数据类型组成时，该操作耗费系统资源较大，这也是 `Pandas` 和 `NumPy` 的本质区别：`NumPy` 数组只有一种数据类型，`DataFrame` 每列的数据类型各不相同。调用 `DataFrame.to_numpy()` 时，`Pandas` 查找支持 `DataFrame` 里所有数据类型的 `NumPy` 数据类型。还有一种数据类型是 `object`，可以把 `DataFrame` 列里的值强制转换为 `Python` 对象。

下面的 `df` 这个 `DataFrame` 里的值都是浮点数，`DataFrame.to_numpy()` 的操作会很快，而且不复制数据。
```python
df
                   A         B         C         D
2013-01-01  0.469112 -0.282863 -1.509059 -1.135632
2013-01-02  1.212112 -0.173215  0.119209 -1.044236
2013-01-03 -0.861849 -2.104569 -0.494929  1.071804
2013-01-04  0.721555 -0.706771 -1.039575  0.271860
2013-01-05 -0.424972  0.567020  0.276232 -1.087401
2013-01-06 -0.673690  0.113648 -1.478427  0.524988
df.to_numpy() 
array([[ 0.4691, -0.2829, -1.5091, -1.1356],
       [ 1.2121, -0.1732,  0.1192, -1.0442],
       [-0.8618, -2.1046, -0.4949,  1.0718],
       [ 0.7216, -0.7068, -1.0396,  0.2719],
       [-0.425 ,  0.567 ,  0.2762, -1.0874],
       [-0.6737,  0.1136, -1.4784,  0.525 ]])
```
`df2` 这个 `DataFrame` 包含了多种类型，`DataFrame.to_numpy()` 操作就会耗费较多资源。
```python
df2
     A          B    C  D      E    F
0  1.0 2013-01-02  1.0  3   test  foo
1  1.0 2013-01-02  1.0  3  train  foo
2  1.0 2013-01-02  1.0  3   test  foo
3  1.0 2013-01-02  1.0  3  train  foo
df2.to_numpy() 
array([[1.0, Timestamp('2013-01-02 00:00:00'), 1.0, 3, 'test', 'foo'],
       [1.0, Timestamp('2013-01-02 00:00:00'), 1.0, 3, 'train', 'foo'],
       [1.0, Timestamp('2013-01-02 00:00:00'), 1.0, 3, 'test', 'foo'],
       [1.0, Timestamp('2013-01-02 00:00:00'), 1.0, 3, 'train', 'foo']], dtype=object)
```
>提醒:`DataFrame.to_numpy()` 的输出不包含行索引和列标签。
`DataFrame` 为同构型数据时，`Pandas` 直接修改原始 `ndarray`，所做修改会直接反应在数据结构里。对于异质型数据，即 `DataFrame` 列的数据类型不一样时，就不是这种操作模式了。与轴标签不同，不能为值的属性赋值。
>注意:处理异质型数据时，输出结果 `ndarray` 的数据类型适用于涉及的各类数据。若 `DataFrame` 里包含字符串，输出结果的数据类型就是 `object`。要是只有浮点数或整数，则输出结果的数据类型是浮点数。
`describe()` 可以快速查看数据的统计摘要：
```python
df.describe() 
              A         B         C         D
count  6.000000  6.000000  6.000000  6.000000
mean   0.073711 -0.431125 -0.687758 -0.233103
std    0.843157  0.922818  0.779887  0.973118
min   -0.861849 -2.104569 -1.509059 -1.135632
25%   -0.611510 -0.600794 -1.368714 -1.076610
50%    0.022070 -0.228039 -0.767252 -0.386188
75%    0.658444  0.041933 -0.034326  0.461706
max    1.212112  0.567020  0.276232  1.071804
```
转置数据：
```python
df.T 
   2013-01-01  2013-01-02  2013-01-03  2013-01-04  2013-01-05  2013-01-06
A    0.469112    1.212112   -0.861849    0.721555   -0.424972   -0.673690
B   -0.282863   -0.173215   -2.104569   -0.706771    0.567020    0.113648
C   -1.509059    0.119209   -0.494929   -1.039575    0.276232   -1.478427
D   -1.135632   -1.044236    1.071804    0.271860   -1.087401    0.524988
```
按轴排序：
```python
df.sort_index(axis=1, ascending=False) 
                   D         C         B         A
2013-01-01 -1.135632 -1.509059 -0.282863  0.469112
2013-01-02 -1.044236  0.119209 -0.173215  1.212112
2013-01-03  1.071804 -0.494929 -2.104569 -0.861849
2013-01-04  0.271860 -1.039575 -0.706771  0.721555
2013-01-05 -1.087401  0.276232  0.567020 -0.424972
2013-01-06  0.524988 -1.478427  0.113648 -0.673690
```
按值排序：
```python
df.sort_values(by='B') 
                   A         B         C         D
2013-01-03 -0.861849 -2.104569 -0.494929  1.071804
2013-01-04  0.721555 -0.706771 -1.039575  0.271860
2013-01-01  0.469112 -0.282863 -1.509059 -1.135632
2013-01-02  1.212112 -0.173215  0.119209 -1.044236
2013-01-06 -0.673690  0.113648 -1.478427  0.524988
2013-01-05 -0.424972  0.567020  0.276232 -1.087401
```
## 二进制操作
### 匹配/广播机制
`DataFrame` 支持 `add()`、`sub()`、`mul()`、`div()` 及 `radd()`、`rsub()` 等方法执行二进制操作。广播机制重点关注输入的 `Series`。通过 `axis` 关键字，匹配 `index` 或 `columns`即可调用这些函数。
```python
df = pd.DataFrame({'one': pd.Series(np.random.randn(3), index=['a', 'b', 'c']),
                   'two': pd.Series(np.random.randn(4), index=['a', 'b', 'c', 'd']),
                   'three': pd.Series(np.random.randn(3), index=['b', 'c', 'd'])})
df
        one       two     three
a  1.394981  1.772517       NaN
b  0.343054  1.912123 -0.050390
c  0.695246  1.478369  1.227435
d       NaN  0.279344 -0.613172
row = df.iloc[1]
column = df['two']
df.sub(row, axis='columns')
        one       two     three
a  1.051928 -0.139606       NaN
b  0.000000  0.000000  0.000000
c  0.352192 -0.433754  1.277825
d       NaN -1.632779 -0.562782
df.sub(row, axis=1)
        one       two     three
a  1.051928 -0.139606       NaN
b  0.000000  0.000000  0.000000
c  0.352192 -0.433754  1.277825
d       NaN -1.632779 -0.562782
df.sub(column, axis='index')
        one  two     three
a -0.377535  0.0       NaN
b -1.569069  0.0 -1.962513
c -0.783123  0.0 -0.250933
d       NaN  0.0 -0.892516
df.sub(column, axis=0)
        one  two     three
a -0.377535  0.0       NaN
b -1.569069  0.0 -1.962513
c -0.783123  0.0 -0.250933
d       NaN  0.0 -0.892516
```
还可以用 `Series` 对齐多层索引 `DataFrame` 的某一层级。
```
dfmi = df.copy()
dfmi.index = pd.MultiIndex.from_tuples([(1, 'a'), (1, 'b'),
                                        (1, 'c'), (2, 'a')],
                                        names=['first', 'second'])
dfmi.sub(column, axis=0, level='second')
                   one       two     three
first second                              
1     a      -0.377535  0.000000       NaN
      b      -1.569069  0.000000 -1.962513
      c      -0.783123  0.000000 -0.250933
2     a            NaN -1.493173 -2.385688
```
`Series` 与 `Index` 还支持 `divmod()` 内置函数，该函数同时执行向下取整除与模运算，返回两个与左侧类型相同的元组。示例如下：
```python
s = pd.Series(np.arange(10))
s
0    0
1    1
2    2
3    3
4    4
5    5
6    6
7    7
8    8
9    9
dtype: int64
```
div, rem = divmod(s, 3)
div
0    0
1    0
2    0
3    1
4    1
5    1
6    2
7    2
8    2
9    3
dtype: int64
```
rem
0    0
1    1
2    2
3    0
4    1
5    2
6    0
7    1
8    2
9    0
dtype: int64
idx = pd.Index(np.arange(10))
idx
Int64Index([0, 1, 2, 3, 4, 5, 6, 7, 8, 9], dtype='int64')
div, rem = divmod(idx, 3)
div
Int64Index([0, 0, 0, 1, 1, 1, 2, 2, 2, 3], dtype='int64')
rem
Int64Index([0, 1, 2, 0, 1, 2, 0, 1, 2, 0], dtype='int64')
```
`divmod()` 还支持元素级运算：
```python
div, rem = divmod(s, [2, 2, 3, 3, 4, 4, 5, 5, 6, 6])
div 
0    0
1    0
2    0
3    1
4    1
5    1
6    1
7    1
8    1
9    1
dtype: int64
rem
0    0
1    1
2    2
3    0
4    0
5    1
6    1
7    2
8    2
9    3
dtype: int64
```
### 比较操作
`Series` 与 `DataFrame` 还支持 `eq`、`ne`、`lt`、`gt`、`le`、`ge` 等二进制比较操作的方法：
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200402184957.png)
```python
df.gt(df2)
     one    two  three
a  False  False  False
b  False  False  False
c  False  False  False
d  False  False  False
df2.ne(df)
     one    two  three
a  False  False   True
b  False  False  False
c  False  False  False
d   True  False  False
```
这些操作生成一个与左侧输入对象类型相同的 `Pandas` 对象，即，`dtype` 为 `bool`。`boolean` 对象可用于索引操作。
### 布尔简化
`empty`、`any()`、`all()`、`bool()` 可以把数据汇总简化至单个布尔值。
```python
(df > 0).all() 
one      False
two       True
three    False
dtype: bool
(df > 0).any()
one      True
two      True
three    True
dtype: bool
```
还可以进一步把上面的结果简化为单个布尔值。
```python
(df > 0).any().any()
True
```
通过 `empty` 属性，可以验证 `Pandas` 对象是否为空。
```python
df.empty
False
pd.DataFrame(columns=list('ABC')).empty
True
```
用 `bool()` 方法验证单元素 `pandas` 对象的布尔值。
```python
pd.Series([True]).bool()
True
pd.Series([False]).bool()
False
pd.DataFrame([[True]]).bool()
True
pd.DataFrame([[False]]).bool()
False
```
以下代码：
```python
if df:
   pass
```
或
```python
df and df2
```
上述代码试图比对多个值，因此，这两种操作都会触发错误：
```python
ValueError: The truth value of an array is ambiguous. Use a.empty, a.any() or a.all().
```
### 比较对象是否等效
一般情况下，多种方式都能得出相同的结果。以 `df + df` 与 `df * 2` 为例。应用上一小节学到的知识，测试这两种计算方式的结果是否一致，一般人都会用 `(df + df == df * 2).all()`，不过，这个表达式的结果是 `False`：
```python
df + df == df * 2
     one   two  three
a   True  True  False
b   True  True   True
c   True  True   True
d  False  True   True
(df + df == df * 2).all()
one      False
two       True
three    False
dtype: bool
```
注意：布尔型 `DataFrame` `df + df == df * 2` 中有 `False` 值！这是因为两个 `NaN` 值的比较结果为不等：
```python
np.nan == np.nan
False
```
为了验证数据是否等效，`Series` 与 `DataFrame` 等 `N` 维框架提供了 `equals()` 方法，用这个方法验证 `NaN` 值的结果为相等。
```python
(df + df).equals(df * 2)
True
```
注意：`Series` 与 `DataFrame` 索引的顺序必须一致，验证结果才能为 `True`：
```python
df1 = pd.DataFrame({'col': ['foo', 0, np.nan]})
df2 = pd.DataFrame({'col': [np.nan, 0, 'foo']}, index=[2, 1, 0])
df1.equals(df2)
False
df1.equals(df2.sort_index())
True
```
### 比较 `array` 型对象
用标量值与 `Pandas` 数据结构对比数据元素非常简单：
```python
pd.Series(['foo', 'bar', 'baz']) == 'foo' 
0     True
1    False
2    False
dtype: bool
pd.Index(['foo', 'bar', 'baz']) == 'foo'
array([ True, False, False])
```
`Pandas` 还能对比两个等长 `array` 对象里的数据元素：
```python
pd.Series(['foo', 'bar', 'baz']) == pd.Index(['foo', 'bar', 'qux']) 
0     True
1     True
2    False
dtype: bool
pd.Series(['foo', 'bar', 'baz']) == np.array(['foo', 'bar', 'qux'])
0     True
1     True
2    False
dtype: bool
```
对比不等长的 `Index` 或 `Series` 对象会触发 `ValueError`：
```python
pd.Series(['foo', 'bar', 'baz']) == pd.Series(['foo', 'bar'])
ValueError: Series lengths must match to compare
pd.Series(['foo', 'bar', 'baz']) == pd.Series(['foo'])
ValueError: Series lengths must match to compare
```
注意： 这里的操作与 `NumPy` 的广播机制不同：
```python
np.array([1, 2, 3]) == np.array([2])
array([False,  True, False])
```
`NumPy` 无法执行广播操作时，返回 `False`:
```python
np.array([1, 2, 3]) == np.array([1, 2])
False
```
### 合并重叠数据集
有时，要合并两个相似的数据集，两个数据集里的其中一个的数据比另一个多。比如，展示特定经济指标的两个数据序列，其中一个是“高质量”指标，另一个是“低质量”指标。一般来说，低质量序列可能包含更多的历史数据，或覆盖更广的数据。因此，要合并这两个 `DataFrame` 对象，其中一个 `DataFrame` 中的缺失值将按指定条件用另一个 `DataFrame` 里类似标签中的数据进行填充。要实现这一操作，请用下列代码中的 `combine_first()` 函数。
```python
df1 = pd.DataFrame({'A': [1., np.nan, 3., 5., np.nan],
                    'B': [np.nan, 2., 3., np.nan, 6.]}) 
df2 = pd.DataFrame({'A': [5., 2., 4., np.nan, 3., 7.],
                    'B': [np.nan, np.nan, 3., 4., 6., 8.]})
df1 
     A    B
0  1.0  NaN
1  NaN  2.0
2  3.0  3.0
3  5.0  NaN
4  NaN  6.0
df2
     A    B
0  5.0  NaN
1  2.0  NaN
2  4.0  3.0
3  NaN  4.0
4  3.0  6.0
5  7.0  8.0
df1.combine_first(df2)
     A    B
0  1.0  NaN
1  2.0  2.0
2  3.0  3.0
3  5.0  4.0
4  3.0  6.0
5  7.0  8.0
```
### `DataFrame` 通用合并方法
上述 `combine_first()` 方法调用了更普适的 `DataFrame.combine()` 方法。该方法提取另一个 `DataFrame` 及合并器函数，并将之与输入的 `DataFrame` 对齐，再传递与 `Series` 配对的合并器函数（比如，名称相同的列）。
下面的代码复现了上述的 `combine_first()` 函数：
```python
def combiner(x, y):
   return np.where(pd.isna(x), y, x)
```
## 描述性统计
`Series` 与 `DataFrame` 支持大量计算描述性统计的方法与操作。这些方法大部分都是 `sum()`、`mean()`、`quantile()` 等聚合函数，其输出结果比原始数据集小；此外，还有输出结果与原始数据集同样大小的 `cumsum()` 、 `cumprod()`等函数。这些方法都基本上都接受 `axis` 参数，如， `ndarray.{sum,std,…}`，但这里的 `axis` 可以用名称或整数指定：
- `Series`：无需 `axis` 参数
- `DataFrame`：
   - `index`，即 `axis=0`，默认值
   - `columns`, 即 `axis=1`
```python
df 
        one       two     three
a  1.394981  1.772517       NaN
b  0.343054  1.912123 -0.050390
c  0.695246  1.478369  1.227435
d       NaN  0.279344 -0.613172
df.mean(0)
one      0.811094
two      1.360588
three    0.187958
dtype: float64
df.mean(1)
a    1.583749
b    0.734929
c    1.133683
d   -0.166914
dtype: float64
```
上述方法都支持 `skipna` 关键字，指定是否要排除缺失数据，默认值为 `True`。
```python
df.sum(0, skipna=False)
one           NaN
two      5.442353
three         NaN
dtype: float64
df.sum(axis=1, skipna=True)
a    3.167498
b    2.204786
c    3.401050
d   -0.333828
dtype: float64
```
结合广播机制或算数操作，可以描述不同统计过程，比如标准化，即渲染数据零均值与标准差 1，这种操作非常简单：
```python
ts_stand = (df - df.mean()) / df.std()
ts_stand.std()
one      1.0
two      1.0
three    1.0
dtype: float64
xs_stand = df.sub(df.mean(1), axis=0).div(df.std(1), axis=0)
xs_stand.std(1)
a    1.0
b    1.0
c    1.0
d    1.0
dtype: float64
```
注 ：`cumsum()` 与 `cumprod()` 等方法保留 `NaN` 值的位置。这与 `expanding()` 和 `rolling()` 略显不同。
```python
df.cumsum()
        one       two     three
a  1.394981  1.772517       NaN
b  1.738035  3.684640 -0.050390
c  2.433281  5.163008  1.177045
d       NaN  5.442353  0.563873
```
下表为常用函数汇总表。每个函数都支持 `level` 参数，仅在数据对象为结构化 `Index` 时使用。
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200402211548.png)
注意：`NumPy` 的 `mean`、`std`、`sum` 等方法默认**不统计** `Series` 里的空值。
```python
np.mean(df['one'])
0.8110935116651192
np.mean(df['one'].to_numpy())
nan
```
`Series.nunique()` 返回 `Series` 里所有非空值的唯一值。
```python
series = pd.Series(np.random.randn(500))
series[20:500] = np.nan
series[10:20] = 5
series.nunique()
11
```
### 数据总结：`describe`
`describe()` 函数计算 `Series` 与 `DataFrame` 数据列的各种数据统计量，注意，这里排除了空值。
```python
series = pd.Series(np.random.randn(1000))
series[::2] = np.nan
series.describe()
count    500.000000
mean      -0.021292
std        1.015906
min       -2.683763
25%       -0.699070
50%       -0.069718
75%        0.714483
max        3.160915
dtype: float64
frame = pd.DataFrame(np.random.randn(1000, 5),columns=['a', 'b', 'c', 'd', 'e'])
frame.iloc[::2] = np.nan
frame.describe()
                a           b           c           d           e
count  500.000000  500.000000  500.000000  500.000000  500.000000
mean     0.033387    0.030045   -0.043719   -0.051686    0.005979
std      1.017152    0.978743    1.025270    1.015988    1.006695
min     -3.000951   -2.637901   -3.303099   -3.159200   -3.188821
25%     -0.647623   -0.576449   -0.712369   -0.691338   -0.691115
50%      0.047578   -0.021499   -0.023888   -0.032652   -0.025363
75%      0.729907    0.775880    0.618896    0.670047    0.649748
max      2.740139    2.752332    3.004229    2.728702    3.240991
```
此外，还可以指定输出结果包含的分位数：
```python
series.describe(percentiles=[.05, .25, .75, .95])
count    500.000000
mean      -0.021292
std        1.015906
min       -2.683763
5%        -1.645423
25%       -0.699070
50%       -0.069718
75%        0.714483
95%        1.711409
max        3.160915
dtype: float64
```
一般情况下，默认值包含中位数。
对于非数值型 `Series` 对象， `describe()` 返回值的总数、唯一值数量、出现次数最多的值及出现的次数。
```python
s = pd.Series(['a', 'a', 'b', 'b', 'a', 'a', np.nan, 'c', 'd', 'a'])
s.describe()
count     9
unique    4
top       a
freq      5
dtype: object
```
注意：对于混合型的 `DataFrame` 对象， `describe()` 只返回数值列的汇总统计量，如果没有数值列，则只显示类别型的列。
```python
frame = pd.DataFrame({'a': ['Yes', 'Yes', 'No', 'No'], 'b': range(4)})
frame.describe()
              b
count  4.000000
mean   1.500000
std    1.290994
min    0.000000
25%    0.750000
50%    1.500000
75%    2.250000
max    3.000000
```
`include`/`exclude` 参数的值为列表，用该参数可以控制包含或排除的数据类型。这里还有一个特殊值，`all`：
```python
frame.describe(include=['object'])
          a
count     4
unique    2
top     Yes
freq      2
frame.describe(include=['number'])
              b
count  4.000000
mean   1.500000
std    1.290994
min    0.000000
25%    0.750000
50%    1.500000
75%    2.250000
max    3.000000
frame.describe(include='all')
          a         b
count     4  4.000000
unique    2       NaN
top     Yes       NaN
freq      2       NaN
mean    NaN  1.500000
std     NaN  1.290994
min     NaN  0.000000
25%     NaN  0.750000
50%     NaN  1.500000
75%     NaN  2.250000
max     NaN  3.000000
```
本功能依托于 `select_dtypes`。
### 最大值与最小值对应的索引
`Series` 与 `DataFrame` 的 `idxmax()` 与 `idxmin()` 函数计算最大值与最小值对应的索引。
```python
s1 = pd.Series(np.random.randn(5))
s1
0    1.118076
1   -0.352051
2   -1.242883
3   -1.277155
4   -0.641184
dtype: float64
s1.idxmin(), s1.idxmax()
(3, 0)
df1 = pd.DataFrame(np.random.randn(5, 3), columns=['A', 'B', 'C'])
df1
          A         B         C
0 -0.327863 -0.946180 -0.137570
1 -0.186235 -0.257213 -0.486567
2 -0.507027 -0.871259 -0.111110
3  2.000339 -2.430505  0.089759
4 -0.321434 -0.033695  0.096271
df1.idxmin(axis=0)
A    2
B    3
C    1
dtype: int64
df1.idxmax(axis=1)
0    C
1    A
2    C
3    A
4    C
dtype: object
```
多行或多列中存在多个最大值或最小值时，`idxmax()` 与 `idxmin()` 只返回匹配到的第一个值的 `Index`：
```python
df3 = pd.DataFrame([2, 1, 1, 3, np.nan], columns=['A'], index=list('edcba'))
df3
     A
e  2.0
d  1.0
c  1.0
b  3.0
a  NaN
df3['A'].idxmin()
'd'
```
注意:`idxmin` 与 `idxmax` 对应 `NumPy` 里的 `argmin` 与 `argmax`。
### 值计数（直方图）与众数
`Series` 的 `value_counts()` 方法及顶级函数计算一维数组中数据值的直方图，还可以用作常规数组的函数：
```python
data = np.random.randint(0, 7, size=50)
data
array([6, 6, 2, 3, 5, 3, 2, 5, 4, 5, 4, 3, 4, 5, 0, 2, 0, 4, 2, 0, 3, 2,
       2, 5, 6, 5, 3, 4, 6, 4, 3, 5, 6, 4, 3, 6, 2, 6, 6, 2, 3, 4, 2, 1,
       6, 2, 6, 1, 5, 4])
s = pd.Series(data)
s.value_counts()
6    10
2    10
4     9
5     8
3     8
0     3
1     2
dtype: int64
pd.value_counts(data)
6    10
2    10
4     9
5     8
3     8
0     3
1     2
dtype: int64
```
与上述操作类似，还可以统计 `Series` 或 `DataFrame` 的众数，即出现频率最高的值：
```python
s5 = pd.Series([1, 1, 3, 3, 3, 5, 5, 7, 7, 7])
s5.mode() 
0    3
1    7
dtype: int64
df5 = pd.DataFrame({"A": np.random.randint(0, 7, size=50),
                    "B": np.random.randint(-10, 15, size=50)})
df5.mode()
     A   B
0  1.0  -9
1  NaN  10
2  NaN  13
```
### 离散化与分位数
`cut()` 函数（以值为依据实现分箱）及 `qcut()` 函数（以样本分位数为依据实现分箱）用于连续值的离散化：
```python
arr = np.random.randn(20)
factor = pd.cut(arr, 4)
factor
[(-0.251, 0.464], (-0.968, -0.251], (0.464, 1.179], (-0.251, 0.464], (-0.968, -0.251], ..., (-0.251, 0.464], (-0.968, -0.251], (-0.968, -0.251], (-0.968, -0.251], (-0.968, -0.251]]
Length: 20
Categories (4, interval[float64]): [(-0.968, -0.251] < (-0.251, 0.464] < (0.464, 1.179] <
                                    (1.179, 1.893]]
factor = pd.cut(arr, [-5, -1, 0, 1, 5])
factor
[(0, 1], (-1, 0], (0, 1], (0, 1], (-1, 0], ..., (-1, 0], (-1, 0], (-1, 0], (-1, 0], (-1, 0]]
Length: 20
Categories (4, interval[int64]): [(-5, -1] < (-1, 0] < (0, 1] < (1, 5]]
```
`qcut()` 计算样本分位数。比如，下列代码按等距分位数分割正态分布的数据：
```python
arr = np.random.randn(30)
factor = pd.qcut(arr, [0, .25, .5, .75, 1])
factor
[(0.569, 1.184], (-2.278, -0.301], (-2.278, -0.301], (0.569, 1.184], (0.569, 1.184], ..., (-0.301, 0.569], (1.184, 2.346], (1.184, 2.346], (-0.301, 0.569], (-2.278, -0.301]]
Length: 30
Categories (4, interval[float64]): [(-2.278, -0.301] < (-0.301, 0.569] < (0.569, 1.184] <
                                    (1.184, 2.346]]
pd.value_counts(factor)
(1.184, 2.346]      8
(-2.278, -0.301]    8
(0.569, 1.184]      7
(-0.301, 0.569]     7
dtype: int64
```
定义分箱时，还可以传递无穷值：
```python
arr = np.random.randn(20)
factor = pd.cut(arr, [-np.inf, 0, np.inf])
factor
[(-inf, 0.0], (0.0, inf], (0.0, inf], (-inf, 0.0], (-inf, 0.0], ..., (-inf, 0.0], (-inf, 0.0], (-inf, 0.0], (0.0, inf], (0.0, inf]]
Length: 20
Categories (2, interval[float64]): [(-inf, 0.0] < (0.0, inf]]
```
## 函数应用
不管是为 `Pandas` 对象应用自定义函数，还是应用第三方函数，都离不开以下三种方法。用哪种方法取决于操作的对象是 `DataFrame`，还是 `Series` ；是行、列，还是元素。
1. 表级函数应用：`pipe()`
2. 行列级函数应用：`apply()`
3. 聚合 `API`： `agg()` 与 `transform()`
4. 元素级函数应用：`applymap()`
### 表级函数应用
虽然可以把 `DataFrame` 与 `Series` 传递给函数，不过链式调用函数时，最好使用 `pipe()` 方法。对比以下两种方式：
```python
# f、g、h 是提取、返回 `DataFrames` 的函数
f(g(h(df), arg1=1), arg2=2, arg3=3)
```
下列代码与上述代码等效：
```python
(df.pipe(h).pipe(g, arg1=1).pipe(f, arg2=2, arg3=3))
```
`Pandas` 鼓励使用第二种方式，即链式方法。在链式方法中调用自定义函数或第三方支持库函数时，用 `pipe` 更容易，与用 `Pandas` 自身方法一样。
上例中，`f`、`g` 与 `h` 这几个函数都把 `DataFrame` 当作首位参数。要是想把数据作为第二个参数，该怎么办？本例中，`pipe` 为元组 `(callable,data_keyword)`形式。`.pipe` 把 `DataFrame` 作为元组里指定的参数。

下例用 `statsmodels` 拟合回归。该 `API` 先接收一个公式，`DataFrame` 是第二个参数，`data`。要传递函数，则要用`pipe` 接收关键词对 `(sm.ols,'data')`。
```python
import statsmodels.formula.api as sm
bb = pd.read_csv('data/baseball.csv', index_col='id')
(bb.query('h > 0').assign(ln_h=lambda df: np.log(df.h)).pipe((sm.ols, 'data'), 'hr ~ ln_h + year + g + C(lg)').fit().summary()) 
<class 'statsmodels.iolib.summary.Summary'>
"""
                            OLS Regression Results                            
==============================================================================
Dep. Variable:                     hr   R-squared:                       0.685
Model:                            OLS   Adj. R-squared:                  0.665
Method:                 Least Squares   F-statistic:                     34.28
Date:                Thu, 22 Aug 2019   Prob (F-statistic):           3.48e-15
Time:                        15:48:59   Log-Likelihood:                -205.92
No. Observations:                  68   AIC:                             421.8
Df Residuals:                      63   BIC:                             432.9
Df Model:                           4                                         
Covariance Type:            nonrobust                                         
===============================================================================
                  coef    std err          t      P>|t|      [0.025      0.975]
-------------------------------------------------------------------------------
Intercept   -8484.7720   4664.146     -1.819      0.074   -1.78e+04     835.780
C(lg)[T.NL]    -2.2736      1.325     -1.716      0.091      -4.922       0.375
ln_h           -1.3542      0.875     -1.547      0.127      -3.103       0.395
year            4.2277      2.324      1.819      0.074      -0.417       8.872
g               0.1841      0.029      6.258      0.000       0.125       0.243
==============================================================================
Omnibus:                       10.875   Durbin-Watson:                   1.999
Prob(Omnibus):                  0.004   Jarque-Bera (JB):               17.298
Skew:                           0.537   Prob(JB):                     0.000175
Kurtosis:                       5.225   Cond. No.                     1.49e+07
==============================================================================

Warnings:
[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
[2] The condition number is large, 1.49e+07. This might indicate that there are
strong multicollinearity or other numerical problems.
"""
```
`unix` 的 `pipe` 与后来出现的 `dplyr` 及 `magrittr` 启发了`pipe` 方法，在此，引入了 `R` 语言里用于读取 `pipe` 的操作符 `(%>%)`。`pipe`的实现思路非常清晰，仿佛 `Python` 源生的一样。强烈建议大家阅读 `pipe()` 的源代码。
### 行列级函数应用
`apply()` 方法沿着 `DataFrame` 的轴应用函数，比如，描述性统计方法，该方法支持 `axis` 参数。
```python
df 
        one       two     three
a  1.394981  1.772517       NaN
b  0.343054  1.912123 -0.050390
c  0.695246  1.478369  1.227435
d       NaN  0.279344 -0.613172
df.apply(np.mean) 
one      0.811094
two      1.360588
three    0.187958
dtype: float64
df.apply(np.mean, axis=1) 
a    1.583749
b    0.734929
c    1.133683
d   -0.166914
dtype: float64
df.apply(lambda x: x.max() - x.min())
one      1.051928
two      1.632779
three    1.840607
dtype: float64
df.apply(np.cumsum)
        one       two     three
a  1.394981  1.772517       NaN
b  1.738035  3.684640 -0.050390
c  2.433281  5.163008  1.177045
d       NaN  5.442353  0.563873
df.apply(np.exp)
        one       two     three
a  4.034899  5.885648       NaN
b  1.409244  6.767440  0.950858
c  2.004201  4.385785  3.412466
d       NaN  1.322262  0.541630
```
`apply()` 方法还支持通过函数名字符串调用函数。
```python
df.apply('mean')
one      0.811094
two      1.360588
three    0.187958
dtype: float64
df.apply('mean', axis=1)
a    1.583749
b    0.734929
c    1.133683
d   -0.166914
dtype: float64
```
默认情况下，`apply()` 调用的函数返回的类型会影响 `DataFrame.apply` 输出结果的类型。
函数返回的是 `Series` 时，最终输出结果是 `DataFrame`。输出的列与函数返回的 `Series` 索引相匹配。
函数返回其它任意类型时，输出结果是 `Series`。
`result_type` 会覆盖默认行为，该参数有三个选项：`reduce`、`broadcast`、`expand`。这些选项决定了列表型返回值是否扩展为 `DataFrame`。
用好 `apply()` 可以了解数据集的很多信息。比如可以提取每列的最大值对应的日期：
```python
tsdf = pd.DataFrame(np.random.randn(1000, 3), columns=['A', 'B', 'C'],index=pd.date_range('1/1/2000', periods=1000))
tsdf.apply(lambda x: x.idxmax())
A   2000-08-06
B   2001-01-18
C   2001-07-18
dtype: datetime64[ns]
```
还可以向 `apply()` 方法传递额外的参数与关键字参数。比如下例中要应用的这个函数：
```python
def subtract_and_divide(x, sub, divide=1):
    return (x - sub) / divide
```
可以用下列方式应用该函数：
```python
df.apply(subtract_and_divide, args=(5,), divide=3)
```
为每行或每列执行 `Series` 方法的功能也很实用：
```python
tsdf
                   A         B         C
2000-01-01 -0.158131 -0.232466  0.321604
2000-01-02 -1.810340 -3.105758  0.433834
2000-01-03 -1.209847 -1.156793 -0.136794
2000-01-04       NaN       NaN       NaN
2000-01-05       NaN       NaN       NaN
2000-01-06       NaN       NaN       NaN
2000-01-07       NaN       NaN       NaN
2000-01-08 -0.653602  0.178875  1.008298
2000-01-09  1.007996  0.462824  0.254472
2000-01-10  0.307473  0.600337  1.643950
tsdf.apply(pd.Series.interpolate)
                   A         B         C
2000-01-01 -0.158131 -0.232466  0.321604
2000-01-02 -1.810340 -3.105758  0.433834
2000-01-03 -1.209847 -1.156793 -0.136794
2000-01-04 -1.098598 -0.889659  0.092225
2000-01-05 -0.987349 -0.622526  0.321243
2000-01-06 -0.876100 -0.355392  0.550262
2000-01-07 -0.764851 -0.088259  0.779280
2000-01-08 -0.653602  0.178875  1.008298
2000-01-09  1.007996  0.462824  0.254472
2000-01-10  0.307473  0.600337  1.643950
```
`apply()` 有一个参数 `raw`，默认值为 `False`，在应用函数前，使用该参数可以将每行或列转换为 `Series`。该参数为 `True` 时，传递的函数接收 `ndarray` 对象，若不需要索引功能，这种操作能显著提高性能。

### 聚合 `API`
聚合 `API` 可以快速、简洁地执行多个聚合操作。`Pandas` 对象支持多个类似的 `API`，如 `groupby API`、`window functions API`、`resample API`。聚合函数为`DataFrame.aggregate()`，它的别名是 `DataFrame.agg()`。
此处用与上例类似的 `DataFrame`：
```python
tsdf = pd.DataFrame(np.random.randn(10, 3), columns=['A', 'B', 'C'],index=pd.date_range('1/1/2000', periods=10))
tsdf.iloc[3:7] = np.nan
tsdf
                   A         B         C
2000-01-01  1.257606  1.004194  0.167574
2000-01-02 -0.749892  0.288112 -0.757304
2000-01-03 -0.207550 -0.298599  0.116018
2000-01-04       NaN       NaN       NaN
2000-01-05       NaN       NaN       NaN
2000-01-06       NaN       NaN       NaN
2000-01-07       NaN       NaN       NaN
2000-01-08  0.814347 -0.257623  0.869226
2000-01-09 -0.250663 -1.206601  0.896839
2000-01-10  2.169758 -1.333363  0.283157
```
应用单个函数时，该操作与 `apply()` 等效，这里也可以用字符串表示聚合函数名。下面的聚合函数输出的结果为 `Series`：
```python
tsdf.agg(np.sum)
A    3.033606
B   -1.803879
C    1.575510
dtype: float64
tsdf.agg('sum')
A    3.033606
B   -1.803879
C    1.575510
dtype: float64
'''
因为应用的是单个函数，该操作与`.sum()` 是等效的
'''
tsdf.sum()
A    3.033606
B   -1.803879
C    1.575510
dtype: float64
```
`Series` 单个聚合操作返回标量值：
```python
tsdf.A.agg('sum')
3.033606102414146
```
### 多函数聚合
还可以用列表形式传递多个聚合函数。每个函数在输出结果 `DataFrame` 里以行的形式显示，行名是每个聚合函数的函数名。
```python
tsdf.agg(['sum'])
            A         B        C
sum  3.033606 -1.803879  1.57551
```
多个函数输出多行：
```python
tsdf.agg(['sum', 'mean'])
             A         B         C
sum   3.033606 -1.803879  1.575510
mean  0.505601 -0.300647  0.262585
```
`Series` 聚合多函数返回结果还是 `Series`，索引为函数名：
```python
tsdf.A.agg(['sum', 'mean'])
sum     3.033606
mean    0.505601
Name: A, dtype: float64
```
传递 `lambda` 函数时，输出名为 `<lambda>` 的行：
```python
tsdf.A.agg(['sum', lambda x: x.mean()])
sum         3.033606
<lambda>    0.505601
Name: A, dtype: float64
```
应用自定义函数时，该函数名为输出结果的行名：
```python
def mymean(x):
   return x.mean()
tsdf.A.agg(['sum', mymean])
sum       3.033606
mymean    0.505601
Name: A, dtype: float64
```
### 用字典实现聚合
指定为哪些列应用哪些聚合函数时，需要把包含列名与标量（或标量列表）的字典传递给 `DataFrame.agg`。

注意：这里输出结果的顺序不是固定的，要想让输出顺序与输入顺序一致，请使用 `OrderedDict`。
```python
tsdf.agg({'A': 'mean', 'B': 'sum'})
A    0.505601
B   -1.803879
dtype: float64
```
输入的参数是列表时，输出结果为 `DataFrame`，并以矩阵形式显示所有聚合函数的计算结果，且输出结果由所有唯一函数组成。未执行聚合操作的列输出结果为 `NaN` 值：
```python
 tsdf.agg({'A': ['mean', 'min'], 'B': 'sum'})
             A         B
mean  0.505601       NaN
min  -0.749892       NaN
sum        NaN -1.803879
```
### 多种数据类型（`Dtype`）
与 `groupby` 的 `.agg` 操作类似，`DataFrame` 含不能执行聚合的数据类型时，`.agg` 只计算可聚合的列：
```python
mdf = pd.DataFrame({'A': [1, 2, 3],
                    'B': [1., 2., 3.],
                    'C': ['foo', 'bar', 'baz'],
                    'D': pd.date_range('20130101', periods=3)})
mdf.dtypes
A             int64
B           float64
C            object
D    datetime64[ns]
dtype: object
mdf.agg(['min', 'sum'])
     A    B          C          D
min  1  1.0        bar 2013-01-01
sum  6  6.0  foobarbaz        NaT
```
### 自定义 `Describe`
`.agg()` 可以创建类似于内置 `describe` 函数 的自定义 `describe` 函数。
```python
from functools import partial
q_25 = partial(pd.Series.quantile, q=0.25)
q_25.__name__ = '25%'
q_75 = partial(pd.Series.quantile, q=0.75)
q_75.__name__ = '75%'
tsdf.agg(['count', 'mean', 'std', 'min', q_25, 'median', q_75, 'max'])
               A         B         C
count   6.000000  6.000000  6.000000
mean    0.505601 -0.300647  0.262585
std     1.103362  0.887508  0.606860
min    -0.749892 -1.333363 -0.757304
25%    -0.239885 -0.979600  0.128907
median  0.303398 -0.278111  0.225365
75%     1.146791  0.151678  0.722709
max     2.169758  1.004194  0.896839
```
### `Transform API`
`transform()` 方法的返回结果与原始数据的索引相同，大小相同。与 `.agg API` 类似，该 `API` 支持同时处理多种操作，不用一个一个操作。
下面，先创建一个 `DataFrame`：
```python
tsdf = pd.DataFrame(np.random.randn(10, 3), columns=['A', 'B', 'C'],index=pd.date_range('1/1/2000', periods=10))
tsdf.iloc[3:7] = np.nan
tsdf
                   A         B         C
2000-01-01 -0.428759 -0.864890 -0.675341
2000-01-02 -0.168731  1.338144 -1.279321
2000-01-03 -1.621034  0.438107  0.903794
2000-01-04       NaN       NaN       NaN
2000-01-05       NaN       NaN       NaN
2000-01-06       NaN       NaN       NaN
2000-01-07       NaN       NaN       NaN
2000-01-08  0.254374 -1.240447 -0.201052
2000-01-09 -0.157795  0.791197 -1.144209
2000-01-10 -0.030876  0.371900  0.061932
```
这里转换的是整个 `DataFrame`。`.transform()` 支持 `NumPy` 函数、字符串函数及自定义函数。
```python
tsdf.transform(np.abs)
                   A         B         C
2000-01-01  0.428759  0.864890  0.675341
2000-01-02  0.168731  1.338144  1.279321
2000-01-03  1.621034  0.438107  0.903794
2000-01-04       NaN       NaN       NaN
2000-01-05       NaN       NaN       NaN
2000-01-06       NaN       NaN       NaN
2000-01-07       NaN       NaN       NaN
2000-01-08  0.254374  1.240447  0.201052
2000-01-09  0.157795  0.791197  1.144209
2000-01-10  0.030876  0.371900  0.061932
 tsdf.transform('abs')
Out[180]: 
                   A         B         C
2000-01-01  0.428759  0.864890  0.675341
2000-01-02  0.168731  1.338144  1.279321
2000-01-03  1.621034  0.438107  0.903794
2000-01-04       NaN       NaN       NaN
2000-01-05       NaN       NaN       NaN
2000-01-06       NaN       NaN       NaN
2000-01-07       NaN       NaN       NaN
2000-01-08  0.254374  1.240447  0.201052
2000-01-09  0.157795  0.791197  1.144209
2000-01-10  0.030876  0.371900  0.061932
tsdf.transform(lambda x: x.abs())
                   A         B         C
2000-01-01  0.428759  0.864890  0.675341
2000-01-02  0.168731  1.338144  1.279321
2000-01-03  1.621034  0.438107  0.903794
2000-01-04       NaN       NaN       NaN
2000-01-05       NaN       NaN       NaN
2000-01-06       NaN       NaN       NaN
2000-01-07       NaN       NaN       NaN
2000-01-08  0.254374  1.240447  0.201052
2000-01-09  0.157795  0.791197  1.144209
2000-01-10  0.030876  0.371900  0.061932
```
这里的 `transform()`接受单个函数；与 `ufunc` 等效。
```python
np.abs(tsdf)
                   A         B         C
2000-01-01  0.428759  0.864890  0.675341
2000-01-02  0.168731  1.338144  1.279321
2000-01-03  1.621034  0.438107  0.903794
2000-01-04       NaN       NaN       NaN
2000-01-05       NaN       NaN       NaN
2000-01-06       NaN       NaN       NaN
2000-01-07       NaN       NaN       NaN
2000-01-08  0.254374  1.240447  0.201052
2000-01-09  0.157795  0.791197  1.144209
2000-01-10  0.030876  0.371900  0.061932
```
`.transform()` 向 `Series` 传递单个函数时，返回的结果也是单个 `Series`。
```python
tsdf.A.transform(np.abs) 
2000-01-01    0.428759
2000-01-02    0.168731
2000-01-03    1.621034
2000-01-04         NaN
2000-01-05         NaN
2000-01-06         NaN
2000-01-07         NaN
2000-01-08    0.254374
2000-01-09    0.157795
2000-01-10    0.030876
Freq: D, Name: A, dtype: float64
```
### 多函数 `Transform`
`transform()` 调用多个函数时，生成多层索引 `DataFrame`。第一层是原始数据集的列名；第二层是 `transform()` 调用的函数名。
```python
tsdf.transform([np.abs, lambda x: x + 1])
                   A                   B                   C          
            absolute  <lambda>  absolute  <lambda>  absolute  <lambda>
2000-01-01  0.428759  0.571241  0.864890  0.135110  0.675341  0.324659
2000-01-02  0.168731  0.831269  1.338144  2.338144  1.279321 -0.279321
2000-01-03  1.621034 -0.621034  0.438107  1.438107  0.903794  1.903794
2000-01-04       NaN       NaN       NaN       NaN       NaN       NaN
2000-01-05       NaN       NaN       NaN       NaN       NaN       NaN
2000-01-06       NaN       NaN       NaN       NaN       NaN       NaN
2000-01-07       NaN       NaN       NaN       NaN       NaN       NaN
2000-01-08  0.254374  1.254374  1.240447 -0.240447  0.201052  0.798948
2000-01-09  0.157795  0.842205  0.791197  1.791197  1.144209 -0.144209
2000-01-10  0.030876  0.969124  0.371900  1.371900  0.061932  1.061932
```
为 `Series` 应用多个函数时，输出结果是 `DataFrame`，列名是 `transform()` 调用的函数名。
```python
tsdf.A.transform([np.abs, lambda x: x + 1])
            absolute  <lambda>
2000-01-01  0.428759  0.571241
2000-01-02  0.168731  0.831269
2000-01-03  1.621034 -0.621034
2000-01-04       NaN       NaN
2000-01-05       NaN       NaN
2000-01-06       NaN       NaN
2000-01-07       NaN       NaN
2000-01-08  0.254374  1.254374
2000-01-09  0.157795  0.842205
2000-01-10  0.030876  0.969124
```
### 用字典执行 `transform` 操作
函数字典可以为每列执行指定 `transform()` 操作。
```python
tsdf.transform({'A': np.abs, 'B': lambda x: x + 1})
                   A         B
2000-01-01  0.428759  0.135110
2000-01-02  0.168731  2.338144
2000-01-03  1.621034  1.438107
2000-01-04       NaN       NaN
2000-01-05       NaN       NaN
2000-01-06       NaN       NaN
2000-01-07       NaN       NaN
2000-01-08  0.254374 -0.240447
2000-01-09  0.157795  1.791197
2000-01-10  0.030876  1.371900
```
`transform()` 的参数是列表字典时，生成的是以 `transform()` 调用的函数为名的多层索引 `DataFrame`。
```python
tsdf.transform({'A': np.abs, 'B': [lambda x: x + 1, 'sqrt']})
                   A         B          
            absolute  <lambda>      sqrt
2000-01-01  0.428759  0.135110       NaN
2000-01-02  0.168731  2.338144  1.156782
2000-01-03  1.621034  1.438107  0.661897
2000-01-04       NaN       NaN       NaN
2000-01-05       NaN       NaN       NaN
2000-01-06       NaN       NaN       NaN
2000-01-07       NaN       NaN       NaN
2000-01-08  0.254374 -0.240447       NaN
2000-01-09  0.157795  1.791197  0.889493
2000-01-10  0.030876  1.371900  0.609836
```
### 元素级函数应用
并非所有函数都能矢量化，即接受 `NumPy` 数组，返回另一个数组或值，`DataFrame` 的 `applymap()` 及 `Series` 的 `map()` ，支持任何接收单个值并返回单个值的 `Python` 函数。
```python
df4
        one       two     three
a  1.394981  1.772517       NaN
b  0.343054  1.912123 -0.050390
c  0.695246  1.478369  1.227435
d       NaN  0.279344 -0.613172
def f(x):
   return len(str(x))
df4['one'].map(f)
a    18
b    19
c    18
d     3
Name: one, dtype: int64
df4.applymap(f)
   one  two  three
a   18   17      3
b   19   18     20
c   18   18     16
d    3   19     19
```
`Series.map()` 还有个功能，可以“连接”或“映射”第二个 `Series` 定义的值。这与 `merging` / `joining` 功能联系非常紧密：
```python
s = pd.Series(['six', 'seven', 'six', 'seven', 'six'],index=['a', 'b', 'c', 'd', 'e'])
t = pd.Series({'six': 6., 'seven': 7.})
s
a      six
b    seven
c      six
d    seven
e      six
dtype: object
s.map(t)
a    6.0
b    7.0
c    6.0
d    7.0
e    6.0
dtype: float64
```
## 重置索引与更换标签
`reindex()` 是 `Pandas`里实现数据对齐的基本方法，该方法执行几乎所有功能都要用到的标签对齐功能。 `reindex` 指的是沿着指定轴，让数据与给定的一组标签进行匹配。该功能完成以下几项操作：
- 让现有数据匹配一组新标签，并重新排序；
- 在无数据但有标签的位置插入缺失值（`NA`）标记；
- 如果指定，则按逻辑填充无标签的数据，该操作多见于时间序列数据。
示例如下：
```python
s = pd.Series(np.random.randn(5), index=['a', 'b', 'c', 'd', 'e'])
s
a    1.695148
b    1.328614
c    1.234686
d   -0.385845
e   -1.326508
dtype: float64
s.reindex(['e', 'b', 'f', 'd'])
e   -1.326508
b    1.328614
f         NaN
d   -0.385845
dtype: float64
```
本例中，原 `Series` 里没有标签 `f`，因此，输出结果里 `f` 对应的值为 `NaN`。
`DataFrame` 支持同时 `reindex` 索引与列：
```python
df
        one       two     three
a  1.394981  1.772517       NaN
b  0.343054  1.912123 -0.050390
c  0.695246  1.478369  1.227435
d       NaN  0.279344 -0.613172
df.reindex(index=['c', 'f', 'b'], columns=['three', 'two', 'one'])
      three       two       one
c  1.227435  1.478369  0.695246
f       NaN       NaN       NaN
b -0.050390  1.912123  0.343054
```
`reindex` 还支持 `axis` 关键字：
```python
df.reindex(['c', 'f', 'b'], axis='index')
        one       two     three
c  0.695246  1.478369  1.227435
f       NaN       NaN       NaN
b  0.343054  1.912123 -0.050390
```
不同对象可以共享 `Index` 包含的轴标签。比如，有一个 `Series`，还有一个 `DataFrame`，可以执行下列操作：
```python
rs = s.reindex(df.index)
rs
a    1.695148
b    1.328614
c    1.234686
d   -0.385845
dtype: float64
rs.index is df.index
True
```
这里指的是，重置后，`Series` 的索引与 `DataFrame` 的索引是同一个 `Python` 对象。
`DataFrame.reindex()` 还支持 “轴样式”调用习语，可以指定单个 `labels` 参数，并指定应用于哪个 `axis`。
```python
df.reindex(['c', 'f', 'b'], axis='index')
        one       two     three
c  0.695246  1.478369  1.227435
f       NaN       NaN       NaN
b  0.343054  1.912123 -0.050390
df.reindex(['three', 'two', 'one'], axis='columns')
      three       two       one
a       NaN  1.772517  1.394981
b -0.050390  1.912123  0.343054
c  1.227435  1.478369  0.695246
d -0.613172  0.279344       NaN
```
>注意:多层索引与高级索引介绍了怎样用更简洁的方式重置索引。
>注意:编写注重性能的代码时，最好花些时间深入理解 `reindex`：预对齐数据后，操作会更快。两个未对齐的 `DataFrame` 相加，后台操作会执行 `reindex`。探索性分析时很难注意到这点有什么不同，这是因为 `reindex` 已经进行了高度优化，但需要注重 `CPU` 周期时，显式调用 `reindex` 还是有一些影响的。
### 重置索引，并与其它对象对齐
提取一个对象，并用另一个具有相同标签的对象 `reindex` 该对象的轴。这种操作的语法虽然简单，但未免有些啰嗦。这时，最好用 `reindex_like()` 方法，这是一种既有效，又简单的方式：
```python
df2
        one       two
a  1.394981  1.772517
b  0.343054  1.912123
c  0.695246  1.478369
df3
        one       two
a  0.583888  0.051514
b -0.468040  0.191120
c -0.115848 -0.242634
df.reindex_like(df2)
        one       two
a  1.394981  1.772517
b  0.343054  1.912123
c  0.695246  1.478369
```
### 用 `align` 对齐多个对象
`align()` 方法是对齐两个对象最快的方式，该方法支持 `join` 参数：
- `join='outer'`：使用两个对象索引的合集，默认值
- `join='left'`：使用左侧调用对象的索引
- `join='right'`：使用右侧传递对象的索引
- `join='inner'`：使用两个对象索引的交集
该方法返回重置索引后的两个 `Series` 元组：
```python
s = pd.Series(np.random.randn(5), index=['a', 'b', 'c', 'd', 'e'])
s1 = s[:4]
s2 = s[1:]
s1.align(s2)
(a   -0.186646
 b   -1.692424
 c   -0.303893
 d   -1.425662
 e         NaN
 dtype: float64, 
 a         NaN
 b   -1.692424
 c   -0.303893
 d   -1.425662
 e    1.114285
 dtype: float64)
s1.align(s2, join='inner')
(b   -1.692424
 c   -0.303893
 d   -1.425662
 dtype: float64, 
 b   -1.692424
 c   -0.303893
 d   -1.425662
 dtype: float64)
s1.align(s2, join='left')
(a   -0.186646
 b   -1.692424
 c   -0.303893
 d   -1.425662
 dtype: float64, 
 a         NaN
 b   -1.692424
 c   -0.303893
 d   -1.425662
 dtype: float64)
 ```
默认条件下， `join` 方法既应用于索引，也应用于列：
```python
df.align(df2, join='inner')
(        one       two
 a  1.394981  1.772517
 b  0.343054  1.912123
 c  0.695246  1.478369,         
         one       two
 a  1.394981  1.772517
 b  0.343054  1.912123
 c  0.695246  1.478369)
 ```
`align` 方法还支持 `axis` 选项，用来指定要对齐的轴：
```python
df.align(df2, join='inner', axis=0)
(        one       two     three
 a  1.394981  1.772517       NaN
 b  0.343054  1.912123 -0.050390
 c  0.695246  1.478369  1.227435,         
         one       two
 a  1.394981  1.772517
 b  0.343054  1.912123
 c  0.695246  1.478369)
 ```
如果把 `Series` 传递给 `DataFrame.align()`，可以用 `axis` 参数选择是在 `DataFrame` 的索引，还是列上对齐两个对象：
```python
df.align(df2.iloc[0], axis=1)
(        one     three       two
 a  1.394981       NaN  1.772517
 b  0.343054 -0.050390  1.912123
 c  0.695246  1.227435  1.478369
 d       NaN -0.613172  0.279344, one      1.394981
 three         NaN
 two      1.772517
 Name: a, dtype: float64)
 ```
 ![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200403130813.png)
 下面用一个简单的 `Series` 展示 `fill` 方法：
```python
rng = pd.date_range('1/3/2000', periods=8)
ts = pd.Series(np.random.randn(8), index=rng)
ts2 = ts[[0, 3, 6]]
ts
2000-01-03    0.183051
2000-01-04    0.400528
2000-01-05   -0.015083
2000-01-06    2.395489
2000-01-07    1.414806
2000-01-08    0.118428
2000-01-09    0.733639
2000-01-10   -0.936077
Freq: D, dtype: float64
ts2
2000-01-03    0.183051
2000-01-06    2.395489
2000-01-09    0.733639
dtype: float64
ts2.reindex(ts.index)
2000-01-03    0.183051
2000-01-04         NaN
2000-01-05         NaN
2000-01-06    2.395489
2000-01-07         NaN
2000-01-08         NaN
2000-01-09    0.733639
2000-01-10         NaN
Freq: D, dtype: float64
ts2.reindex(ts.index, method='ffill')
Out[225]: 
2000-01-03    0.183051
2000-01-04    0.183051
2000-01-05    0.183051
2000-01-06    2.395489
2000-01-07    2.395489
2000-01-08    2.395489
2000-01-09    0.733639
2000-01-10    0.733639
Freq: D, dtype: float64
ts2.reindex(ts.index, method='bfill')
Out[226]: 
2000-01-03    0.183051
2000-01-04    2.395489
2000-01-05    2.395489
2000-01-06    2.395489
2000-01-07    0.733639
2000-01-08    0.733639
2000-01-09    0.733639
2000-01-10         NaN
Freq: D, dtype: float64
ts2.reindex(ts.index, method='nearest')
2000-01-03    0.183051
2000-01-04    0.183051
2000-01-05    2.395489
2000-01-06    2.395489
2000-01-07    2.395489
2000-01-08    0.733639
2000-01-09    0.733639
2000-01-10    0.733639
Freq: D, dtype: float64
```
上述操作要求索引按递增或递减排序。

>注意：除了 `method='nearest'`，用 `fillna` 或 `interpolate` 也能实现同样的效果：
```python
ts2.reindex(ts.index).fillna(method='ffill')
2000-01-03    0.183051
2000-01-04    0.183051
2000-01-05    0.183051
2000-01-06    2.395489
2000-01-07    2.395489
2000-01-08    2.395489
2000-01-09    0.733639
2000-01-10    0.733639
Freq: D, dtype: float64
```
如果索引不是按递增或递减排序，`reindex()` 会触发 `ValueError` 错误。`fillna()` 与 `interpolate()` 则不检查索引的排序。

### 重置索引填充的限制
`limit` 与 `tolerance` 参数可以控制 `reindex` 的填充操作。`limit`限定了连续匹配的最大数量：
```python
ts2.reindex(ts.index, method='ffill', limit=1)
2000-01-03    0.183051
2000-01-04    0.183051
2000-01-05         NaN
2000-01-06    2.395489
2000-01-07    2.395489
2000-01-08         NaN
2000-01-09    0.733639
2000-01-10    0.733639
Freq: D, dtype: float64
```
反之，`tolerance` 限定了索引与索引器值之间的最大距离：
```python
ts2.reindex(ts.index, method='ffill', tolerance='1 day')
2000-01-03    0.183051
2000-01-04    0.183051
2000-01-05         NaN
2000-01-06    2.395489
2000-01-07    2.395489
2000-01-08         NaN
2000-01-09    0.733639
2000-01-10    0.733639
Freq: D, dtype: float64
```
>注意：索引为 `DatetimeIndex`、`TimedeltaIndex` 或 `PeriodIndex` 时，`tolerance` 会尽可能将这些索引强制转换为 `Timedelta`，这里要求用户用恰当的字符串设定 `tolerance` 参数。
### 去掉轴上的标签
`drop()` 函数与 `reindex` 经常配合使用，该函数用于删除轴上的一组标签：
```python
df
        one       two     three
a  1.394981  1.772517       NaN
b  0.343054  1.912123 -0.050390
c  0.695246  1.478369  1.227435
d       NaN  0.279344 -0.613172
df.drop(['a', 'd'], axis=0)
        one       two     three
b  0.343054  1.912123 -0.050390
c  0.695246  1.478369  1.227435
df.drop(['one'], axis=1)
        two     three
a  1.772517       NaN
b  1.912123 -0.050390
c  1.478369  1.227435
d  0.279344 -0.613172
```
注意：下面的代码可以运行，但不够清晰：
```python
df.reindex(df.index.difference(['a', 'd']))
        one       two     three
b  0.343054  1.912123 -0.050390
c  0.695246  1.478369  1.227435
```
### 重命名或映射标签
`rename()` 方法支持按不同的轴基于映射（字典或 `Series`）调整标签。
```python
s
a   -0.186646
b   -1.692424
c   -0.303893
d   -1.425662
e    1.114285
dtype: float64
s.rename(str.upper)
A   -0.186646
B   -1.692424
C   -0.303893
D   -1.425662
E    1.114285
dtype: float64
```
如果调用的是函数，该函数在处理标签时，必须返回一个值，而且生成的必须是一组唯一值。此外，`rename()` 还可以调用字典或 `Series`。
```python
df.rename(columns={'one': 'foo', 'two': 'bar'},index={'a': 'apple', 'b': 'banana', 'd': 'durian'})
             foo       bar     three
apple   1.394981  1.772517       NaN
banana  0.343054  1.912123 -0.050390
c       0.695246  1.478369  1.227435
durian       NaN  0.279344 -0.613172
```
`Pandas` 不会重命名标签未包含在映射里的列或索引。注意，映射里多出的标签不会触发错误。

`DataFrame.rename()` 还支持“轴式”习语，用这种方式可以指定单个 `mapper`，及执行映射的 `axis`。
```python
df.rename({'one': 'foo', 'two': 'bar'}, axis='columns')
        foo       bar     three
a  1.394981  1.772517       NaN
b  0.343054  1.912123 -0.050390
c  0.695246  1.478369  1.227435
d       NaN  0.279344 -0.613172
df.rename({'a': 'apple', 'b': 'banana', 'd': 'durian'}, axis='index')
             one       two     three
apple   1.394981  1.772517       NaN
banana  0.343054  1.912123 -0.050390
c       0.695246  1.478369  1.227435
durian       NaN  0.279344 -0.613172
```
`rename()` 方法还提供了 `inplace` 命名参数，默认为 `False`，并会复制底层数据。`inplace=True` 时，会直接在原数据上重命名。

`rename()` 还支持用标量或列表更改 `Series.name` 属性。
```python
s.rename("scalar-name")
a   -0.186646
b   -1.692424
c   -0.303893
d   -1.425662
e    1.114285
Name: scalar-name, dtype: float64
```
`rename_axis()` 方法支持指定 多层索引 名称，与标签相对应。
```python
df = pd.DataFrame({'x': [1, 2, 3, 4, 5, 6],'y': [10, 20, 30, 40, 50, 60]},index=pd.MultiIndex.from_product([['a', 'b', 'c'], [1, 2]],names=['let', 'num']))
df
         x   y
let num       
a   1    1  10
    2    2  20
b   1    3  30
    2    4  40
c   1    5  50
    2    6  60

df.rename_axis(index={'let': 'abc'})
         x   y
abc num       
a   1    1  10
    2    2  20
b   1    3  30
    2    4  40
c   1    5  50
    2    6  60
df.rename_axis(index=str.upper)
         x   y
LET NUM       
a   1    1  10
    2    2  20
b   1    3  30
    2    4  40
c   1    5  50
    2    6  60
```
## 迭代
`Pandas` 对象基于类型进行迭代操作。`Series` 迭代时被视为数组，基础迭代生成值。`DataFrame` 则遵循字典式习语，用对象的 `key` 实现迭代操作。
简言之，基础迭代（`for i in object`）生成：

- `Series` ：值
- `DataFrame`：列标签
例如，`DataFrame` 迭代时输出列名：
```python
df = pd.DataFrame({'col1': np.random.randn(3),'col2': np.random.randn(3)}, index=['a', 'b', 'c'])
for col in df:
   print(col)
col1
col2
```
`Pandas` 对象还支持字典式的 `items()` 方法，通过键值对迭代。
用下列方法可以迭代 `DataFrame` 里的行：
- `iterrows()`：把 `DataFrame` 里的行当作 （`index`， `Series`）对进行迭代。该操作把行转为 `Series`，同时改变数据类型，并对性能有影响。
- `itertuples()`: 把 `DataFrame` 的行当作值的命名元组进行迭代。该操作比 `iterrows()` 快的多，建议尽量用这种方法迭代 `DataFrame` 的值。
警告：`Pandas` 对象迭代的速度较慢。大部分情况下，没必要对行执行迭代操作，建议用以下几种替代方式：
- 矢量化：很多操作可以用内置方法或 `NumPy` 函数，布尔索引……
- 调用的函数不能在完整的 `DataFrame` / `Series` 上运行时，最好用 `apply()`，不要对值进行迭代操作。
- 如果必须对值进行迭代，请务必注意代码的性能，建议在 `cython` 或 `numba` 环境下实现内循环。参阅性能优化一节，查看这种操作方法的示例。
警告
永远不要修改迭代的内容，这种方式不能确保所有操作都能正常运作。基于数据类型，迭代器返回的是复制（`copy`）的结果，不是视图（`view`），这种写入可能不会生效！

下例中的赋值就不会生效：
```python
df = pd.DataFrame({'a': [1, 2, 3], 'b': ['a', 'b', 'c']})
for index, row in df.iterrows():
   row['a'] = 10 
df
   a  b
0  1  a
1  2  b
2  3  c
```
### 项目（`items`）
与字典型接口类似，`items()` 通过键值对进行迭代：
- `Series`：（`Index`，标量值）对
- `DataFrame`：（列，`Series`）对
```python
for label, ser in df.items():
   print(label)
   print(ser)
a
0    1
1    2
2    3
Name: a, dtype: int64
b
0    a
1    b
2    c
Name: b, dtype: object
```
### `iterrows()`
`iterrows()` 迭代 `DataFrame` 或 `Series` 里的每一行数据。这个操作返回一个迭代器，生成索引值及包含每行数据的 `Series`：
```python
for row_index, row in df.iterrows():
   print(row_index, row, sep='\n')
0
a    1
b    a
Name: 0, dtype: object
1
a    2
b    b
Name: 1, dtype: object
2
a    3
b    c
Name: 2, dtype: object
```
注意：`iterrows()` 返回的是 `Series` 里的每一行数据，该操作不保留每行数据的数据类型，因为数据类型是通过 `DataFrame` 的列界定的。
示例如下：
```python
df_orig = pd.DataFrame([[1, 1.5]], columns=['int', 'float'])
df_orig.dtypes
int        int64
float    float64
dtype: object
row = next(df_orig.iterrows())[1]
row
int      1.0
float    1.5
Name: 0, dtype: float64
```
`row` 里的值以 `Series` 形式返回，并被转换为浮点数，原始的整数值则在列 `X`：
```python
row['int'].dtype
dtype('float64')
df_orig['int'].dtype
dtype('int64')
```
### `itertuples()`
要想在行迭代时保存数据类型，最好用 `itertuples()`，这个函数返回值的命名元组，总的来说，该操作比 `iterrows()` 速度更快。

下例展示了怎样转置 `DataFrame`：
```python
df2 = pd.DataFrame({'x': [1, 2, 3], 'y': [4, 5, 6]})
print(df2)
   x  y
0  1  4
1  2  5
2  3  6
print(df2.T)
   0  1  2
x  1  2  3
y  4  5  6
df2_t = pd.DataFrame({idx: values for idx, values in df2.iterrows()})
print(df2_t)
   0  1  2
x  1  2  3
y  4  5  6
```
`itertuples()` 方法返回为 `DataFrame` 里每行数据生成命名元组的迭代器。该元组的第一个元素是行的索引值，其余的值则是行的值。
```python
for row in df.itertuples():   
   print(row)
Pandas(Index=0, a=1, b='a')
Pandas(Index=1, a=2, b='b')
Pandas(Index=2, a=3, b='c')
```
该方法不会把行转换为 `Series`，只是返回命名元组里的值。`itertuples()` 保存值的数据类型，而且比 `iterrows()` 快。

注意:包含无效 `Python` 识别符的列名、重复的列名及以下划线开头的列名，会被重命名为位置名称。如果列数较大，比如大于 `255` 列，则返回正则元组。

## `.dt` 访问器
`Series` 提供一个可以简单、快捷地返回 `datetime` 属性值的访问器。这个访问器返回的也是 `Series`，索引与现有的 `Series` 一样。
```python
s = pd.Series(pd.date_range('20130101 09:10:12', periods=4))
s
0   2013-01-01 09:10:12
1   2013-01-02 09:10:12
2   2013-01-03 09:10:12
3   2013-01-04 09:10:12
dtype: datetime64[ns]
s.dt.hour
0    9
1    9
2    9
3    9
dtype: int64
s.dt.second
0    12
1    12
2    12
3    12
dtype: int64
s.dt.day
0    1
1    2
2    3
3    4
dtype: int64
```
用下列表达式进行筛选非常方便：
```python
s[s.dt.day == 2]
1   2013-01-02 09:10:12
dtype: datetime64[ns]
```
还可以用 `Series.dt.strftime()` 把 `datetime` 的值当成字符串进行格式化，支持与标准 `strftime()` 同样的格式。

### `DatetimeIndex`
```python
s = pd.Series(pd.date_range('20130101', periods=4))
s
0   2013-01-01
1   2013-01-02
2   2013-01-03
3   2013-01-04
dtype: datetime64[ns]
s.dt.strftime('%Y/%m/%d')
0    2013/01/01
1    2013/01/02
2    2013/01/03
3    2013/01/04
dtype: object
```
### PeriodIndex
```python
s = pd.Series(pd.period_range('20130101', periods=4))
s
0    2013-01-01
1    2013-01-02
2    2013-01-03
3    2013-01-04
dtype: period[D]
s.dt.strftime('%Y/%m/%d')
0    2013/01/01
1    2013/01/02
2    2013/01/03
3    2013/01/04
dtype: object
```
`.dt` 访问器还支持 `period` 与 `timedelta`。

### `period`
```python
s = pd.Series(pd.period_range('20130101', periods=4, freq='D'))
s
0    2013-01-01
1    2013-01-02
2    2013-01-03
3    2013-01-04
dtype: period[D]
s.dt.year
0    2013
1    2013
2    2013
3    2013
dtype: int64
s.dt.day
0    1
1    2
2    3
3    4
dtype: int64
```
### `timedelta`
```python
s = pd.Series(pd.timedelta_range('1 day 00:00:05', periods=4, freq='s'))
s
0   1 days 00:00:05
1   1 days 00:00:06
2   1 days 00:00:07
3   1 days 00:00:08
dtype: timedelta64[ns]
s.dt.days
0    1
1    1
2    1
3    1
dtype: int64
s.dt.seconds
0    5
1    6
2    7
3    8
dtype: int64
s.dt.components
   days  hours  minutes  seconds  milliseconds  microseconds  nanoseconds
0     1      0        0        5             0             0            0
1     1      0        0        6             0             0            0
2     1      0        0        7             0             0            0
3     1      0        0        8             0             0            0
```
注意:用这个访问器处理不是 `datetime` 类型的值时，`Series.dt` 会触发 `TypeError` 错误。
## 矢量化字符串方法
`Series` 支持字符串处理方法，可以非常方便地操作数组里的每个元素。这些方法会自动排除缺失值与空值，这也许是其最重要的特性。这些方法通过 `Series` 的 `str` 属性访问，一般情况下，这些操作的名称与内置的字符串方法一致。示例如下：
```python
s = pd.Series(['A', 'B', 'C', 'Aaba', 'Baca', np.nan, 'CABA', 'dog', 'cat'])
s.str.lower()
0       a
1       b
2       c
3    aaba
4    baca
5     NaN
6    caba
7     dog
8     cat
dtype: object
```
这里还提供了强大的模式匹配方法，但工业注意，模式匹配方法默认使用正则表达式
## 排序
`Pandas` 支持三种排序方式，按索引标签排序，按列里的值排序，按两种方式混合排序。
### 按索引排序
`Series.sort_index()` 与 `DataFrame.sort_index()` 方法用于按索引层级对 `Pandas` 对象排序。
```python
df = pd.DataFrame({'one': pd.Series(np.random.randn(3), index=['a', 'b', 'c']),
                   'two': pd.Series(np.random.randn(4), index=['a', 'b', 'c', 'd']),
                   'three': pd.Series(np.random.randn(3), index=['b', 'c', 'd'])})
unsorted_df = df.reindex(index=['a', 'd', 'c', 'b'],columns=['three', 'two', 'one'])
unsorted_df
      three       two       one
a       NaN -1.152244  0.562973
d -0.252916 -0.109597       NaN
c  1.273388 -0.167123  0.640382
b -0.098217  0.009797 -1.299504

unsorted_df.sort_index()
      three       two       one
a       NaN -1.152244  0.562973
b -0.098217  0.009797 -1.299504
c  1.273388 -0.167123  0.640382
d -0.252916 -0.109597       NaN
unsorted_df.sort_index(ascending=False)
      three       two       one
d -0.252916 -0.109597       NaN
c  1.273388 -0.167123  0.640382
b -0.098217  0.009797 -1.299504
a       NaN -1.152244  0.562973
unsorted_df.sort_index(axis=1)
        one     three       two
a  0.562973       NaN -1.152244
d       NaN -0.252916 -0.109597
c  0.640382  1.273388 -0.167123
b -1.299504 -0.098217  0.009797
unsorted_df['three'].sort_index()
a         NaN
b   -0.098217
c    1.273388
d   -0.252916
Name: three, dtype: float64
```
### 按值排序
`Series.sort_values()` 方法用于按值对 `Series` 排序。`DataFrame.sort_values()` 方法用于按行列的值对 `DataFrame` 排序。`DataFrame`.`sort_values()` 的可选参数 `by` 用于指定按哪列排序，该参数的值可以是一列或多列数据。
```python
df1 = pd.DataFrame({'one': [2, 1, 1, 1],
                    'two': [1, 3, 2, 4],
                    'three': [5, 4, 3, 2]})
df1.sort_values(by='two')
   one  two  three
0    2    1      5
2    1    2      3
1    1    3      4
3    1    4      2
```
参数 `by` 支持列名列表，示例如下：
```python
df1[['one', 'two', 'three']].sort_values(by=['one', 'two'])
   one  two  three
2    1    2      3
1    1    3      4
3    1    4      2
0    2    1      5
```
这些方法支持用 `na_position` 参数处理空值。
```python
s[2] = np.nan
s.sort_values()
0       A
3    Aaba
1       B
4    Baca
6    CABA
8     cat
7     dog
2     NaN
5     NaN
dtype: object
s.sort_values(na_position='first')
2     NaN
5     NaN
0       A
3    Aaba
1       B
4    Baca
6    CABA
8     cat
7     dog
dtype: object
```
### 按索引与值排序
通过参数 `by` 传递给 `DataFrame.sort_values()` 的字符串可以引用列或索引层名。

#### 创建 `MultiIndex`
```python
idx = pd.MultiIndex.from_tuples([('a', 1), ('a', 2), ('a', 2),
                                 ('b', 2), ('b', 1), ('b', 1)])
idx.names = ['first', 'second']
```
#### 创建 `DataFrame`
```python
df_multi = pd.DataFrame({'A': np.arange(6, 0, -1)},index=idx)
df_multi
              A
first second   
a     1       6
      2       5
      2       4
b     2       3
      1       2
      1       1
```
按 `second`（索引）与 `A`（列）排序。
```python
df_multi.sort_values(by=['second', 'A'])
              A
first second   
b     1       1
      1       2
a     1       6
b     2       3
a     2       4
      2       5
```
注意：字符串、列名、索引层名重名时，会触发警告提示，并以列名为准。后期版本中，这种情况将会触发模糊错误。
### 搜索排序
`Series` 支持 `searchsorted()` 方法，这与`numpy.ndarray.searchsorted()` 的操作方式类似。
```python
ser = pd.Series([1, 2, 3])
ser.searchsorted([0, 3])
array([0, 2])
ser.searchsorted([0, 4])
array([0, 3])
ser.searchsorted([1, 3], side='right')
array([1, 3])
ser.searchsorted([1, 3], side='left')
array([0, 2])
ser = pd.Series([3, 1, 2])
ser.searchsorted([0, 3], sorter=np.argsort(ser))
array([0, 2])
```
### 最大值与最小值
`Series` 支持 `nsmallest()` 与 `nlargest()` 方法，本方法返回 `N` 个最大或最小的值。对于数据量大的 `Series` 来说，该方法比先为整个 `Series` 排序，再调用 `head(n)` 这种方式的速度要快得多。
```python
s = pd.Series(np.random.permutation(10))
s
0    2
1    0
2    3
3    7
4    1
5    5
6    9
7    6
8    8
9    4
dtype: int64
s.sort_values()
1    0
4    1
0    2
2    3
9    4
5    5
7    6
3    7
8    8
6    9
dtype: int64
s.nsmallest(3)
1    0
4    1
0    2
dtype: int64
s.nlargest(3)
6    9
8    8
3    7
dtype: int64
```
`DataFrame` 也支持 `nlargest` 与 `nsmallest` 方法。
```python
df = pd.DataFrame({'a': [-2, -1, 1, 10, 8, 11, -1],
                   'b': list('abdceff'),
                   'c': [1.0, 2.0, 4.0, 3.2, np.nan, 3.0, 4.0]}) 
df.nlargest(3, 'a')
    a  b    c
5  11  f  3.0
3  10  c  3.2
4   8  e  NaN
df.nlargest(5, ['a', 'c'])
    a  b    c
5  11  f  3.0
3  10  c  3.2
4   8  e  NaN
2   1  d  4.0
6  -1  f  4.0
df.nsmallest(3, 'a')
   a  b    c
0 -2  a  1.0
1 -1  b  2.0
6 -1  f  4.0
df.nsmallest(5, ['a', 'c'])
   a  b    c
0 -2  a  1.0
1 -1  b  2.0
6 -1  f  4.0
2  1  d  4.0
4  8  e  NaN
```
### 用多层索引的列排序
列为多层索引时，可以显式排序，用 `by` 指定所有层级。
```python
df1.columns = pd.MultiIndex.from_tuples([('a', 'one'),
                                         ('a', 'two'),
                                         ('b', 'three')])
df1.sort_values(by=('a', 'two'))
    a         b
  one two three
0   2   1     5
2   1   2     3
1   1   3     4
3   1   4     2
```
## 选择
>提醒:选择、设置标准 `Python` / `Numpy` 的表达式已经非常直观，交互也很方便，但对于生产代码，我们还是推荐优化过的 `Pandas` 数据访问方法：`.at`、`.iat`、`.loc` 和 `.iloc`。
### 获取数据
选择单列，产生 `Series`，与 `df.A` 等效：
```python
df['A']
2013-01-01    0.469112
2013-01-02    1.212112
2013-01-03   -0.861849
2013-01-04    0.721555
2013-01-05   -0.424972
2013-01-06   -0.673690
Freq: D, Name: A, dtype: float64
```
用 `[ ]` 切片行：
```python
df[0:3] 
                   A         B         C         D
2013-01-01  0.469112 -0.282863 -1.509059 -1.135632
2013-01-02  1.212112 -0.173215  0.119209 -1.044236
2013-01-03 -0.861849 -2.104569 -0.494929  1.071804
df['20130102':'20130104'] 
                   A         B         C         D
2013-01-02  1.212112 -0.173215  0.119209 -1.044236
2013-01-03 -0.861849 -2.104569 -0.494929  1.071804
2013-01-04  0.721555 -0.706771 -1.039575  0.271860
```
### 按标签选择

用标签提取一行数据：
```python
df.loc[dates[0]]
A    0.469112
B   -0.282863
C   -1.509059
D   -1.135632
Name: 2013-01-01 00:00:00, dtype: float64
```
用标签选择多列数据：
```python
df.loc[:, ['A', 'B']] 
                   A         B
2013-01-01  0.469112 -0.282863
2013-01-02  1.212112 -0.173215
2013-01-03 -0.861849 -2.104569
2013-01-04  0.721555 -0.706771
2013-01-05 -0.424972  0.567020
2013-01-06 -0.673690  0.113648
```
用标签切片，包含行与列结束点：
```python
df.loc['20130102':'20130104', ['A', 'B']]
                   A         B
2013-01-02  1.212112 -0.173215
2013-01-03 -0.861849 -2.104569
2013-01-04  0.721555 -0.706771
```
返回对象降维：
```python
df.loc['20130102', ['A', 'B']]
A    1.212112
B   -0.173215
Name: 2013-01-02 00:00:00, dtype: float64
```
提取标量值：
```python
df.loc[dates[0], 'A']
0.46911229990718628
```
快速访问标量，与上述方法等效：
```python
df.at[dates[0], 'A']
0.46911229990718628
```
### 按位置选择
用整数位置选择：
```python
df.iloc[3]
A    0.721555
B   -0.706771
C   -1.039575
D    0.271860
Name: 2013-01-04 00:00:00, dtype: float64
```
类似 `NumPy` / `Python`，用整数切片：
```python
df.iloc[3:5, 0:2] 
                   A         B
2013-01-04  0.721555 -0.706771
2013-01-05 -0.424972  0.567020
```
类似 `NumPy` / `Python`，用整数列表按位置切片：
```python
df.iloc[[1, 2, 4], [0, 2]]
                   A         C
2013-01-02  1.212112  0.119209
2013-01-03 -0.861849 -0.494929
2013-01-05 -0.424972  0.276232
```
显式整行切片：
```python
df.iloc[1:3, :]
                   A         B         C         D
2013-01-02  1.212112 -0.173215  0.119209 -1.044236
2013-01-03 -0.861849 -2.104569 -0.494929  1.071804
```
显式整列切片：
```python
df.iloc[:, 1:3]
                   B         C
2013-01-01 -0.282863 -1.509059
2013-01-02 -0.173215  0.119209
2013-01-03 -2.104569 -0.494929
2013-01-04 -0.706771 -1.039575
2013-01-05  0.567020  0.276232
2013-01-06  0.113648 -1.478427
```
显式提取值：
```python
df.iloc[1, 1]
-0.17321464905330858
```
快速访问标量，与上述方法等效：
```python
df.iat[1, 1]
-0.17321464905330858
```
### 布尔索引
用单列的值选择数据：
```python
df[df.A > 0] 
                   A         B         C         D
2013-01-01  0.469112 -0.282863 -1.509059 -1.135632
2013-01-02  1.212112 -0.173215  0.119209 -1.044236
2013-01-04  0.721555 -0.706771 -1.039575  0.271860
```
选择 `DataFrame` 里满足条件的值：
```python
df[df > 0]
                   A         B         C         D
2013-01-01  0.469112       NaN       NaN       NaN
2013-01-02  1.212112       NaN  0.119209       NaN
2013-01-03       NaN       NaN       NaN  1.071804
2013-01-04  0.721555       NaN       NaN  0.271860
2013-01-05       NaN  0.567020  0.276232       NaN
2013-01-06       NaN  0.113648       NaN  0.524988
```
用 `isin()` 筛选：
```python
df2 = df.copy()
df2['E'] = ['one', 'one', 'two', 'three', 'four', 'three']
df2
                   A         B         C         D      E
2013-01-01  0.469112 -0.282863 -1.509059 -1.135632    one
2013-01-02  1.212112 -0.173215  0.119209 -1.044236    one
2013-01-03 -0.861849 -2.104569 -0.494929  1.071804    two
2013-01-04  0.721555 -0.706771 -1.039575  0.271860  three
2013-01-05 -0.424972  0.567020  0.276232 -1.087401   four
2013-01-06 -0.673690  0.113648 -1.478427  0.524988  three
df2[df2['E'].isin(['two', 'four'])]
                   A         B         C         D     E
2013-01-03 -0.861849 -2.104569 -0.494929  1.071804   two
2013-01-05 -0.424972  0.567020  0.276232 -1.087401  four
```
### 赋值
用索引自动对齐新增列的数据：
```python
s1 = pd.Series([1, 2, 3, 4, 5, 6], index=pd.date_range('20130102', periods=6))
s1
2013-01-02    1
2013-01-03    2
2013-01-04    3
2013-01-05    4
2013-01-06    5
2013-01-07    6
Freq: D, dtype: int64
df['F'] = s1
```
按标签赋值：
```python
df.at[dates[0], 'A'] = 0
```
按位置赋值：
```python
df.iat[0, 1] = 0
```
按 `NumPy` 数组赋值：
```python
df.loc[:, 'D'] = np.array([5] * len(df))
```
上述赋值结果：
```python
df
                   A         B         C  D    F
2013-01-01  0.000000  0.000000 -1.509059  5  NaN
2013-01-02  1.212112 -0.173215  0.119209  5  1.0
2013-01-03 -0.861849 -2.104569 -0.494929  5  2.0
2013-01-04  0.721555 -0.706771 -1.039575  5  3.0
2013-01-05 -0.424972  0.567020  0.276232  5  4.0
2013-01-06 -0.673690  0.113648 -1.478427  5  5.0
```
用 `where` 条件赋值：
```python
df2 = df.copy()
df2[df2 > 0] = -df2
df2 
                   A         B         C  D    F
2013-01-01  0.000000  0.000000 -1.509059 -5  NaN
2013-01-02 -1.212112 -0.173215 -0.119209 -5 -1.0
2013-01-03 -0.861849 -2.104569 -0.494929 -5 -2.0
2013-01-04 -0.721555 -0.706771 -1.039575 -5 -3.0
2013-01-05 -0.424972 -0.567020 -0.276232 -5 -4.0
2013-01-06 -0.673690 -0.113648 -1.478427 -5 -5.0
```
### 基于 `dtype` 选择列
`select_dtypes()` 方法基于 `dtype` 选择列。
首先，创建一个由多种数据类型组成的 `DataFrame`：
```python
df = pd.DataFrame({'string': list('abc'),
                   'int64': list(range(1, 4)),
                   'uint8': np.arange(3, 6).astype('u1'),
                   'float64': np.arange(4.0, 7.0),
                   'bool1': [True, False, True],
                   'bool2': [False, True, False],
                   'dates': pd.date_range('now', periods=3),
                   'category': pd.Series(list("ABC")).astype('category')})
df['tdeltas'] = df.dates.diff()
df['uint64'] = np.arange(3, 6).astype('u8')
df['other_dates'] = pd.date_range('20130101', periods=3)
df['tz_aware_dates'] = pd.date_range('20130101', periods=3, tz='US/Eastern')
df
  string  int64  uint8  float64  bool1  bool2                      dates category tdeltas  uint64 other_dates        tz_aware_dates
0      a      1      3      4.0   True  False 2019-08-22 15:49:01.870038        A     NaT       3  2013-01-01 2013-01-01 00:00:00-05:00
1      b      2      4      5.0  False   True 2019-08-23 15:49:01.870038        B  1 days       4  2013-01-02 2013-01-02 00:00:00-05:00
2      c      3      5      6.0   True  False 2019-08-24 15:49:01.870038        C  1 days       5  2013-01-03 2013-01-03 00:00:00-05:00
```
该 `DataFrame` 的数据类型：
```python
df.dtypes
string                                object
int64                                  int64
uint8                                  uint8
float64                              float64
bool1                                   bool
bool2                                   bool
dates                         datetime64[ns]
category                            category
tdeltas                      timedelta64[ns]
uint64                                uint64
other_dates                   datetime64[ns]
tz_aware_dates    datetime64[ns, US/Eastern]
dtype: object
```
`select_dtypes()` 有两个参数，`include` 与 `exclude`，用于实现“提取这些数据类型的列” （`include`）或 “提取不是这些数据类型的列”（`exclude`）。

选择 `bool` 型的列，示例如下：
```python
df.select_dtypes(include=[bool])
   bool1  bool2
0   True  False
1  False   True
2   True  False
```
该方法还支持输入 `NumPy` 数据类型的名称：
```python
df.select_dtypes(include=['bool'])
   bool1  bool2
0   True  False
1  False   True
2   True  False
```
`select_dtypes()` 还支持通用数据类型。
比如，选择所有数值型与布尔型的列，同时，排除无符号整数：
```python
df.select_dtypes(include=['number', 'bool'], exclude=['unsignedinteger'])
   int64  float64  bool1  bool2 tdeltas
0      1      4.0   True  False     NaT
1      2      5.0  False   True  1 days
2      3      6.0   True  False  1 days
```
选择字符串型的列必须要用 `object`：
```python
df.select_dtypes(include=['object'])
  string
0      a
1      b
2      c
```
要查看 `numpy.number` 等通用 `dtype` 的所有子类型，可以定义一个函数，返回子类型树：
```python
def subdtypes(dtype):
   subs = dtype.__subclasses__()
   if not subs:
      return dtype
   return [dtype, [subdtypes(dt) for dt in subs]]
```
所有 `NumPy` 数据类型都是 `numpy.generic` 的子类：
```python
subdtypes(np.generic)
[numpy.generic,
 [[numpy.number,
   [[numpy.integer,
     [[numpy.signedinteger,
       [numpy.int8,
        numpy.int16,
        numpy.int32,
        numpy.int64,
        numpy.int64,
        numpy.timedelta64]],
      [numpy.unsignedinteger,
       [numpy.uint8,
        numpy.uint16,
        numpy.uint32,
        numpy.uint64,
        numpy.uint64]]]],
    [numpy.inexact,
     [[numpy.floating,
       [numpy.float16, numpy.float32, numpy.float64, numpy.float128]],
      [numpy.complexfloating,
       [numpy.complex64, numpy.complex128, numpy.complex256]]]]]],
  [numpy.flexible,
   [[numpy.character, [numpy.bytes_, numpy.str_]],
    [numpy.void, [numpy.record]]]],
  numpy.bool_,
  numpy.datetime64,
  numpy.object_]]
```
注意:`Pandas` 支持 `category` 与 `datetime64[ns, tz]` 类型，但这两种类型未整合到 `NumPy` 架构，因此，上面的函数没有显示。
## 缺失值
`Pandas` 主要用 `np.nan` 表示缺失数据。计算时，默认不包含空值。
重建索引（`reindex`）可以更改、添加、删除指定轴的索引，并返回数据副本，即不更改原数据。

```python
df
                   A         B         C         D
2013-01-01  0.469112 -0.282863 -1.509059 -1.135632
2013-01-02  1.212112 -0.173215  0.119209 -1.044236
2013-01-03 -0.861849 -2.104569 -0.494929  1.071804
2013-01-04  0.721555 -0.706771 -1.039575  0.271860
2013-01-05 -0.424972  0.567020  0.276232 -1.087401
2013-01-06 -0.673690  0.113648 -1.478427  0.524988
df1 = df.reindex(index=dates[0:4], columns=list(df.columns) + ['E'])
df1.loc[dates[0]:dates[1], 'E'] = 1
df1
                   A         B         C  D    F    E
2013-01-01  0.000000  0.000000 -1.509059  5  NaN  1.0
2013-01-02  1.212112 -0.173215  0.119209  5  1.0  1.0
2013-01-03 -0.861849 -2.104569 -0.494929  5  2.0  NaN
2013-01-04  0.721555 -0.706771 -1.039575  5  3.0  NaN
```
删除所有含缺失值的行：
```python
df1.dropna(how='any')
                   A         B         C  D    F    E
2013-01-02  1.212112 -0.173215  0.119209  5  1.0  1.0
```
填充缺失值：
```python
df1.fillna(value=5)
                   A         B         C  D    F    E
2013-01-01  0.000000  0.000000 -1.509059  5  5.0  1.0
2013-01-02  1.212112 -0.173215  0.119209  5  1.0  1.0
2013-01-03 -0.861849 -2.104569 -0.494929  5  2.0  5.0
2013-01-04  0.721555 -0.706771 -1.039575  5  3.0  5.0
```
提取 `nan` 值的布尔掩码：
```python
pd.isna(df1)
                A      B      C      D      F      E
2013-01-01  False  False  False  False   True  False
2013-01-02  False  False  False  False  False  False
2013-01-03  False  False  False  False  False   True
2013-01-04  False  False  False  False  False   True
```
`Series` 与 `DataFrame` 的算数函数支持 `fill_value` 选项，即用指定值替换某个位置的缺失值。比如，两个 `DataFrame` 相加，除非两个 `DataFrame` 里同一个位置都有缺失值，其相加的和仍为 `NaN`，如果只有一个 `DataFrame` 里存在缺失值，则可以用 `fill_value` 指定一个值来替代 `NaN`，当然，也可以用 `fillna` 把 `NaN` 替换为想要的值。
```python
df
        one       two     three
a  1.394981  1.772517       NaN
b  0.343054  1.912123 -0.050390
c  0.695246  1.478369  1.227435
d       NaN  0.279344 -0.613172
df2 = pd.DataFrame({'one': pd.Series(np.random.randn(3), index=['a', 'b', 'c']),
                    'two': pd.Series(np.random.randn(4), index=['a', 'b', 'c', 'd']),
                    'three': pd.Series(np.random.randn(3), index=['a', 'b', 'c', 'd'])})
df2
        one       two     three
a  1.394981  1.772517  1.000000
b  0.343054  1.912123 -0.050390
c  0.695246  1.478369  1.227435
d       NaN  0.279344 -0.613172
df + df2
        one       two     three
a  2.789963  3.545034       NaN
b  0.686107  3.824246 -0.100780
c  1.390491  2.956737  2.454870
d       NaN  0.558688 -1.226343
df.add(df2, fill_value=0)
        one       two     three
a  2.789963  3.545034  1.000000
b  0.686107  3.824246 -0.100780
c  1.390491  2.956737  2.454870
d       NaN  0.558688 -1.226343
```
## 运算
### 统计
```python
df
                   A         B         C         D
2013-01-01  0.469112 -0.282863 -1.509059 -1.135632
2013-01-02  1.212112 -0.173215  0.119209 -1.044236
2013-01-03 -0.861849 -2.104569 -0.494929  1.071804
2013-01-04  0.721555 -0.706771 -1.039575  0.271860
2013-01-05 -0.424972  0.567020  0.276232 -1.087401
2013-01-06 -0.673690  0.113648 -1.478427  0.524988
```
一般情况下，运算时**排除**缺失值。
描述性统计：
```python
df.mean()
A   -0.004474
B   -0.383981
C   -0.687758
D    5.000000
F    3.000000
dtype: float64
```
在另一个轴(即，行)上执行同样的操作：
```python
df.mean(1)
2013-01-01    0.872735
2013-01-02    1.431621
2013-01-03    0.707731
2013-01-04    1.395042
2013-01-05    1.883656
2013-01-06    1.592306
Freq: D, dtype: float64
```
不同维度对象运算时，要先对齐。 此外，`Pandas` 自动沿指定维度广播。
```python
s = pd.Series([1, 3, 5, np.nan, 6, 8], index=dates).shift(2)
s
2013-01-01    NaN
2013-01-02    NaN
2013-01-03    1.0
2013-01-04    3.0
2013-01-05    5.0
2013-01-06    NaN
Freq: D, dtype: float64
df.sub(s, axis='index')
                   A         B         C    D    F
2013-01-01       NaN       NaN       NaN  NaN  NaN
2013-01-02       NaN       NaN       NaN  NaN  NaN
2013-01-03 -1.861849 -3.104569 -1.494929  4.0  1.0
2013-01-04 -2.278445 -3.706771 -4.039575  2.0  0.0
2013-01-05 -5.424972 -4.432980 -4.723768  0.0 -1.0
2013-01-06       NaN       NaN       NaN  NaN  NaN
```
### `Apply` 函数
`Apply` 函数处理数据：
```python
df.apply(np.cumsum)
                   A         B         C   D     F
2013-01-01  0.000000  0.000000 -1.509059   5   NaN
2013-01-02  1.212112 -0.173215 -1.389850  10   1.0
2013-01-03  0.350263 -2.277784 -1.884779  15   3.0
2013-01-04  1.071818 -2.984555 -2.924354  20   6.0
2013-01-05  0.646846 -2.417535 -2.648122  25  10.0
2013-01-06 -0.026844 -2.303886 -4.126549  30  15.0
df.apply(lambda x: x.max() - x.min())
A    2.073961
B    2.671590
C    1.785291
D    0.000000
F    4.000000
dtype: float64
```
### 直方图
```python
s = pd.Series(np.random.randint(0, 7, size=10))
s
0    4
1    2
2    1
3    2
4    6
5    4
6    4
7    6
8    4
9    4
dtype: int64
s.value_counts()
4    5
6    2
2    2
1    1
dtype: int64
```
### 字符串方法
`Series` 的 `str` 属性包含一组字符串处理功能，如下列代码所示。注意，`str` 的模式匹配默认使用正则表达式。
```python
s = pd.Series(['A', 'B', 'C', 'Aaba', 'Baca', np.nan, 'CABA', 'dog', 'cat'])
s.str.lower()
0       a
1       b
2       c
3    aaba
4    baca
5     NaN
6    caba
7     dog
8     cat
dtype: object
```
## 合并（`Merge`）
### 结合（`Concat`）
`Pandas` 提供了多种将 `Series`、`DataFrame` 对象组合在一起的功能，用索引与关联代数功能的多种设置逻辑可执行连接（`join`）与合并（`merge`）操作。
`concat()` 用于连接 `Pandas` 对象：
```python
df = pd.DataFrame(np.random.randn(10, 4))
df
          0         1         2         3
0 -0.548702  1.467327 -1.015962 -0.483075
1  1.637550 -1.217659 -0.291519 -1.745505
2 -0.263952  0.991460 -0.919069  0.266046
3 -0.709661  1.669052  1.037882 -1.705775
4 -0.919854 -0.042379  1.247642 -0.009920
5  0.290213  0.495767  0.362949  1.548106
6 -1.131345 -0.089329  0.337863 -0.945867
7 -0.932132  1.956030  0.017587 -0.016692
8 -0.575247  0.254161 -1.143704  0.215897
9  1.193555 -0.077118 -0.408530 -0.862495
# 分解为多组
pieces = [df[:3], df[3:7], df[7:]]
pd.concat(pieces)
          0         1         2         3
0 -0.548702  1.467327 -1.015962 -0.483075
1  1.637550 -1.217659 -0.291519 -1.745505
2 -0.263952  0.991460 -0.919069  0.266046
3 -0.709661  1.669052  1.037882 -1.705775
4 -0.919854 -0.042379  1.247642 -0.009920
5  0.290213  0.495767  0.362949  1.548106
6 -1.131345 -0.089329  0.337863 -0.945867
7 -0.932132  1.956030  0.017587 -0.016692
8 -0.575247  0.254161 -1.143704  0.215897
9  1.193555 -0.077118 -0.408530 -0.862495
```
### 连接（`join`）
`SQL` 风格的合并。 
```python
left = pd.DataFrame({'key': ['foo', 'foo'], 'lval': [1, 2]})
right = pd.DataFrame({'key': ['foo', 'foo'], 'rval': [4, 5]})
left
   key  lval
0  foo     1
1  foo     2
right
   key  rval
0  foo     4
1  foo     5
pd.merge(left, right, on='key')
   key  lval  rval
0  foo     1     4
1  foo     1     5
2  foo     2     4
3  foo     2     5
```
这里还有一个例子：
```python
left = pd.DataFrame({'key': ['foo', 'bar'], 'lval': [1, 2]})
right = pd.DataFrame({'key': ['foo', 'bar'], 'rval': [4, 5]})
left
   key  lval
0  foo     1
1  bar     2
right
   key  rval
0  foo     4
1  bar     5
pd.merge(left, right, on='key')
   key  lval  rval
0  foo     1     4
1  bar     2     5
```
### 追加（`Append`）
为 DataFrame 追加行。
```python
df = pd.DataFrame(np.random.randn(8, 4), columns=['A', 'B', 'C', 'D'])
df
          A         B         C         D
0  1.346061  1.511763  1.627081 -0.990582
1 -0.441652  1.211526  0.268520  0.024580
2 -1.577585  0.396823 -0.105381 -0.532532
3  1.453749  1.208843 -0.080952 -0.264610
4 -0.727965 -0.589346  0.339969 -0.693205
5 -0.339355  0.593616  0.884345  1.591431
6  0.141809  0.220390  0.435589  0.192451
7 -0.096701  0.803351  1.715071 -0.708758
s = df.iloc[3]
df.append(s, ignore_index=True)
          A         B         C         D
0  1.346061  1.511763  1.627081 -0.990582
1 -0.441652  1.211526  0.268520  0.024580
2 -1.577585  0.396823 -0.105381 -0.532532
3  1.453749  1.208843 -0.080952 -0.264610
4 -0.727965 -0.589346  0.339969 -0.693205
5 -0.339355  0.593616  0.884345  1.591431
6  0.141809  0.220390  0.435589  0.192451
7 -0.096701  0.803351  1.715071 -0.708758
8  1.453749  1.208843 -0.080952 -0.264610
```
## 分组（`Grouping`）
`group by` 指的是涵盖下列一项或多项步骤的处理流程：
- 分割：按条件把数据分割成多组；
- 应用：为每组单独应用函数；
- 组合：将处理结果组合成一个数据结构。
```python
df = pd.DataFrame({'A': ['foo', 'bar', 'foo', 'bar','foo', 'bar', 'foo', 'foo'],
                   'B': ['one', 'one', 'two', 'three','two', 'two', 'one', 'three'],
                   'C': np.random.randn(8),
                   'D': np.random.randn(8)})
df
     A      B         C         D
0  foo    one -1.202872 -0.055224
1  bar    one -1.814470  2.395985
2  foo    two  1.018601  1.552825
3  bar  three -0.595447  0.166599
4  foo    two  1.395433  0.047609
5  bar    two -0.392670 -0.136473
6  foo    one  0.007207 -0.561757
7  foo  three  1.928123 -1.623033
```
先分组，再用 `sum()`函数计算每组的汇总数据：
```python
df.groupby('A').sum()
            C        D
A                     
bar -2.802588  2.42611
foo  3.146492 -0.63958
```
多列分组后，生成多层索引，也可以应用 `sum` 函数：
```python
df.groupby(['A', 'B']).sum()
                  C         D
A   B                        
bar one   -1.814470  2.395985
    three -0.595447  0.166599
    two   -0.392670 -0.136473
foo one   -1.195665 -0.616981
    three  1.928123 -1.623033
    two    2.414034  1.600434
```
重塑（`Reshaping`）

### 堆叠（`Stack`）
```python
tuples = list(zip(*[['bar', 'bar', 'baz', 'baz',
                     'foo', 'foo', 'qux', 'qux'],
                    ['one', 'two', 'one', 'two',
                     'one', 'two', 'one', 'two']]))
index = pd.MultiIndex.from_tuples(tuples, names=['first', 'second'])
df = pd.DataFrame(np.random.randn(8, 2), index=index, columns=['A', 'B'])
df2 = df[:4]
df2
                     A         B
first second                    
bar   one     1.624345 -0.611756
      two    -0.528172 -1.072969
baz   one     0.865408 -2.301539
      two     1.744812 -0.761207
```
`stack()`方法把 `DataFrame` 列压缩至一层：
```python
stacked = df2.stack()
stacked
first  second   
bar    one     A    1.624345
               B   -0.611756
       two     A   -0.528172
               B   -1.072969
baz    one     A    0.865408
               B   -2.301539
       two     A    1.744812
               B   -0.76
```
压缩后的 `DataFrame` 或 `Series` 具有多层索引， `stack()` 的逆操作是 `unstack()`，默认为拆叠最后一层：
```python
stacked.unstack()
                     A         B
first second                    
bar   one     1.624345 -0.611756
      two    -0.528172 -1.072969
baz   one     0.865408 -2.301539
      two     1.744812 -0.761207
stacked.unstack(1)
second        one       two
first                      
bar   A  1.624345 -0.528172
      B -0.611756 -1.072969
baz   A  0.865408  1.744812
      B -2.301539 -0.761207
stacked.unstack(0)
first          bar       baz
second                      
one    A  1.624345  0.865408
       B -0.611756 -2.301539
two    A -0.528172  1.744812
       B -1.072969 -0.761207
```
## 数据透视表（`Pivot Tables`）
```python
df = pd.DataFrame({'A': ['one', 'one', 'two', 'three'] * 3,
                   'B': ['A', 'B', 'C'] * 4,
                   'C': ['foo', 'foo', 'foo', 'bar', 'bar', 'bar'] * 2,
                   'D': np.random.randn(12),
                   'E': np.random.randn(12)})
df
        A  B    C         D         E
0     one  A  foo  1.418757 -0.179666
1     one  B  foo -1.879024  1.291836
2     two  C  foo  0.536826 -0.009614
3   three  A  bar  1.006160  0.392149
4     one  B  bar -0.029716  0.264599
5     one  C  bar -1.146178 -0.057409
6     two  A  foo  0.100900 -1.425638
7   three  B  foo -1.035018  1.024098
8     one  C  foo  0.314665 -0.106062
9     one  A  bar -0.773723  1.824375
10    two  B  bar -1.170653  0.595974
11  three  C  bar  0.648740  1.167115
```
用上述数据生成数据透视表非常简单：
```python
pd.pivot_table(df, values='D', index=['A', 'B'], columns=['C'])
C             bar       foo
A     B                    
one   A -0.773723  1.418757
      B -0.029716 -1.879024
      C -1.146178  0.314665
three A  1.006160       NaN
      B       NaN -1.035018
      C  0.648740       NaN
two   A       NaN  0.100900
      B -1.170653       NaN
      C       NaN  0.536826
```
## 时间序列(`TimeSeries`)
`Pandas` 为频率转换时重采样提供了虽然简单易用，但强大高效的功能，如，将秒级的数据转换为 5 分钟为频率的数据。这种操作常见于财务应用程序，但又不仅限于此。
```python
rng = pd.date_range('1/1/2012', periods=100, freq='S')
ts = pd.Series(np.random.randint(0, 500, len(rng)), index=rng)
ts.resample('5Min').sum() 
2012-01-01    25083
Freq: 5T, dtype: int64
```
时区表示：
```python
rng = pd.date_range('3/6/2012 00:00', periods=5, freq='D')
ts = pd.Series(np.random.randn(len(rng)), rng)
ts 
2012-03-06    0.464000
2012-03-07    0.227371
2012-03-08   -0.496922
2012-03-09    0.306389
2012-03-10   -2.290613
Freq: D, dtype: float64
ts_utc = ts.tz_localize('UTC')
ts_utc
2012-03-06 00:00:00+00:00    0.464000
2012-03-07 00:00:00+00:00    0.227371
2012-03-08 00:00:00+00:00   -0.496922
2012-03-09 00:00:00+00:00    0.306389
2012-03-10 00:00:00+00:00   -2.290613
Freq: D, dtype: float64
```
转换成其它时区：
```python
ts_utc.tz_convert('US/Eastern')

2012-03-05 19:00:00-05:00    0.464000
2012-03-06 19:00:00-05:00    0.227371
2012-03-07 19:00:00-05:00   -0.496922
2012-03-08 19:00:00-05:00    0.306389
2012-03-09 19:00:00-05:00   -2.290613
Freq: D, dtype: float64
```
转换时间段：
```python
rng = pd.date_range('1/1/2012', periods=5, freq='M')
ts = pd.Series(np.random.randn(len(rng)), index=rng)
ts
2012-01-31   -1.134623
2012-02-29   -1.561819
2012-03-31   -0.260838
2012-04-30    0.281957
2012-05-31    1.523962
Freq: M, dtype: float64
ps = ts.to_period()
ps
2012-01   -1.134623
2012-02   -1.561819
2012-03   -0.260838
2012-04    0.281957
2012-05    1.523962
Freq: M, dtype: float64
ps.to_timestamp()
2012-01-01   -1.134623
2012-02-01   -1.561819
2012-03-01   -0.260838
2012-04-01    0.281957
2012-05-01    1.523962
Freq: MS, dtype: float64
```
`Pandas` 函数可以很方便地转换时间段与时间戳。下例把以 11 月为结束年份的季度频率转换为下一季度月末上午 9 点：
```python
prng = pd.period_range('1990Q1', '2000Q4', freq='Q-NOV')
ts = pd.Series(np.random.randn(len(prng)), prng)
ts.index = (prng.asfreq('M', 'e') + 1).asfreq('H', 's') + 9
ts.head()
1990-03-01 09:00   -0.902937
1990-06-01 09:00    0.068159
1990-09-01 09:00   -0.057873
1990-12-01 09:00   -0.368204
1991-03-01 09:00   -1.144073
Freq: H, dtype: float64
```
## 类别型（`Categoricals`）
`Pandas` 的 `DataFrame` 里可以包含类别数据。
```python
df = pd.DataFrame({"id": [1, 2, 3, 4, 5, 6],
                   "raw_grade": ['a', 'b', 'b', 'a', 'a', 'e']})
```
将 `grade` 的原生数据转换为类别型数据：
```python
df["grade"] = df["raw_grade"].astype("category")
df["grade"]
0    a
1    b
2    b
3    a
4    a
5    e
Name: grade, dtype: category
Categories (3, object): [a, b, e]
```
用有含义的名字重命名不同类型，调用 `Series.cat.categories`。
```python
df["grade"].cat.categories = ["very good", "good", "very bad"]
```
重新排序各类别，并添加缺失类，`Series.cat` 的方法默认返回新 `Series`。
```python
df["grade"] = df["grade"].cat.set_categories(["very bad", "bad", "medium","good", "very good"]) 
df["grade"]
0    very good
1         good
2         good
3    very good
4    very good
5     very bad
Name: grade, dtype: category
Categories (5, object): [very bad, bad, medium, good, very good]
```
注意，这里是按生成类别时的顺序排序，不是按词汇排序：
```python
df.sort_values(by="grade")
   id raw_grade      grade
5   6         e   very bad
1   2         b       good
2   3         b       good
0   1         a  very good
3   4         a  very good
4   5         a  very good
```
按类列分组（`groupby`）时，即便某类别为空，也会显示：
```python
df.groupby("grade").size()
grade
very bad     1
bad          0
medium       0
good         2
very good    3
dtype: int64
```
## 可视化
```python
ts = pd.Series(np.random.randn(1000),index=pd.date_range('1/1/2000', periods=1000))
ts = ts.cumsum()
ts.plot()
<matplotlib.axes._subplots.AxesSubplot at 0x7f2b5771ac88>
```
`DataFrame` 的 `plot()` 方法可以快速绘制所有带标签的列：
```python
df = pd.DataFrame(np.random.randn(1000, 4), index=ts.index,columns=['A', 'B', 'C', 'D'])
df = df.cumsum()
plt.figure()
<Figure size 640x480 with 0 Axes>
df.plot()
<matplotlib.axes._subplots.AxesSubplot at 0x7f2b53a2d7f0>
plt.legend(loc='best')
<matplotlib.legend.Legend at 0x7f2b539728d0>
```
## 数据输入 / 输出
### `CSV`
写入 `CSV` 文件。
```python
df.to_csv('foo.csv')
```
读取 `CSV` 文件数据：
```python
pd.read_csv('foo.csv')
     Unnamed: 0          A          B         C          D
0    2000-01-01   0.266457  -0.399641 -0.219582   1.186860
1    2000-01-02  -1.170732  -0.345873  1.653061  -0.282953
2    2000-01-03  -1.734933   0.530468  2.060811  -0.515536
3    2000-01-04  -1.555121   1.452620  0.239859  -1.156896
4    2000-01-05   0.578117   0.511371  0.103552  -2.428202
5    2000-01-06   0.478344   0.449933 -0.741620  -1.962409
6    2000-01-07   1.235339  -0.091757 -1.543861  -1.084753
..          ...        ...        ...       ...        ...
993  2002-09-20 -10.628548  -9.153563 -7.883146  28.313940
994  2002-09-21 -10.390377  -8.727491 -6.399645  30.914107
995  2002-09-22  -8.985362  -8.485624 -4.669462  31.367740
996  2002-09-23  -9.558560  -8.781216 -4.499815  30.518439
997  2002-09-24  -9.902058  -9.340490 -4.386639  30.105593
998  2002-09-25 -10.216020  -9.480682 -3.933802  29.758560
999  2002-09-26 -11.856774 -10.671012 -3.216025  29.369368

[1000 rows x 5 columns]
```
### `HDF5`
写入 `HDF5 Store`：
```python
df.to_hdf('foo.h5', 'df')
```
读取 `HDF5 Store`：
```python
pd.read_hdf('foo.h5', 'df')
                    A          B         C          D
2000-01-01   0.266457  -0.399641 -0.219582   1.186860
2000-01-02  -1.170732  -0.345873  1.653061  -0.282953
2000-01-03  -1.734933   0.530468  2.060811  -0.515536
2000-01-04  -1.555121   1.452620  0.239859  -1.156896
2000-01-05   0.578117   0.511371  0.103552  -2.428202
2000-01-06   0.478344   0.449933 -0.741620  -1.962409
2000-01-07   1.235339  -0.091757 -1.543861  -1.084753
...               ...        ...       ...        ...
2002-09-20 -10.628548  -9.153563 -7.883146  28.313940
2002-09-21 -10.390377  -8.727491 -6.399645  30.914107
2002-09-22  -8.985362  -8.485624 -4.669462  31.367740
2002-09-23  -9.558560  -8.781216 -4.499815  30.518439
2002-09-24  -9.902058  -9.340490 -4.386639  30.105593
2002-09-25 -10.216020  -9.480682 -3.933802  29.758560
2002-09-26 -11.856774 -10.671012 -3.216025  29.369368

[1000 rows x 4 columns]
```
## 各种坑（`Gotchas`）
执行某些操作，将触发异常，如:
```python
pd.Series([False, True, False]):
print("I was true")
Traceback
ValueError: The truth value of an array is ambiguous. Use a.empty, a.any() or a.all().
```
## 与`SQL`比较
按照惯例，我们按如下方式导入 `pandas` 和 `NumPy`：
```python
import pandas as pd
import numpy as np
```
大多数示例将使用`tipspandas`测试中找到的数据集。我们将数据读入名为`tips`的`DataFrame`中，并假设我们有一个具有相同名称和结构的数据库表。
```python
url = ('https://raw.github.com/pandas-dev/pandas/master/pandas/tests/data/tips.csv')
tips = pd.read_csv(url)
tips.head()
   total_bill   tip     sex smoker  day    time  size
0       16.99  1.01  Female     No  Sun  Dinner     2
1       10.34  1.66    Male     No  Sun  Dinner     3
2       21.01  3.50    Male     No  Sun  Dinner     3
3       23.68  3.31    Male     No  Sun  Dinner     2
4       24.59  3.61  Female     No  Sun  Dinner     4
```
### `SELECT`
在`SQL`中，使用您要选择的以逗号分隔的列列表（或`*` 选择所有列）来完成选择：
```sql
SELECT total_bill, tip, smoker, time
FROM tips
LIMIT 5;
```
使用`pandas`，通过将列名列表传递给`DataFrame`来完成列选择：
```python
tips[['total_bill', 'tip', 'smoker', 'time']].head(5)
   total_bill   tip smoker    time
0       16.99  1.01     No  Dinner
1       10.34  1.66     No  Dinner
2       21.01  3.50     No  Dinner
3       23.68  3.31     No  Dinner
4       24.59  3.61     No  Dinner
```
在没有列名列表的情况下调用`DataFrame`将显示所有列（类似于`SQL*`）。

### `WHERE`
`SQL`中的过滤是通过`WHERE`子句完成的。
```sql
SELECT *
FROM tips
WHERE time = 'Dinner'
LIMIT 5;
```
`DataFrame`可以通过多种方式进行过滤; 最直观的是使用 布尔索引。
```python
tips[tips['time'] == 'Dinner'].head(5)
   total_bill   tip     sex smoker  day    time  size
0       16.99  1.01  Female     No  Sun  Dinner     2
1       10.34  1.66    Male     No  Sun  Dinner     3
2       21.01  3.50    Male     No  Sun  Dinner     3
3       23.68  3.31    Male     No  Sun  Dinner     2
4       24.59  3.61  Female     No  Sun  Dinner     4
```
上面的语句只是将一个 `Series` 的 `True` / `False` 对象传递给 `DataFrame`，返回所有带有`True`的行。
```python
is_dinner = tips['time'] == 'Dinner'
is_dinner.value_counts()
True     176
False     68
Name: time, dtype: int64
tips[is_dinner].head(5)
   total_bill   tip     sex smoker  day    time  size
0       16.99  1.01  Female     No  Sun  Dinner     2
1       10.34  1.66    Male     No  Sun  Dinner     3
2       21.01  3.50    Male     No  Sun  Dinner     3
3       23.68  3.31    Male     No  Sun  Dinner     2
4       24.59  3.61  Female     No  Sun  Dinner     4
```
就像`SQL`的`OR`和`AND`一样，可以使用`|`将多个条件传递给`DataFrame` （`OR`）和`＆`（`AND`）。

-- tips of more than $5.00 at Dinner meals
```sql
SELECT *
FROM tips
WHERE time = 'Dinner' AND tip > 5.00;
```
tips of more than $5.00 at Dinner meals
```python
tips[(tips['time'] == 'Dinner') & (tips['tip'] > 5.00)] 
     total_bill    tip     sex smoker  day    time  size
23        39.42   7.58    Male     No  Sat  Dinner     4
44        30.40   5.60    Male     No  Sun  Dinner     4
47        32.40   6.00    Male     No  Sun  Dinner     4
52        34.81   5.20  Female     No  Sun  Dinner     4
59        48.27   6.73    Male     No  Sat  Dinner     4
116       29.93   5.07    Male     No  Sun  Dinner     4
155       29.85   5.14  Female     No  Sun  Dinner     5
170       50.81  10.00    Male    Yes  Sat  Dinner     3
172        7.25   5.15    Male    Yes  Sun  Dinner     2
181       23.33   5.65    Male    Yes  Sun  Dinner     2
183       23.17   6.50    Male    Yes  Sun  Dinner     4
211       25.89   5.16    Male    Yes  Sat  Dinner     4
212       48.33   9.00    Male     No  Sat  Dinner     4
214       28.17   6.50  Female    Yes  Sat  Dinner     3
239       29.03   5.92    Male     No  Sat  Dinner     3
```
-- tips by parties of at least 5 diners OR bill total was more than $45
```sql
SELECT *
FROM tips
WHERE size >= 5 OR total_bill > 45;
```
tips by parties of at least 5 diners OR bill total was more than $45
```python
tips[(tips['size'] >= 5) | (tips['total_bill'] > 45)]
     total_bill    tip     sex smoker   day    time  size
59        48.27   6.73    Male     No   Sat  Dinner     4
125       29.80   4.20  Female     No  Thur   Lunch     6
141       34.30   6.70    Male     No  Thur   Lunch     6
142       41.19   5.00    Male     No  Thur   Lunch     5
143       27.05   5.00  Female     No  Thur   Lunch     6
155       29.85   5.14  Female     No   Sun  Dinner     5
156       48.17   5.00    Male     No   Sun  Dinner     6
170       50.81  10.00    Male    Yes   Sat  Dinner     3
182       45.35   3.50    Male    Yes   Sun  Dinner     3
185       20.69   5.00    Male     No   Sun  Dinner     5
187       30.46   2.00    Male    Yes   Sun  Dinner     5
212       48.33   9.00    Male     No   Sat  Dinner     4
216       28.15   3.00    Male    Yes   Sat  Dinner     5
```
使用`notna()`和`isna()` 方法完成`NULL`检查。
```python
frame = pd.DataFrame({'col1': ['A', 'B', np.NaN, 'C', 'D'],
                      'col2': ['F', np.NaN, 'G', 'H', 'I']}) 
frame
  col1 col2
0    A    F
1    B  NaN
2  NaN    G
3    C    H
4    D    I
```
假设我们有一个与上面的`DataFrame`结构相同的表。我们只能`col2`通过以下查询看到`IS NULL` 的记录：
```sql
SELECT *
FROM frame
WHERE col2 IS NULL;
```
```python
frame[frame['col2'].isna()]
  col1 col2
1    B  NaN
```
获取`col1 IS NOT NULL`的项目可以完成`notna()`。
```sql
SELECT *
FROM frame
WHERE col1 IS NOT NULL;
```
```python
frame[frame['col1'].notna()]
  col1 col2
0    A    F
1    B  NaN
3    C    H
4    D    I
```
### `GROUP BY`
在`pandas`中，`SQL`的`GROUP BY`操作使用类似命名的 `groupby()`方法执行。`groupby()`通常是指我们想要将数据集拆分成组，应用某些功能（通常是聚合），然后将这些组合在一起的过程。

常见的`SQL`操作是获取整个数据集中每个组中的记录数。例如，有一个需要向我们提供提示中的性别的数量的查询语句：
```sql
SELECT sex, count(*)
FROM tips
GROUP BY sex;
/*
Female     87
Male      157
*/
```
在 `pandas` 中可以这样：
```python
tips.groupby('sex').size() 
sex
Female     87
Male      157
dtype: int64
```
请注意，在我们使用的`pandas`代码中`size()`，没有 `count()`。这是因为 `count()`将函数应用于每个列，返回每个列中的记录数。
```python
tips.groupby('sex').count()
        total_bill  tip  smoker  day  time  size
sex                                             
Female          87   87      87   87    87    87
Male           157  157     157  157   157   157
```
或者，我们可以将该`count()`方法应用于单个列：
```python
tips.groupby('sex')['total_bill'].count()
sex
Female     87
Male      157
Name: total_bill, dtype: int64
```
也可以一次应用多个功能。例如，假设我们希望查看提示量与星期几的不同之处 - `agg()`允许您将字典传递给分组的`DataFrame`，指示要应用于特定列的函数。
```sql
SELECT day, AVG(tip), COUNT(*)
FROM tips
GROUP BY day;
/*
Fri   2.734737   19
Sat   2.993103   87
Sun   3.255132   76
Thur  2.771452   62
*/
```
```python
tips.groupby('day').agg({'tip': np.mean, 'day': np.size})
           tip  day
day                
Fri   2.734737   19
Sat   2.993103   87
Sun   3.255132   76
Thur  2.771452   62
```
通过将列列表传递给`groupby()`方法来完成多个列的分组 。
```sql
SELECT smoker, day, COUNT(*), AVG(tip)
FROM tips
GROUP BY smoker, day;
/*
smoker day
No     Fri      4  2.812500
       Sat     45  3.102889
       Sun     57  3.167895
       Thur    45  2.673778
Yes    Fri     15  2.714000
       Sat     42  2.875476
       Sun     19  3.516842
       Thur    17  3.030000
*/
```
```python
tips.groupby(['smoker', 'day']).agg({'tip': [np.size, np.mean]})
              tip          
             size      mean
smoker day                 
No     Fri    4.0  2.812500
       Sat   45.0  3.102889
       Sun   57.0  3.167895
       Thur  45.0  2.673778
Yes    Fri   15.0  2.714000
       Sat   42.0  2.875476
       Sun   19.0  3.516842
       Thur  17.0  3.030000
```
### `JOIN`
可以使用`join()`或执行`JOIN merge()`。默认情况下， `join()`将在其索引上加入`DataFrame`。每个方法都有参数，允许您指定要执行的连接类型（`LEFT`，`RIGHT`，`INNER`，`FULL`）或要连接的列（列名称或索引）。
```python
df1 = pd.DataFrame({'key': ['A', 'B', 'C', 'D'],
                    'value': np.random.randn(4)})
df2 = pd.DataFrame({'key': ['B', 'D', 'D', 'E'],
                    'value': np.random.randn(4)})
```
假设我们有两个与`DataFrames`名称和结构相同的数据库表。

现在让我们来看看各种类型的`JOIN`。
```sql
#INNER JOIN
SELECT *
FROM df1
INNER JOIN df2
  ON df1.key = df2.key;
```
```python
#merge performs an INNER JOIN by default
pd.merge(df1, df2, on='key')
  key   value_x   value_y
0   B -0.282863  1.212112
1   D -1.135632 -0.173215
2   D -1.135632  0.119209
```
`merge()` 当您想要将`一个DataFrame`列与另一个`DataFrame`索引连接时，还会为这些情况提供参数。
```python
indexed_df2 = df2.set_index('key')
pd.merge(df1, indexed_df2, left_on='key', right_index=True)
  key   value_x   value_y
1   B -0.282863  1.212112
3   D -1.135632 -0.173215
3   D -1.135632  0.119209
```
### `LEFT OUTER JOIN`
-- show all records from df1
```sql
SELECT *
FROM df1
LEFT OUTER JOIN df2
  ON df1.key = df2.key;
```
```python
#show all records from df1
pd.merge(df1, df2, on='key', how='left')
  key   value_x   value_y
0   A  0.469112       NaN
1   B -0.282863  1.212112
2   C -1.509059       NaN
3   D -1.135632 -0.173215
4   D -1.135632  0.119209
```
### `RIGHT JOIN`
-- show all records from df2
```sql
SELECT *
FROM df1
RIGHT OUTER JOIN df2
  ON df1.key = df2.key;
```
```python
# show all records from df2
pd.merge(df1, df2, on='key', how='right')
  key   value_x   value_y
0   B -0.282863  1.212112
1   D -1.135632 -0.173215
2   D -1.135632  0.119209
3   E       NaN -1.044236
```
### `FULL JOIN`
`pandas`还允许显示数据集两侧的`FULL JOIN`，无论连接列是否找到匹配项。在编写时，所有`RDBMS`（`MySQL`）都不支持`FULL JOIN`。

-- show all records from both tables
```sql
SELECT *
FROM df1
FULL OUTER JOIN df2
  ON df1.key = df2.key;
```
```python
#show all records from both frames
pd.merge(df1, df2, on='key', how='outer')
  key   value_x   value_y
0   A  0.469112       NaN
1   B -0.282863  1.212112
2   C -1.509059       NaN
3   D -1.135632 -0.173215
4   D -1.135632  0.119209
5   E       NaN -1.044236
```
### `UNION`
`UNION ALL`可以使用`concat()`。
```python
df1 = pd.DataFrame({'city': ['Chicago', 'San Francisco', 'New York City'],
                    'rank': range(1, 4)})
df2 = pd.DataFrame({'city': ['Chicago', 'Boston', 'Los Angeles'],
                    'rank': [1, 4, 5]})
```
```sql
SELECT city, rank
FROM df1
UNION ALL
SELECT city, rank
FROM df2;
/*
         city  rank
      Chicago     1
San Francisco     2
New York City     3
      Chicago     1
       Boston     4
  Los Angeles     5
*/
```
```python
pd.concat([df1, df2])
            city  rank
0        Chicago     1
1  San Francisco     2
2  New York City     3
0        Chicago     1
1         Boston     4
2    Los Angeles     5
```
`SQL`的`UNION`类似于`UNION ALL`，但是`UNION`将删除重复的行。
```sql
SELECT city, rank
FROM df1
UNION
SELECT city, rank
FROM df2;
-- notice that there is only one Chicago record this time
/*
         city  rank
      Chicago     1
San Francisco     2
New York City     3
       Boston     4
  Los Angeles     5
*/
```
在 `pandas` 中，您可以`concat()`结合使用 `drop_duplicates()`。
```python
pd.concat([df1, df2]).drop_duplicates()
            city  rank
0        Chicago     1
1  San Francisco     2
2  New York City     3
1         Boston     4
2    Los Angeles     5
```
### `Pandas`等同于某些`SQL`分析和聚合函数
#### 带有偏移量的前N行
```sql
-- MySQL
SELECT * FROM tips
ORDER BY tip DESC
LIMIT 10 OFFSET 5;
```
```python
tips.nlargest(10 + 5, columns='tip').tail(10)
     total_bill   tip     sex smoker   day    time  size
183       23.17  6.50    Male    Yes   Sun  Dinner     4
214       28.17  6.50  Female    Yes   Sat  Dinner     3
47        32.40  6.00    Male     No   Sun  Dinner     4
239       29.03  5.92    Male     No   Sat  Dinner     3
88        24.71  5.85    Male     No  Thur   Lunch     2
181       23.33  5.65    Male    Yes   Sun  Dinner     2
44        30.40  5.60    Male     No   Sun  Dinner     4
52        34.81  5.20  Female     No   Sun  Dinner     4
85        34.83  5.17  Female     No  Thur   Lunch     4
211       25.89  5.16    Male    Yes   Sat  Dinner     4
```
#### 每组前`N`行
```sql
-- Oracle's ROW_NUMBER() analytic function
SELECT * FROM (
  SELECT
    t.*,
    ROW_NUMBER() OVER(PARTITION BY day ORDER BY total_bill DESC) AS rn
  FROM tips t
)
WHERE rn < 3
ORDER BY day, rn;
```
```python
(tips.assign(rn=tips.sort_values(['total_bill'], ascending=False).groupby(['day']).cumcount() + 1).query('rn < 3').sort_values(['day', 'rn']))
     total_bill    tip     sex smoker   day    time  size  rn
95        40.17   4.73    Male    Yes   Fri  Dinner     4   1
90        28.97   3.00    Male    Yes   Fri  Dinner     2   2
170       50.81  10.00    Male    Yes   Sat  Dinner     3   1
212       48.33   9.00    Male     No   Sat  Dinner     4   2
156       48.17   5.00    Male     No   Sun  Dinner     6   1
182       45.35   3.50    Male    Yes   Sun  Dinner     3   2
197       43.11   5.00  Female    Yes  Thur   Lunch     4   1
142       41.19   5.00    Male     No  Thur   Lunch     5   2
```
同样使用 `rank` (`method ='first'`) 函数
```python
(tips.assign(rnk=tips.groupby(['day'])['total_bill'].rank(method='first', ascending=False)).query('rnk < 3').sort_values(['day', 'rnk']))
     total_bill    tip     sex smoker   day    time  size  rnk
95        40.17   4.73    Male    Yes   Fri  Dinner     4  1.0
90        28.97   3.00    Male    Yes   Fri  Dinner     2  2.0
170       50.81  10.00    Male    Yes   Sat  Dinner     3  1.0
212       48.33   9.00    Male     No   Sat  Dinner     4  2.0
156       48.17   5.00    Male     No   Sun  Dinner     6  1.0
182       45.35   3.50    Male    Yes   Sun  Dinner     3  2.0
197       43.11   5.00  Female    Yes  Thur   Lunch     4  1.0
142       41.19   5.00    Male     No  Thur   Lunch     5  2.0
```
```sql
-- Oracle's RANK() analytic function
SELECT * FROM (
  SELECT
    t.*,
    RANK() OVER(PARTITION BY sex ORDER BY tip) AS rnk
  FROM tips t
  WHERE tip < 2
)
WHERE rnk < 3
ORDER BY sex, rnk;
```
让我们找到每个性别组（等级<3）的提示（提示<2）。请注意，使用`rank(method='min')`函数时 `rnk_min`对于相同的提示保持不变 （如`Oracle`的`RANK()`函数）
```python
(tips[tips['tip'] < 2].assign(rnk_min=tips.groupby(['sex'])['tip'].rank(method='min')).query('rnk_min < 3').sort_values(['sex', 'rnk_min']))
     total_bill   tip     sex smoker  day    time  size  rnk_min
67         3.07  1.00  Female    Yes  Sat  Dinner     1      1.0
92         5.75  1.00  Female    Yes  Fri  Dinner     2      1.0
111        7.25  1.00  Female     No  Sat  Dinner     1      1.0
236       12.60  1.00    Male    Yes  Sat  Dinner     2      1.0
237       32.83  1.17    Male    Yes  Sat  Dinner     2      2.0
```
### 更新（`UPDATE`）
```sql
UPDATE tips
SET tip = tip*2
WHERE tip < 2;
```
```python
tips.loc[tips['tip'] < 2, 'tip'] *= 2
```
### 删除（`DELETE`）
```sql
DELETE FROM tips
WHERE tip > 9;
```
在`pandas`中，我们选择应保留的行，而不是删除它们
```python
tips = tips.loc[tips['tip'] <= 9]
```
