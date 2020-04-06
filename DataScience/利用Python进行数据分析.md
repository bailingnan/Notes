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
