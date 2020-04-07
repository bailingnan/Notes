<!-- TOC -->

- [利用`Python`进行数据分析](#利用python进行数据分析)
  - [入门](#入门)
    - [数据结构介绍](#数据结构介绍)
      - [`Series`](#series)
      - [`DataFrame`](#dataframe)
      - [索引对象](#索引对象)
    - [基本功能](#基本功能)
      - [重新索引](#重新索引)
      - [丢弃指定轴上的项](#丢弃指定轴上的项)
      - [索引、选取和过滤](#索引选取和过滤)
      - [用`loc`和`iloc`进行选取](#用loc和iloc进行选取)
      - [整数索引](#整数索引)
      - [算术运算和数据对齐](#算术运算和数据对齐)
      - [在算术方法中填充值](#在算术方法中填充值)
      - [`DataFrame`和`Series`之间的运算](#dataframe和series之间的运算)
      - [函数应用和映射](#函数应用和映射)
      - [排序和排名](#排序和排名)
      - [带有重复标签的轴索引](#带有重复标签的轴索引)
    - [汇总和计算描述统计](#汇总和计算描述统计)
      - [相关系数与协方差](#相关系数与协方差)
      - [唯一值、值计数以及成员资格](#唯一值值计数以及成员资格)
  - [数据加载、存储与文件格式](#数据加载存储与文件格式)
    - [读写文本格式的数据](#读写文本格式的数据)
      - [逐块读取文本文件](#逐块读取文本文件)
      - [将数据写出到文本格式](#将数据写出到文本格式)
      - [处理分隔符格式](#处理分隔符格式)
      - [`JSON`数据](#json数据)
    - [二进制数据格式](#二进制数据格式)
      - [使用`HDF5`格式](#使用hdf5格式)
  - [数据清洗和准备](#数据清洗和准备)
    - [处理缺失数据](#处理缺失数据)
      - [滤除缺失数据](#滤除缺失数据)
      - [填充缺失数据](#填充缺失数据)
    - [数据转换](#数据转换)
      - [移除重复数据](#移除重复数据)
      - [利用函数或映射进行数据转换](#利用函数或映射进行数据转换)
      - [替换值](#替换值)
      - [重命名轴索引](#重命名轴索引)
      - [离散化和面元划分](#离散化和面元划分)
      - [检测和过滤异常值](#检测和过滤异常值)
      - [排列和随机采样](#排列和随机采样)
      - [计算指标/哑变量](#计算指标哑变量)
    - [字符串操作](#字符串操作)
      - [字符串对象方法](#字符串对象方法)
      - [正则表达式](#正则表达式)
  - [数据规整：聚合、合并和重塑](#数据规整聚合合并和重塑)
    - [层次化索引](#层次化索引)
    - [重排与分级排序](#重排与分级排序)
    - [根据级别汇总统计](#根据级别汇总统计)
    - [使用`DataFrame`的列进行索引](#使用dataframe的列进行索引)
  - [合并数据集](#合并数据集)
    - [数据库风格的`DataFrame`合并](#数据库风格的dataframe合并)
    - [索引上的合并](#索引上的合并)
    - [轴向连接](#轴向连接)
    - [合并重叠数据](#合并重叠数据)
  - [重塑和轴向旋转](#重塑和轴向旋转)
    - [重塑层次化索引](#重塑层次化索引)
    - [将“长格式”旋转为“宽格式”](#将长格式旋转为宽格式)
    - [将“宽格式”旋转为“长格式”](#将宽格式旋转为长格式)

<!-- /TOC -->
# 利用`Python`进行数据分析

## 入门

### 数据结构介绍

#### `Series`

`Series`是一种类似于一维数组的对象，它由一组数据（各种`NumPy`数据类型）以及一组与之相关的数据标签（即索引）组成。仅由一组数据即可产生最简单的`Series`：
```python
obj = pd.Series([4, 7, -5, 3])

obj
0    4
1    7
2   -5
3    3
dtype: int64
```
`Series`的字符串表现形式为：索引在左边，值在右边。由于我们没有为数据指定索引，于是会自动创建一个0到N-1（N为数据的长度）的整数型索引。你可以通过`Series` 的`values`和`index`属性获取其数组表示形式和索引对象：
```python
obj.values
array([ 4,  7, -5,  3])
obj.index  # like range(4)
RangeIndex(start=0, stop=4, step=1)
```
通常，我们希望所创建的`Series`带有一个可以对各个数据点进行标记的索引：
```python
obj2 = pd.Series([4, 7, -5, 3], index=['d', 'b', 'a', 'c'])
obj2
d    4
b    7
a   -5
c    3
dtype: int64
obj2.index
Index(['d', 'b', 'a', 'c'], dtype='object')
```
与普通`NumPy`数组相比，你可以通过索引的方式选取`Series`中的单个或一组值：
```python
obj2['a']
-5
obj2['d'] = 6
obj2[['c', 'a', 'd']]
c    3
a   -5
d    6
dtype: int64
```
`['c', 'a', 'd']`是索引列表，即使它包含的是字符串而不是整数。

使用`NumPy`函数或类似`NumPy`的运算（如根据布尔型数组进行过滤、标量乘法、应用数学函数等）都会保留索引值的链接：
```python
obj2[obj2 > 0]
d    6
b    7
c    3
dtype: int64
obj2 * 2
d    12
b    14
a   -10
c     6
dtype: int64
np.exp(obj2)
d     403.428793
b    1096.633158
a       0.006738
c      20.085537
dtype: float64
```
还可以将`Series`看成是一个定长的有序字典，因为它是索引值到数据值的一个映射。它可以用在许多原本需要字典参数的函数中：
```python
'b' in obj2
True
'e' in obj2
False
```
如果数据被存放在一个`Python`字典中，也可以直接通过这个字典来创建`Series`：
```python
sdata = {'Ohio': 35000, 'Texas': 71000, 'Oregon': 16000, 'Utah': 5000}
obj3 = pd.Series(sdata)
obj3
Ohio      35000
Oregon    16000
Texas     71000
Utah       5000
dtype: int64
```
如果只传入一个字典，则结果`Series`中的索引就是原字典的键（有序排列）。你可以传入排好序的字典的键以改变顺序：
```python
states = ['California', 'Ohio', 'Oregon', 'Texas']
obj4 = pd.Series(sdata, index=states)
obj4
California        NaN
Ohio          35000.0
Oregon        16000.0
Texas         71000.0
dtype: float64
```
在这个例子中，`sdata`中跟`states`索引相匹配的那3个值会被找出来并放到相应的位置上，但由于`"California"`所对应的`sdata`值找不到，所以其结果就为`NaN`（即“非数字”（`not a number`），在`pandas`中，它用于表示缺失或`NA`值）。因为`‘Utah’`不在`states`中，它被从结果中除去。

使用缺失（`missing`）或`NA`表示缺失数据。`pandas`的`isnull`和`notnull`函数可用于检测缺失数据：
```python
pd.isnull(obj4)
California     True
Ohio          False
Oregon        False
Texas         False
dtype: bool
pd.notnull(obj4)
California    False
Ohio           True
Oregon         True
Texas          True
dtype: bool
```
`Series`也有类似的实例方法：
```python
obj4.isnull()
California     True
Ohio          False
Oregon        False
Texas         False
dtype: bool
```
对于许多应用而言，`Series`最重要的一个功能是，它会根据运算的索引标签自动对齐数据：
```python
obj3
Ohio      35000
Oregon    16000
Texas     71000
Utah       5000
dtype: int64
obj4
California        NaN
Ohio          35000.0
Oregon        16000.0
Texas         71000.0
dtype: float64
obj3 + obj4
California         NaN
Ohio           70000.0
Oregon         32000.0
Texas         142000.0
Utah               NaN
dtype: float64
```
`Series`对象本身及其索引都有一个`name`属性，该属性跟`pandas`其他的关键功能关系非常密切：
```python
obj4.name = 'population'
obj4.index.name = 'state'
obj4
state
California        NaN
Ohio          35000.0
Oregon        16000.0
Texas         71000.0
Name: population, dtype: float64
```
`Series`的索引可以通过赋值的方式就地修改：
```python
obj
0    4
1    7
2   -5
3    3
dtype: int64
obj.index = ['Bob', 'Steve', 'Jeff', 'Ryan']
obj
Bob      4
Steve    7
Jeff    -5
Ryan     3
dtype: int64
```

#### `DataFrame`

`DataFrame`是一个表格型的数据结构，它含有一组有序的列，每列可以是不同的值类型（数值、字符串、布尔值等）。`DataFrame`既有行索引也有列索引，它可以被看做由`Series`组成的字典（共用同一个索引）。`DataFrame`中的数据是以一个或多个二维块存放的（而不是列表、字典或别的一维数据结构）。
建DataFrame的办法有很多，最常用的一种是直接传入一个由等长列表或NumPy数组组成的字典：
```python
data = {'state': ['Ohio', 'Ohio', 'Ohio', 'Nevada', 'Nevada', 'Nevada'],
        'year': [2000, 2001, 2002, 2001, 2002, 2003],
        'pop': [1.5, 1.7, 3.6, 2.4, 2.9, 3.2]}
frame = pd.DataFrame(data)
```
结果`DataFrame`会自动加上索引（跟`Series`一样），且全部列会被有序排列：
```python
frame
   pop   state  year
0  1.5    Ohio  2000
1  1.7    Ohio  2001
2  3.6    Ohio  2002
3  2.4  Nevada  2001
4  2.9  Nevada  2002
5  3.2  Nevada  2003
```
如果指定了列序列，则`DataFrame`的列就会按照指定顺序进行排列：
```python
pd.DataFrame(data, columns=['year', 'state', 'pop'])
   year   state  pop
0  2000    Ohio  1.5
1  2001    Ohio  1.7
2  2002    Ohio  3.6
3  2001  Nevada  2.4
4  2002  Nevada  2.9
5  2003  Nevada  3.2
```
如果传入的列在数据中找不到，就会在结果中产生缺失值：
```python
frame2 = pd.DataFrame(data, columns=['year', 'state', 'pop', 'debt'],
                     index=['one', 'two', 'three', 'four',
                     'five', 'six'])
frame2
       year   state  pop debt
one    2000    Ohio  1.5  NaN
two    2001    Ohio  1.7  NaN
three  2002    Ohio  3.6  NaN
four   2001  Nevada  2.4  NaN
five   2002  Nevada  2.9  NaN
six    2003  Nevada  3.2  NaN
frame2.columns
Index(['year', 'state', 'pop', 'debt'], dtype='object')
```
通过类似字典标记的方式或属性的方式，可以将`DataFrame`的列获取为一个`Series`：
```python
frame2['state']
one        Ohio
two        Ohio
three      Ohio
four     Nevada
five     Nevada
six      Nevada
Name: state, dtype: object
frame2.year
one      2000
two      2001
three    2002
four     2001
five     2002
six      2003
Name: year, dtype: int64
```
注意，返回的`Series`拥有原`DataFrame`相同的索引，且其`name`属性也已经被相应地设置好了。

行也可以通过位置或名称的方式进行获取，比如用`loc`属性：
```python
frame2.loc['three']
year     2002
state    Ohio
pop       3.6
debt      NaN
Name: three, dtype: object
```
列可以通过赋值的方式进行修改。例如，我们可以给那个空的`"debt"`列赋上一个标量值或一组值：
```python
frame2['debt'] = 16.5
frame2
       year   state  pop  debt
one    2000    Ohio  1.5  16.5
two    2001    Ohio  1.7  16.5
three  2002    Ohio  3.6  16.5
four   2001  Nevada  2.4  16.5
five   2002  Nevada  2.9  16.5
six    2003  Nevada  3.2  16.5
frame2['debt'] = np.arange(6.)
frame2
       year   state  pop  debt
one    2000    Ohio  1.5   0.0
two    2001    Ohio  1.7   1.0
three  2002    Ohio  3.6   2.0
four   2001  Nevada  2.4   3.0
five   2002  Nevada  2.9   4.0
six    2003  Nevada  3.2   5.0
```
将列表或数组赋值给某个列时，其长度必须跟`DataFrame`的长度相匹配。如果赋值的是一个`Series`，就会精确匹配`DataFrame`的索引，所有的空位都将被填上缺失值：
```python
val = pd.Series([-1.2, -1.5, -1.7], index=['two', 'four', 'five'])
frame2['debt'] = val
frame2
       year   state  pop  debt
one    2000    Ohio  1.5   NaN
two    2001    Ohio  1.7  -1.2
three  2002    Ohio  3.6   NaN
four   2001  Nevada  2.4  -1.5
five   2002  Nevada  2.9  -1.7
six    2003  Nevada  3.2   NaN
```
为不存在的列赋值会创建出一个新列。关键字`del`用于删除列。

作为`del`的例子，先添加一个新的布尔值的列，`state`是否为`'Ohio'`：
```python
frame2['eastern'] = frame2.state == 'Ohio'
frame2
       year   state  pop  debt  eastern
one    2000    Ohio  1.5   NaN     True
two    2001    Ohio  1.7  -1.2     True
three  2002    Ohio  3.6   NaN     True
four   2001  Nevada  2.4  -1.5    False
five   2002  Nevada  2.9  -1.7    False
six    2003  Nevada  3.2   NaN    False
```
注意：不能用`frame2.eastern`创建新的列。

`del`方法可以用来删除这列：
```python
del frame2['eastern']
frame2.columns
Index(['year', 'state', 'pop', 'debt'], dtype='object')
```
注意：通过索引方式返回的列只是相应数据的视图而已，并不是副本。因此，对返回的`Series`所做的任何就地修改全都会反映到源`DataFrame`上。通过`Series`的`copy`方法即可指定复制列。

另一种常见的数据形式是嵌套字典：
```python
pop = {'Nevada': {2001: 2.4, 2002: 2.9},'Ohio': {2000: 1.5, 2001: 1.7, 2002: 3.6}}
```
如果嵌套字典传给`DataFrame`，pandas就会被解释为：外层字典的键作为列，内层键则作为行索引：
```python
frame3 = pd.DataFrame(pop)
frame3
      Nevada  Ohio
2000     NaN   1.5
2001     2.4   1.7
2002     2.9   3.6
```
你也可以使用类似`NumPy`数组的方法，对`DataFrame`进行转置（交换行和列）：
```python
frame3.T
        2000  2001  2002
Nevada   NaN   2.4   2.9
Ohio     1.5   1.7   3.6
```
内层字典的键会被合并、排序以形成最终的索引。如果明确指定了索引，则不会这样：
```python
pd.DataFrame(pop, index=[2001, 2002, 2003])
      Nevada  Ohio
2001     2.4   1.7
2002     2.9   3.6
2003     NaN   NaN
```
由`Series`组成的字典差不多也是一样的用法：
```python
pdata = {'Ohio': frame3['Ohio'][:-1],
         'Nevada': frame3['Nevada'][:2]}
pd.DataFrame(pdata)
      Nevada  Ohio
2000     NaN   1.5
2001     2.4   1.7
```
下表列出了`DataFrame`构造函数所能接受的各种数据。
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200407034118.png)
如果设置了`DataFrame`的`index`和`columns`的`name`属性，则这些信息也会被显示出来：
```python
frame3.index.name = 'year'; frame3.columns.name = 'state'
frame3
state  Nevada  Ohio
year
2000      NaN   1.5
2001      2.4   1.7
2002      2.9   3.6
```
跟`Series`一样，`values`属性也会以二维`ndarray`的形式返回`DataFrame`中的数据：
```python
frame3.values
array([[ nan,  1.5],
       [ 2.4,  1.7],
       [ 2.9,  3.6]])
```
如果`DataFrame`各列的数据类型不同，则值数组的`dtype`就会选用能兼容所有列的数据类型：
```python
frame2.values
array([[2000, 'Ohio', 1.5, nan],
       [2001, 'Ohio', 1.7, -1.2],
       [2002, 'Ohio', 3.6, nan],
       [2001, 'Nevada', 2.4, -1.5],
       [2002, 'Nevada', 2.9, -1.7],
       [2003, 'Nevada', 3.2, nan]], dtype=object)
```
#### 索引对象
`pandas`的索引对象负责管理轴标签和其他元数据（比如轴名称等）。构建`Series`或`DataFrame`时，所用到的任何数组或其他序列的标签都会被转换成一个`Index`：
```python
obj = pd.Series(range(3), index=['a', 'b', 'c'])
index = obj.index
index
Index(['a', 'b', 'c'], dtype='object')
index[1:]
Index(['b', 'c'], dtype='object')
```
`Index`对象是不可变的，因此用户不能对其进行修改：
```python
index[1] = 'd'  # TypeError
```
不可变可以使`Index`对象在多个数据结构之间安全共享：
```python
labels = pd.Index(np.arange(3))
labels
Int64Index([0, 1, 2], dtype='int64')
obj2 = pd.Series([1.5, -2.5, 0], index=labels)
obj2
0    1.5
1   -2.5
2    0.0
dtype: float64
obj2.index is labels
True
```
注意：虽然用户不需要经常使用`Index`的功能，但是因为一些操作会生成包含被索引化的数据，理解它们的工作原理是很重要的。

除了类似于数组，`Index`的功能也类似一个固定大小的集合：
```python
frame3
state  Nevada  Ohio
year               
2000      NaN   1.5
2001      2.4   1.7
2002      2.9   3.6
frame3.columns
Index(['Nevada', 'Ohio'], dtype='object', name='state')
'Ohio' in frame3.columns
True
2003 in frame3.index
False
```
与`python`的集合不同，`pandas`的`Index`可以包含重复的标签：
```python
dup_labels = pd.Index(['foo', 'foo', 'bar', 'bar'])
dup_labels
Index(['foo', 'foo', 'bar', 'bar'], dtype='object')
```
选择重复的标签，会显示所有的结果。
每个索引都有一些方法和属性，它们可用于设置逻辑并回答有关该索引所包含的数据的常见问题。下表列出了这些函数。
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200407034949.png)
### 基本功能
#### 重新索引
`pandas`对象的一个重要方法是`reindex`，其作用是创建一个新对象，它的数据符合新的索引。看下面的例子：
```python
obj = pd.Series([4.5, 7.2, -5.3, 3.6], index=['d', 'b', 'a', 'c'])
obj
d    4.5
b    7.2
a   -5.3
c    3.6
dtype: float64
```
用该`Series`的`reindex`将会根据新索引进行重排。如果某个索引值当前不存在，就引入缺失值：
```python
obj2 = obj.reindex(['a', 'b', 'c', 'd', 'e'])
obj2
a   -5.3
b    7.2
c    3.6
d    4.5
e    NaN
dtype: float64
```
对于时间序列这样的有序数据，重新索引时可能需要做一些插值处理。`method`选项即可达到此目的，例如，使用`ffill`可以实现前向值填充：
```python
obj3 = pd.Series(['blue', 'purple', 'yellow'], index=[0, 2, 4])
obj3
0      blue
2    purple
4    yellow
dtype: object
obj3.reindex(range(6), method='ffill')
0      blue
1      blue
2    purple
3    purple
4    yellow
5    yellow
dtype: object
```
借助`DataFrame`，`reindex`可以修改（行）索引和列。只传递一个序列时，会重新索引结果的行：
```python
frame = pd.DataFrame(np.arange(9).reshape((3, 3)),index=['a', 'c', 'd'],columns=['Ohio', 'Texas', 'California'])
frame
   Ohio  Texas  California
a     0      1           2
c     3      4           5
d     6      7           8
frame2 = frame.reindex(['a', 'b', 'c', 'd'])
frame2
   Ohio  Texas  California
a   0.0    1.0         2.0
b   NaN    NaN         NaN
c   3.0    4.0         5.0
d   6.0    7.0         8.0
```
列可以用`columns`关键字重新索引：
```python
states = ['Texas', 'Utah', 'California']
frame.reindex(columns=states)
   Texas  Utah  California
a      1   NaN           2
c      4   NaN           5
d      7   NaN           8
```
下表列出了`reindex`函数的各参数及说明。
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200407035512.png)
#### 丢弃指定轴上的项
丢弃某条轴上的一个或多个项很简单，只要有一个索引数组或列表即可。由于需要执行一些数据整理和集合逻辑，所以`drop`方法返回的是一个在指定轴上删除了指定值的新对象：
```python
obj = pd.Series(np.arange(5.), index=['a', 'b', 'c', 'd', 'e'])
obj
a    0.0
b    1.0
c    2.0
d    3.0
e    4.0
dtype: float64
new_obj = obj.drop('c')
new_obj
a    0.0
b    1.0
d    3.0
e    4.0
dtype: float64
obj.drop(['d', 'c'])
a    0.0
b    1.0
e    4.0
dtype: float64
```
对于`DataFrame`，可以删除任意轴上的索引值。为了演示，先新建一个`DataFrame`例子：
```python
data = pd.DataFrame(np.arange(16).reshape((4, 4)),index=['Ohio', 'Colorado', 'Utah', 'New York'],columns=['one', 'two', 'three', 'four'])
data
          one  two  three  four
Ohio        0    1      2     3
Colorado    4    5      6     7
Utah        8    9     10    11
New York   12   13     14    15
```
用标签序列调用`drop`会从行标签（`axis 0`）删除值：
```python
data.drop(['Colorado', 'Ohio'])
          one  two  three  four
Utah        8    9     10    11
New York   12   13     14    15
```
通过传递`axis=1`或`axis='columns'`可以删除列的值：
```python
data.drop('two', axis=1)
          one  three  four
Ohio        0      2     3
Colorado    4      6     7
Utah        8     10    11
New York   12     14    15
data.drop(['two', 'four'], axis='columns')
          one  three
Ohio        0      2
Colorado    4      6
Utah        8     10
New York   12     14
```
许多函数，如`drop`，会修改`Series`或`DataFrame`的大小或形状，可以就地修改对象，不会返回新的对象：
```python
obj.drop('c', inplace=True)
obj
a    0.0
b    1.0
d    3.0
e    4.0
dtype: float64
```
小心使用`inplace`，它会销毁所有被删除的数据。
#### 索引、选取和过滤
`Series`索引（`obj[...]`）的工作方式类似于`NumPy`数组的索引，只不过`Series`的索引值不只是整数。下面是几个例子：
```python
obj = pd.Series(np.arange(4.), index=['a', 'b', 'c', 'd'])
obj
a    0.0
b    1.0
c    2.0
d    3.0
dtype: float64
obj['b']
1.0
obj[1]
1.0
obj[2:4]
c    2.0
d    3.0
dtype: float64
obj[['b', 'a', 'd']]
b    1.0
a    0.0
d    3.0
dtype: float64
obj[[1, 3]]
b    1.0
d    3.0
dtype: float64
obj[obj < 2]
a    0.0
b    1.0
dtype: float64
```
利用标签的切片运算与普通的`Python`切片运算不同，其末端是包含的：
```python
obj['b':'c']
b    1.0
c    2.0
dtype: float64
```
用切片可以对`Series`的相应部分进行设置：
```python
obj['b':'c'] = 5
obj
a    0.0
b    5.0
c    5.0
d    3.0
dtype: float64
```
用一个值或序列对`DataFrame`进行索引其实就是获取一个或多个列：
```python
data = pd.DataFrame(np.arange(16).reshape((4, 4)),index=['Ohio', 'Colorado', 'Utah', 'New York'],columns=['one', 'two', 'three', 'four'])
data
          one  two  three  four
Ohio        0    1      2     3
Colorado    4    5      6     7
Utah        8    9     10    11
New York   12   13     14    15
data['two']
Ohio         1
Colorado     5
Utah         9
New York    13
Name: two, dtype: int64
data[['three', 'one']]
          three  one
Ohio          2    0
Colorado      6    4
Utah         10    8
New York     14   12
```
这种索引方式有几个特殊的情况。首先通过切片或布尔型数组选取数据：
```python
data[:2]
          one  two  three  four
Ohio        0    1      2     3
Colorado    4    5      6     7
data[data['three'] > 5]
          one  two  three  four
Colorado    4    5      6     7
Utah        8    9     10    11
New York   12   13     14    15
```
选取行的语法`data[:2]`十分方便。向`[ ]`传递单一的元素或列表，就可选择列。

另一种用法是通过布尔型`DataFrame`（比如下面这个由标量比较运算得出的）进行索引：
```python
data < 5

            one    two  three   four
Ohio       True   True   True   True
Colorado   True  False  False  False
Utah      False  False  False  False
New York  False  False  False  False
data[data < 5] = 0
data
          one  two  three  four
Ohio        0    0      0     0
Colorado    0    5      6     7
Utah        8    9     10    11
New York   12   13     14    15
```
这使得`DataFrame`的语法与`NumPy`二维数组的语法很像。
#### 用`loc`和`iloc`进行选取
对于`DataFrame`的行的标签索引，我引入了特殊的标签运算符`loc`和`iloc`。它们可以让你用类似`NumPy`的标记，使用轴标签（`loc`）或整数索引（`iloc`），从`DataFrame`选择行和列的子集。

作为一个初步示例，让我们通过标签选择一行和多列：
```python
data.loc['Colorado', ['two', 'three']]
two      5
three    6
Name: Colorado, dtype: int64
```
然后用`iloc`和整数进行选取：
```python
data.iloc[2, [3, 0, 1]]
four    11
one      8
two      9
Name: Utah, dtype: int64
data.iloc[2]
one       8
two       9
three    10
four     11
Name: Utah, dtype: int64
data.iloc[[1, 2], [3, 0, 1]]
          four  one  two
Colorado     7    0    5
Utah        11    8    9
```
这两个索引函数也适用于一个标签或多个标签的切片：
```python
data.loc[:'Utah', 'two']
Ohio        0
Colorado    5
Utah        9
Name: two, dtype: int64
data.iloc[:, :3][data.three > 5]
          one  two  three
Colorado    0    5      6
Utah        8    9     10
New York   12   13     14
```
所以，在`pandas`中，有多个方法可以选取和重新组合数据。对于`DataFrame`，下表进行了总结。后面会看到，还有更多的方法进行层级化索引。
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200407040459.png)
#### 整数索引
处理整数索引的`pandas`对象常常难住新手，因为它与`Python`内置的列表和元组的索引语法不同。例如，你可能不认为下面的代码会出错：
```python
ser = pd.Series(np.arange(3.))
ser
ser[-1]
```
这里，`pandas`可以勉强进行整数索引，但是会导致小`bug`。我们有包含0,1,2的索引，但是引入用户想要的东西（基于标签或位置的索引）很难：
```python
In [144]: ser
0    0.0
1    1.0
2    2.0
dtype: float64
```
另外，对于非整数索引，不会产生歧义：
```python
ser2 = pd.Series(np.arange(3.), index=['a', 'b', 'c'])
ser2[-1]
2.0
```
为了进行统一，如果轴索引含有整数，数据选取总会使用标签。为了更准确，请使用`loc`（标签）或`iloc`（整数）：
```python
ser[:1]
0    0.0
dtype: float64
ser.loc[:1]
0    0.0
1    1.0
dtype: float64
ser.iloc[:1]
0    0.0
dtype: float64
```
#### 算术运算和数据对齐
`pandas`最重要的一个功能是，它可以对不同索引的对象进行算术运算。在将对象相加时，如果存在不同的索引对，则结果的索引就是该索引对的并集。对于有数据库经验的用户，这就像在索引标签上进行自动外连接。看一个简单的例子：
```python
s1 = pd.Series([7.3, -2.5, 3.4, 1.5], index=['a', 'c', 'd', 'e'])
s2 = pd.Series([-2.1, 3.6, -1.5, 4, 3.1],index=['a', 'c', 'e', 'f', 'g'])
s1
a    7.3
c   -2.5
d    3.4
e    1.5
dtype: float64
s2
a   -2.1
c    3.6
e   -1.5
f    4.0
g    3.1
dtype: float64
```
将它们相加就会产生：
```python
s1 + s2
a    5.2
c    1.1
d    NaN
e    0.0
f    NaN
g    NaN
dtype: float64
```
自动的数据对齐操作在不重叠的索引处引入了`NA`值。缺失值会在算术运算过程中传播。

对于`DataFrame`，对齐操作会同时发生在行和列上：
```python
df1 = pd.DataFrame(np.arange(9.).reshape((3, 3)), columns=list('bcd'),index=['Ohio', 'Texas', 'Colorado'])
df2 = pd.DataFrame(np.arange(12.).reshape((4, 3)), columns=list('bde'),index=['Utah', 'Ohio', 'Texas', 'Oregon'])
df1
            b    c    d
Ohio      0.0  1.0  2.0
Texas     3.0  4.0  5.0
Colorado  6.0  7.0  8.0
df2
          b     d     e
Utah    0.0   1.0   2.0
Ohio    3.0   4.0   5.0
Texas   6.0   7.0   8.0
Oregon  9.0  10.0  11.0
```
把它们相加后将会返回一个新的`DataFrame`，其索引和列为原来那两个`DataFrame`的并集：
```python
df1 + df2
            b   c     d   e
Colorado  NaN NaN   NaN NaN
Ohio      3.0 NaN   6.0 NaN
Oregon    NaN NaN   NaN NaN
Texas     9.0 NaN  12.0 NaN
Utah      NaN NaN   NaN NaN
```
因为`'c'`和`'e'`列均不在两个`DataFrame`对象中，在结果中以缺省值呈现。行也是同样。

如果`DataFrame`对象相加，没有共用的列或行标签，结果都会是空：
```python
df1 = pd.DataFrame({'A': [1, 2]})
df2 = pd.DataFrame({'B': [3, 4]})
df1
   A
0  1
1  2
df2
   B
0  3
1  4
df1 - df2
    A   B
0 NaN NaN
1 NaN NaN
```
#### 在算术方法中填充值
在对不同索引的对象进行算术运算时，你可能希望当一个对象中某个轴标签在另一个对象中找不到时填充一个特殊值（比如0）：
```python
df1 = pd.DataFrame(np.arange(12.).reshape((3, 4)),columns=list('abcd'))
df2 = pd.DataFrame(np.arange(20.).reshape((4, 5)),columns=list('abcde'))
df2.loc[1, 'b'] = np.nan
df1
     a    b     c     d
0  0.0  1.0   2.0   3.0
1  4.0  5.0   6.0   7.0
2  8.0  9.0  10.0  11.0
df2
      a     b     c     d     e
0   0.0   1.0   2.0   3.0   4.0
1   5.0   NaN   7.0   8.0   9.0
2  10.0  11.0  12.0  13.0  14.0
3  15.0  16.0  17.0  18.0  19.0
```
将它们相加时，没有重叠的位置就会产生`NA`值：
```python
df1 + df2
      a     b     c     d   e
0   0.0   2.0   4.0   6.0 NaN
1   9.0   NaN  13.0  15.0 NaN
2  18.0  20.0  22.0  24.0 NaN
3   NaN   NaN   NaN   NaN NaN
```
使用`df1`的`add`方法，传入`df2`以及一个`fill_value`参数：
```python
df1.add(df2, fill_value=0)
      a     b     c     d     e
0   0.0   2.0   4.0   6.0   4.0
1   9.0   5.0  13.0  15.0   9.0
2  18.0  20.0  22.0  24.0  14.0
3  15.0  16.0  17.0  18.0  19.0
```
下表列出了`Series`和`DataFrame`的算术方法。它们每个都有一个副本，以字母`r`开头，它会翻转参数。因此这两个语句是等价的：
```python
1 / df1
          a         b         c         d
0       inf  1.000000  0.500000  0.333333
1  0.250000  0.200000  0.166667  0.142857
2  0.125000  0.111111  0.100000  0.090909
df1.rdiv(1)
          a         b         c         d
0       inf  1.000000  0.500000  0.333333
1  0.250000  0.200000  0.166667  0.142857
2  0.125000  0.111111  0.100000  0.090909
```
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200407040459.png)
与此类似，在对`Series`或`DataFrame`重新索引时，也可以指定一个填充值：
```python
df1.reindex(columns=df2.columns, fill_value=0)
     a    b     c     d  e
0  0.0  1.0   2.0   3.0  0
1  4.0  5.0   6.0   7.0  0
2  8.0  9.0  10.0  11.0  0
```
#### `DataFrame`和`Series`之间的运算

跟不同维度的`NumPy`数组一样，`DataFrame`和`Series`之间算术运算也是有明确规定的。先来看一个具有启发性的例子，计算一个二维数组与其某行之间的差：
```python
arr = np.arange(12.).reshape((3, 4))
arr
array([[  0.,   1.,   2.,   3.],
       [  4.,   5.,   6.,   7.],
       [  8.,   9.,  10.,  11.]])
arr[0]
array([ 0.,  1.,  2.,  3.])
arr - arr[0]
array([[ 0.,  0.,  0.,  0.],
       [ 4.,  4.,  4.,  4.],
       [ 8.,  8.,  8.,  8.]])
```
当我们从`arr`减去`arr[0]`，每一行都会执行这个操作。这就叫做广播（`broadcasting`）。`DataFrame`和`Series`之间的运算差不多也是如此：
```python
frame = pd.DataFrame(np.arange(12.).reshape((4, 3)),columns=list('bde'),index=['Utah', 'Ohio', 'Texas', 'Oregon'])
series = frame.iloc[0]
frame
          b     d     e
Utah    0.0   1.0   2.0
Ohio    3.0   4.0   5.0
Texas   6.0   7.0   8.0
Oregon  9.0  10.0  11.0
series
b    0.0
d    1.0
e    2.0
Name: Utah, dtype: float64
```
默认情况下，`DataFrame`和`Series`之间的算术运算会将`Series`的索引匹配到`DataFrame`的列，然后沿着行一直向下广播：
```python
frame - series
          b    d    e
Utah    0.0  0.0  0.0
Ohio    3.0  3.0  3.0
Texas   6.0  6.0  6.0
Oregon  9.0  9.0  9.0
```
如果某个索引值在`DataFrame`的列或`Series`的索引中找不到，则参与运算的两个对象就会被重新索引以形成并集：
```python
series2 = pd.Series(range(3), index=['b', 'e', 'f'])
frame + series2
          b   d     e   f
Utah    0.0 NaN   3.0 NaN
Ohio    3.0 NaN   6.0 NaN
Texas   6.0 NaN   9.0 NaN
Oregon  9.0 NaN  12.0 NaN
```
如果你希望匹配行且在列上广播，则必须使用算术运算方法。例如：
```python
series3 = frame['d']
frame
          b     d     e
Utah    0.0   1.0   2.0
Ohio    3.0   4.0   5.0
Texas   6.0   7.0   8.0
Oregon  9.0  10.0  11.0
series3
Utah       1.0
Ohio       4.0
Texas      7.0
Oregon    10.0
Name: d, dtype: float64
frame.sub(series3, axis='index')
          b    d    e
Utah   -1.0  0.0  1.0
Ohio   -1.0  0.0  1.0
Texas  -1.0  0.0  1.0
Oregon -1.0  0.0  1.0
```
传入的轴号就是希望匹配的轴。在本例中，我们的目的是匹配`DataFrame`的行索引（`axis='index'` or `axis=0`）并进行广播。
#### 函数应用和映射
`NumPy`的`ufuncs`（元素级数组方法）也可用于操作`pandas`对象：
```pythoh
frame = pd.DataFrame(np.random.randn(4, 3), columns=list('bde'),index=['Utah', 'Ohio', 'Texas', 'Oregon'])
frame
               b         d         e
Utah   -0.204708  0.478943 -0.519439
Ohio   -0.555730  1.965781  1.393406
Texas   0.092908  0.281746  0.769023
Oregon  1.246435  1.007189 -1.296221
np.abs(frame)
               b         d         e
Utah    0.204708  0.478943  0.519439
Ohio    0.555730  1.965781  1.393406
Texas   0.092908  0.281746  0.769023
Oregon  1.246435  1.007189  1.296221
```
另一个常见的操作是，将函数应用到由各列或行所形成的一维数组上。`DataFrame`的`apply`方法即可实现此功能：
```python
f = lambda x: x.max() - x.min()
frame.apply(f)
b    1.802165
d    1.684034
e    2.689627
dtype: float64
```
这里的函数`f`，计算了一个`Series`的最大值和最小值的差，在`frame`的每列都执行了一次。结果是一个`Series`，使用`frame`的列作为索引。

如果传递`axis='columns'`到`apply`，这个函数会在每行执行：
```python
frame.apply(f, axis='columns')
Utah      0.998382
Ohio      2.521511
Texas     0.676115
Oregon    2.542656
dtype: float64
```
许多最为常见的数组统计功能都被实现成`DataFrame`的方法（如`sum`和`mean`），因此无需使用`apply`方法。

传递到`apply`的函数不是必须返回一个标量，还可以返回由多个值组成的`Series`：
```python
def f(x):
    return pd.Series([x.min(), x.max()], index=['min', 'max'])
frame.apply(f)
            b         d         e
min -0.555730  0.281746 -1.296221
max  1.246435  1.965781  1.393406
```
元素级的`Python`函数也是可以用的。假如你想得到`frame`中各个浮点值的格式化字符串，使用`applymap`即可：
```python
format = lambda x: '%.2f' % x
frame.applymap(format)
            b     d      e
Utah    -0.20  0.48  -0.52
Ohio    -0.56  1.97   1.39
Texas    0.09  0.28   0.77
Oregon   1.25  1.01  -1.30
```
之所以叫做`applymap`，是因为`Series`有一个用于应用元素级函数的`map`方法：
```python
frame['e'].map(format)
Utah      -0.52
Ohio       1.39
Texas      0.77
Oregon    -1.30
Name: e, dtype: object
```
#### 排序和排名
根据条件对数据集排序（`sorting`）也是一种重要的内置运算。要对行或列索引进行排序（按字典顺序），可使用`sort_index`方法，它将返回一个已排序的新对象：
```python
obj = pd.Series(range(4), index=['d', 'a', 'b', 'c'])
obj.sort_index()
a    1
b    2
c    3
d    0
dtype: int64
```
对于`DataFrame`，则可以根据任意一个轴上的索引进行排序：
```python
frame = pd.DataFrame(np.arange(8).reshape((2, 4)),index=['three', 'one'],columns=['d', 'a', 'b', 'c'])
frame.sort_index()
       d  a  b  c
one    4  5  6  7
three  0  1  2  3
frame.sort_index(axis=1)
       a  b  c  d
three  1  2  3  0
one    5  6  7  4
```
数据默认是按升序排序的，但也可以降序排序：
```python
frame.sort_index(axis=1, ascending=False)
       d  c  b  a
three  0  3  2  1
one    4  7  6  5
```
若要按值对`Series`进行排序，可使用其`sort_values`方法：
```python
obj = pd.Series([4, 7, -3, 2])
obj.sort_values()
2   -3
3    2
0    4
1    7
dtype: int64
```
在排序时，任何缺失值默认都会被放到`Series`的末尾：
```python
obj = pd.Series([4, np.nan, 7, np.nan, -3, 2])
obj.sort_values()
4   -3.0
5    2.0
0    4.0
2    7.0
1    NaN
3    NaN
dtype: float64
```
当排序一个`DataFrame`时，你可能希望根据一个或多个列中的值进行排序。将一个或多个列的名字传递给`sort_values`的`by`选项即可达到该目的：
```python
frame = pd.DataFrame({'b': [4, 7, -3, 2], 'a': [0, 1, 0, 1]})
frame
   a  b
0  0  4
1  1  7
2  0 -3
3  1  2
frame.sort_values(by='b')
   a  b
2  0 -3
3  1  2
0  0  4
1  1  7
```
要根据多个列进行排序，传入名称的列表即可：
```python
frame.sort_values(by=['a', 'b'])
   a  b
2  0 -3
0  0  4
3  1  2
1  1  7
```
排名会从1开始一直到数组中有效数据的数量。接下来介绍`Series`和`DataFrame`的`rank`方法。默认情况下，`rank`是通过“为各组分配一个平均排名”的方式破坏平级关系的：
```python
obj = pd.Series([7, -5, 7, 4, 2, 0, 4])
obj.rank()
0    6.5
1    1.0
2    6.5
3    4.5
4    3.0
5    2.0
6    4.5
dtype: float64
```
也可以根据值在原数据中出现的顺序给出排名：
```python
obj.rank(method='first')
0    6.0
1    1.0
2    7.0
3    4.0
4    3.0
5    2.0
6    5.0
dtype: float64
```
这里，条目0和2没有使用平均排名6.5，它们被设成了6和7，因为数据中标签0位于标签2的前面。

你也可以按降序进行排名：
```python
#Assign tie values the maximum rank in the group
obj.rank(ascending=False, method='max')
0    2.0
1    7.0
2    2.0
3    4.0
4    5.0
5    6.0
6    4.0
dtype: float64
```
下表列出了所有用于破坏平级关系的`method`选项。`DataFrame`可以在行或列上计算排名：
```python
frame = pd.DataFrame({'b': [4.3, 7, -3, 2], 'a': [0, 1, 0, 1],'c': [-2, 5, 8, -2.5]})
frame
   a    b    c
0  0  4.3 -2.0
1  1  7.0  5.0
2  0 -3.0  8.0
3  1  2.0 -2.5
frame.rank(axis='columns')
     a    b    c
0  2.0  3.0  1.0
1  1.0  3.0  2.0
2  2.0  1.0  3.0
3  2.0  3.0  1.0
```
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200407043605.png)
#### 带有重复标签的轴索引
直到目前为止，我所介绍的所有范例都有着唯一的轴标签（索引值）。虽然许多`pandas`函数（如`reindex`）都要求标签唯一，但这并不是强制性的。我们来看看下面这个简单的带有重复索引值的`Series`：
```python
obj = pd.Series(range(5), index=['a', 'a', 'b', 'b', 'c'])
obj
a    0
a    1
b    2
b    3
c    4
dtype: int64
```
索引的`is_unique`属性可以告诉你它的值是否是唯一的：
```python
obj.index.is_unique
False
```
对于带有重复值的索引，数据选取的行为将会有些不同。如果某个索引对应多个值，则返回一个`Series`；而对应单个值的，则返回一个标量值：
```python
obj['a']
a    0
a    1
dtype: int64
obj['c']
4
```
这样会使代码变复杂，因为索引的输出类型会根据标签是否有重复发生变化。

对`DataFrame`的行进行索引时也是如此：
```python
df = pd.DataFrame(np.random.randn(4, 3), index=['a', 'a', 'b', 'b'])
df
          0         1         2
a  0.274992  0.228913  1.352917
a  0.886429 -2.001637 -0.371843
b  1.669025 -0.438570 -0.539741
b  0.476985  3.248944 -1.021228
df.loc['b']
          0         1         2
b  1.669025 -0.438570 -0.539741
b  0.476985  3.248944 -1.021228
```
### 汇总和计算描述统计
`pandas`对象拥有一组常用的数学和统计方法。它们大部分都属于约简和汇总统计，用于从`Series`中提取单个值（如`sum`或`mean`）或从`DataFrame`的行或列中提取一个`Series`。跟对应的`NumPy`数组方法相比，它们都是基于没有缺失数据的假设而构建的。看一个简单的`DataFrame`：
```python
df = pd.DataFrame([[1.4, np.nan], [7.1, -4.5],[np.nan, np.nan], [0.75, -1.3]],index=['a', 'b', 'c', 'd'],columns=['one', 'two'])
df
    one  two
a  1.40  NaN
b  7.10 -4.5
c   NaN  NaN
d  0.75 -1.3
```
调用`DataFrame`的`sum`方法将会返回一个含有列的和的`Series`：
```python
df.sum()
one    9.25
two   -5.80
dtype: float64
```
传入`axis='columns'`或`axis=1`将会按行进行求和运算：
```python
df.sum(axis=1)
a    1.40
b    2.60
c     NaN
d   -0.55
```
`NA`值会自动被排除，除非整个切片（这里指的是行或列）都是`NA`。通过`skipna`选项可以禁用该功能：
```python
df.mean(axis='columns', skipna=False)
a      NaN
b    1.300
c      NaN
d   -0.275
dtype: float64
```
下表列出了这些约简方法的常用选项。
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200407044135.png)
有些方法（如`idxmin`和`idxmax`）返回的是间接统计（比如达到最小值或最大值的索引）：
```python
df.idxmax()
one    b
two    d
dtype: object
```
另一些方法则是累计型的：
```python
df.cumsum()
    one  two
a  1.40  NaN
b  8.50 -4.5
c   NaN  NaN
d  9.25 -5.8
```
还有一种方法，它既不是约简型也不是累计型。`describe`就是一个例子，它用于一次性产生多个汇总统计：
```python
df.describe()
            one       two
count  3.000000  2.000000
mean   3.083333 -2.900000
std    3.493685  2.262742
min    0.750000 -4.500000
25%    1.075000 -3.700000
50%    1.400000 -2.900000
75%    4.250000 -2.100000
max    7.100000 -1.300000
```
对于非数值型数据，`describe`会产生另外一种汇总统计：
```python
obj = pd.Series(['a', 'a', 'b', 'c'] * 4)
obj.describe()
count     16
unique     3
top        a
freq       8
dtype: object
```
下表列出了所有与描述统计相关的方法。
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200407044400.png)
#### 相关系数与协方差
有些汇总统计（如相关系数和协方差）是通过参数对计算出来的。我们来看几个`DataFrame`，它们的数据来自Yahoo!Finance的股票价格和成交量。
我使用`pandas_datareader`模块下载了一些股票数据：
```python
import pandas_datareader.data as web
all_data = {ticker: web.get_data_yahoo(ticker)
            for ticker in ['AAPL', 'IBM', 'MSFT', 'GOOG']}

price = pd.DataFrame({ticker: data['Adj Close']
                     for ticker, data in all_data.items()})
volume = pd.DataFrame({ticker: data['Volume']
                      for ticker, data in all_data.items()})
```

现在计算价格的百分数变化：
```python
returns = price.pct_change()
returns.tail()
                AAPL      GOOG       IBM      MSFT
Date                                              
2016-10-17 -0.000680  0.001837  0.002072 -0.003483
2016-10-18 -0.000681  0.019616 -0.026168  0.007690
2016-10-19 -0.002979  0.007846  0.003583 -0.002255
2016-10-20 -0.000512 -0.005652  0.001719 -0.004867
2016-10-21 -0.003930  0.003011 -0.012474  0.042096
```
`Series`的`corr`方法用于计算两个`Series`中重叠的、非`NA`的、按索引对齐的值的相关系数。与此类似，`cov`用于计算协方差：
```python
returns['MSFT'].corr(returns['IBM'])
0.49976361144151144
returns['MSFT'].cov(returns['IBM'])
8.8706554797035462e-05
```
因为`MSTF`是一个合理的`Python`属性，我们还可以用更简洁的语法选择列：
```python
returns.MSFT.corr(returns.IBM)
0.49976361144151144
```
另一方面，`DataFrame`的`corr`和`cov`方法将以`DataFrame`的形式分别返回完整的相关系数或协方差矩阵：
```python
returns.corr()
          AAPL      GOOG       IBM      MSFT
AAPL  1.000000  0.407919  0.386817  0.389695
GOOG  0.407919  1.000000  0.405099  0.465919
IBM   0.386817  0.405099  1.000000  0.499764
MSFT  0.389695  0.465919  0.499764  1.000000
returns.cov()
          AAPL      GOOG       IBM      MSFT
AAPL  0.000277  0.000107  0.000078  0.000095
GOOG  0.000107  0.000251  0.000078  0.000108
IBM   0.000078  0.000078  0.000146  0.000089
MSFT  0.000095  0.000108  0.000089  0.000215
```
利用`DataFrame`的`corrwith`方法，你可以计算其列或行跟另一个`Series`或`DataFrame`之间的相关系数。传入一个`Series`将会返回一个相关系数值`Series`（针对各列进行计算）：
```python
returns.corrwith(returns.IBM)
AAPL    0.386817
GOOG    0.405099
IBM     1.000000
MSFT    0.499764
dtype: float64
```
传入一个`DataFrame`则会计算按列名配对的相关系数。这里，我计算百分比变化与成交量的相关系数：
```python
returns.corrwith(volume)
AAPL   -0.075565
GOOG   -0.007067
IBM    -0.204849
MSFT   -0.092950
dtype: float64
```
传入`axis='columns'`即可按行进行计算。无论如何，在计算相关系数之前，所有的数据项都会按标签对齐。

#### 唯一值、值计数以及成员资格
还有一类方法可以从一维`Series`的值中抽取信息。看下面的例子：
```python
obj = pd.Series(['c', 'a', 'd', 'a', 'a', 'b', 'b', 'c', 'c'])
```
第一个函数是`unique`，它可以得到`Series`中的唯一值数组：
```python
uniques = obj.unique()
uniques
array(['c', 'a', 'd', 'b'], dtype=object)
```
返回的唯一值是未排序的，如果需要的话，可以对结果再次进行排序（`uniques.sort()`）。相似的，`value_counts`用于计算一个`Series`中各值出现的频率：
```python
obj.value_counts()
c    3
a    3
b    2
d    1
dtype: int64
```
为了便于查看，结果`Series`是按值频率降序排列的。`value_counts`还是一个顶级`pandas`方法，可用于任何数组或序列：
```python
pd.value_counts(obj.values, sort=False)
a    3
b    2
c    3
d    1
dtype: int64
```
`isin`用于判断矢量化集合的成员资格，可用于过滤`Series`中或`DataFrame`列中数据的子集：
```python
obj
0    c
1    a
2    d
3    a
4    a
5    b
6    b
7    c
8    c
dtype: object
mask = obj.isin(['b', 'c'])
mask
0     True
1    False
2    False
3    False
4    False
5     True
6     True
7     True
8     True
dtype: bool
obj[mask]
0    c
5    b
6    b
7    c
8    c
dtype: object
```
与`isin`类似的是`Index.get_indexer`方法，它可以给你一个索引数组，从可能包含重复值的数组到另一个不同值的数组：
```python
to_match = pd.Series(['c', 'a', 'b', 'b', 'c', 'a'])
unique_vals = pd.Series(['c', 'b', 'a'])
pd.Index(unique_vals).get_indexer(to_match)
array([0, 2, 1, 1, 0, 2])
```
下表给出了这几个方法的一些参考信息。
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200407045054.png)
有时，你可能希望得到`DataFrame`中多个相关列的一张柱状图。例如：
```python
data = pd.DataFrame({'Qu1': [1, 3, 4, 3, 4],
                     'Qu2': [2, 3, 1, 2, 3],
                     'Qu3': [1, 5, 2, 4, 4]})
data
   Qu1  Qu2  Qu3
0    1    2    1
1    3    3    5
2    4    1    2
3    3    2    4
4    4    3    4
```
将`pandas.value_counts`传给该`DataFrame`的`apply`函数，就会出现：
```python
result = data.apply(pd.value_counts).fillna(0)
result
   Qu1  Qu2  Qu3
1  1.0  1.0  1.0
2  0.0  2.0  1.0
3  2.0  2.0  0.0
4  2.0  0.0  2.0
5  0.0  0.0  1.0
```
这里，结果中的行标签是所有列的唯一值。后面的频率值是每个列中这些值的相应计数。

## 数据加载、存储与文件格式
### 读写文本格式的数据
`pandas`提供了一些用于将表格型数据读取为`DataFrame`对象的函数。下表对它们进行了总结，其中`read_csv`和`read_table`可能会是你今后用得最多的。
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200407045624.png)
我将大致介绍一下这些函数在将文本数据转换为`DataFrame`时所用到的一些技术。这些函数的选项可以划分为以下几个大类：

- 索引：将一个或多个列当做返回的`DataFrame`处理，以及是否从文件、用户获取列名。
- 类型推断和数据转换：包括用户定义值的转换、和自定义的缺失值标记列表等。
- 日期解析：包括组合功能，比如将分散在多个列中的日期时间信息组合成结果中的单个列。
- 迭代：支持对大文件进行逐块迭代。
- 不规整数据问题：跳过一些行、页脚、注释或其他一些不重要的东西（比如由成千上万个逗号隔开的数值数据）。
因为工作中实际碰到的数据可能十分混乱，一些数据加载函数（尤其是`read_csv`）的选项逐渐变得复杂起来。面对不同的参数，感到头痛很正常（`read_csv`有超过`50`个参数）。`pandas`文档有这些参数的例子，如果你感到阅读某个文件很难，可以通过相似的足够多的例子找到正确的参数。

其中一些函数，比如`pandas.read_csv`，有类型推断功能，因为列数据的类型不属于数据类型。也就是说，你不需要指定列的类型到底是数值、整数、布尔值，还是字符串。其它的数据格式，如`HDF5`、`Feather`和`msgpack`，会在格式中存储数据类型。

日期和其他自定义类型的处理需要多花点工夫才行。首先我们来看一个以逗号分隔的（`CSV`）文本文件：
```python
a,b,c,d,message
1,2,3,4,hello
5,6,7,8,world
9,10,11,12,foo
```

由于该文件以逗号分隔，所以我们可以使用`read_csv`将其读入一个`DataFrame`：
```python
df = pd.read_csv('examples/ex1.csv')
df
   a   b   c   d message
0  1   2   3   4   hello
1  5   6   7   8   world
2  9  10  11  12     foo
```
我们还可以使用`read_table`，并指定分隔符：
```python
pd.read_table('examples/ex1.csv', sep=',')
   a   b   c   d message
0  1   2   3   4   hello
1  5   6   7   8   world
2  9  10  11  12     foo
```
并不是所有文件都有标题行。看看下面这个文件：
```python
1,2,3,4,hello
5,6,7,8,world
9,10,11,12,foo
```
读入该文件的办法有两个。你可以让`pandas`为其分配默认的列名，也可以自己定义列名：
```python
pd.read_csv('examples/ex2.csv', header=None)
   0   1   2   3      4
0  1   2   3   4  hello
1  5   6   7   8  world
2  9  10  11  12    foo
pd.read_csv('examples/ex2.csv', names=['a', 'b', 'c', 'd', 'message'])
   a   b   c   d message
0  1   2   3   4   hello
1  5   6   7   8   world
2  9  10  11  12     foo
```
假设你希望将`message`列做成`DataFrame`的索引。你可以明确表示要将该列放到索引4的位置上，也可以通过`index_col`参数指定`"message"`：
```python
names = ['a', 'b', 'c', 'd', 'message']
pd.read_csv('examples/ex2.csv', names=names, index_col='message')
         a   b   c   d
message               
hello    1   2   3   4
world    5   6   7   8
foo      9  10  11  12
```
如果希望将多个列做成一个层次化索引，只需传入由列编号或列名组成的列表即可：
```python
key1,key2,value1,value2
one,a,1,2
one,b,3,4
one,c,5,6
one,d,7,8
two,a,9,10
two,b,11,12
two,c,13,14
two,d,15,16
parsed = pd.read_csv('examples/csv_mindex.csv',index_col=['key1', 'key2'])
parsed
           value1  value2
key1 key2                
one  a          1       2
     b          3       4
     c          5       6
     d          7       8
two  a          9      10
     b         11      12
     c         13      14
     d         15      16
```
有些情况下，有些表格可能不是用固定的分隔符去分隔字段的（比如空白符或其它模式）。看看下面这个文本文件：
```python
list(open('examples/ex3.txt'))
['            A         B         C\n',
 'aaa -0.264438 -1.026059 -0.619500\n',
 'bbb  0.927272  0.302904 -0.032399\n',
 'ccc -0.264273 -0.386314 -0.217601\n',
 'ddd -0.871858 -0.348382  1.100491\n']
```
虽然可以手动对数据进行规整，这里的字段是被数量不同的空白字符间隔开的。这种情况下，你可以传递一个正则表达式作为`read_table`的分隔符。可以用正则表达式表达为`\s+`，于是有：
```python
result = pd.read_table('examples/ex3.txt', sep='\s+')
result
            A         B         C
aaa -0.264438 -1.026059 -0.619500
bbb  0.927272  0.302904 -0.032399
ccc -0.264273 -0.386314 -0.217601
ddd -0.871858 -0.348382  1.100491
```
这里，由于列名比数据行的数量少，所以`read_table`推断第一列应该是`DataFrame`的索引。

这些解析器函数还有许多参数可以帮助你处理各种各样的异形文件格式。
缺失值处理是文件解析任务中的一个重要组成部分。缺失数据经常是要么没有（空字符串），要么用某个标记值表示。默认情况下，`pandas`会用一组经常出现的标记值进行识别，比如`NA`及`NULL`：
```python
something,a,b,c,d,message
one,1,2,3,4,NA
two,5,6,,8,world
three,9,10,11,12,foo
result = pd.read_csv('examples/ex5.csv')
result
  something  a   b     c   d message
0       one  1   2   3.0   4     NaN
1       two  5   6   NaN   8   world
2     three  9  10  11.0  12     foo
pd.isnull(result)
   something      a      b      c      d  message
0      False  False  False  False  False     True
1      False  False  False   True  False    False
2      False  False  False  False  False    False
```
`na_values`可以用一个列表或集合的字符串表示缺失值：
```python
result = pd.read_csv('examples/ex5.csv', na_values=['NULL'])
result
  something  a   b     c   d message
0       one  1   2   3.0   4     NaN
1       two  5   6   NaN   8   world
2     three  9  10  11.0  12     foo
```
字典的各列可以使用不同的`NA`标记值：
```python
sentinels = {'message': ['foo', 'NA'], 'something': ['two']}
pd.read_csv('examples/ex5.csv', na_values=sentinels)
something  a   b     c   d message
0       one  1   2   3.0   4     NaN
1       NaN  5   6   NaN   8   world
2     three  9  10  11.0  12     NaN
```
下表列出了`pandas.read_csv`和`pandas.read_table`常用的选项。
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200407050809.png)
#### 逐块读取文本文件
在处理很大的文件时，或找出大文件中的参数集以便于后续处理时，你可能只想读取文件的一小部分或逐块对文件进行迭代。

在看大文件之前，我们先设置`pandas`显示地更紧些：
```python
result = pd.read_csv('examples/ex6.csv')
result
           one       two     three      four key
0     0.467976 -0.038649 -0.295344 -1.824726   L
1    -0.358893  1.404453  0.704965 -0.200638   B
2    -0.501840  0.659254 -0.421691 -0.057688   G
3     0.204886  1.074134  1.388361 -0.982404   R
4     0.354628 -0.133116  0.283763 -0.837063   Q
...        ...       ...       ...       ...  ..
9995  2.311896 -0.417070 -1.409599 -0.515821   L
9996 -0.479893 -0.650419  0.745152 -0.646038   E
9997  0.523331  0.787112  0.486066  1.093156   K
9998 -0.362559  0.598894 -1.843201  0.887292   G
9999 -0.096376 -1.012999 -0.657431 -0.573315   0
[10000 rows x 5 columns]
```
如果只想读取几行（避免读取整个文件），通过`nrows`进行指定即可：
```python
pd.read_csv('examples/ex6.csv', nrows=5)

        one       two     three      four key
0  0.467976 -0.038649 -0.295344 -1.824726   L
1 -0.358893  1.404453  0.704965 -0.200638   B
2 -0.501840  0.659254 -0.421691 -0.057688   G
3  0.204886  1.074134  1.388361 -0.982404   R
4  0.354628 -0.133116  0.283763 -0.837063   Q
```
要逐块读取文件，可以指定`chunksize`（行数）：
```python
chunker = pd.read_csv('ch06/ex6.csv', chunksize=1000)
chunker
<pandas.io.parsers.TextParser at 0x8398150>
```
`read_csv`所返回的这个`TextParser`对象使你可以根据`chunksize`对文件进行逐块迭代。比如说，我们可以迭代处理`ex6.csv`，将值计数聚合到`"key"`列中，如下所示：
```python
chunker = pd.read_csv('examples/ex6.csv', chunksize=1000)

tot = pd.Series([])
for piece in chunker:
    tot = tot.add(piece['key'].value_counts(), fill_value=0)

tot = tot.sort_values(ascending=False)
```
然后有：
```python
tot[:10]
E    368.0
X    364.0
L    346.0
O    343.0
Q    340.0
M    338.0
J    337.0
F    335.0
K    334.0
H    330.0
dtype: float64
```
`TextParser`还有一个`get_chunk`方法，它使你可以读取任意大小的块。

#### 将数据写出到文本格式
数据也可以被输出为分隔符格式的文本。我们再来看看之前读过的一个`CSV`文件：
```python
data = pd.read_csv('examples/ex5.csv')
data
  something  a   b     c   d message
0       one  1   2   3.0   4     NaN
1       two  5   6   NaN   8   world
2     three  9  10  11.0  12     foo
```
利用`DataFrame`的`to_csv`方法，我们可以将数据写到一个以逗号分隔的文件中：
```python
data.to_csv('examples/out.csv')
,something,a,b,c,d,message
0,one,1,2,3.0,4,
1,two,5,6,,8,world
2,three,9,10,11.0,12,foo
```
当然，还可以使用其他分隔符（由于这里直接写出到`sys.stdout`，所以仅仅是打印出文本结果而已）：
```python
import sys
data.to_csv(sys.stdout, sep='|')
|something|a|b|c|d|message
0|one|1|2|3.0|4|
1|two|5|6||8|world
2|three|9|10|11.0|12|foo
```
缺失值在输出结果中会被表示为空字符串。你可能希望将其表示为别的标记值：
```python
data.to_csv(sys.stdout, na_rep='NULL')
,something,a,b,c,d,message
0,one,1,2,3.0,4,NULL
1,two,5,6,NULL,8,world
2,three,9,10,11.0,12,foo
```
如果没有设置其他选项，则会写出行和列的标签。当然，它们也都可以被禁用：
```python
data.to_csv(sys.stdout, index=False, header=False)
one,1,2,3.0,4,
two,5,6,,8,world
three,9,10,11.0,12,foo
```
此外，你还可以只写出一部分的列，并以你指定的顺序排列：
```python
data.to_csv(sys.stdout, index=False, columns=['a', 'b', 'c'])
a,b,c
1,2,3.0
5,6,
9,10,11.0
```
`Series`也有一个`to_csv`方法：
```python
dates = pd.date_range('1/1/2000', periods=7)
ts = pd.Series(np.arange(7), index=dates)
ts.to_csv('examples/tseries.csv')
2000-01-01,0
2000-01-02,1
2000-01-03,2
2000-01-04,3
2000-01-05,4
2000-01-06,5
2000-01-07,6
```
#### 处理分隔符格式
大部分存储在磁盘上的表格型数据都能用`pandas.read_table`进行加载。然而，有时还是需要做一些手工处理。由于接收到含有畸形行的文件而使`read_table`出毛病的情况并不少见。为了说明这些基本工具，看看下面这个简单的`CSV`文件：
```python
"a","b","c"
"1","2","3"
"1","2","3"
```
对于任何单字符分隔符文件，可以直接使用`Python`内置的`csv`模块。将任意已打开的文件或文件型的对象传给`csv.reader`：
```python
import csv
f = open('examples/ex7.csv')

reader = csv.reader(f)
```
对这个`reader`进行迭代将会为每行产生一个元组（并移除了所有的引号）：
```python
for line in reader:
    print(line)
['a', 'b', 'c']
['1', '2', '3']
['1', '2', '3']
```
现在，为了使数据格式合乎要求，你需要对其做一些整理工作。我们一步一步来做。首先，读取文件到一个多行的列表中：
```python
with open('examples/ex7.csv') as f:
    lines = list(csv.reader(f))
```
然后，我们将这些行分为标题行和数据行：
```python
header, values = lines[0], lines[1:]
```
然后，我们可以用字典构造式和`zip(*values)`，后者将行转置为列，创建数据列的字典：
```python
data_dict = {h: v for h, v in zip(header, zip(*values))}
data_dict
{'a': ('1', '1'), 'b': ('2', '2'), 'c': ('3', '3')}
```
`CSV`文件的形式有很多。只需定义`csv.Dialect`的一个子类即可定义出新格式（如专门的分隔符、字符串引用约定、行结束符等）：
```python
class my_dialect(csv.Dialect):
    lineterminator = '\n'
    delimiter = ';'
    quotechar = '"'
    quoting = csv.QUOTE_MINIMAL
reader = csv.reader(f, dialect=my_dialect)
```
各个`CSV`语支的参数也可以用关键字的形式提供给`csv.reader`，而无需定义子类：
```python
reader = csv.reader(f, delimiter='|')
```
可用的选项（`csv.Dialect`的属性）及其功能如下表所示。
笔记：对于那些使用复杂分隔符或多字符分隔符的文件，`csv`模块就无能为力了。这种情况下，你就只能使用字符串的`split`方法或正则表达式方法`re.split`进行行拆分和其他整理工作了。
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200407051656.png)
要手工输出分隔符文件，你可以使用`csv.writer`。它接受一个已打开且可写的文件对象以及跟`csv.reader`相同的那些语支和格式化选项：
```python
with open('mydata.csv', 'w') as f:
    writer = csv.writer(f, dialect=my_dialect)
    writer.writerow(('one', 'two', 'three'))
    writer.writerow(('1', '2', '3'))
    writer.writerow(('4', '5', '6'))
    writer.writerow(('7', '8', '9'))
```
#### `JSON`数据
`JSON`（JavaScript Object Notation的简称）已经成为通过`HTTP`请求在`Web`浏览器和其他应用程序之间发送数据的标准格式之一。它是一种比表格型文本格式（如`CSV`）灵活得多的数据格式。下面是一个例子：
```js
obj = """
{"name": "Wes",
 "places_lived": ["United States", "Spain", "Germany"],
 "pet": null,
 "siblings": [{"name": "Scott", "age": 30, "pets": ["Zeus", "Zuko"]},
              {"name": "Katie", "age": 38,
               "pets": ["Sixes", "Stache", "Cisco"]}]
}
"""
```
除其空值`null`和一些其他的细微差别（如列表末尾不允许存在多余的逗号）之外，`JSON`非常接近于有效的`Python`代码。基本类型有对象（字典）、数组（列表）、字符串、数值、布尔值以及`null`。对象中所有的键都必须是字符串。许多`Python`库都可以读写`JSON`数据。我将使用`json`，因为它是构建`于Python`标准库中的。通过`json.loads`即可将`JSON`字符串转换成`Python`形式：
```python
import json
result = json.loads(obj)
result
{'name': 'Wes',
 'pet': None,
 'places_lived': ['United States', 'Spain', 'Germany'],
 'siblings': [{'age': 30, 'name': 'Scott', 'pets': ['Zeus', 'Zuko']},
  {'age': 38, 'name': 'Katie', 'pets': ['Sixes', 'Stache', 'Cisco']}]}
  ```
`json.dumps`则将`Python`对象转换成`JSON`格式：
```python
asjson = json.dumps(result)
```
如何将（一个或一组）`JSON`对象转换为`DataFrame`或其他便于分析的数据结构就由你决定了。最简单方便的方式是：向`DataFrame`构造器传入一个字典的列表（就是原先的`JSON`对象），并选取数据字段的子集：
```python
siblings = pd.DataFrame(result['siblings'], columns=['name', 'age'])
siblings
    name  age
0  Scott   30
1  Katie   38
```
`pandas.read_json`可以自动将特别格式的`JSON`数据集转换为`Series`或`DataFrame`。例如：
```python
[{"a": 1, "b": 2, "c": 3},
 {"a": 4, "b": 5, "c": 6},
 {"a": 7, "b": 8, "c": 9}]
 ```
`pandas.read_json`的默认选项假设`JSON`数组中的每个对象是表格中的一行：
```python
data = pd.read_json('examples/example.json')
data
   a  b  c
0  1  2  3
1  4  5  6
2  7  8  9
```
如果你需要将数据从`pandas`输出到`JSON`，可以使用`to_json`方法：
```python
print(data.to_json())
{"a":{"0":1,"1":4,"2":7},"b":{"0":2,"1":5,"2":8},"c":{"0":3,"1":6,"2":9}}
print(data.to_json(orient='records'))
[{"a":1,"b":2,"c":3},{"a":4,"b":5,"c":6},{"a":7,"b":8,"c":9}]
```
### 二进制数据格式
实现数据的高效二进制格式存储最简单的办法之一是使用`Python`内置的`pickle`序列化。`pandas`对象都有一个用于将数据以`pickle`格式保存到磁盘上的`to_pickle`方法：
```python
frame = pd.read_csv('examples/ex1.csv')
frame
   a   b   c   d message
0  1   2   3   4   hello
1  5   6   7   8   world
2  9  10  11  12     foo
frame.to_pickle('examples/frame_pickle')
```
你可以通过`pickle`直接读取被`pickle`化的数据，或是使用更为方便的`pandas.read_pickle`：
```python
pd.read_pickle('examples/frame_pickle')
   a   b   c   d message
0  1   2   3   4   hello
1  5   6   7   8   world
2  9  10  11  12     foo
```
注意：`pickle`仅建议用于短期存储格式。其原因是很难保证该格式永远是稳定的；今天`pickle`的对象可能无法被后续版本的库`unpickle`出来。虽然我尽力保证这种事情不会发生在`pandas`中，但是今后的某个时候说不定还是得“打破”该`pickle`格式。

`pandas`内置支持两个二进制数据格式：`HDF5`和`MessagePack`。

#### 使用`HDF5`格式
`HDF5`是一种存储大规模科学数组数据的非常好的文件格式。它可以被作为`C`标准库。`HDF5`中的`HDF`指的是层次型数据格式（hierarchical data format）。每个`HDF5`文件都含有一个文件系统式的节点结构，它使你能够存储多个数据集并支持元数据。与其他简单格式相比，`HDF5`支持多种压缩器的即时压缩，还能更高效地存储重复模式数据。对于那些非常大的无法直接放入内存的数据集，`HDF5`就是不错的选择，因为它可以高效地分块读写。

虽然可以用`PyTables`或`h5py`库直接访问`HDF5`文件，`pandas`提供了更为高级的接口，可以简化存储`Series`和`DataFrame`对象。`HDFStore`类可以像字典一样，处理低级的细节：
```python
frame = pd.DataFrame({'a': np.random.randn(100)})

store = pd.HDFStore('mydata.h5')
store['obj1'] = frame
store['obj1_col'] = frame['a']
store
<class 'pandas.io.pytables.HDFStore'>
File path: mydata.h5
/obj1                frame        (shape->[100,1])                               
        
/obj1_col            series       (shape->[100])                                 
        
/obj2                frame_table  (typ->appendable,nrows->100,ncols->1,indexers->
[index])
/obj3                frame_table  (typ->appendable,nrows->100,ncols->1,indexers->
[index])
```
`HDF5`文件中的对象可以通过与字典一样的`API`进行获取：
```python
store['obj1']
           a
0  -0.204708
1   0.478943
2  -0.519439
3  -0.555730
4   1.965781
..       ...
95  0.795253
96  0.118110
97 -0.748532
98  0.584970
99  0.152677
[100 rows x 1 columns]
```
`HDFStore`支持两种存储模式，`'fixed'`和`'table'`。后者通常会更慢，但是支持使用特殊语法进行查询操作：
```python
store.put('obj2', frame, format='table')

store.select('obj2', where=['index >= 10 and index <= 15'])

           a
10  1.007189
11 -1.296221
12  0.274992
13  0.228913
14  1.352917
15  0.886429

store.close()
```
`put`是`store['obj2'] = frame`方法的显示版本，允许我们设置其它的选项，比如格式。

`pandas.read_hdf`函数可以快捷使用这些工具：
```python
frame.to_hdf('mydata.h5', 'obj3', format='table')
pd.read_hdf('mydata.h5', 'obj3', where=['index < 5'])
          a
0 -0.204708
1  0.478943
2 -0.519439
3 -0.555730
4  1.965781
```

如果需要本地处理海量数据，我建议你好好研究一下`PyTables`和`h5py`，看看它们能满足你的哪些需求。。由于许多数据分析问题都是`IO`密集型（而不是`CPU`密集型），利用`HDF5`这样的工具能显著提升应用程序的效率。

注意：`HDF5`不是数据库。它最适合用作“一次写多次读”的数据集。虽然数据可以在任何时候被添加到文件中，但如果同时发生多个写操作，文件就可能会被破坏。

## 数据清洗和准备
### 处理缺失数据
在许多数据分析工作中，缺失数据是经常发生的。`pandas`的目标之一就是尽量轻松地处理缺失数据。例如，`pandas`对象的所有描述性统计默认都不包括缺失数据。

缺失数据在`pandas`中呈现的方式有些不完美，但对于大多数用户可以保证功能正常。对于数值数据，`pandas`使用浮点值`NaN`（`Not a Number`）表示缺失数据。我们称其为哨兵值，可以方便的检测出来：
```python
string_data = pd.Series(['aardvark', 'artichoke', np.nan, 'avocado'])
string_data
0     aardvark
1    artichoke
2          NaN
3      avocado
dtype: object
string_data.isnull()
0    False
1    False
2     True
3    False
dtype: bool
```
在`pandas`中，我们采用了`R`语言中的惯用法，即将缺失值表示为`NA`，它表示不可用`not available`。在统计应用中，`NA`数据可能是不存在的数据或者虽然存在，但是没有观察到（例如，数据采集中发生了问题）。当进行数据清洗以进行分析时，最好直接对缺失数据进行分析，以判断数据采集的问题或缺失数据可能导致的偏差。

`Python`内置的`None`值在对象数组中也可以作为`NA`：
```python
string_data[0] = None
string_data.isnull()
0     True
1    False
2     True
3    False
dtype: bool
```
`pandas`项目中还在不断优化内部细节以更好处理缺失数据，像用户`API`功能，例如`pandas.isnull`，去除了许多恼人的细节。下表列出了一些关于缺失数据处理的函数。
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200407145654.png)
#### 滤除缺失数据
过滤掉缺失数据的办法有很多种。你可以通过`pandas.isnull`或布尔索引的手工方法，但`dropna`可能会更实用一些。对于一个`Series`，`dropna`返回一个仅含非空数据和索引值的`Series`：
```python
from numpy import nan as NA
data = pd.Series([1, NA, 3.5, NA, 7])
data.dropna()
0    1.0
2    3.5
4    7.0
dtype: float64
```
这等价于：
```python
data[data.notnull()]
0    1.0
2    3.5
4    7.0
dtype: float64
```
而对于`DataFrame`对象，事情就有点复杂了。你可能希望丢弃全`NA`或含有`NA`的行或列。`dropna`默认丢弃任何含有缺失值的行：
```python
data = pd.DataFrame([[1., 6.5, 3.], [1., NA, NA],[NA, NA, NA], [NA, 6.5, 3.]])
cleaned = data.dropna()
data
     0    1    2
0  1.0  6.5  3.0
1  1.0  NaN  NaN
2  NaN  NaN  NaN
3  NaN  6.5  3.0
cleaned
     0    1    2
0  1.0  6.5  3.0
```
传入`how='all'`将只丢弃全为`NA`的那些行：
```python
data.dropna(how='all')
     0    1    2
0  1.0  6.5  3.0
1  1.0  NaN  NaN
3  NaN  6.5  3.0
```
用这种方式丢弃列，只需传入`axis=1`即可：
```python
data[4] = NA
data 
     0    1    2   4
0  1.0  6.5  3.0 NaN
1  1.0  NaN  NaN NaN
2  NaN  NaN  NaN NaN
3  NaN  6.5  3.0 NaN
data.dropna(axis=1, how='all')
     0    1    2
0  1.0  6.5  3.0
1  1.0  NaN  NaN
2  NaN  NaN  NaN
3  NaN  6.5  3.0
```
另一个滤除`DataFrame`行的问题涉及时间序列数据。假设你只想留下一部分观测数据，可以用`thresh`参数实现此目的：
```python
df = pd.DataFrame(np.random.randn(7, 3))
df.iloc[:4, 1] = NA
df.iloc[:2, 2] = NA
df
          0         1         2
0 -0.204708       NaN       NaN
1 -0.555730       NaN       NaN
2  0.092908       NaN  0.769023
3  1.246435       NaN -1.296221
4  0.274992  0.228913  1.352917
5  0.886429 -2.001637 -0.371843
6  1.669025 -0.438570 -0.539741
df.dropna()
          0         1         2
4  0.274992  0.228913  1.352917
5  0.886429 -2.001637 -0.371843
6  1.669025 -0.438570 -0.539741
df.dropna(thresh=2)
          0         1         2
2  0.092908       NaN  0.769023
3  1.246435       NaN -1.296221
4  0.274992  0.228913  1.352917
5  0.886429 -2.001637 -0.371843
6  1.669025 -0.438570 -0.539741
```
#### 填充缺失数据
你可能不想滤除缺失数据（有可能会丢弃跟它有关的其他数据），而是希望通过其他方式填补那些“空洞”。对于大多数情况而言，`fillna`方法是最主要的函数。通过一个常数调用`fillna`就会将缺失值替换为那个常数值：
```python
df.fillna(0)
          0         1         2
0 -0.204708  0.000000  0.000000
1 -0.555730  0.000000  0.000000
2  0.092908  0.000000  0.769023
3  1.246435  0.000000 -1.296221
4  0.274992  0.228913  1.352917
5  0.886429 -2.001637 -0.371843
6  1.669025 -0.438570 -0.539741
```
若是通过一个字典调用`fillna`，就可以实现对不同的列填充不同的值：
```python
df.fillna({1: 0.5, 2: 0})
          0         1         2
0 -0.204708  0.500000  0.000000
1 -0.555730  0.500000  0.000000
2  0.092908  0.500000  0.769023
3  1.246435  0.500000 -1.296221
4  0.274992  0.228913  1.352917
5  0.886429 -2.001637 -0.371843
6  1.669025 -0.438570 -0.539741
```
`fillna`默认会返回新对象，但也可以对现有对象进行就地修改：
```python
_ = df.fillna(0, inplace=True)
df
          0         1         2
0 -0.204708  0.000000  0.000000
1 -0.555730  0.000000  0.000000
2  0.092908  0.000000  0.769023
3  1.246435  0.000000 -1.296221
4  0.274992  0.228913  1.352917
5  0.886429 -2.001637 -0.371843
6  1.669025 -0.438570 -0.539741
```
对`reindexing`有效的那些插值方法也可用于`fillna`：
```python
df = pd.DataFrame(np.random.randn(6, 3))
df.iloc[2:, 1] = NA
df.iloc[4:, 2] = NA
df 
          0         1         2
0  0.476985  3.248944 -1.021228
1 -0.577087  0.124121  0.302614
2  0.523772       NaN  1.343810
3 -0.713544       NaN -2.370232
4 -1.860761       NaN       NaN
5 -1.265934       NaN       NaN
df.fillna(method='ffill')
          0         1         2
0  0.476985  3.248944 -1.021228
1 -0.577087  0.124121  0.302614
2  0.523772  0.124121  1.343810
3 -0.713544  0.124121 -2.370232
4 -1.860761  0.124121 -2.370232
5 -1.265934  0.124121 -2.370232
df.fillna(method='ffill', limit=2)
          0         1         2
0  0.476985  3.248944 -1.021228
1 -0.577087  0.124121  0.302614
2  0.523772  0.124121  1.343810
3 -0.713544  0.124121 -2.370232
4 -1.860761       NaN -2.370232
5 -1.265934       NaN -2.370232
```
只要有些创新，你就可以利用`fillna`实现许多别的功能。比如说，你可以传入`Series`的平均值或中位数：
```python
data = pd.Series([1., NA, 3.5, NA, 7])
data.fillna(data.mean())
0    1.000000
1    3.833333
2    3.500000
3    3.833333
4    7.000000
dtype: float64
```
下表列出了`fillna`的参考。
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200407151513.png)
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200407151527.png)
### 数据转换
#### 移除重复数据
`DataFrame`中出现重复行有多种原因。下面就是一个例子：
```python
data = pd.DataFrame({'k1': ['one', 'two'] * 3 + ['two'],'k2': [1, 1, 2, 3, 3, 4, 4]})
data
    k1  k2
0  one   1
1  two   1
2  one   2
3  two   3
4  one   3
5  two   4
6  two   4
```
`DataFrame`的`duplicated`方法返回一个布尔型`Series`，表示各行是否是重复行（前面出现过的行）：
```python
data.duplicated()
0    False
1    False
2    False
3    False
4    False
5    False
6     True
dtype: bool
```
还有一个与此相关的`drop_duplicates`方法，它会返回一个`DataFrame`，重复的数组会标为`False`：
```python
data.drop_duplicates()
    k1  k2
0  one   1
1  two   1
2  one   2
3  two   3
4  one   3
5  two   4
```
这两个方法默认会判断全部列，你也可以指定部分列进行重复项判断。假设我们还有一列值，且只希望根据`k1`列过滤重复项：
```python
data['v1'] = range(7)
data.drop_duplicates(['k1'])
    k1  k2  v1
0  one   1   0
1  two   1   1
```
`duplicated`和`drop_duplicates`默认保留的是第一个出现的值组合。传入`keep='last'`则保留最后一个：
```python
data.drop_duplicates(['k1', 'k2'], keep='last')
    k1  k2  v1
0  one   1   0
1  two   1   1
2  one   2   2
3  two   3   3
4  one   3   4
6  two   4   6
```
#### 利用函数或映射进行数据转换
对于许多数据集，你可能希望根据数组、`Series`或`DataFrame`列中的值来实现转换工作。我们来看看下面这组有关肉类的数据：
```python
data = pd.DataFrame({'food': ['bacon', 'pulled pork', 'bacon','Pastrami', 'corned beef', 'Bacon','pastrami', 'honey ham', 'nova lox'],'ounces': [4, 3, 12, 6, 7.5, 8, 3, 5, 6]})
data
          food  ounces
0        bacon     4.0
1  pulled pork     3.0
2        bacon    12.0
3     Pastrami     6.0
4  corned beef     7.5
5        Bacon     8.0
6     pastrami     3.0
7    honey ham     5.0
8     nova lox     6.0
```
假设你想要添加一列表示该肉类食物来源的动物类型。我们先编写一个不同肉类到动物的映射：
```python
meat_to_animal = {
  'bacon': 'pig',
  'pulled pork': 'pig',
  'pastrami': 'cow',
  'corned beef': 'cow',
  'honey ham': 'pig',
  'nova lox': 'salmon'
}
```
`Series`的`map`方法可以接受一个函数或含有映射关系的字典型对象，但是这里有一个小问题，即有些肉类的首字母大写了，而另一些则没有。因此，我们还需要使用`Series`的`str.lower`方法，将各个值转换为小写：
```python
lowercased = data['food'].str.lower()
lowercased
0          bacon
1    pulled pork
2          bacon
3       pastrami
4    corned beef
5          bacon
6       pastrami
7      honey ham
8       nova lox
Name: food, dtype: object
data['animal'] = lowercased.map(meat_to_animal)
data
          food  ounces  animal
0        bacon     4.0     pig
1  pulled pork     3.0     pig
2        bacon    12.0     pig
3     Pastrami     6.0     cow
4  corned beef     7.5     cow
5        Bacon     8.0     pig
6     pastrami     3.0     cow
7    honey ham     5.0     pig
8     nova lox     6.0  salmon
```
我们也可以传入一个能够完成全部这些工作的函数：
```python
data['food'].map(lambda x: meat_to_animal[x.lower()])
0       pig
1       pig
2       pig
3       cow
4       cow
5       pig
6       cow
7       pig
8    salmon
Name: food, dtype: object
```
使用`map`是一种实现元素级转换以及其他数据清理工作的便捷方式。
#### 替换值
利用`fillna`方法填充缺失数据可以看做值替换的一种特殊情况。前面已经看到，`map`可用于修改对象的数据子集，而`replace`则提供了一种实现该功能的更简单、更灵活的方式。我们来看看下面这个`Series`：
```python
data = pd.Series([1., -999., 2., -999., -1000., 3.])
data
0       1.0
1    -999.0
2       2.0
3    -999.0
4   -1000.0
5       3.0
```
-999这个值可能是一个表示缺失数据的标记值。要将其替换为`pandas`能够理解的`NA`值，我们可以利用`replace`来产生一个新的`Series`（除非传入`inplace=True`）：
```python
data.replace(-999, np.nan)
0       1.0
1       NaN
2       2.0
3       NaN
4   -1000.0
5       3.0
dtype: float64
```
如果你希望一次性替换多个值，可以传入一个由待替换值组成的列表以及一个替换值：：
```python
data.replace([-999, -1000], np.nan)
0    1.0
1    NaN
2    2.0
3    NaN
4    NaN
5    3.0
dtype: float64
```
要让每个值有不同的替换值，可以传递一个替换列表：
```python
data.replace([-999, -1000], [np.nan, 0])
0    1.0
1    NaN
2    2.0
3    NaN
4    0.0
5    3.0
dtype: float64
```
传入的参数也可以是字典：
```python
data.replace({-999: np.nan, -1000: 0})
0    1.0
1    NaN
2    2.0
3    NaN
4    0.0
5    3.0
dtype: float64
```
笔记：`data.replace`方法与`data.str.replace`不同，后者做的是字符串的元素级替换。我们会在后面学习`Series`的字符串方法。

#### 重命名轴索引
跟`Series`中的值一样，轴标签也可以通过函数或映射进行转换，从而得到一个新的不同标签的对象。轴还可以被就地修改，而无需新建一个数据结构。接下来看看下面这个简单的例子：
```python
data = pd.DataFrame(np.arange(12).reshape((3, 4)),index=['Ohio', 'Colorado', 'New York'],columns=['one', 'two', 'three', 'four'])
```
跟`Series`一样，轴索引也有一个`map`方法：
```python
transform = lambda x: x[:4].upper()
data.index.map(transform)
Index(['OHIO', 'COLO', 'NEW '], dtype='object')
```
你可以将其赋值给`index`，这样就可以对`DataFrame`进行就地修改：
```python
data.index = data.index.map(transform)
data
one  two  three  four
OHIO    0    1      2     3
COLO    4    5      6     7
NEW     8    9     10    11
```
如果想要创建数据集的转换版（而不是修改原始数据），比较实用的方法是`rename`：
```python
data.rename(index=str.title, columns=str.upper)
      ONE  TWO  THREE  FOUR
Ohio    0    1      2     3
Colo    4    5      6     7
New     8    9     10    11
```
特别说明一下，`rename`可以结合字典型对象实现对部分轴标签的更新：
```python
data.rename(index={'OHIO': 'INDIANA'},columns={'three': 'peekaboo'})
one  two  peekaboo  four
INDIANA    0    1         2     3
COLO       4    5         6     7
NEW        8    9        10    11
```
`rename`可以实现复制`DataFrame`并对其索引和列标签进行赋值。如果希望就地修改某个数据集，传入`inplace=True`即可：
```python
data.rename(index={'OHIO': 'INDIANA'}, inplace=True)
data
         one  two  three  four
INDIANA    0    1      2     3
COLO       4    5      6     7
NEW        8    9     10    11
```
#### 离散化和面元划分
为了便于分析，连续数据常常被离散化或拆分为“面元”（`bin`）。假设有一组人员数据，而你希望将它们划分为不同的年龄组：
```python
ages = [20, 22, 25, 27, 21, 23, 37, 31, 61, 45, 41, 32]
```
接下来将这些数据划分为“18到25”、“26到35”、“35到60”以及“60以上”几个面元。要实现该功能，你需要使用`pandas`的`cut`函数：
```python
bins = [18, 25, 35, 60, 100]
cats = pd.cut(ages, bins)
cats
[(18, 25], (18, 25], (18, 25], (25, 35], (18, 25], ..., (25, 35], (60, 100], (35,60], (35, 60], (25, 35]]
Length: 12
Categories (4, interval[int64]): [(18, 25] < (25, 35] < (35, 60] < (60, 100]]
```
`pandas`返回的是一个特殊的`Categorical`对象。结果展示了`pandas.cut`划分的面元。你可以将其看做一组表示面元名称的字符串。它的底层含有一个表示不同分类名称的类型数组，以及一个`codes`属性中的年龄数据的标签：
```python
cats.codes
array([0, 0, 0, 1, 0, 0, 2, 1, 3, 2, 2, 1], dtype=int8)
cats.categories
IntervalIndex([(18, 25], (25, 35], (35, 60], (60, 100]]closed='right',dtype='interval[int64]')
pd.value_counts(cats)
(18, 25]     5
(35, 60]     3
(25, 35]     3
(60, 100]    1
dtype: int64
```
`pd.value_counts(cats)`是`pandas.cut`结果的面元计数。

跟“区间”的数学符号一样，圆括号表示开端，而方括号则表示闭端（包括）。哪边是闭端可以通过`right=False`进行修改：
```python
pd.cut(ages, [18, 26, 36, 61, 100], right=False)
[[18, 26), [18, 26), [18, 26), [26, 36), [18, 26), ..., [26, 36), [61, 100), [36,
 61), [36, 61), [26, 36)]
Length: 12
Categories (4, interval[int64]): [[18, 26) < [26, 36) < [36, 61) < [61, 100)]
```
你可 以通过传递一个列表或数组到`labels`，设置自己的面元名称：
```python
group_names = ['Youth', 'YoungAdult', 'MiddleAged', 'Senior']
pd.cut(ages, bins, labels=group_names)
[Youth, Youth, Youth, YoungAdult, Youth, ..., YoungAdult, Senior, MiddleAged, Mid
dleAged, YoungAdult]
Length: 12
Categories (4, object): [Youth < YoungAdult < MiddleAged < Senior]
```
如果向`cut`传入的是面元的数量而不是确切的面元边界，则它会根据数据的最小值和最大值计算等长面元。下面这个例子中，我们将一些均匀分布的数据分成四组：
```python
data = np.random.rand(20)
pd.cut(data, 4, precision=2)
[(0.34, 0.55], (0.34, 0.55], (0.76, 0.97], (0.76, 0.97], (0.34, 0.55], ..., (0.34
, 0.55], (0.34, 0.55], (0.55, 0.76], (0.34, 0.55], (0.12, 0.34]]
Length: 20
Categories (4, interval[float64]): [(0.12, 0.34] < (0.34, 0.55] < (0.55, 0.76] < 
(0.76, 0.97]]
```
选项`precision=2`，限定小数只有两位。

`qcut`是一个非常类似于`cut`的函数，它可以根据样本分位数对数据进行面元划分。根据数据的分布情况，`cut`可能无法使各个面元中含有相同数量的数据点。而`qcut`由于使用的是样本分位数，因此可以得到大小基本相等的面元：
```python
data = np.random.randn(1000)  # Normally distributed
cats = pd.qcut(data, 4)  # Cut into quartiles
cats
[(-0.0265, 0.62], (0.62, 3.928], (-0.68, -0.0265], (0.62, 3.928], (-0.0265, 0.62]
, ..., (-0.68, -0.0265], (-0.68, -0.0265], (-2.95, -0.68], (0.62, 3.928], (-0.68,
 -0.0265]]
Length: 1000
Categories (4, interval[float64]): [(-2.95, -0.68] < (-0.68, -0.0265] < (-0.0265,
 0.62] <(0.62, 3.928]]
pd.value_counts(cats)
(0.62, 3.928]       250
(-0.0265, 0.62]     250
(-0.68, -0.0265]    250
(-2.95, -0.68]      250
dtype: int64
```
与`cut`类似，你也可以传递自定义的分位数（0到1之间的数值，包含端点）：
```python
pd.qcut(data, [0, 0.1, 0.5, 0.9, 1.])
[(-0.0265, 1.286], (-0.0265, 1.286], (-1.187, -0.0265], (-0.0265, 1.286], (-0.026
5, 1.286], ..., (-1.187, -0.0265], (-1.187, -0.0265], (-2.95, -1.187], (-0.0265, 
1.286], (-1.187, -0.0265]]
Length: 1000
Categories (4, interval[float64]): [(-2.95, -1.187] < (-1.187, -0.0265] < (-0.026
5, 1.286] <(1.286, 3.928]]
```
本章稍后在讲解聚合和分组运算时会再次用到`cut`和`qcut`，因为这两个离散化函数对分位和分组分析非常重要。
#### 检测和过滤异常值
过滤或变换异常值（`outlier`）在很大程度上就是运用数组运算。来看一个含有正态分布数据的`DataFrame`：
```python
data = pd.DataFrame(np.random.randn(1000, 4))
data.describe()
                 0            1            2            3
count  1000.000000  1000.000000  1000.000000  1000.000000
mean      0.049091     0.026112    -0.002544    -0.051827
std       0.996947     1.007458     0.995232     0.998311
min      -3.645860    -3.184377    -3.745356    -3.428254
25%      -0.599807    -0.612162    -0.687373    -0.747478
50%       0.047101    -0.013609    -0.022158    -0.088274
75%       0.756646     0.695298     0.699046     0.623331
max       2.653656     3.525865     2.735527     3.366626
```
假设你想要找出某列中绝对值大小超过3的值：
```python
col = data[2]
col[np.abs(col) > 3]
41    -3.399312
136   -3.745356
Name: 2, dtype: float64
```
要选出全部含有“超过3或－3的值”的行，你可以在布尔型`DataFrame`中使用`any`方法：
```python
data[(np.abs(data) > 3).any(1)]
            0         1         2         3
41   0.457246 -0.025907 -3.399312 -0.974657
60   1.951312  3.260383  0.963301  1.201206
136  0.508391 -0.196713 -3.745356 -1.520113
235 -0.242459 -3.056990  1.918403 -0.578828
258  0.682841  0.326045  0.425384 -3.428254
322  1.179227 -3.184377  1.369891 -1.074833
544 -3.548824  1.553205 -2.186301  1.277104
635 -0.578093  0.193299  1.397822  3.366626
782 -0.207434  3.525865  0.283070  0.544635
803 -3.645860  0.255475 -0.549574 -1.907459
```
根据这些条件，就可以对值进行设置。下面的代码可以将值限制在区间－3到3以内：
```python
data[np.abs(data) > 3] = np.sign(data) * 3
data.describe()
                 0            1            2            3
count  1000.000000  1000.000000  1000.000000  1000.000000
mean      0.050286     0.025567    -0.001399    -0.051765
std       0.992920     1.004214     0.991414     0.995761
min      -3.000000    -3.000000    -3.000000    -3.000000
25%      -0.599807    -0.612162    -0.687373    -0.747478
50%       0.047101    -0.013609    -0.022158    -0.088274
75%       0.756646     0.695298     0.699046     0.623331
max       2.653656     3.000000     2.735527     3.000000
```
根据数据的值是正还是负，`np.sign(data)`可以生成1和-1：
```python
np.sign(data).head()
     0    1    2    3
0 -1.0  1.0 -1.0  1.0
1  1.0 -1.0  1.0 -1.0
2  1.0  1.0  1.0 -1.0
3 -1.0 -1.0  1.0 -1.0
4 -1.0  1.0 -1.0 -1.0
```
#### 排列和随机采样
利用`numpy.random.permutation`函数可以轻松实现对`Series`或`DataFrame`的列的排列工作（`permuting`，随机重排序）。通过需要排列的轴的长度调用`permutation`，可产生一个表示新顺序的整数数组：
```python
df = pd.DataFrame(np.arange(5 * 4).reshape((5, 4)))
sampler = np.random.permutation(5)
sampler
array([3, 1, 4, 2, 0])
```
然后就可以在基于`iloc`的索引操作或`take`函数中使用该数组了：
```python
df
    0   1   2   3
0   0   1   2   3
1   4   5   6   7
2   8   9  10  11
3  12  13  14  15
4  16  17  18  19
df.take(sampler)
    0   1   2   3
3  12  13  14  15
1   4   5   6   7
4  16  17  18  19
2   8   9  10  11
0   0   1   2   3
```
如果不想用替换的方式选取随机子集，可以在`Series`和`DataFrame`上使用`sample`方法：
```python
df.sample(n=3)
    0   1   2   3
3  12  13  14  15
4  16  17  18  19
2   8   9  10  11
```
要通过替换的方式产生样本（允许重复选择），可以传递`replace=True`到`sample`：
```python
choices = pd.Series([5, 7, -1, 6, 4])
draws = choices.sample(n=10, replace=True)
draws
4    4
1    7
4    4
2   -1
0    5
3    6
1    7
4    4
0    5
4    4
dtype: int64
```
#### 计算指标/哑变量
另一种常用于统计建模或机器学习的转换方式是：将分类变量（`categorical variable`）转换为“哑变量”或“指标矩阵”。

如果`DataFrame`的某一列中含有`k`个不同的值，则可以派生出一个`k`列矩阵或`DataFrame`（其值全为1和0）。`pandas`有一个`get_dummies`函数可以实现该功能（其实自己动手做一个也不难）。使用之前的一个`DataFrame`例子：
```python
df = pd.DataFrame({'key': ['b', 'b', 'a', 'c', 'a', 'b'],'data1': range(6)})
pd.get_dummies(df['key'])
   a  b  c
0  0  1  0
1  0  1  0
2  1  0  0
3  0  0  1
4  1  0  0
5  0  1  0
```
有时候，你可能想给指标`DataFrame`的列加上一个前缀，以便能够跟其他数据进行合并。`get_dummies`的`prefix`参数可以实现该功能：
```python
dummies = pd.get_dummies(df['key'], prefix='key')
df_with_dummy = df[['data1']].join(dummies)
df_with_dummy
   data1  key_a  key_b  key_c
0      0      0      1      0
1      1      0      1      0
2      2      1      0      0
3      3      0      0      1
4      4      1      0      0
5      5      0      1      0
```
如果`DataFrame`中的某行同属于多个分类，则事情就会有点复杂。看一下MovieLens 1M数据集，14章会更深入地研究它：
```python
mnames = ['movie_id', 'title', 'genres']
movies = pd.read_table('datasets/movielens/movies.dat', sep='::',header=None, names=mnames)
movies[:10]
   movie_id                               title                        genres
0         1                    Toy Story (1995)   Animation|Children's|Comedy
1         2                      Jumanji (1995)  Adventure|Children's|Fantasy
2         3             Grumpier Old Men (1995)                Comedy|Romance
3         4            Waiting to Exhale (1995)                  Comedy|Drama
4         5  Father of the Bride Part II (1995)                        Comedy
5         6                         Heat (1995)         Action|Crime|Thriller
6         7                      Sabrina (1995)                Comedy|Romance
7         8                 Tom and Huck (1995)          Adventure|Children's
8         9                 Sudden Death (1995)                        Action
9        10                    GoldenEye (1995)     Action|Adventure|Thriller
```
要为每个`genre`添加指标变量就需要做一些数据规整操作。首先，我们从数据集中抽取出不同的`genre`值：
```python
all_genres = []
for x in movies.genres:
    all_genres.extend(x.split('|'))
genres = pd.unique(all_genres)
genres
array(['Animation', "Children's", 'Comedy', 'Adventure', 'Fantasy',
       'Romance', 'Drama', 'Action', 'Crime', 'Thriller','Horror',
       'Sci-Fi', 'Documentary', 'War', 'Musical', 'Mystery', 'Film-Noir',
       'Western'], dtype=object)
```
构建指标`DataFrame`的方法之一是从一个全零`DataFrame`开始：
```python
zero_matrix = np.zeros((len(movies), len(genres)))
dummies = pd.DataFrame(zero_matrix, columns=genres)
```
现在，迭代每一部电影，并将`dummies`各行的条目设为1。要这么做，我们使用`dummies.columns`来计算每个类型的列索引：
```python
gen = movies.genres[0]
gen.split('|')
['Animation', "Children's", 'Comedy']
dummies.columns.get_indexer(gen.split('|'))
array([0, 1, 2])
```
然后，根据索引，使用`.iloc`设定值：
```python
for i, gen in enumerate(movies.genres):
    indices = dummies.columns.get_indexer(gen.split('|'))
    dummies.iloc[i, indices] = 1
```
然后，和以前一样，再将其与movies合并起来：
```python
movies_windic = movies.join(dummies.add_prefix('Genre_'))
movies_windic.iloc[0]
movie_id                                       1
title                           Toy Story (1995)
genres               Animation|Children's|Comedy
Genre_Animation                                1
Genre_Children's                               1
Genre_Comedy                                   1
Genre_Adventure                                0
Genre_Fantasy                                  0
Genre_Romance                                  0
Genre_Drama                                    0
                                ...             
Genre_Crime                                    0
Genre_Thriller                                 0
Genre_Horror                                   0
Genre_Sci-Fi                                   0
Genre_Documentary                              0
Genre_War                                      0
Genre_Musical                                  0
Genre_Mystery                                  0
Genre_Film-Noir                                0
Genre_Western                                  0
Name: 0, Length: 21, dtype: object
```
笔记：对于很大的数据，用这种方式构建多成员指标变量就会变得非常慢。最好使用更低级的函数，将其写入`NumPy`数组，然后结果包装在`DataFrame`中。

一个对统计应用有用的秘诀是：结合`get_dummies`和诸如`cut`之类的离散化函数：
```python
np.random.seed(12345)
values = np.random.rand(10)
values
array([ 0.9296,  0.3164,  0.1839,  0.2046,  0.5677,  0.5955,  0.9645,
        0.6532,  0.7489,  0.6536])
bins = [0, 0.2, 0.4, 0.6, 0.8, 1]
pd.get_dummies(pd.cut(values, bins))
   (0.0, 0.2]  (0.2, 0.4]  (0.4, 0.6]  (0.6, 0.8]  (0.8, 1.0]
0           0           0           0           0           1
1           0           1           0           0           0
2           1           0           0           0           0
3           0           1           0           0           0
4           0           0           1           0           0
5           0           0           1           0           0
6           0           0           0           0           1
7           0           0           0           1           0
8           0           0           0           1           0
9           0           0           0           1           0
```
我们用`numpy.random.seed`，使这个例子具有确定性。本书后面会介绍`pandas.get_dummies`。
### 字符串操作
`Python`能够成为流行的数据处理语言，部分原因是其简单易用的字符串和文本处理功能。大部分文本运算都直接做成了字符串对象的内置方法。对于更为复杂的模式匹配和文本操作，则可能需要用到正则表达式。`pandas`对此进行了加强，它使你能够对整组数据应用字符串表达式和正则表达式，而且能处理烦人的缺失数据。

#### 字符串对象方法
对于许多字符串处理和脚本应用，内置的字符串方法已经能够满足要求了。例如，以逗号分隔的字符串可以用`split`拆分成数段：
```python
val = 'a,b,  guido'
val.split(',')
['a', 'b', '  guido']
```
`split`常常与`strip`一起使用，以去除空白符（包括换行符）：
```python
pieces = [x.strip() for x in val.split(',')]
pieces
['a', 'b', 'guido']
```
利用加法，可以将这些子字符串以双冒号分隔符的形式连接起来：
```python
first, second, third = pieces
first + '::' + second + '::' + third
'a::b::guido'
```
但这种方式并不是很实用。一种更快更符合`Python`风格的方式是，向字符串`"::"`的`join`方法传入一个列表或元组：
```python
'::'.join(pieces)
'a::b::guido'
```
其它方法关注的是子串定位。检测子串的最佳方式是利用`Python`的`in`关键字，还可以使用`index`和`find`：
```python
'guido' in val
True
val.index(',')
1
val.find(':')
-1
```
注意`find`和`index`的区别：如果找不到字符串，`index`将会引发一个异常（而不是返回`－1`）：
```python
val.index(':')
---------------------------------------------------------------------------
ValueError                                Traceback (most recent call last)
<ipython-input-144-280f8b2856ce> in <module>()
----> 1 val.index(':')
ValueError: substring not found
```
与此相关，`count`可以返回指定子串的出现次数：
```python
val.count(',')
2
```
`replace`用于将指定模式替换为另一个模式。通过传入空字符串，它也常常用于删除模式：
```python
val.replace(',', '::')
'a::b::  guido'
val.replace(',', '')
'ab  guido'
```
下表列出了`Python`内置的字符串方法。
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200407190549.png)
这些运算大部分都能使用正则表达式实现（马上就会看到）。
`casefold` 将字符转换为小写，并将任何特定区域的变量字符组合转换成一个通用的可比较形式。

#### 正则表达式
正则表达式提供了一种灵活的在文本中搜索或匹配（通常比前者复杂）字符串模式的方式。正则表达式，常称作`regex`，是根据正则表达式语言编写的字符串。`Python`内置的`re`模块负责对字符串应用正则表达式。我将通过一些例子说明其使用方法。

`re`模块的函数可以分为三个大类：模式匹配、替换以及拆分。当然，它们之间是相辅相成的。一个`regex`描述了需要在文本中定位的一个模式，它可以用于许多目的。我们先来看一个简单的例子：假设我想要拆分一个字符串，分隔符为数量不定的一组空白符（制表符、空格、换行符等）。描述一个或多个空白符的`regex`是`\s+`：
```python
import re
text = "foo    bar\t baz  \tqux"
re.split('\s+', text)
['foo', 'bar', 'baz', 'qux']
```
调用`re.split('\s+',text)`时，正则表达式会先被编译，然后再在`text`上调用其`split`方法。你可以用`re.compile`自己编译`regex`以得到一个可重用的`regex`对象：
```python
regex = re.compile('\s+')
regex.split(text)
['foo', 'bar', 'baz', 'qux']
```
如果只希望得到匹配`regex`的所有模式，则可以使用`findall`方法：
```python
regex.findall(text)
['    ', '\t ', '  \t']
```
笔记：如果想避免正则表达式中不需要的转义（`\`），则可以使用原始字符串字面量如r`'C:\x'`（也可以编写其等价式`'C:\x'`）。

如果打算对许多字符串应用同一条正则表达式，强烈建议通过`re.compile`创建`regex`对象。这样将可以节省大量的`CPU`时间。

`match`和`search`跟`findall`功能类似。`findall`返回的是字符串中所有的匹配项，而`search`则只返回第一个匹配项。`match`更加严格，它只匹配字符串的首部。来看一个小例子，假设我们有一段文本以及一条能够识别大部分电子邮件地址的正则表达式：
```python
text = """Dave dave@google.com
Steve steve@gmail.com
Rob rob@gmail.com
Ryan ryan@yahoo.com
"""
pattern = r'[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}'
#re.IGNORECASE makes the regex case-insensitive
regex = re.compile(pattern, flags=re.IGNORECASE)
```
对`text`使用`findall`将得到一组电子邮件地址：
```python
regex.findall(text)
['dave@google.com',
 'steve@gmail.com',
 'rob@gmail.com',
 'ryan@yahoo.com']
```
`search`返回的是文本中第一个电子邮件地址（以特殊的匹配项对象形式返回）。对于上面那个`regex`，匹配项对象只能告诉我们模式在原字符串中的起始和结束位置：
```python
m = regex.search(text)
m
<_sre.SRE_Match object; span=(5, 20), match='dave@google.com'>
text[m.start():m.end()]
'dave@google.com'
```
`regex.match`则将返回`None`，因为它只匹配出现在字符串开头的模式：
```python
print(regex.match(text))
None
```
相关的，`sub`方法可以将匹配到的模式替换为指定字符串，并返回所得到的新字符串：
```python
print(regex.sub('REDACTED', text))
Dave REDACTED
Steve REDACTED
Rob REDACTED
Ryan REDACTED
```
假设你不仅想要找出电子邮件地址，还想将各个地址分成3个部分：用户名、域名以及域后缀。要实现此功能，只需将待分段的模式的各部分用圆括号包起来即可：
```python
pattern = r'([A-Z0-9._%+-]+)@([A-Z0-9.-]+)\.([A-Z]{2,4})'
regex = re.compile(pattern, flags=re.IGNORECASE)
```
由这种修改过的正则表达式所产生的匹配项对象，可以通过其`groups`方法返回一个由模式各段组成的元组：
```python
m = regex.match('wesm@bright.net')
m.groups()
('wesm', 'bright', 'net')
```
对于带有分组功能的模式，`findall`会返回一个元组列表：
```python
regex.findall(text)
[('dave', 'google', 'com'),
 ('steve', 'gmail', 'com'),
 ('rob', 'gmail', 'com'),
 ('ryan', 'yahoo', 'com')]
 ```
`sub`还能通过诸如`\1`、`\2`之类的特殊符号访问各匹配项中的分组。符号`\1`对应第一个匹配的组，`\2`对应第二个匹配的组，以此类推：
```python
print(regex.sub(r'Username: \1, Domain: \2, Suffix: \3', text))
Dave Username: dave, Domain: google, Suffix: com
Steve Username: steve, Domain: gmail, Suffix: com
Rob Username: rob, Domain: gmail, Suffix: com
Ryan Username: ryan, Domain: yahoo, Suffix: com
```
`Python`中还有许多的正则表达式，但大部分都超出了本书的范围。下表是一个简要概括。
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200407191324.png)
`pandas`的矢量化字符串函数
清理待分析的散乱数据时，常常需要做一些字符串规整化工作。更为复杂的情况是，含有字符串的列有时还含有缺失数据：
```python
data = {'Dave': 'dave@google.com', 'Steve': 'steve@gmail.com','Rob': 'rob@gmail.com', 'Wes': np.nan}
data = pd.Series(data)
data
Dave     dave@google.com
Rob        rob@gmail.com
Steve    steve@gmail.com
Wes                  NaN
dtype: object
data.isnull()
Dave     False
Rob      False
Steve    False
Wes       True
dtype: bool
```
通过`data.map`，所有字符串和正则表达式方法都能被应用于（传入`lambda`表达式或其他函数）各个值，但是如果存在`NA`（`null`）就会报错。为了解决这个问题，`Series`有一些能够跳过`NA`值的面向数组方法，进行字符串操作。通过`Series`的`str`属性即可访问这些方法。例如，我们可以通过`str.contains`检查各个电子邮件地址是否含有"gmail"：
```python
data.str.contains('gmail')
Dave     False
Rob       True
Steve     True
Wes        NaN
dtype: object
```
也可以使用正则表达式，还可以加上任意`re`选项（如IGNORECASE）：
```python
pattern
'([A-Z0-9._%+-]+)@([A-Z0-9.-]+)\\.([A-Z]{2,4})'
data.str.findall(pattern, flags=re.IGNORECASE)
Dave     [(dave, google, com)]
Rob        [(rob, gmail, com)]
Steve    [(steve, gmail, com)]
Wes                        NaN
dtype: object
```
有两个办法可以实现矢量化的元素获取操作：要么使用`str.get`，要么在`str`属性上使用索引：
```python
matches = data.str.match(pattern, flags=re.IGNORECASE)
matches
Dave     True
Rob      True
Steve    True
Wes       NaN
dtype: object
```
要访问嵌入列表中的元素，我们可以传递索引到这两个函数中：
```python
matches.str.get(1)
Dave    NaN
Rob     NaN
Steve   NaN
Wes     NaN
dtype: float64
matches.str[0]
Dave    NaN
Rob     NaN
Steve   NaN
Wes     NaN
dtype: float64
```
你可以利用这种方法对字符串进行截取：
```python
data.str[:5]
Dave     dave@
Rob      rob@g
Steve    steve
Wes        NaN
dtype: object
```
下表介绍了更多的`pandas`字符串方法。
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200407192315.png)
## 数据规整：聚合、合并和重塑
### 层次化索引
层次化索引（hierarchical indexing）是pandas的一项重要功能，它使你能在一个轴上拥有多个（两个以上）索引级别。抽象点说，它使你能以低维度形式处理高维度数据。我们先来看一个简单的例子：创建一个`Series`，并用一个由列表或数组组成的列表作为索引：
```python
data = pd.Series(np.random.randn(9),index=[['a', 'a', 'a', 'b', 'b', 'c', 'c', 'd', 'd'],[1, 2, 3, 1, 3, 1, 2, 2, 3]])
data
a  1   -0.204708
   2    0.478943
   3   -0.519439
b  1   -0.555730
   3    1.965781
c  1    1.393406
   2    0.092908
d  2    0.281746
   3    0.769023
dtype: float64
```
看到的结果是经过美化的带有`MultiIndex`索引的`Series`的格式。索引之间的“间隔”表示“直接使用上面的标签”：
```python
data.index
MultiIndex(levels=[['a', 'b', 'c', 'd'], [1, 2, 3]],labels=[[0, 0, 0, 1, 1, 2, 2, 3, 3], [0, 1, 2, 0, 2, 0, 1, 1, 2]])
```
对于一个层次化索引的对象，可以使用所谓的部分索引，使用它选取数据子集的操作更简单：
```python
data['b']
1   -0.555730
3    1.965781
dtype: float64
data['b':'c']
b  1   -0.555730
   3    1.965781
c  1    1.393406
   2    0.092908
dtype: float64
data.loc[['b', 'd']]
b  1   -0.555730
   3    1.965781
d  2    0.281746
   3    0.769023
dtype: float64
```
有时甚至还可以在“内层”中进行选取：
```python
data.loc[:, 2]
a    0.478943
c    0.092908
d    0.281746
dtype: float64
```
层次化索引在数据重塑和基于分组的操作（如透视表生成）中扮演着重要的角色。例如，可以通过`unstack`方法将这段数据重新安排到一个`DataFrame`中：
```python
data.unstack()
          1         2         3
a -0.204708  0.478943 -0.519439
b -0.555730       NaN  1.965781
c  1.393406  0.092908       NaN
d       NaN  0.281746  0.769023
```
`unstack`的逆运算是`stack`：
```python
data.unstack().stack()
a  1   -0.204708
   2    0.478943
   3   -0.519439
b  1   -0.555730
   3    1.965781
c  1    1.393406
   2    0.092908
d  2    0.281746
   3    0.769023
dtype: float64
```
对于一个`DataFrame`，每条轴都可以有分层索引：
```python
frame = pd.DataFrame(np.arange(12).reshape((4, 3)),index=[['a', 'a', 'b', 'b'], [1, 2, 1, 2]],columns=[['Ohio', 'Ohio', 'Colorado'],
['Green', 'Red', 'Green']])
frame
     Ohio     Colorado
    Green Red    Green
a 1     0   1        2
  2     3   4        5
b 1     6   7        8
  2     9  10       11
```
各层都可以有名字（可以是字符串，也可以是别的`Python`对象）。如果指定了名称，它们就会显示在控制台输出中：
```python
frame.index.names = ['key1', 'key2']
frame.columns.names = ['state', 'color']
frame
state      Ohio     Colorado
color     Green Red    Green
key1 key2                   
a    1        0   1        2
     2        3   4        5
b    1        6   7        8
     2        9  10       11
```
注意：小心区分索引名`state`、`color`与行标签。

有了部分列索引，因此可以轻松选取列分组：
```python
frame['Ohio']
color      Green  Red
key1 key2            
a    1         0    1
     2         3    4
b    1         6    7
     2         9   10
```
可以单独创建`MultiIndex`然后复用。上面那个`DataFrame`中的（带有分级名称）列可以这样创建：
```python
MultiIndex.from_arrays([['Ohio', 'Ohio', 'Colorado'], ['Green', 'Red', 'Green']],
                       names=['state', 'color'])
```
### 重排与分级排序
有时，你需要重新调整某条轴上各级别的顺序，或根据指定级别上的值对数据进行排序。`swaplevel`接受两个级别编号或名称，并返回一个互换了级别的新对象（但数据不会发生变化）：
```python
frame.swaplevel('key1', 'key2') 
state      Ohio     Colorado
color     Green Red    Green
key2 key1                   
1    a        0   1        2
2    a        3   4        5
1    b        6   7        8
2    b        9  10       11
```
而`sort_index`则根据单个级别中的值对数据进行排序。交换级别时，常常也会用到`sort_index`，这样最终结果就是按照指定顺序进行字母排序了：
```python
frame.sort_index(level=1)
state      Ohio     Colorado
color     Green Red    Green
key1 key2                   
a    1        0   1        2
b    1        6   7        8
a    2        3   4        5
b    2        9  10       11
frame.swaplevel(0, 1).sort_index(level=0)
state      Ohio     Colorado
color     Green Red    Green
key2 key1                   
1    a        0   1        2
     b        6   7        8
2    a        3   4        5
     b        9  10       11
```
### 根据级别汇总统计
许多对`DataFrame`和`Series`的描述和汇总统计都有一个`level`选项，它用于指定在某条轴上求和的级别。再以上面那个`DataFrame`为例，我们可以根据行或列上的级别来进行求和：
```python
frame.sum(level='key2')
state  Ohio     Colorado
color Green Red    Green
key2                    
1         6   8       10
2        12  14       16
frame.sum(level='color', axis=1)
color      Green  Red
key1 key2            
a    1         2    1
     2         8    4
b    1        14    7
     2        20   10
```
这其实是利用了`pandas`的`groupby`功能。
### 使用`DataFrame`的列进行索引
人们经常想要将`DataFrame`的一个或多个列当做行索引来用，或者可能希望将行索引变成`DataFrame`的列。以下面这个`DataFrame`为例：
```python
frame = pd.DataFrame({'a': range(7), 'b': range(7, 0, -1),'c': ['one', 'one', 'one', 'two', 'two','two', 'two'],'d': [0, 1, 2, 0, 1, 2, 3]})
frame
   a  b    c  d
0  0  7  one  0
1  1  6  one  1
2  2  5  one  2
3  3  4  two  0
4  4  3  two  1
5  5  2  two  2
6  6  1  two  3
```
`DataFrame`的`set_index`函数会将其一个或多个列转换为行索引，并创建一个新的`DataFrame`：
```python
frame2 = frame.set_index(['c', 'd'])
frame2
       a  b
c   d      
one 0  0  7
    1  1  6
    2  2  5
two 0  3  4
    1  4  3
    2  5  2
    3  6  1
```
默认情况下，那些列会从`DataFrame`中移除，但也可以将其保留下来：
```python
frame.set_index(['c', 'd'], drop=False)
       a  b    c  d
c   d              
one 0  0  7  one  0
    1  1  6  one  1
    2  2  5  one  2
two 0  3  4  two  0
    1  4  3  two  1
    2  5  2  two  2
    3  6  1  two  3
```
`reset_index`的功能跟`set_index`刚好相反，层次化索引的级别会被转移到列里面：
```python
frame2.reset_index()
    c  d  a  b
0  one  0  0  7
1  one  1  1  6
2  one  2  2  5
3  two  0  3  4
4  two  1  4  3
5  two  2  5  2
6  two  3  6  1
```
## 合并数据集
`pandas`对象中的数据可以通过一些方式进行合并：

`pandas.merge`可根据一个或多个键将不同`DataFrame`中的行连接起来。`SQL`或其他关系型数据库的用户对此应该会比较熟悉，因为它实现的就是数据库的`join`操作。
`pandas.concat`可以沿着一条轴将多个对象堆叠到一起。
实例方法`combine_first`可以将重复数据拼接在一起，用一个对象中的值填充另一个对象中的缺失值。

### 数据库风格的`DataFrame`合并

数据集的合并（`merge`）或连接（`join`）运算是通过一个或多个键将行连接起来的。这些运算是关系型数据库（基于`SQL`）的核心。`pandas`的`merge`函数是对数据应用这些算法的主要切入点。

以一个简单的例子开始：
```python
df1 = pd.DataFrame({'key': ['b', 'b', 'a', 'c', 'a', 'a', 'b'],'data1': range(7)})
df2 = pd.DataFrame({'key': ['a', 'b', 'd'],'data2': range(3)})
df1
   data1 key
0      0   b
1      1   b
2      2   a
3      3   c
4      4   a
5      5   a
6      6   b
df2
   data2 key
0      0   a
1      1   b
2      2   d
```
这是一种多对一的合并。`df1`中的数据有多个被标记为`a`和`b`的行，而`df2`中`key`列的每个值则仅对应一行。对这些对象调用`merge`即可得到：
```python
pd.merge(df1, df2)
   data1 key  data2
0      0   b      1
1      1   b      1
2      6   b      1
3      2   a      0
4      4   a      0
5      5   a      0
```
注意，我并没有指明要用哪个列进行连接。如果没有指定，`merge`就会将重叠列的列名当做键。不过，最好明确指定一下：
```python
pd.merge(df1, df2, on='key')
   data1 key  data2
0      0   b      1
1      1   b      1
2      6   b      1
3      2   a      0
4      4   a      0
5      5   a      0
```
如果两个对象的列名不同，也可以分别进行指定：
```python
df3 = pd.DataFrame({'lkey': ['b', 'b', 'a', 'c', 'a', 'a', 'b'],'data1': range(7)})
df4 = pd.DataFrame({'rkey': ['a', 'b', 'd'],'data2': range(3)})
pd.merge(df3, df4, left_on='lkey', right_on='rkey')
   data1 lkey  data2 rkey
0      0    b      1    b
1      1    b      1    b
2      6    b      1    b
3      2    a      0    a
4      4    a      0    a
5      5    a      0    a
```
可能你已经注意到了，结果里面c和d以及与之相关的数据消失了。默认情况下，`merge`做的是“内连接”；结果中的键是交集。其他方式还有`"left"`、`"right"`以及`"outer"`。外连接求取的是键的并集，组合了左连接和右连接的效果：
```python
pd.merge(df1, df2, how='outer')
   data1 key  data2
0    0.0   b    1.0
1    1.0   b    1.0
2    6.0   b    1.0
3    2.0   a    0.0
4    4.0   a    0.0
5    5.0   a    0.0
6    3.0   c    NaN
7    NaN   d    2.0
```
下表对这些选项进行了总结。
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200408000000.png)
多对多的合并有些不直观。看下面的例子：
```python
df1 = pd.DataFrame({'key': ['b', 'b', 'a', 'c', 'a', 'b'],'data1': range(6)})
df2 = pd.DataFrame({'key': ['a', 'b', 'a', 'b', 'd'],'data2': range(5)})
df1
   data1 key
0      0   b
1      1   b
2      2   a
3      3   c
4      4   a
5      5   b
df2
   data2 key
0      0   a
1      1   b
2      2   a
3      3   b
4      4   d
pd.merge(df1, df2, on='key', how='left')
    data1 key  data2
0       0   b    1.0
1       0   b    3.0
2       1   b    1.0
3       1   b    3.0
4       2   a    0.0
5       2   a    2.0
6       3   c    NaN
7       4   a    0.0
8       4   a    2.0
9       5   b    1.0
10      5   b    3.0
```
多对多连接产生的是行的笛卡尔积。由于左边的`DataFrame`有3个"b"行，右边的有2个，所以最终结果中就有6个"b"行。连接方式只影响出现在结果中的不同的键的值：
```python
pd.merge(df1, df2, how='inner')
   data1 key  data2
0      0   b      1
1      0   b      3
2      1   b      1
3      1   b      3
4      5   b      1
5      5   b      3
6      2   a      0
7      2   a      2
8      4   a      0
9      4   a      2
```
要根据多个键进行合并，传入一个由列名组成的列表即可：
```python
left = pd.DataFrame({'key1': ['foo', 'foo', 'bar'],'key2': ['one', 'two', 'one'],'lval': [1, 2, 3]})
right = pd.DataFrame({'key1': ['foo', 'foo', 'bar', 'bar'],'key2': ['one', 'one', 'one', 'two'],'rval': [4, 5, 6, 7]})
pd.merge(left, right, on=['key1', 'key2'], how='outer')
  key1 key2  lval  rval
0  foo  one   1.0   4.0
1  foo  one   1.0   5.0
2  foo  two   2.0   NaN
3  bar  one   3.0   6.0
4  bar  two   NaN   7.0
```
结果中会出现哪些键组合取决于所选的合并方式，你可以这样来理解：多个键形成一系列元组，并将其当做单个连接键（当然，实际上并不是这么回事）。

注意：在进行列－列连接时，`DataFrame`对象中的索引会被丢弃。

对于合并运算需要考虑的最后一个问题是对重复列名的处理。虽然你可以手工处理列名重叠的问题（查看前面介绍的重命名轴标签），但`merge`有一个更实用的`suffixes`选项，用于指定附加到左右两个`DataFrame`对象的重叠列名上的字符串：
```python
pd.merge(left, right, on='key1')
  key1 key2_x  lval key2_y  rval
0  foo    one     1    one     4
1  foo    one     1    one     5
2  foo    two     2    one     4
3  foo    two     2    one     5
4  bar    one     3    one     6
5  bar    one     3    two     7
pd.merge(left, right, on='key1', suffixes=('_left', '_right'))
  key1 key2_left  lval key2_right  rval
0  foo       one     1        one     4
1  foo       one     1        one     5
2  foo       two     2        one     4
3  foo       two     2        one     5
4  bar       one     3        one     6
5  bar       one     3        two     7
```
merge的参数请参见下表。使用`DataFrame`的行索引合并是下一节的主题。
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200408005252.png)
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200408005321.png)
### 索引上的合并
有时候，`DataFrame`中的连接键位于其索引中。在这种情况下，你可以传入`left_index=True`或`right_index=True`（或两个都传）以说明索引应该被用作连接键：
```python
left1 = pd.DataFrame({'key': ['a', 'b', 'a', 'a', 'b', 'c'],'value': range(6)})
right1 = pd.DataFrame({'group_val': [3.5, 7]}, index=['a', 'b'])
left1
  key  value
0   a      0
1   b      1
2   a      2
3   a      3
4   b      4
5   c      5
right1
   group_val
a        3.5
b        7.0
pd.merge(left1, right1, left_on='key', right_index=True)
  key  value  group_val
0   a      0        3.5
2   a      2        3.5
3   a      3        3.5
1   b      1        7.0
4   b      4        7.0
```
由于默认的`merge`方法是求取连接键的交集，因此你可以通过外连接的方式得到它们的并集：
```python
pd.merge(left1, right1, left_on='key', right_index=True, how='outer')
  key  value  group_val
0   a      0        3.5
2   a      2        3.5
3   a      3        3.5
1   b      1        7.0
4   b      4        7.0
5   c      5        NaN
```
对于层次化索引的数据，事情就有点复杂了，因为索引的合并默认是多键合并：
```python
lefth = pd.DataFrame({'key1': ['Ohio', 'Ohio', 'Ohio','Nevada', 'Nevada'],'key2': [2000, 2001, 2002, 2001, 2002],'data': np.arange(5.)})
righth = pd.DataFrame(np.arange(12).reshape((6, 2)),index=[['Nevada', 'Nevada', 'Ohio', 'Ohio','Ohio', 'Ohio'],[2001, 2000, 2000, 2000, 2001, 2002]],columns=['event1', 'event2'])
lefth
   data    key1  key2
0   0.0    Ohio  2000
1   1.0    Ohio  2001
2   2.0    Ohio  2002
3   3.0  Nevada  2001
4   4.0  Nevada  2002
righth
             event1  event2
Nevada 2001       0       1
       2000       2       3
Ohio   2000       4       5
       2000       6       7
       2001       8       9
       2002      10      11
```
这种情况下，你必须以列表的形式指明用作合并键的多个列（注意用`how='outer'`对重复索引值的处理）：
```python
pd.merge(lefth, righth, left_on=['key1', 'key2'], right_index=True)
   data    key1  key2  event1  event2
0   0.0    Ohio  2000       4       5
0   0.0    Ohio  2000       6       7
1   1.0    Ohio  2001       8       9
2   2.0    Ohio  2002      10      11
3   3.0  Nevada  2001       0       1
pd.merge(lefth, righth, left_on=['key1', 'key2'],right_index=True, how='outer')
   data    key1  key2  event1  event2
0   0.0    Ohio  2000     4.0     5.0
0   0.0    Ohio  2000     6.0     7.0
1   1.0    Ohio  2001     8.0     9.0
2   2.0    Ohio  2002    10.0    11.0
3   3.0  Nevada  2001     0.0     1.0
4   4.0  Nevada  2002     NaN     NaN
4   NaN  Nevada  2000     2.0     3.0
```
同时使用合并双方的索引也没问题：
```python
left2 = pd.DataFrame([[1., 2.], [3., 4.], [5., 6.]],index=['a', 'c', 'e'],columns=['Ohio', 'Nevada'])
right2 = pd.DataFrame([[7., 8.], [9., 10.], [11., 12.], [13, 14]],index=['b', 'c', 'd', 'e'],columns=['Missouri', 'Alabama'])
left2
   Ohio  Nevada
a   1.0     2.0
c   3.0     4.0
e   5.0     6.0
right2
   Missouri  Alabama
b       7.0      8.0
c       9.0     10.0
d      11.0     12.0
e      13.0     14.0
pd.merge(left2, right2, how='outer', left_index=True, right_index=True)
   Ohio  Nevada  Missouri  Alabama
a   1.0     2.0       NaN      NaN
b   NaN     NaN       7.0      8.0
c   3.0     4.0       9.0     10.0
d   NaN     NaN      11.0     12.0
e   5.0     6.0      13.0     14.0
```
`DataFrame`还有一个便捷的`join`实例方法，它能更为方便地实现按索引合并。它还可用于合并多个带有相同或相似索引的`DataFrame`对象，但要求没有重叠的列。在上面那个例子中，我们可以编写：
```python
left2.join(right2, how='outer')
   Ohio  Nevada  Missouri  Alabama
a   1.0     2.0       NaN      NaN
b   NaN     NaN       7.0      8.0
c   3.0     4.0       9.0     10.0
d   NaN     NaN      11.0     12.0
e   5.0     6.0      13.0     14.0
```
因为一些历史版本的遗留原因，`DataFrame`的`join`方法默认使用的是左连接，保留左边表的行索引。它还支持在调用的`DataFrame`的列上，连接传递的`DataFrame`索引：
```python
left1.join(right1, on='key')
  key  value  group_val
0   a      0        3.5
1   b      1        7.0
2   a      2        3.5
3   a      3        3.5
4   b      4        7.0
5   c      5        NaN
```
最后，对于简单的索引合并，你还可以向`join`传入一组`DataFrame`，下一节会介绍更为通用的`concat`函数，也能实现此功能：
```python
another = pd.DataFrame([[7., 8.], [9., 10.], [11., 12.], [16., 17.]],index=['a', 'c', 'e', 'f'],columns=['New York','Oregon'])
another
   New York  Oregon
a       7.0     8.0
c       9.0    10.0
e      11.0    12.0
f      16.0    17.0
left2.join([right2, another])
   Ohio  Nevada  Missouri  Alabama  New York  Oregon
a   1.0     2.0       NaN      NaN       7.0     8.0
c   3.0     4.0       9.0     10.0       9.0    10.0
e   5.0     6.0      13.0     14.0      11.0    12.0
left2.join([right2, another], how='outer')
   Ohio  Nevada  Missouri  Alabama  New York  Oregon
a   1.0     2.0       NaN      NaN       7.0     8.0
b   NaN     NaN       7.0      8.0       NaN     NaN
c   3.0     4.0       9.0     10.0       9.0    10.0
d   NaN     NaN      11.0     12.0       NaN     NaN
e   5.0     6.0      13.0     14.0      11.0    12.0
f   NaN     NaN       NaN      NaN      16.0    17.0
```
### 轴向连接
另一种数据合并运算也被称作连接（`concatenation`）、绑定（`binding`）或堆叠（`stacking`）。`NumPy`的`concatenation`函数可以用`NumPy`数组来做：
```python
arr = np.arange(12).reshape((3, 4))
arr
array([[ 0,  1,  2,  3],
       [ 4,  5,  6,  7],
       [ 8,  9, 10, 11]])
np.concatenate([arr, arr], axis=1)
array([[ 0,  1,  2,  3,  0,  1,  2,  3],
       [ 4,  5,  6,  7,  4,  5,  6,  7],
       [ 8,  9, 10, 11,  8,  9, 10, 11]])
```
对于`pandas`对象（如`Series`和`DataFrame`），带有标签的轴使你能够进一步推广数组的连接运算。具体点说，你还需要考虑以下这些东西：

- 如果对象在其它轴上的索引不同，我们应该合并这些轴的不同元素还是只使用交集？
- 连接的数据集是否需要在结果对象中可识别？
- 连接轴中保存的数据是否需要保留？许多情况下，`DataFrame`默认的整数标签最好在连接时删掉。
`pandas`的`concat`函数提供了一种能够解决这些问题的可靠方式。我将给出一些例子来讲解其使用方式。假设有三个没有重叠索引的`Series`：
```python
s1 = pd.Series([0, 1], index=['a', 'b'])
s2 = pd.Series([2, 3, 4], index=['c', 'd', 'e'])
s3 = pd.Series([5, 6], index=['f', 'g'])
```
对这些对象调用`concat`可以将值和索引粘合在一起：
```python
pd.concat([s1, s2, s3])
a    0
b    1
c    2
d    3
e    4
f    5
g    6
dtype: int64
```
默认情况下，`concat`是在`axis=0`上工作的，最终产生一个新的`Series`。如果传入`axis=1`，则结果就会变成一个`DataFrame`（`axis=1`是列）：
```python
pd.concat([s1, s2, s3], axis=1)
     0    1    2
a  0.0  NaN  NaN
b  1.0  NaN  NaN
c  NaN  2.0  NaN
d  NaN  3.0  NaN
e  NaN  4.0  NaN
f  NaN  NaN  5.0
g  NaN  NaN  6.0
```
这种情况下，另外的轴上没有重叠，从索引的有序并集（外连接）上就可以看出来。传入`join='inner'`即可得到它们的交集：
```python
s4 = pd.concat([s1, s3])
s4
a    0
b    1
f    5
g    6
dtype: int64
pd.concat([s1, s4], axis=1)
     0  1
a  0.0  0
b  1.0  1
f  NaN  5
g  NaN  6
pd.concat([s1, s4], axis=1, join='inner')
   0  1
a  0  0
b  1  1
```
在这个例子中，f和g标签消失了，是因为使用的是`join='inner'`选项。

你可以通过`join_axes`指定要在其它轴上使用的索引：
```python
pd.concat([s1, s4], axis=1, join_axes=[['a', 'c', 'b', 'e']])
     0    1
a  0.0  0.0
c  NaN  NaN
b  1.0  1.0
e  NaN  NaN
```
不过有个问题，参与连接的片段在结果中区分不开。假设你想要在连接轴上创建一个层次化索引。使用`keys`参数即可达到这个目的：
```python
result = pd.concat([s1, s1, s3], keys=['one','two', 'three'])
result
one    a    0
       b    1
two    a    0
       b    1
three  f    5
       g    6
dtype: int64
result.unstack()
         a    b    f    g
one    0.0  1.0  NaN  NaN
two    0.0  1.0  NaN  NaN
three  NaN  NaN  5.0  6.0
```
如果沿着`axis=1`对`Series`进行合并，则`keys`就会成为`DataFrame`的列头：
```python
pd.concat([s1, s2, s3], axis=1, keys=['one','two', 'three'])
   one  two  three
a  0.0  NaN    NaN
b  1.0  NaN    NaN
c  NaN  2.0    NaN
d  NaN  3.0    NaN
e  NaN  4.0    NaN
f  NaN  NaN    5.0
g  NaN  NaN    6.0
```
同样的逻辑也适用于`DataFrame`对象：
```python
df1 = pd.DataFrame(np.arange(6).reshape(3, 2), index=['a', 'b', 'c'],columns=['one', 'two'])
df2 = pd.DataFrame(5 + np.arange(4).reshape(2, 2), index=['a', 'c'],columns=['three', 'four'])
df1
   one  two
a    0    1
b    2    3
c    4    5
df2
   three  four
a      5     6
c      7     8
pd.concat([df1, df2], axis=1, keys=['level1', 'level2'])
  level1     level2     
     one two  three four
a      0   1    5.0  6.0
b      2   3    NaN  NaN
c      4   5    7.0  8.0
```
如果传入的不是列表而是一个字典，则字典的键就会被当做`keys`选项的值：
```python
pd.concat({'level1': df1, 'level2': df2}, axis=1)

  level1     level2     
     one two  three four
a      0   1    5.0  6.0
b      2   3    NaN  NaN
c      4   5    7.0  8.0
```
此外还有两个用于管理层次化索引创建方式的参数（参见下表）。举个例子，我们可以用`names`参数命名创建的轴级别：
```python
pd.concat([df1, df2], axis=1, keys=['level1', 'level2'],names=['upper', 'lower'])
upper level1     level2     
lower    one two  three four
a          0   1    5.0  6.0
b          2   3    NaN  NaN
c          4   5    7.0  8.0
```
最后一个关于`DataFrame`的问题是，`DataFrame`的行索引不包含任何相关数据：
```python
df1 = pd.DataFrame(np.random.randn(3, 4), columns=['a', 'b', 'c', 'd'])
df2 = pd.DataFrame(np.random.randn(2, 3), columns=['b', 'd', 'a'])
df1
          a         b         c         d
0  1.246435  1.007189 -1.296221  0.274992
1  0.228913  1.352917  0.886429 -2.001637
2 -0.371843  1.669025 -0.438570 -0.539741
df2
          b         d         a
0  0.476985  3.248944 -1.021228
1 -0.577087  0.124121  0.302614
```
在这种情况下，传入`ignore_index=True`即可：
```python
pd.concat([df1, df2], ignore_index=True)
          a         b         c         d
0  1.246435  1.007189 -1.296221  0.274992
1  0.228913  1.352917  0.886429 -2.001637
2 -0.371843  1.669025 -0.438570 -0.539741
3 -1.021228  0.476985       NaN  3.248944
4  0.302614 -0.577087       NaN  0.124121
```
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200408005321.png)
### 合并重叠数据
还有一种数据组合问题不能用简单的合并（`merge`）或连接（`concatenation`）运算来处理。比如说，你可能有索引全部或部分重叠的两个数据集。举个有启发性的例子，我们使用`NumPy`的`where`函数，它表示一种等价于面向数组的`if-else`：
```python
a = pd.Series([np.nan, 2.5, np.nan, 3.5, 4.5, np.nan],index=['f', 'e', 'd', 'c', 'b', 'a'])
b = pd.Series(np.arange(len(a), dtype=np.float64),index=['f', 'e', 'd', 'c', 'b', 'a'])
b[-1] = np.nan
a
f    NaN
e    2.5
d    NaN
c    3.5
b    4.5
a    NaN
dtype: float64
b
f    0.0
e    1.0
d    2.0
c    3.0
b    4.0
a    NaN
dtype: float64
np.where(pd.isnull(a), b, a)
array([ 0. ,  2.5,  2. ,  3.5,  4.5,  nan])
```
`Series`有一个`combine_first`方法，实现的也是一样的功能，还带有`pandas`的数据对齐：
```python
b[:-2].combine_first(a[2:])
a    NaN
b    4.5
c    3.0
d    2.0
e    1.0
f    0.0
dtype: float64
```
对于`DataFrame`，`combine_first`自然也会在列上做同样的事情，因此你可以将其看做：用传递对象中的数据为调用对象的缺失数据“打补丁”：
```python
df1 = pd.DataFrame({'a': [1., np.nan, 5., np.nan],'b': [np.nan, 2., np.nan, 6.],'c': range(2, 18, 4)})
df2 = pd.DataFrame({'a': [5., 4., np.nan, 3., 7.],'b': [np.nan, 3., 4., 6., 8.]})
df1
     a    b   c
0  1.0  NaN   2
1  NaN  2.0   6
2  5.0  NaN  10
3  NaN  6.0  14
df2
     a    b
0  5.0  NaN
1  4.0  3.0
2  NaN  4.0
3  3.0  6.0
4  7.0  8.0
df1.combine_first(df2)
     a    b     c
0  1.0  NaN   2.0
1  4.0  2.0   6.0
2  5.0  4.0  10.0
3  3.0  6.0  14.0
4  7.0  8.0   NaN
```
## 重塑和轴向旋转
有许多用于重新排列表格型数据的基础运算。这些函数也称作重塑（`reshape`）或轴向旋转（`pivot`）运算。

### 重塑层次化索引
层次化索引为`DataFrame`数据的重排任务提供了一种具有良好一致性的方式。主要功能有二：

- `stack`：将数据的列“旋转”为行。
- `unstack`：将数据的行“旋转”为列。
我将通过一系列的范例来讲解这些操作。接下来看一个简单的`DataFrame`，其中的行列索引均为字符串数组：
```python
data = pd.DataFrame(np.arange(6).reshape((2, 3)),index=pd.Index(['Ohio','Colorado'], name='state'),columns=pd.Index(['one', 'two', 'three'],name='number'))
data
number    one  two  three
state                    
Ohio        0    1      2
Colorado    3    4      5
```
对该数据使用`stack`方法即可将列转换为行，得到一个`Series`：
```python
result = data.stack()
result
state     number
Ohio      one       0
          two       1
          three     2
Colorado  one       3
          two       4
          three     5
dtype: int64
```
对于一个层次化索引的`Series`，你可以用`unstack`将其重排为一个`DataFrame`：
```python
result.unstack()
number    one  two  three
state                    
Ohio        0    1      2
Colorado    3    4      5
```
默认情况下，`unstack`操作的是最内层（`stack`也是如此）。传入分层级别的编号或名称即可对其它级别进行`unstack`操作：
```python
result.unstack(0)
state   Ohio  Colorado
number                
one        0         3
two        1         4
three      2         5
result.unstack('state')
state   Ohio  Colorado
number                
one        0         3
two        1         4
three      2         5
```
如果不是所有的级别值都能在各分组中找到的话，则`unstack`操作可能会引入缺失数据：
```python
s1 = pd.Series([0, 1, 2, 3], index=['a', 'b', 'c', 'd'])
s2 = pd.Series([4, 5, 6], index=['c', 'd', 'e'])
data2 = pd.concat([s1, s2], keys=['one', 'two'])
data2
one  a    0
     b    1
     c    2
     d    3
two  c    4
     d    5
     e    6
dtype: int64
data2.unstack()
       a    b    c    d    e
one  0.0  1.0  2.0  3.0  NaN
two  NaN  NaN  4.0  5.0  6.0
```
`stack`默认会滤除缺失数据，因此该运算是可逆的：
```python
data2.unstack()
       a    b    c    d    e
one  0.0  1.0  2.0  3.0  NaN
two  NaN  NaN  4.0  5.0  6.0
data2.unstack().stack()
one  a    0.0
     b    1.0
     c    2.0
     d    3.0
two  c    4.0
     d    5.0
     e    6.0
dtype: float64
data2.unstack().stack(dropna=False)
one  a    0.0
     b    1.0
     c    2.0
     d    3.0
     e    NaN
two  a    NaN
     b    NaN
     c    4.0
     d    5.0
     e    6.0
dtype: float64
```
在对`DataFrame`进行`unstack`操作时，作为旋转轴的级别将会成为结果中的最低级别：
```python
df = pd.DataFrame({'left': result, 'right': result + 5},columns=pd.Index(['left', 'right'], name='side'))
df
side             left  right
state    number             
Ohio     one        0      5
         two        1      6
         three      2      7
Colorado one        3      8
         two        4      9
         three      5     10
df.unstack('state')
side   left          right
state  Ohio Colorado  Ohio Colorado
number                             
one       0        3     5        8
two       1        4     6        9
three     2        5     7       10
```
当调用`stack`，我们可以指明轴的名字：
```python
df.unstack('state').stack('side')
state         Colorado  Ohio
number side                 
one    left          3     0
       right         8     5
two    left          4     1
       right         9     6
three  left          5     2
       right        10     7
```
### 将“长格式”旋转为“宽格式”
多个时间序列数据通常是以所谓的“长格式”（`long`）或“堆叠格式”（`stacked`）存储在数据库和`CSV`中的。我们先加载一些示例数据，做一些时间序列规整和数据清洗：
```python
data = pd.read_csv('examples/macrodata.csv')
data.head()
     year  quarter   realgdp  realcons  realinv  realgovt  realdpi    cpi  \
0  1959.0      1.0  2710.349    1707.4  286.898   470.045   1886.9  28.98   
1  1959.0      2.0  2778.801    1733.7  310.859   481.301   1919.7  29.15   
2  1959.0      3.0  2775.488    1751.8  289.226   491.260   1916.4  29.35   
3  1959.0      4.0  2785.204    1753.7  299.356   484.052   1931.3  29.37   
4  1960.0      1.0  2847.699    1770.5  331.722   462.199   1955.5  29.54   
      m1  tbilrate  unemp      pop  infl  realint  
0  139.7      2.82    5.8  177.146  0.00     0.00
1  141.7      3.08    5.1  177.830  2.34     0.74  
2  140.5      3.82    5.3  178.657  2.74     1.09  
3  140.0      4.33    5.6  179.386  0.27     4.06  
4  139.6      3.50    5.2  180.007  2.31     1.19  
periods = pd.PeriodIndex(year=data.year, quarter=data.quarter,name='date')
columns = pd.Index(['realgdp', 'infl', 'unemp'], name='item')
data = data.reindex(columns=columns)
data.index = periods.to_timestamp('D', 'end')
ldata = data.stack().reset_index().rename(columns={0: 'value'})
```
这就是多个时间序列（或者其它带有两个或多个键的可观察数据，这里，我们的键是`date`和`item`）的长格式。表中的每行代表一次观察。

关系型数据库（如`MySQL`）中的数据经常都是这样存储的，因为固定架构（即列名和数据类型）有一个好处：随着表中数据的添加，`item`列中的值的种类能够增加。在前面的例子中，`date`和`item`通常就是主键（用关系型数据库的说法），不仅提供了关系完整性，而且提供了更为简单的查询支持。有的情况下，使用这样的数据会很麻烦，你可能会更喜欢`DataFrame`，不同的`item`值分别形成一列，`date`列中的时间戳则用作索引。`DataFrame`的`pivot`方法完全可以实现这个转换：
```python
pivoted = ldata.pivot('date', 'item', 'value')
pivoted
item        infl    realgdp  unemp
date                              
1959-03-31  0.00   2710.349    5.8
1959-06-30  2.34   2778.801    5.1
1959-09-30  2.74   2775.488    5.3
1959-12-31  0.27   2785.204    5.6
1960-03-31  2.31   2847.699    5.2
1960-06-30  0.14   2834.390    5.2
1960-09-30  2.70   2839.022    5.6
1960-12-31  1.21   2802.616    6.3
1961-03-31 -0.40   2819.264    6.8
1961-06-30  1.47   2872.005    7.0
...          ...        ...    ...
2007-06-30  2.75  13203.977    4.5
2007-09-30  3.45  13321.109    4.7
2007-12-31  6.38  13391.249    4.8
2008-03-31  2.82  13366.865    4.9
2008-06-30  8.53  13415.266    5.4
2008-09-30 -3.16  13324.600    6.0
2008-12-31 -8.79  13141.920    6.9
2009-03-31  0.94  12925.410    8.1
2009-06-30  3.37  12901.504    9.2
2009-09-30  3.56  12990.341    9.6
[203 rows x 3 columns]
```
前两个传递的值分别用作行和列索引，最后一个可选值则是用于填充`DataFrame`的数据列。假设有两个需要同时重塑的数据列：
```python
ldata['value2'] = np.random.randn(len(ldata))
ldata[:10]
        date     item     value    value2
0 1959-03-31  realgdp  2710.349  0.523772
1 1959-03-31     infl     0.000  0.000940
2 1959-03-31    unemp     5.800  1.343810
3 1959-06-30  realgdp  2778.801 -0.713544
4 1959-06-30     infl     2.340 -0.831154
5 1959-06-30    unemp     5.100 -2.370232
6 1959-09-30  realgdp  2775.488 -1.860761
7 1959-09-30     infl     2.740 -0.860757
8 1959-09-30    unemp     5.300  0.560145
9 1959-12-31  realgdp  2785.204 -1.265934
```
如果忽略最后一个参数，得到的`DataFrame`就会带有层次化的列：
```python
pivoted = ldata.pivot('date', 'item')
pivoted[:5]
           value                    value2                    
item        infl   realgdp unemp      infl   realgdp     unemp
date                                                          
1959-03-31  0.00  2710.349   5.8  0.000940  0.523772  1.343810
1959-06-30  2.34  2778.801   5.1 -0.831154 -0.713544 -2.370232
1959-09-30  2.74  2775.488   5.3 -0.860757 -1.860761  0.560145
1959-12-31  0.27  2785.204   5.6  0.119827 -1.265934 -1.063512
1960-03-31  2.31  2847.699   5.2 -2.359419  0.332883 -0.199543
pivoted['value'][:5]
item        infl   realgdp  unemp
date                             
1959-03-31  0.00  2710.349    5.8
1959-06-30  2.34  2778.801    5.1
1959-09-30  2.74  2775.488    5.3
1959-12-31  0.27  2785.204    5.6
1960-03-31  2.31  2847.699    5.2
```
注意，`pivot`其实就是用`set_index`创建层次化索引，再用`unstack`重塑：
```python
unstacked = ldata.set_index(['date', 'item']).unstack('item')
unstacked[:7]
           value                    value2                    
item        infl   realgdp unemp      infl   realgdp     unemp
date                                                          
1959-03-31  0.00  2710.349   5.8  0.000940  0.523772  1.343810
1959-06-30  2.34  2778.801   5.1 -0.831154 -0.713544 -2.370232
1959-09-30  2.74  2775.488   5.3 -0.860757 -1.860761  0.560145
1959-12-31  0.27  2785.204   5.6  0.119827 -1.265934 -1.063512
1960-03-31  2.31  2847.699   5.2 -2.359419  0.332883 -0.199543
1960-06-30  0.14  2834.390   5.2 -0.970736 -1.541996 -1.307030
1960-09-30  2.70  2839.022   5.6  0.377984  0.286350 -0.753887
```
### 将“宽格式”旋转为“长格式”
旋转`DataFrame`的逆运算是`pandas.melt`。它不是将一列转换到多个新的`DataFrame`，而是合并多个列成为一个，产生一个比输入长的`DataFrame`。看一个例子：
```python
df = pd.DataFrame({'key': ['foo', 'bar', 'baz'],'A': [1, 2, 3],'B': [4, 5, 6],'C': [7, 8, 9]})
df
   A  B  C  key
0  1  4  7  foo
1  2  5  8  bar
2  3  6  9  baz
```
`key`列可能是分组指标，其它的列是数据值。当使用`pandas.melt`，我们必须指明哪些列是分组指标。下面使用`key`作为唯一的分组指标：
```python
melted = pd.melt(df, ['key'])
melted 
   key variable  value
0  foo        A      1
1  bar        A      2
2  baz        A      3
3  foo        B      4
4  bar        B      5
5  baz        B      6
6  foo        C      7
7  bar        C      8
8  baz        C      9
```
使用`pivot`，可以重塑回原来的样子：
```python
reshaped = melted.pivot('key', 'variable', 'value')
reshaped
variable  A  B  C
key              
bar       2  5  8
baz       3  6  9
foo       1  4  7
```
因为`pivot`的结果从列创建了一个索引，用作行标签，我们可以使用`reset_index`将数据移回列：
```python
reshaped.reset_index()
variable  key  A  B  C
0         bar  2  5  8
1         baz  3  6  9
2         foo  1  4  7
```
你还可以指定列的子集，作为值的列：
```python
pd.melt(df, id_vars=['key'], value_vars=['A', 'B'])
   key variable  value
0  foo        A      1
1  bar        A      2
2  baz        A      3
3  foo        B      4
4  bar        B      5
5  baz        B      6
```
`pandas.melt`也可以不用分组指标：
```python
pd.melt(df, value_vars=['A', 'B', 'C'])
  variable  value
0        A      1
1        A      2
2        A      3
3        B      4
4        B      5
5        B      6
6        C      7
7        C      8
8        C      9
pd.melt(df, value_vars=['key', 'A', 'B'])
  variable value
0      key   foo
1      key   bar
2      key   baz
3        A     1
4        A     2
5        A     3
6        B     4
7        B     5
8        B     6
```