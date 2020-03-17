# `Python`笔记
## 对象
### 可变与不可变对象
- `Python`中的大多数对象，比如列表、字典、NumPy数组，和用户定义的类型（类），都是可变的。意味着这些对象或包含的值可以被修改。
- 字符串和元组，是不可变的。
```python
a= 'abc'
b = a.replace('a', 'A')
print(b)
'Abc'
```
要始终牢记的是，`a`是变量，而`'abc'`才是字符串对象，有些时候，我们经常说，对象`a`的内容是`'abc'`，但其实是指，`a`本身是一个变量，它指向的对象的内容才是`'abc'`：
当我们调用`a.replace('a', 'A')`时，实际上调用方法`replace`是作用在字符串对象`'abc'`上的，而这个方法虽然名字叫`replace`，但却没有改变字符串`'abc'`的内容。相反，`replace`方法创建了一个新字符串`'Abc'`并返回，如果我们用变量`b`指向该新字符串，就容易理解了，变量`a`仍指向原有的字符串`'abc'`，但变量`b`却指向新字符串`'Abc'`了。
所以，对于不变对象来说，调用对象自身的任意方法，也不会改变该对象自身的内容。相反，这些方法会创建新的对象并返回，这样，就保证了不可变对象本身永远是不可变的。
### 元组
- 如果要定义一个空的tuple，可以写成()：
```python
t = ()
print(t)
()
```
- 但是，要定义一个只有1个元素的`tuple`，如果这么定义：
```python
t = (1)
print(t)
1
```
定义的不是`tuple`，是`1`这个数！这是因为括号()既可以表示`tuple`，又可以表示数学公式中的小括号，这就产生了歧义，因此，`Python`规定，这种情况下，按小括号进行计算，计算结果自然是1。
所以，只有1个元素的`tuple`定义时必须加一个逗号,，来消除歧义：
```python
t = (1,)
print(t）
(1,)
```
`Python`在显示只有1个元素的tuple时，也会加一个逗号,，以免你误解成数学计算意义上的括号。
- 如果元组中的某个对象是可变的，比如列表，可以在原位进行修改：
```python
tup = tuple(['foo', [1, 2], True])
tup[1].append(3)
print(tup)
('foo', [1, 2, 3], True)
```
#### 拆分元组
- 使用特殊的语法`*rest`，函数签名中以抓取任意长度列表的位置参数：
```python
values = 1, 2, 3, 4, 5
a, b, *rest = values
print(a, b)
(1, 2)
print(rest)
[3, 4, 5]
```
- `rest`的部分是想要舍弃的部分:
```python
a, b, *_ = values
```
#### `tuple`方法
- 统计值出现频率：
```python
a = (1, 2, 2, 2, 3, 4, 2)
print(a.count(2))
4
```
### 列表
#### 添加和删除元素
- `insert`在特定的位置插入元素：
```python
b_list=['foo', 'bar', 'baz']
b_list.insert(1, 'red')
print(b_list)
['foo', 'red', 'peekaboo', 'baz', 'dwarf']
```
与`append`相比，`insert`耗费的计算量大，因为对后续元素的引用必须在内部迁移，以便为新元素提供空间。如果要在序列的头部和尾部插入元素，使用`collections.deque`，一个双尾部队列。
- `insert`的逆运算是`pop`，它移除并返回指定位置的元素,pop()默认删除最后一个元素：
```python
print(b_list.pop(2))
'peekaboo'
print(b_list)
['foo', 'red', 'baz', 'dwarf']
```
- `remove`去除某个值，`remove`会先寻找第一个值并除去：
```python
b_list.append('foo')
print(b_list)
['foo', 'red', 'baz', 'dwarf', 'foo']
b_list.remove('foo')
print(b_list)
['red', 'baz', 'dwarf', 'foo']
```
#### 串联和组合列表
- 可以用加号将两个列表串联起来,如果已经定义了一个列表，用`extend`方法可以追加多个元素：
```python
x = [4, None, 'foo']
x.extend([7, 8, (2, 3)])
print(x)
[4, None, 'foo', 7, 8, (2, 3)]
```
- 通过加法将列表串联的计算量较大，因为要新建一个列表，并且要复制对象。用`extend`追加元素，尤其是到一个大列表中，更为可取。因此：
```python
#快
everything = []
for chunk in list_of_lists:
    everything.extend(chunk)
#慢
everything = []
for chunk in list_of_lists:
    everything = everything + chunk
```
#### 排序
- `sort`函数将一个列表原地排序（不创建新的对象）
```python
a = [7, 2, 5, 1, 3]
a.sort()
print(a)
[1, 2, 3, 5, 7]
```
- `sort`有一些选项，有时会很好用。其中之一是二级排序`key`，可以用这个`key`进行排序。例如，我们可以按长度对字符串进行排序：
```python
b = ['saw', 'small', 'He', 'foxes', 'six']
b.sort(key=len)
print(b)
['He', 'saw', 'six', 'small', 'foxes']
```
#### 二分搜索和维护已排序的列表
- `bisect`模块支持二分查找，和向已排序的列表插入值。`bisect.bisect`可以找到插入值后仍保证排序的位置，`bisect.insort`是向这个位置插入值:
```python
import bisect
c = [1, 2, 2, 2, 3, 4, 7]
print(bisect.bisect(c, 2))
4
print(bisect.bisect(c, 5))
6
print(bisect.insort(c, 6))
print(c)
[1, 2, 2, 2, 3, 4, 6, 7]
```
#### zip函数
`zip`可以将多个列表、元组或其它序列成对组合成一个元组列表：
```python
seq1 = ['foo', 'bar', 'baz']
seq2 = ['one', 'two', 'three']
zipped = zip(seq1, seq2)
print(list(zipped))
[('foo', 'one'), ('bar', 'two'), ('baz', 'three')]
```
`zip`可以处理任意多的序列，元素的个数取决于最短的序列：
```python
seq3 = [False, True]
print(list(zip(seq1, seq2, seq3)))
[('foo', 'one', False), ('bar', 'two', True)]
```
`zip`的常见用法之一是同时迭代多个序列，可能结合`enumerate`使用：
```python
for i, (a, b) in enumerate(zip(seq1, seq2)):
    print('{0}: {1}, {2}'.format(i, a, b))
0: foo, one
1: bar, two
2: baz, three
```
- 给出一个“被压缩的”序列，`zip`可以被用来解压序列。也可以当作把行的列表转换为列的列表。
```python
pitchers = [('Nolan', 'Ryan'), ('Roger', 'Clemens'),('Schilling', 'Curt')]
first_names, last_names = zip(*pitchers)
print(first_names)
('Nolan', 'Roger', 'Schilling')
print(last_names)
('Ryan', 'Clemens', 'Curt')
```
#### reversed函数
`reversed`是一个生成器（后面详细介绍），只有实体化（即列表或`for`循环）之后才能创建翻转的序列。
```python
print(list(reversed(range(10))))
[9, 8, 7, 6, 5, 4, 3, 2, 1, 0]
```
### 字典
#### 删除值
用`del`关键字或`pop`方法（返回值的同时删除键）删除值：
```python
d1={'a': 'some value', 'b': [1, 2, 3, 4], 7: 'an integer',5: 'some value','dummy': 'another value'}
del d1[5]
print(d1)
{'a': 'some value',
 'b': [1, 2, 3, 4],
 7: 'an integer',
 'dummy': 'another value'}
ret = d1.pop('dummy')
print(ret)
'another value'
print(d1)
{'a': 'some value', 'b': [1, 2, 3, 4], 7: 'an integer'}
```
#### 更新字典
- 用`update`方法可以将一个字典与另一个融合,`update`方法是原地改变字典，因此任何传递给`update`的键的旧的值都会被舍弃。
```python
d1.update({'b' : 'foo', 'c' : 12})
print(d1)
{'a': 'some value', 'b': 'foo', 7: 'an integer', 'c': 12}
```
#### 用序列创建字典
```python
mapping = {}
for key, value in zip(key_list, value_list):
    mapping[key] = value
```
- 因为字典本质上是2元元组的集合，`dict`可以接受2元元组的列表：
```python
mapping = dict(zip(range(5), reversed(range(5))))
print(mapping)
{0: 4, 1: 3, 2: 2, 3: 1, 4: 0}
```
#### 默认值
```python
if key in some_dict:
    value = some_dict[key]
else:
    value = default_value
```
- `dict`的方法`get`和`pop`可以取默认值进行返回，上面的`if-else`语句可以简写成下面：
```python
value = some_dict.get(key, default_value)
```
- `get`默认会返回`None`，如果不存在键，`pop`会抛出一个例外。关于设定值，常见的情况是在字典的值是属于其它集合，如列表。例如，你可以通过首字母，将一个列表中的单词分类：
```python
words = ['apple', 'bat', 'bar', 'atom', 'book']
by_letter = {}
for word in words:
    letter = word[0]
    if letter not in by_letter:
        by_letter[letter] = [word]
    else:
        by_letter[letter].append(word)
print(by_letter)
{'a': ['apple', 'atom'], 'b': ['bat', 'bar', 'book']}
```
- 使用`setdefault`方法：
```python
for word in words:
    letter = word[0]
    by_letter.setdefault(letter, []).append(word)
```
- `collections`模块有一个很有用的类，`defaultdict`，它可以进一步简化上面。传递类型或函数以生成每个位置的默认值：
```python
from collections import defaultdict
by_letter = defaultdict(list)
for word in words:
    by_letter[word[0]].append(word)
```
#### 有效的键类型
字典的值可以是任意`Python`对象，而键通常是不可变的标量类型（整数、浮点型、字符串）或元组（元组中的对象必须是不可变的）。这被称为“可哈希性”。可以用`hash`函数检测一个对象是否是可哈希的（可被用作字典的键）：
```python
print(hash('string'))
5023931463650008331
print(hash((1, 2, (2, 3))))
1097636502276347782
print(hash((1, 2, [2, 3]))) # fails because lists are mutable
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-129-800cd14ba8be> in <module>()
----> 1 hash((1, 2, [2, 3])) # fails because lists are mutable
TypeError: unhashable type: 'list'
```
### 集合
- 集合是无序的不可重复的元素的集合。你可以把它当做字典，但是只有键没有值。可以用两种方式创建集合：通过`set`函数或使用尖括号`set`语句：
```python
print(set([2, 2, 2, 1, 3, 3]))
{1, 2, 3}
print({2, 2, 2, 1, 3, 3})
{1, 2, 3}
```
- 通过`add(key)`方法可以添加元素到set中
- 通过`remove(key)`方法可以删除元素：
- 合并是取两个集合中不重复的元素。可以用`union`方法，或者`|`运算符：
```python
a = {1, 2, 3, 4, 5}
b = {3, 4, 5, 6, 7, 8}
print(a.union(b))
{1, 2, 3, 4, 5, 6, 7, 8}
print(a | b)
{1, 2, 3, 4, 5, 6, 7, 8}
- 交集的元素包含在两个集合中。可以用`intersection`或`&`运算符：
print(a.intersection(b))
{3, 4, 5}
print(a & b)
{3, 4, 5}
```
- 常用集合方法
  ![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200317202703.png)
- 所有逻辑集合操作都有另外的原地实现方法，可以直接用结果替代集合的内容。对于大的集合，这么做效率更高：
```python
c = a.copy()
c |= b
print(c)
{1, 2, 3, 4, 5, 6, 7, 8}
d = a.copy()
d &= b
print(d)
{3, 4, 5}
```
- 检测一个集合是否是另一个集合的子集或父集：
```python
a_set = {1, 2, 3, 4, 5}
print({1, 2, 3}.issubset(a_set))
True
print(a_set.issuperset({1, 2, 3}))
True
```
### 列表、集合和字典推导式
```python
[expr for val in collection if condition]
```
等同于：
```python
result = []
for val in collection:
    if condition:
        result.append(expr)
```
- 字典:
```python
dict_comp = {key-expr : value-expr for value in collection if condition}
```
- 集合
```python
set_comp = {expr for value in collection if condition}
```
- `map`函数可以进一步简化：
```python
print(set(map(len, strings)))
{1, 2, 3, 4, 6}
```
#### 嵌套列表推导式
```python
all_data = [['John', 'Emily', 'Michael', 'Mary', 'Steven'],['Maria', 'Juan', 'Javier', 'Natalia', 'Pilar']]
names_of_interest = []
for names in all_data:
    enough_es = [name for name in names if name.count('e') >= 2]
    names_of_interest.extend(enough_es)
```
嵌套列表推导式：
```python
result = [name for names in all_data for name in names if name.count('e') >= 2]
print(result)
result=['Steven']
```
以下代码正常输出偶数：
```python
[x for x in range(1, 11) if x % 2 == 0]
[2, 4, 6, 8, 10]
```
但是，我们不能在最后的`if`加上`else`：
```python
[x for x in range(1, 11) if x % 2 == 0 else 0]
  File "<stdin>", line 1
    [x for x in range(1, 11) if x % 2 == 0 else 0]
                                              ^
SyntaxError: invalid syntax
```
这是因为跟在`for`后面的`if`是一个筛选条件，不能带`else`，否则如何筛选？
另一些童鞋发现把`if`写在`for`前面必须加`else`，否则报错：
```python
[x if x % 2 == 0 for x in range(1, 11)]
  File "<stdin>", line 1
    [x if x % 2 == 0 for x in range(1, 11)]
                       ^
SyntaxError: invalid syntax
```
这是因为`for`前面的部分是一个表达式，它必须根据`x`计算出一个结果。因此，考察表达式：`x if x % 2 == 0`，它无法根据`x`计算出结果，因为缺少`else`，必须加上`else`:
```python
[x if x % 2 == 0 else -x for x in range(1, 11)]
[-1, 2, -3, 4, -5, 6, -7, 8, -9, 10]
```
上述for前面的表达式`x if x % 2 == 0 else -x`才能根据`x`计算出确定的结果。
可见，在一个列表生成式中，`for`前面的`if ... else`是表达式，而`for`后面的`if`是过滤条件，不能带`else`。
```python
print([[x for x in tup] for tup in some_tuples])
[[1, 2, 3], [4, 5, 6], [7, 8, 9]]
```
### 函数
#### 参数
对象作为参数传递给函数时，新的局域变量创建了对原始对象的引用，而不是复制。如果在函数里绑定一个新对象到一个变量，这个变动不会反映到上一层。因此可以改变可变参数的内容:
```python
def append_element(some_list, element):
    some_list.append(element)
data = [1, 2, 3]
append_element(data, 4)
print(data)
[1, 2, 3, 4]
```
函数可以有一些位置参数(`positional`)和一些关键字参数(`keyword`)。关键字参数通常用于指定默认值或可选参数
#### 默认参数
```python
def add_end(L=[]):
    L.append('END')
    return L
print(add_end())
['END']
print(add_end())
['END', 'END']
```
Python函数在定义的时候，默认参数L的值就被计算出来了，即`[]`，因为默认参数L也是一个变量，它指向对象`[]`，每次调用该函数，如果改变了L的内容，则下次调用时，默认参数的内容就变了，不再是函数定义时的`[]`了。
**定义默认参数要牢记一点：默认参数必须指向不变对象！**
要修改上面的例子，我们可以用`None`这个不变对象来实现：
```python
def add_end(L=None):
    if L is None:
        L = []
    L.append('END')
    return L
```
为什么要设计`str`、`None`这样的不变对象呢？因为不变对象一旦创建，对象内部的数据就不能修改，这样就减少了由于修改数据导致的错误。此外，由于对象不变，多任务环境下同时读取对象不需要加锁，同时读一点问题都没有。我们在编写程序时，如果可以设计一个不变对象，那就尽量设计成不变对象。
#### 可变参数
```python
def calc(*numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum
```
定义可变参数和定义一个`list`或`tuple`参数相比，仅仅在参数前面加了一个`*`号。在函数内部，参数`numbers`接收到的是一个`tuple`，因此，函数代码完全不变。但是，调用该函数时，可以传入任意个参数，包括`0`个参数。
如果已经有一个list或者tuple：
```python
nums = [1, 2, 3]
print(calc(*nums))
14
```
#### 关键字参数
关键字参数允许你传入`0`个或任意个含参数名的参数，这些关键字参数在函数内部自动组装为一个`dict`。
```python
def person(name, age, **kw):
    print('name:', name, 'age:', age, 'other:', kw)
```
函数`person`除了必选参数`name`和`age`外，还接受关键字参数`kw`。在调用该函数时，可以只传入必选参数：
```python
person('Michael', 30)
name: Michael age: 30 other: {}
```
也可以传入任意个数的关键字参数：
```python
person('Bob', 35, city='Beijing')
name: Bob age: 35 other: {'city': 'Beijing'}
person('Adam', 45, gender='M', job='Engineer')
name: Adam age: 45 other: {'gender': 'M', 'job': 'Engineer'}
```
关键字参数可以扩展函数的功能。比如，在`person`函数里，我们保证能接收到`name`和`age`这两个参数，但是，如果调用者愿意提供更多的参数，我们也能收到。试想你正在做一个用户注册的功能，除了用户名和年龄是必填项外，其他都是可选项，利用关键字参数来定义这个函数就能满足注册的需求。
和可变参数类似，也可以先组装出一个`dict`，然后，把该dict转换为关键字参数传进去：
```python
extra = {'city': 'Beijing', 'job': 'Engineer'}
person('Jack', 24, city=extra['city'], job=extra['job'])
name: Jack age: 24 other: {'city': 'Beijing', 'job': 'Engineer'}
```
上面复杂的调用可以用简化的写法：
```
extra = {'city': 'Beijing', 'job': 'Engineer'}
person('Jack', 24, **extra)
name: Jack age: 24 other: {'city': 'Beijing', 'job': 'Engineer'}
```
`**extra`表示把`extra`这个`dict`的所有`key-value`用关键字参数传入到函数的`**kw`参数，`kw`将获得一个`dict`，注意``kw获得的`dict`是`extra`的一份拷贝，对`kw`的改动不会影响到函数外的`extra`。
#### 命名关键字参数
对于关键字参数，函数的调用者可以传入任意不受限制的关键字参数。至于到底传入了哪些，就需要在函数内部通过`kw`检查。
仍以`person()`函数为例，我们希望检查是否有`city`和`job`参数：
```python
def person(name, age, **kw):
    if 'city' in kw:
        # 有city参数
        pass
    if 'job' in kw:
        # 有job参数
        pass
    print('name:', name, 'age:', age, 'other:', kw)
```
如果要限制关键字参数的名字，就可以用命名关键字参数，例如，只接收`city`和`job`作为关键字参数。这种方式定义的函数如下：
```python
def person(name, age, *, city, job):
    print(name, age, city, job)
```
和关键字参数`**kw不同`，命名关键字参数需要一个特殊分隔符`*`，`*`后面的参数被视为命名关键字参数。
调用方式如下：
```python
person('Jack', 24, city='Beijing', job='Engineer')
Jack 24 Beijing Engineer
```
如果函数定义中已经有了一个可变参数，后面跟着的命名关键字参数就不再需要一个特殊分隔符`*`了：
```python
def person(name, age, *args, city, job):
    print(name, age, args, city, job)
```
命名关键字参数必须传入参数名，这和位置参数不同。如果没有传入参数名，调用将报错：
```python
person('Jack', 24, 'Beijing', 'Engineer')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: person() takes 2 positional arguments but 4 were given
```
由于调用时缺少参数名`city`和`job`，`Python`解释器把这4个参数均视为位置参数，但`person()`函数仅接受2个位置参数。
命名关键字参数可以有缺省值，从而简化调用：
```python
def person(name, age, *, city='Beijing', job):
    print(name, age, city, job)
```
由于命名关键字参数`city`具有默认值，调用时，可不传入`city`参数：
```python
person('Jack', 24, job='Engineer')
Jack 24 Beijing Engineer
```
使用命名关键字参数时，要特别注意，如果没有可变参数，就必须加一个`*`作为特殊分隔符。如果缺少`*`，`Python`解释器将无法识别位置参数和命名关键字参数：
```python
def person(name, age, city, job):
    # 缺少 *，city和job被视为位置参数
    pass
```
#### 参数组合
在`Python`中定义函数，可以用**必选参数**、**默认参数**、**可变参数**、**关键字参数**和**命名关键字参数**，这5种参数都可以组合使用。但是请注意，参数定义的顺序必须是：**必选参数**、**默认参数**、**可变参数**、**命名关键字参数**和**关键字参数**。
```python
def f1(a, b, c=0, *args, **kw):
    print('a =', a, 'b =', b, 'c =', c, 'args =', args, 'kw =', kw)

def f2(a, b, c=0, *, d, **kw):
    print('a =', a, 'b =', b, 'c =', c, 'd =', d, 'kw =', kw)
```
在函数调用的时候，`Python`解释器自动按照参数位置和参数名把对应的参数传进去。
```python
f1(1, 2)
a = 1 b = 2 c = 0 args = () kw = {}
f1(1, 2, c=3)
a = 1 b = 2 c = 3 args = () kw = {}
f1(1, 2, 3, 'a', 'b')
a = 1 b = 2 c = 3 args = ('a', 'b') kw = {}
f1(1, 2, 3, 'a', 'b', x=99)
a = 1 b = 2 c = 3 args = ('a', 'b') kw = {'x': 99}
f2(1, 2, d=99, ext=None)
a = 1 b = 2 c = 0 d = 99 kw = {'ext': None}
```
通过一个`tuple`和`dict`，你也可以调用上述函数：
```python
args = (1, 2, 3, 4)
kw = {'d': 99, 'x': '#'}
f1(*args, **kw)
a = 1 b = 2 c = 3 args = (4,) kw = {'d': 99, 'x': '#'}
args = (1, 2, 3)
kw = {'d': 88, 'x': '#'}
f2(*args, **kw)
a = 1 b = 2 c = 3 d = 88 kw = {'x': '#'}
```
所以，对于任意函数，都可以通过类似`func(*args, **kw)`的形式调用它，无论它的参数是如何定义的。
**虽然可以组合多达5种参数，但不要同时使用太多的组合，否则函数接口的可理解性很差。**
#### 匿名(lambda)函数
```python
strings = ['foo', 'card', 'bar', 'aaaa', 'abab']
strings.sort(key=lambda x: len(set(list(x))))
print(strings)
['aaaa', 'foo', 'abab', 'bar', 'card']
```
#### 柯里化：部分参数应用
柯里化（currying）是一个有趣的计算机科学术语，它指的是通过“部分参数应用”（partial argument application）从现有函数派生出新函数的技术。例如，假设我们有一个执行两数相加的简单函数:
```python
def add_numbers(x, y):
    return x + y
add_five = lambda y: add_numbers(5, y)
```
`add_numbers`的第二个参数称为“柯里化的”（curried）。这里没什么特别花哨的东西，因为我们其实就只是定义了一个可以调用现有函数的新函数而已。内置的`functools`模块可以用`partial`函数将此过程简化：
```python
from functools import partial
add_five = partial(add_numbers, 5)
```
#### 生成器
能以一种一致的方式对序列进行迭代（比如列表中的对象或文件中的行）是`Python`的一个重要特点。这是通过一种叫做迭代器协议(`iterator protocol`，它是一种使对象可迭代的通用方式)的方式实现的，一个原生的使对象可迭代的方法。
生成器(`generator`)是构造新的可迭代对象的一种简单方式。一般的函数执行之后只会返回单个值，而生成器则是以延迟的方式返回一个值序列，即每返回一个值之后暂停，直到下一个值被请求时再继续。要创建一个生成器，只需将函数中的`return`替换为`yeild`即可：
```python
def squares(n=10):
    print('Generating squares from 1 to {0}'.format(n ** 2))
    for i in range(1, n + 1):
        yield i ** 2
```
调用该生成器时，没有任何代码会被立即执行：
```python
gen = squares()
print(gen)
<generator object squares at 0x7fbbd5ab4570>
```
直到你从该生成器中请求元素时，它才会开始执行其代码：
```python
for x in gen:
    print(x, end=' ')
Generating squares from 1 to 100
1 4 9 16 25 36 49 64 81 100
```
#### 生成器表达式
```python
另一种更简洁的构造生成器的方法是使用生成器表达式(`generator expression`)。这是一种类似于列表、字典、集合推导式的生成器。其创建方式为，把列表推导式两端的方括号改成圆括号：
```python
gen = (x ** 2 for x in range(100))
print(gen)
<generator object <genexpr> at 0x7fbbd5ab29e8>
```
它跟下面这个冗长得多的生成器是完全等价的：
```python
def _make_gen():
    for x in range(100):
        yield x ** 2
gen = _make_gen()
```
生成器表达式也可以取代列表推导式，作为函数参数：
```python
print(sum(x ** 2 for x in range(100)))
328350
print(dict((i, i **2) for i in range(5)))
{0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
```
#### 迭代器
可以被`next()`函数调用并不断返回下一个值的对象称为迭代器：`Iterator`。
可以使用`isinstance()`判断一个对象是否是`Iterator`对象：
```python
from collections.abc import Iterator
isinstance((x for x in range(10)), Iterator)
True
isinstance([], Iterator)
False
isinstance({}, Iterator)
False
isinstance('abc', Iterator)
False
```
生成器都是`Iterator`对象，但`list`、`dict`、`str`虽然是`Iterable`，却不是`Iterator`。

把`list`、`dict`、`str`等`Iterable`变成`Iterator`可以使用`iter()`函数：
```python
isinstance(iter([]), Iterator)
True
isinstance(iter('abc'), Iterator)
True
```
为什么`list`、`dict`、`str`等数据类型不是`Iterator`？

这是因为`Python`的`Iterator`对象表示的是一个数据流，`Iterator`对象可以被`next()`函数调用并不断返回下一个数据，直到没有数据时抛出`StopIteration`错误。可以把这个数据流看做是一个有序序列，但我们却不能提前知道序列的长度，只能不断通过`next()`函数实现按需计算下一个数据，所以`Iterator`的计算是惰性的，只有在需要返回下一个数据时它才会计算。
`Iterator`甚至可以表示一个无限大的数据流，例如全体自然数。而使用`list`是永远不可能存储全体自然数的。
#### 函数式编程
##### 高阶函数
###### `map`函数
`map()` 会根据提供的函数对指定序列做映射。第一个参数 `function` 以参数序列中的每一个元素调用 `function` 函数，返回包含每次 `function` 函数返回值的新列表。
```python
map(function, iterable, ...)
```
- function:函数
- iterable:一个或多个序列
```python
def square(x) :            # 计算平方数
    return x ** 2
print(map(square, [1,2,3,4,5]))   # 计算列表各个元素的平方
[1, 4, 9, 16, 25]
print(map(lambda x: x ** 2, [1, 2, 3, 4, 5]))  # 使用 lambda 匿名函数
[1, 4, 9, 16, 25]
# 提供了两个列表，对相同位置的列表数据进行相加
print(map(lambda x, y: x + y, [1, 3, 5, 7, 9], [2, 4, 6, 8, 10]))
[3, 7, 11, 15, 19]
```
###### `reduce`函数
`reduce`把一个函数作用在一个序列`[x1, x2, x3, ...]`上，这个函数必须接收两个参数，`reduce`把结果继续和序列的下一个元素做累积计算，其效果就是:
```python
reduce(f, [x1, x2, x3, x4]) = f(f(f(x1, x2), x3), x4)
```
如果要把序列`[1, 3, 5, 7, 9]`变换成整数13579，reduce就可以派上用场：
```python
from functools import reduce
def fn(x, y):
    return x * 10 + y
print(reduce(fn, [1, 3, 5, 7, 9]))
13579
```
考虑到字符串str也是一个序列，对上面的例子稍加改动，配合`map()`，我们就可以写出把`str`转换为`int`的函数：
```python
from functools import reduce
DIGITS = {'0': 0, '1': 1, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9}
def char2num(s):
    return DIGITS[s]
def str2int(s):
    return reduce(lambda x, y: x * 10 + y, map(char2num, s))
```
###### `filter`函数
`Python`内建的`filter()`函数用于过滤序列。
和`map()`类似，`filter()`也接收一个函数和一个序列。和`map()`不同的是，`filter()`把传入的函数依次作用于每个元素，然后根据返回值是`True`还是`False`决定保留还是丢弃该元素。
注意到`filter()`函数返回的是一个`Iterator`，也就是一个惰性序列，所以要强迫`filter()`完成计算结果，需要用`list()`函数获得所有结果并返回`list`。
筛法求素数：
```
def _odd_iter():
    n = 1
    while True:
        n = n + 2
        yield n
def _not_divisible(n):
    return lambda x: x % n > 0
def primes():
    yield 2
    it = _odd_iter() # 初始序列
    while True:
        n = next(it) # 返回序列的第一个数
        yield n
        it = filter(_not_divisible(n), it) # 构造新序列
for n in primes():
    if n < 1000:
        print(n)
    else:
        break
```
###### `sorted`函数
`sorted`函数可以从任意序列的元素返回一个新的排好序的列表：
```python
print(sorted([7, 1, 2, 6, 0, 3, 2]))
[0, 1, 2, 2, 3, 6, 7]
print(sorted('horse race'))
[' ', 'a', 'c', 'e', 'e', 'h', 'o', 'r', 'r', 's']
```
`sorted()`函数也是一个高阶函数，它还可以接收一个`key`函数来实现自定义的排序，例如按绝对值大小排序：
```python
sorted([36, 5, -12, 9, -21], key=abs)
[5, 9, -12, -21, 36]
```
进行反向排序，不必改动`key`函数，可以传入第三个参数`reverse=True`：
```python
sorted(['bob', 'about', 'Zoo', 'Credit'], key=str.lower, reverse=True)
['Zoo', 'Credit', 'bob', 'about']
```
##### 返回函数
```python
def lazy_sum(*args):
    def sum():
        ax = 0
        for n in args:
            ax = ax + n
        return ax
    return sum
```
调用`lazy_sum()`时，返回的并不是求和结果，而是求和函数：
```python
f = lazy_sum(1, 3, 5, 7, 9)
f
<function lazy_sum.<locals>.sum at 0x101c6ed90>
调用函数`f`时，才真正计算求和的结果：
```python
f()
25
```
函数`lazy_sum`中又定义了函数`sum`，并且，内部函数`sum`可以引用外部函数`lazy_sum`的参数和局部变量，当`lazy_sum`返回函数`sum`时，相关参数和变量都保存在返回的函数中，这种称为“闭包(Closure)”的程序结构拥有极大的威力。
###### 闭包
返回的函数在其定义内部引用了局部变量`args`，所以，当一个函数返回了一个函数后，其内部的局部变量还被新函数引用，所以，闭包用起来简单，实现起来可不容易。
```python
def count():
    fs = []
    for i in range(1, 4):
        def f():
             return i*i
        fs.append(f)
    return fs

f1, f2, f3 = count()
```
在上面的例子中，每次循环，都创建了一个新的函数，然后，把创建的3个函数都返回了。
你可能认为调用`f1()`，`f2()`和`f3()`结果应该是1，4，9，但实际结果是：
```python
f1()
9
f2()
9
f3()
9
```
全部都是9！原因就在于返回的函数引用了变量`i`，但它并非立刻执行。等到3个函数都返回时，它们所引用的变量`i`已经变成了3，因此最终结果为9。
另一个需要注意的问题是，返回的函数并没有立刻执行，而是直到调用了`f()`才执行。
**返回闭包时牢记一点：返回函数不要引用任何循环变量，或者后续会发生变化的变量。**
如果一定要引用循环变量怎么办？方法是再创建一个函数，用该函数的参数绑定循环变量当前的值，无论该循环变量后续如何更改，已绑定到函数参数的值不变：
```python
def count():
    def f(j):
        def g():
            return j*j
        return g
    fs = []
    for i in range(1, 4):
        fs.append(f(i)) # f(i)立刻被执行，因此i的当前值被传入f()
    return fs
```
```python
 f1, f2, f3 = count()
f1()
1
f2()
4
f3()
9
```
##### 装饰器
在代码运行期间动态增加功能的方式，称之为“装饰器”(`Decorator`)。
本质上，`decorator`就是一个返回函数的高阶函数。所以，我们要定义一个能打印日志的`decorator`，可以定义如下：
```python
def log(func):
    def wrapper(*args, **kw):
        print('call %s():' % func.__name__)
        return func(*args, **kw)
    return wrapper
@log
def now():
    print('2015-3-25')
now()
call now():
2015-3-25
```
由于`log()`是一个`decorator`，返回一个函数，所以，原来的`now()`函数仍然存在，只是现在同名的`now`变量指向了新的函数，于是调用`now()`将执行新函数，即在`log()`函数中返回的`wrapper()`函数。
`wrapper()`函数的参数定义是`(*args, **kw)`，因此，`wrapper()`函数可以接受任意参数的调用。在`wrapper()`函数内，首先打印日志，再紧接着调用原始函数。
如果`decorator`本身需要传入参数，那就需要编写一个返回`decorator`的高阶函数，写出来会更复杂。比如，要自定义`log`的文本：
```python
def log(text):
    def decorator(func):
        def wrapper(*args, **kw):
            print('%s %s():' % (text, func.__name__))
            return func(*args, **kw)
        return wrapper
    return decorator
@log('execute')
def now():
    print('2015-3-25')
now()
execute now():
2015-3-25
```
和两层嵌套的decorator相比，3层嵌套的效果是这样的：
```python
now = log('execute')(now)
```
首先执行`log('execute')`，返回的是`decorator`函数，再调用返回的函数，参数是`now`函数，返回值最终是`wrapper`函数。

以上两种`decorator`的定义都没有问题，但还差最后一步。因为我们讲了函数也是对象，它有`__name__`等属性，但你去看经过`decorator`装饰之后的函数，它们的`__name__`已经从原来的`'now'`变成了`'wrapper'`：
```python
now.__name__
'wrapper'
```
因为返回的那个`wrapper()`函数名字就是`'wrapper'`，所以，需要把原始函数的`__name__`等属性复制到`wrapper()`函数中，否则，有些依赖函数签名的代码执行就会出错。

不需要编写`wrapper.__name__ = func.__name__`这样的代码，`Python`内置的`functools.wraps`就是干这个事的，所以，一个完整的`decorator`的写法如下：
```pythoh
import functools
def log(func):
    @functools.wraps(func)
    def wrapper(*args, **kw):
        print('call %s():' % func.__name__)
        return func(*args, **kw)
    return wrapper
```
或者针对带参数的`decorator`：
```
import functools
def log(text):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kw):
            print('%s %s():' % (text, func.__name__))
            return func(*args, **kw)
        return wrapper
    return decorator
```
在面向对象(OOP)的设计模式中，`decorator`被称为装饰模式。`OOP`的装饰模式需要通过继承和组合来实现，而`Python`除了能支持`OOP`的`decorator`外，直接从语法层次支持`decorator`。`Python`的`decorator`可以用函数实现，也可以用类实现。
`decorator`可以增强函数的功能，定义起来虽然有点复杂，但使用起来非常灵活和方便。
###### `@property`
`Python`内置的`@property`装饰器就是负责把一个方法变成属性调用的：
```python
class Student(object):

    @property
    def score(self):
        return self._score

    @score.setter
    def score(self, value):
        if not isinstance(value, int):
            raise ValueError('score must be an integer!')
        if value < 0 or value > 100:
            raise ValueError('score must between 0 ~ 100!')
        self._score = value
```
把一个`getter`方法变成属性，只需要加上`@property`就可以了，此时，`@property`本身又创建了另一个装饰器`@score.setter`，负责把一个`setter`方法变成属性赋值，于是，我们就拥有一个可控的属性操作：
```python
s = Student()
s.score = 60 # OK，实际转化为s.set_score(60)
s.score # OK，实际转化为s.get_score()
60
s.score = 9999
Traceback (most recent call last):
  ...
ValueError: score must between 0 ~ 100!
```
注意到这个神奇的`@property`，我们在对实例属性操作的时候，就知道该属性很可能不是直接暴露的，而是通过`getter`和`setter`方法来实现的。

还可以定义只读属性，只定义`getter`方法，不定义`setter`方法就是一个只读属性：
```python
class Student(object):

    @property
    def birth(self):
        return self._birth

    @birth.setter
    def birth(self, value):
        self._birth = value

    @property
    def age(self):
        return 2015 - self._birth
```
上面的`birth`是可读写属性，而`age`就是一个只读属性，因为`age`可以根据`birth`和当前时间计算出来。
###### `@classmethod`
`@classmethod`对应的函数不需要实例化，不需要 `self` 参数，但第一个参数需要是表示自身类的 `cls` 参数，可以来调用类的属性，类的方法，实例化对象等。
`@classmethod`因为持有`cls`参数，可以来调用类的属性，类的方法，实例化对象等，避免硬编码。
```python
class A(object):
    bar = 1
    def func1(self):  
        print ('foo') 
    @classmethod
    def func2(cls):
        print ('func2')
        print (cls.bar)
        cls().func1()   # 调用 foo 方法
 
A.func2()               # 不需要实例化
```
###### `@staticmethod`
将类中的方法装饰为静态方法，即类不需要创建实例的情况下，可以通过类名直接引用。到达将函数功能与实例解绑的效果。
`@staticmethod`不需要表示自身对象的`self`和自身类的`cls`参数，就跟使用函数一样。
如果在`@staticmethod`中要调用到这个类的一些属性方法，只能直接类名.属性名或类名.方法名。
```python
class TestClass:
    name = "test"
    def __init__(self, name):
        self.name = name
    @staticmethod
    def fun(self, x, y):
        return  x + y
cls = TestClass("felix")
print "通过实例引用方法"
print cls.fun(None, 2, 3) # 参数个数必须与定义中的个数保持一致，否则报错
print "类名直接引用静态方法"
print TestClass.fun(None, 2, 3) # 参数个数必须与定义中的个数保持一致，否则报错
```
###### `@dataclass`
```python
class MyClass:
    def __init__(self, var_a, var_b):
        self.var_a = var_a
        self.var_b = var_b
@dataclass
class MyClass:
    var_a: str
    var_b: str
@dataclass
class Number:
    val:int = 0
```
##### 偏函数
`int()`函数可以把字符串转换为整数，当仅传入字符串时，`int()`函数默认按十进制转换。
但`int()`函数还提供额外的`base`参数，默认值为10。如果传入`base`参数，就可以做N进制的转换：
`functools.partial`就是帮助我们创建一个偏函数的，不需要我们自己定义`int2()`，可以直接使用下面的代码创建一个新的函数`int2`：
```
import functools
int2 = functools.partial(int, base=2)
int2('1000000')
64
int2('1010101')
85
```
简单总结`functools.partial`的作用就是，把一个函数的某些参数给固定住（也就是设置默认值），返回一个新的函数，调用这个新函数会更简单。
注意到上面的新的`int2`函数，仅仅是把`base`参数重新设定默认值为2，但也可以在函数调用时传入其他值：
```python
int2('1000000', base=10)
1000000
```
最后，创建偏函数时，实际上可以接收函数对象、`*args`和`**kw`这3个参数，当传入：
```python
int2 = functools.partial(int, base=2)
```
实际上固定了`int()`函数的关键字参数`base`，也就是：
`int2('10010')`
相当于：
`kw = { 'base': 2 }`
`int('10010', **kw)
当传入：
`max2 = functools.partial(max, 10)`
实际上会把10作为*args的一部分自动加到左边，也就是：
`max2(5, 6, 7)`
相当于：
```python
args = (10, 5, 6, 7)
max(*args)
```
结果为10。
当函数的参数个数太多，需要简化时，使用`functools.partial`可以创建一个新的函数，这个新函数可以固定住原函数的部分参数，从而在调用时更简单。
#### `itertools`模块
标准库`itertools`模块中有一组用于许多常见数据算法的生成器。例如，`groupby`可以接受任何序列和一个函数。它根据函数的返回值对序列中的连续元素进行分组。下面是一个例子：
```python
import itertools
first_letter = lambda x: x[0]
names = ['Alan', 'Adam', 'Wes', 'Will', 'Albert', 'Steven']
for letter, names in itertools.groupby(names, first_letter):
    print(letter, list(names)) # names is a generator
A ['Alan', 'Adam']
W ['Wes', 'Will']
A ['Albert']
S ['Steven']
```
- 常用`itertools`函数:
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/687474703a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f373137383639312d313131383233643837363761313034642e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f77.png)
#### 错误和异常处理
```python
def attempt_float(x):
    try:
        return float(x)
    except ValueError:
        return x
```
某些情况下，你可能不想抑制异常，你想无论`try`部分的代码是否成功，都执行一段代码。可以使用`finally`：
```python
f = open(path, 'w')

try:
    write_to_file(f)
finally:
    f.close()
```
这里，文件处理`f`总会被关闭。相似的，你可以用`else`让只在`try`部分成功的情况下，才执行代码：
```python
f = open(path, 'w')
try:
    write_to_file(f)
except:
    print('Failed')
else:
    print('Succeeded')
finally:
    f.close()
```
### 文件和操作系统
- 为了打开一个文件以便读写，可以使用内置的`open`函数以及一个相对或绝对的文件路径：
```python
path = 'examples/segismundo.txt'
f = open(path)
```
- 默认情况下，文件是以只读模式('r')打开的。然后，我们就可以像处理列表那样来处理这个文件句柄`f`了，比如对行进行迭代：
```python
for line in f:
    pass
```
- 如果使用open创建文件对象，一定要用`close`关闭它。关闭文件可以返回操作系统资源：
```python
f.close()
```
- 用`with`语句可以可以更容易地清理打开的文件,这样可以在退出代码块时，自动关闭文件：
```
with open(path) as f:
    lines = [x.rstrip() for x in f]
```
- 读写模式：
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200317211525.png)
- 向文件写入，可以使用文件的write或writelines方法。例如，我们可以创建一个无空行版的prof_mod.py：
```python
with open('tmp.txt', 'w') as handle:
    handle.writelines(x for x in open(path) if len(x) > 1)
with open('tmp.txt') as f:
    lines = f.readlines()
```
- 常用文件方法
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200317211726.png)
### 字符串
```python
 a= 'ABC'
```
在内存中创建了一个'ABC'的字符串；
在内存中创建了一个名为a的变量，并把它指向'ABC'。
也可以把一个变量a赋值给另一个变量b，这个操作实际上是把变量b指向变量a所指向的数据，例如下面的代码：
```python
a = 'ABC'
b = a
a = 'XYZ'
print(b)
'ABC`
```
#### 分割
```python
s = 'python'
list(s)
['p', 'y', 't', 'h', 'o', 'n']
```
#### 模板化或格式化
```python
template = '{0:.2f} {1:s} are worth US${2:d}'
```
- `{0:.2f}`表示格式化第一个参数为带有两位小数的浮点数。
- `{1:s}`表示格式化第二个参数为字符串。
- `{2:d}`表示格式化第三个参数为一个整数。
### 运算符
#### `==`和`is`
- 要判断两个引用是否指向同一个对象，可以使用`is`方法:
```python
a = [1, 2, 3]
b = a
c = list(a)
print(a is b)
True
# 因为list总是创建一个新的Python列表（即复制），我们可以断定c是不同于a的。
print(a is not c)
True
print(a == c)
True
```
## 模块
每一个包目录下面都会有一个`__init__.py`的文件，这个文件是必须存在的，否则，`Python`就把这个目录当成普通目录，而不是一个包。
## OOP
### 访问限制
如果要让内部属性不被外部访问，可以把属性的名称前加上两个下划线`__`，在`Python`中，实例的变量名如果以`__`开头，就变成了一个私有变量(`private`)，只有内部可以访问，外部不能访问:
```python
class Student(object):

    def __init__(self, name, score):
        self.__name = name
        self.__score = score

    def print_score(self):
        print('%s: %s' % (self.__name, self.__score))
```
改完后，对于外部代码来说，没什么变动，但是已经无法从外部访问`实例变量.__name`和`实例变量.__score`了：
```python
bart = Student('Bart Simpson', 59)
bart.__name
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Student' object has no attribute '__name'
```
这样就确保了外部代码不能随意修改对象内部的状态，这样通过访问限制的保护，代码更加健壮。

外部代码要获取`name`和`score`,修改属性,可以给`Student`类增加`get_name`和`get_score`这样的方法：
```python
class Student(object):
    ...

    def get_name(self):
        return self.__name

    def get_score(self):
        return self.__score
    def set_score(self, score):
        self.__score = score
```
那种直接通过`bart.score = 99`也可以修改啊，为什么要定义一个方法大费周折？因为在方法中，可以对参数做检查，避免传入无效的参数：
```python
class Student(object):
    ...

    def set_score(self, score):
        if 0 <= score <= 100:
            self.__score = score
        else:
            raise ValueError('bad score')
```
在`Python`中，变量名类似`__xxx__`的，也就是以双下划线开头，并且以双下划线结尾的，是特殊变量，特殊变量是可以直接访问的，不是`private`变量，所以，不能用`__name__`、`__score__`这样的变量名。
下划线开头的实例变量名，比如`_name`，这样的实例变量外部是可以访问的，但是，按照约定俗成的规定，当你看到这样的变量时，意思就是，“虽然我可以被访问，但是，请把我视为私有变量，不要随意访问”。
双下划线开头的实例变量是不是一定不能从外部访问呢？其实也不是。不能直接访问`__name`是因为`Python`解释器对外把`__name`变量改成了`_Student__name`，所以，仍然可以通过`_Student__name来访问__name`变量：
```python
bart._Student__name
'Bart Simpson'
```
但是强烈建议你不要这么干，因为不同版本的`Python`解释器可能会把`__name`改成不同的变量名。
总的来说就是，`Python`本身没有任何机制阻止你干坏事，一切全靠自觉。
最后注意下面的这种错误写法：
```python
bart = Student('Bart Simpson', 59)
bart.get_name()
'Bart Simpson'
bart.__name = 'New Name' # 设置__name变量！
bart.__name
'New Name'
```
表面上看，外部代码“成功”地设置了`__name`变量，但实际上这个`__name`变量和`class`内部的`__name`变量不是一个变量！内部的`__name`变量已经被`Python`解释器自动改成了`_Student__name`，而外部代码给`bart`新增了一个`__name`变量。不信试试：
```python
bart.get_name() # get_name()内部返回self.__name
'Bart Simpson'
```
### 继承和多态
```python
class Animal(object):
    def run(self):
        print('Animal is running...')
class Dog(Animal):
    def run(self):
        print('Dog is running...')
```
子类和父类都存在相同的`run()`方法时，我们说，子类的`run()`覆盖了父类的`run()`，在代码运行的时候，总是会调用子类的`run()`。这样，我们就获得了继承的另一个好处：多态。]
```python
a = list() # a是list类型
b = Animal() # b是Animal类型
c = Dog() # c是Dog类型
isinstance(a, list)
True
isinstance(b, Animal)
True
isinstance(c, Dog)
True
```
看来`a`、`b`、`c`确实对应着`list`、`Animal`、`Dog`这3种类型。
```python
isinstance(c, Animal)
True
```
看来`c`不仅仅是`Dog`，`c`还是`Animal`！因为`Dog`是从`Animal`继承下来的，当我们创建了一个`Dog`的实例`c`时，我们认为`c`的数据类型是`Dog`没错，但`c`同时也是`Animal`也没错，`Dog`本来就是`Animal`的一种！
所以，在继承关系中，如果一个实例的数据类型是某个子类，那它的数据类型也可以被看做是父类。
```python
def run_twice(animal):
    animal.run()
    animal.run()
```
传入`Dog`的实例时，`run_twice()`就打印出：
```python
run_twice(Dog())
Dog is running...
Dog is running...
```
多态的好处就是，当我们需要传入`Dog`时，我们只需要接收`Animal`类型就可以了，因为`Dog`是`Animal`类型，然后，按照`Animal`类型进行操作即可。由于`Animal`类型有`run()`方法，因此，传入的任意类型，只要是`Animal`类或者子类，就会自动调用实际类型的`run()`方法，这就是多态的意思：

对于一个变量，我们只需要知道它是`Animal`类型，无需确切地知道它的子类型，就可以放心地调用`run()`方法，而具体调用的`run()`方法是作用在`Animal`还是`Dog`对象上，由运行时该对象的确切类型决定，这就是多态真正的威力：调用方只管调用，不管细节，而当我们新增一种`Animal`的子类时，只要确保`run()`方法编写正确，不用管原来的代码是如何调用的。这就是著名的“开闭”原则：
对扩展开放：允许新增`Animal`子类；
对修改封闭：不需要修改依赖`Animal`类型的`run_twice()`等函数。
对于静态语言（例如Java）来说，如果需要传入`Animal`类型，则传入的对象必须是`Animal`类型或者它的子类，否则，将无法调用`run()`方法。
对于`Python`这样的动态语言来说，则不一定需要传入`Animal`类型。我们只需要保证传入的对象有一个`run()`方法就可以了：
```python
class Timer(object):
    def run(self):
        print('Start...')
```
这就是动态语言的“鸭子类型”，它并不要求严格的继承体系，一个对象只要“看起来像鸭子，走起路来像鸭子”，那它就可以被看做是鸭子。
`Python`的`“file-like object“`就是一种鸭子类型。对真正的文件对象，它有一个`read()`方法，返回其内容。但是，许多对象，只要有`read()`方法，都被视为`“file-like object“`。许多函数接收的参数就是`“file-like object“`，你不一定要传入真正的文件对象，完全可以传入任何实现了`read()`方法的对象。
### 获取对象信息
#### 使用`type()`
判断对象类型，使用`type()`函数：
```python
type(123)
<class 'int'>
type('str')
<class 'str'>
type(None)
<type(None) 'NoneType'>
type(abs)
<class 'builtin_function_or_method'>
type(a)
<class '__main__.Animal'>
>>> import types
def fn():
    pass
type(fn)==types.FunctionType
True
type(abs)==types.BuiltinFunctionType
True
type(lambda x: x)==types.LambdaType
True
type((x for x in range(10)))==types.GeneratorType
True
```
#### 使用`isinstance()`
判断`class`的类型，可以使用`isinstance()`函数。
```python
isinstance([1, 2, 3], (list, tuple))
True
isinstance((1, 2, 3), (list, tuple))
True
```
**总是优先使用isinstance()判断类型，可以将指定类型及其子类“一网打尽”。**
#### 使用dir()
如果要获得一个对象的所有属性和方法，可以使用`dir()`函数，它返回一个包含字符串的`list`，比如，获得一个`str`对象的所有属性和方法：
```python
dir('ABC')
['__add__', '__class__',..., '__subclasshook__', 'capitalize', 'casefold',..., 'zfill']
```
类似`__xxx__`的属性和方法在`Python`中都是有特殊用途的，比如`__len__`方法返回长度。在`Python`中，如果你调用`len()`函数试图获取一个对象的长度，实际上，在`len()`函数内部，它自动去调用该对象的`__len__()`方法，所以，下面的代码是等价的：
```python
len('ABC')
3
'ABC'.__len__()
3
```
我们自己写的类，如果也想用`len(myObj)`的话，就自己写一个`__len__()`方法：
```python
class MyDog(object):
    def __len__(self):
        return 100
dog = MyDog()
len(dog)
100
```
剩下的都是普通属性或方法，比如`lower()`返回小写的字符串：
```python
'ABC'.lower()
'abc'
```
仅仅把属性和方法列出来是不够的，配合`getattr()`、`setattr()`以及`hasattr()`，我们可以直接操作一个对象的状态：
```python
class MyObject(object):
    def __init__(self):
        self.x = 9
    def power(self):
        return self.x * self.x
obj = MyObject()
```
紧接着，可以测试该对象的属性：
```python
hasattr(obj, 'x') # 有属性'x'吗？
True
obj.x
9
hasattr(obj, 'y') # 有属性'y'吗？
False
setattr(obj, 'y', 19) # 设置一个属性'y'
hasattr(obj, 'y') # 有属性'y'吗？
True
getattr(obj, 'y') # 获取属性'y'
19
obj.y # 获取属性'y'
19
```
如果试图获取不存在的属性，会抛出`AttributeError`的错误：
```python
getattr(obj, 'z') # 获取属性'z'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'MyObject' object has no attribute 'z'
```
可以传入一个`default`参数，如果属性不存在，就返回默认值：
```
getattr(obj, 'z', 404) # 获取属性'z'，如果不存在，返回默认值404
404
```
也可以获得对象的方法：
```python
hasattr(obj, 'power') # 有属性'power'吗？
True
getattr(obj, 'power') # 获取属性'power'
<bound method MyObject.power of <__main__.MyObject object at 0x10077a6a0>>
fn = getattr(obj, 'power') # 获取属性'power'并赋值到变量fn
fn # fn指向obj.power
<bound method MyObject.power of <__main__.MyObject object at 0x10077a6a0>>
fn() # 调用fn()与调用obj.power()是一样的
81
```
一个正确的用法的例子如下：
```
def readImage(fp):
    if hasattr(fp, 'read'):
        return readData(fp)
    return None
```
#### 实例属性和类属性
给实例绑定属性的方法是通过实例变量，或者通过`self`变量：
```python
class Student(object):
    def __init__(self, name):
        self.name = name

s = Student('Bob')
s.score = 90
```
直接在`class`中定义属性，这种属性是类属性，归`Student`类所有：
```python
class Student(object):
    name = 'Student'
```
当我们定义了一个类属性后，这个属性虽然归类所有，但类的所有实例都可以访问到。来测试一下：
```python
class Student(object):
    name = 'Student'
s = Student() # 创建实例s
print(s.name) # 打印name属性，因为实例并没有name属性，所以会继续查找class的name属性
Student
print(Student.name) # 打印类的name属性
Student
s.name = 'Michael' # 给实例绑定name属性
print(s.name) # 由于实例属性优先级比类属性高，因此，它会屏蔽掉类的name属性
Michael
print(Student.name) # 但是类属性并未消失，用Student.name仍然可以访问
Student
del s.name # 如果删除实例的name属性
print(s.name) # 再次调用s.name，由于实例的name属性没有找到，类的name属性就显示出来了
Student
```
在编写程序的时候，千万不要对实例属性和类属性使用相同的名字，因为相同名称的实例属性将屏蔽掉类属性，但是当你删除实例属性后，再使用相同的名称，访问到的将是类属性。
实例属性属于各个实例所有，互不干扰；
类属性属于类所有，所有实例共享一个属性；
不要对实例属性和类属性使用相同的名字，否则将产生难以发现的错误。
#### 使用`__slots__`
给实例绑定一个方法：
```python
def set_age(self, age): # 定义一个函数作为实例方法
    self.age = age
from types import MethodType
s.set_age = MethodType(set_age, s) # 给实例绑定一个方法
s.set_age(25) # 调用实例方法
s.age # 测试结果
25
```
但是，给一个实例绑定的方法，对另一个实例是不起作用的：
```python
s2 = Student() # 创建新的实例
s2.set_age(25) # 尝试调用方法
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Student' object has no attribute 'set_age'
```
为了给所有实例都绑定方法，可以给class绑定方法：
```python
def set_score(self, score):
    self.score = score
Student.set_score = set_score
```
给class绑定方法后，所有实例均可调用.
通常情况下，上面的`set_score`方法可以直接定义在`class`中，但动态绑定允许我们在程序运行的过程中动态给`class`加上功能，这在静态语言中很难实现。
限制实例的属性怎么办？比如，只允许对`Student`实例添加`name`和`age`属性。
为了达到限制的目的，`Python`允许在定义`class`的时候，定义一个特殊的`__slots__`变量，来限制该`class`实例能添加的属性：
```python
class Student(object):
    __slots__ = ('name', 'age') # 用tuple定义允许绑定的属性名称
```python
s = Student() # 创建新的实例
s.name = 'Michael' # 绑定属性'name'
s.age = 25 # 绑定属性'age'
s.score = 99 # 绑定属性'score'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Student' object has no attribute 'score'
```
由于`'score'`没有被放到_`_slots__`中，所以不能绑定`score`属性，试图绑定`score`将得到`AttributeError`的错误。

使用`__slots__`要注意，`__slots__`定义的属性仅对当前类实例起作用，对继承的子类是不起作用的：
```python
class GraduateStudent(Student):
    pass
g = GraduateStudent()
g.score = 9999
```
除非在子类中也定义`__slots__`，这样，子类实例允许定义的属性就是自身的`__slots__`加上父类的`__slots__`。
#### 多重继承
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200318023741.png)
```python
class Animal(object):
    pass
# 大类:
class Mammal(Animal):
    pass
class Bird(Animal):
    pass
# 各种动物:
class Dog(Mammal):
    pass
class Bat(Mammal):
    pass
class Parrot(Bird):
    pass
class Ostrich(Bird):
    pass
```
现在，我们要给动物再加上`Runnable`和`Flyable`的功能，只需要先定义好`Runnable`和`Flyable`的类：
```python
class Runnable(object):
    def run(self):
        print('Running...')

class Flyable(object):
    def fly(self):
        print('Flying...')
```
对于需要`Runnable`功能的动物，就多继承一个`Runnable`，例如`Dog`：
```python
class Dog(Mammal, Runnable):
    pass
```
对于需要`Flyable`功能的动物，就多继承一个`Flyable`，例如`Bat`：
```python
class Bat(Mammal, Flyable):
    pass
```
通过多重继承，一个子类就可以同时获得多个父类的所有功能。
在设计类的继承关系时，通常，主线都是单一继承下来的，例如，`Ostrich`继承自`Bird`。但是，如果需要“混入”额外的功能，通过多重继承就可以实现，比如，让`Ostrich`除了继承自`Bird`外，再同时继承`Runnable`。这种设计通常称之为`MixIn`。

为了更好地看出继承关系，我们把`Runnable`和`Flyable`改为`RunnableMixIn`和`FlyableMixIn`。类似的，你还可以定义出肉食动物`CarnivorousMixIn`和植食动物`HerbivoresMixIn`，让某个动物同时拥有好几个`MixIn`：
```python
class Dog(Mammal, RunnableMixIn, CarnivorousMixIn):
    pass
```
`MixIn`的目的就是给一个类增加多个功能，这样，在设计类的时候，我们优先考虑通过多重继承来组合多个`MixIn`的功能，而不是设计多层次的复杂的继承关系。

`Python`自带的很多库也使用了`MixIn`。举个例子，`Python`自带了`TCPServer`和`UDPServer`这两类网络服务，而要同时服务多个用户就必须使用多进程或多线程模型，这两种模型由`ForkingMixIn`和`ThreadingMixIn`提供。通过组合，我们就可以创造出合适的服务来。

比如，编写一个多进程模式的`TCP`服务，定义如下：
```python
class MyTCPServer(TCPServer, ForkingMixIn):
    pass
```
编写一个多线程模式的`UDP`服务，定义如下：
```python
class MyUDPServer(UDPServer, ThreadingMixIn):
    pass
```
如果你打算搞一个更先进的协程模型，可以编写一个`CoroutineMixIn`：
```python
class MyTCPServer(TCPServer, CoroutineMixIn):
    pass
```
这样一来，我们不需要复杂而庞大的继承链，只要选择组合不同的类的功能，就可以快速构造出所需的子类。
由于`Python`允许使用多重继承，因此，`MixIn`就是一种常见的设计。
只允许单一继承的语言（如`Java`）不能使用`MixIn`的设计。
#### 定制类
##### `__str__`
我们先定义一个`Student`类，打印一个实例：
```python
class Student(object):
    def __init__(self, name):
        self.name = name
print(Student('Michael'))
<__main__.Student object at 0x109afb190>
```
打印出一堆`<__main__.Student object at 0x109afb190>`，不好看。
怎么才能打印得好看呢？只需要定义好`__str__()`方法，返回一个好看的字符串就可以了：
```python
class Student(object):
    def __init__(self, name):
        self.name = name
    def __str__(self):
        return 'Student object (name: %s)' % self.name
print(Student('Michael'))
Student object (name: Michael)
```
这样打印出来的实例，不但好看，而且容易看出实例内部重要的数据。
但是细心的朋友会发现直接敲变量不用`print`，打印出来的实例还是不好看：
```python
s = Student('Michael')
s
<__main__.Student object at 0x109afb310>
```
这是因为直接显示变量调用的不是`__str__()`，而是`__repr__()`，两者的区别是`__str__()`返回用户看到的字符串，而`__repr__()`返回程序开发者看到的字符串，也就是说，`__repr__()`是为调试服务的。

解决办法是再定义一个`__repr__()`。但是通常`__str__()`和`__repr__()`代码都是一样的，所以，有个偷懒的写法：
```python
class Student(object):
    def __init__(self, name):
        self.name = name
    def __str__(self):
        return 'Student object (name=%s)' % self.name
    __repr__ = __str__
```
##### `__iter__`
如果一个类想被用于`for ... in`循环，类似`list`或`tuple`那样，就必须实现一个`__iter__()`方法，该方法返回一个迭代对象，然后，`Python`的`for`循环就会不断调用该迭代对象的`__next__()`方法拿到循环的下一个值，直到遇到`StopIteration`错误时退出循环。
我们以斐波那契数列为例，写一个`Fib`类，可以作用于`for`循环：
```python
class Fib(object):
    def __init__(self):
        self.a, self.b = 0, 1 # 初始化两个计数器a，b
    def __iter__(self):
        return self # 实例本身就是迭代对象，故返回自己
    def __next__(self):
        self.a, self.b = self.b, self.a + self.b # 计算下一个值
        if self.a > 100000: # 退出循环的条件
            raise StopIteration()
        return self.a # 返回下一个值
```
现在，试试把`Fib`实例作用于`for`循环：
```python
for n in Fib():
    print(n)
1
1
2
3
5
...
46368
75025
```
##### `__getitem__`
`Fib`实例虽然能作用于`for`循环，看起来和`list`有点像，但是，把它当成`list`来使用还是不行,要表现得像`list`那样按照下标取出元素，需要实现`__getitem__()`方法：
```python
class Fib(object):
    def __getitem__(self, n):
        a, b = 1, 1
        for x in range(n):
            a, b = b, a + b
        return a
f = Fib()
f[0]
1
f[1]
1
f[2]
2
f[3]
3
f[10]
89
f[100]
573147844013817084101
class Fib(object):
    def __getitem__(self, n):
        if isinstance(n, int): # n是索引
            a, b = 1, 1
            for x in range(n):
                a, b = b, a + b
            return a
        if isinstance(n, slice): # n是切片
            start = n.start
            stop = n.stop
            if start is None:
                start = 0
            a, b = 1, 1
            L = []
            for x in range(stop):
                if x >= start:
                    L.append(a)
                a, b = b, a + b
            return L
f = Fib()
f[0:5]
[1, 1, 2, 3, 5]
f[:10]
[1, 1, 2, 3, 5, 8, 13, 21, 34, 55]
```
如果把对象看成`dict`，`__getitem__()`的参数也可能是一个可以作`key`的`object`，例如`str`。
与之对应的是`__setitem__()`方法，把对象视作`list`或`dict`来对集合赋值。最后，还有一个`__delitem__()`方法，用于删除某个元素。
总之，通过上面的方法，我们自己定义的类表现得和`Python`自带的`list`、`tuple`、`dict`没什么区别，这完全归功于动态语言的“鸭子类型”，不需要强制继承某个接口。
##### `__getattr__`
正常情况下，当我们调用类的方法或属性时，如果不存在，就会报错。比如定义`Student`类：
```python
class Student(object):
    def __init__(self):
        self.name = 'Michael'
```
调用`name`属性，没问题，但是，调用不存在的`score`属性，就有问题了：
```python
s = Student()
print(s.name)
Michael
print(s.score)
Traceback (most recent call last):
AttributeError: 'Student' object has no attribute 'score'
```
错误信息很清楚地告诉我们，没有找到`score`这个`attribute`。

要避免这个错误，除了可以加上一个`score`属性外，`Python`还有另一个机制，那就是写一个`__getattr__()`方法，动态返回一个属性。修改如下：
```python
class Student(object):

    def __init__(self):
        self.name = 'Michael'

    def __getattr__(self, attr):
        if attr=='score':
            return 99
```
当调用不存在的属性时，比如`score`，`Python`解释器会试图调用`__getattr__(self, 'score')`来尝试获得属性，这样，我们就有机会返回`score`的值。
返回函数也是完全可以的：
```oython
class Student(object):
    def __getattr__(self, attr):
        if attr=='age':
            return lambda: 25
```
只是调用方式要变为：
```python
s.age()
25
```
注意，只有在没有找到属性的情况下，才调用`__getattr__`，已有的属性，比如`name`，不会在_`_getattr__`中查找。
此外，注意到任意调用如`s.abc`都会返回`None`，这是因为我们定义的`__getattr__`默认返回就是`None`。要让`class`只响应特定的几个属性，我们就要按照约定，抛出`AttributeError`的错误：
```python
class Student(object):

    def __getattr__(self, attr):
        if attr=='age':
            return lambda: 25
        raise AttributeError('\'Student\' object has no attribute \'%s\'' % attr)
```
这实际上可以把一个类的所有属性和方法调用全部动态化处理了，不需要任何特殊手段。
##### `__call__`
一个对象实例可以有自己的属性和方法，当我们调用实例方法时，我们用`instance.method()`来调用。能不能直接在实例本身上调用呢？在`Python`中，答案是肯定的。
任何类，只需要定义一个`__call__()`方法，就可以直接对实例进行调用。请看示例：
```python
class Student(object):
    def __init__(self, name):
        self.name = name

    def __call__(self):
        print('My name is %s.' % self.name)
调用方式如下：
```python
s = Student('Michael')
s() # self参数不要传入
My name is Michael.
```
`__call__()`还可以定义参数。对实例进行直接调用就好比对一个函数进行调用一样，所以你完全可以把对象看成函数，把函数看成对象，因为这两者之间本来就没啥根本的区别。
如果你把对象看成函数，那么函数本身其实也可以在运行期动态创建出来，因为类的实例都是运行期创建出来的，这么一来，我们就模糊了对象和函数的界限。
那么，怎么判断一个变量是对象还是函数呢？其实，更多的时候，我们需要判断一个对象是否能被调用，能被调用的对象就是一个`Callable`对象，比如函数和我们上面定义的带有`__call__()`的类实例。
通过`callable()`函数，我们就可以判断一个对象是否是“可调用”对象。
#### 枚举类
```python
from enum import Enum
Month = Enum('Month', ('Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'))
```
这样我们就获得了`Month`类型的枚举类，可以直接使用`Month.Jan`来引用一个常量，或者枚举它的所有成员：
```python
for name, member in Month.__members__.items():
    print(name, '=>', member, ',', member.value)
```
`value`属性则是自动赋给成员的`int`常量，默认从1开始计数。
如果需要更精确地控制枚举类型，可以从`Enum`派生出自定义类：
```python
from enum import Enum, unique
@unique
class Weekday(Enum):
    Sun = 0 # Sun的value被设定为0
    Mon = 1
    Tue = 2
    Wed = 3
    Thu = 4
    Fri = 5
    Sat = 6
```
`@unique`装饰器可以帮助我们检查保证没有重复值。
访问这些枚举类型可以有若干种方法：
```python
day1 = Weekday.Mon
print(day1)
Weekday.Mon
print(Weekday.Tue)
Weekday.Tue
print(Weekday['Tue'])
Weekday.Tue
print(Weekday.Tue.value)
2
rint(day1 == Weekday.Mon)
True
print(day1 == Weekday.Tue)
False
rint(Weekday(1))
Weekday.Mon
print(day1 == Weekday(1))
True
Weekday(7)
Traceback (most recent call last):
ValueError: 7 is not a valid Weekday
for name, member in Weekday.__members__.items():
    print(name, '=>', member)
Sun => Weekday.Sun
Mon => Weekday.Mon
Tue => Weekday.Tue
Wed => Weekday.Wed
Thu => Weekday.Thu
Fri => Weekday.Fri
Sat => Weekday.Sat
```
可见，既可以用成员名称引用枚举常量，又可以直接根据`value`的值获得枚举常量。
`Enum`可以把一组相关常量定义在一个`class`中，且`class`不可变，而且成员可以直接比较。
#### 使用元类
动态语言和静态语言最大的不同，就是函数和类的定义，不是编译时定义的，而是运行时动态创建的。
比方说我们要定义一个`Hello`的`class`，就写一个`hello.py`模块：
```python
class Hello(object):
    def hello(self, name='world'):
        print('Hello, %s.' % name)
```
当`Python`解释器载入`hello`模块时，就会依次执行该模块的所有语句，执行结果就是动态创建出一个`Hello`的`class`对象，测试如下：
```python
from hello import Hello
h = Hello()
h.hello()
Hello, world.
print(type(Hello))
<class 'type'>
print(type(h))
<class 'hello.Hello'>
```
`type()`函数可以查看一个类型或变量的类型，`Hello`是一个`class`，它的类型就是`type`，而`h`是一个实例，它的类型就是`class Hello`。

我们说`class`的定义是运行时动态创建的，而创建`class`的方法就是使用`type()`函数。

`type()`函数既可以返回一个对象的类型，又可以创建出新的类型，比如，我们可以通过`type()`函数创建出Hello类，而无需通过`class Hello(object)`...的定义：
```python
def fn(self, name='world'): # 先定义函数
    print('Hello, %s.' % name)
Hello = type('Hello', (object,), dict(hello=fn)) # 创建Hello class
h = Hello()
h.hello()
Hello, world.
print(type(Hello))
<class 'type'>
print(type(h))
<class '__main__.Hello'>
```
要创建一个`class`对象，`type()`函数依次传入3个参数：
`class`的名称；
继承的父类集合，注意`Python`支持多重继承，如果只有一个父类，别忘了`tuple`的单元素写法；
class的方法名称与函数绑定，这里我们把函数`fn`绑定到方法名`hello`上。
通过`type()`函数创建的类和直接写`class`是完全一样的，因为`Python`解释器遇到`class`定义时，仅仅是扫描一下`class`定义的语法，然后调用`type()`函数创建出`class`。
正常情况下，我们都用`class Xxx...`来定义类，但是，`type()`函数也允许我们动态创建出类来，也就是说，动态语言本身支持运行期动态创建类，这和静态语言有非常大的不同，要在静态语言运行期创建类，必须构造源代码字符串再调用编译器，或者借助一些工具生成字节码实现，本质上都是动态编译，会非常复杂。
`metaclass`
除了使用`type()`动态创建类以外，要控制类的创建行为，还可以使用`metaclass`。
`metaclass`，直译为元类，简单的解释就是：
当我们定义了类以后，就可以根据这个类创建出实例，所以：先定义类，然后创建实例。
但是如果我们想创建出类呢？那就必须根据`metaclass`创建出类，所以：先定义`metaclass`，然后创建类。
连接起来就是：先定义`metaclass`，就可以创建类，最后创建实例。
所以，`metaclass`允许你创建类或者修改类。换句话说，你可以把类看成是`metaclass`创建出来的“实例”。

`metaclass`是Python面向对象里最难理解，也是最难使用的魔术代码。正常情况下，你不会碰到需要使用`metaclass`的情况，所以，以下内容看不懂也没关系，因为基本上你不会用到。
我们先看一个简单的例子，这个`metaclass`可以给我们自定义的`MyList`增加一个`add`方法：

定义`ListMetaclass`，按照默认习惯，`metaclass`的类名总是以`Metaclass`结尾，以便清楚地表示这是一个`metaclass`：
```python
# metaclass是类的模板，所以必须从`type`类型派生：
class ListMetaclass(type):
    def __new__(cls, name, bases, attrs):
        attrs['add'] = lambda self, value: self.append(value)
        return type.__new__(cls, name, bases, attrs)
有了`ListMetaclass`，我们在定义类的时候还要指示使用`ListMetaclass`来定制类，传入关键字参数`metaclass`：
```python
class MyList(list, metaclass=ListMetaclass):
    pass
```
当我们传入关键字参数`metaclass`时，魔术就生效了，它指示`Python`解释器在创建`MyList`时，要通过`ListMetaclass.__new__()`来创建，在此，我们可以修改类的定义，比如，加上新的方法，然后，返回修改后的定义。
`__new__()`方法接收到的参数依次是：
当前准备创建的类的对象；
类的名字；
类继承的父类集合；
类的方法集合。
测试一下MyList是否可以调用add()方法：
```python
L = MyList()
L.add(1)
L
[1]
```
而普通的list没有`add()`方法：
```python
L2 = list()
L2.add(1)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'list' object has no attribute 'add'
```
动态修改有什么意义？直接在`MyList`定义中写上`add()`方法不是更简单吗？正常情况下，确实应该直接写，通过`metaclass`修改纯属变态。
## 标准库
### `datetime`
```python
from datetime import datetime, date, time
dt = datetime(2011, 10, 29, 20, 30, 21)
dt.day
29
dt.minute
30
```
- 根据`datetime`实例，你可以用`date`和`time`提取出各自的对象：
```python
dt.date()
datetime.date(2011, 10, 29)
dt.time()
datetime.time(20, 30, 21)
```
- `strftime`方法可以将`datetime`格式化为字符串：
```python
dt.strftime('%m/%d/%Y %H:%M')
'10/29/2011 20:30'
```
- `strptime`可以将字符串转换成`datetime`对象：
```python
datetime.strptime('20091031', '%Y%m%d')
datetime.datetime(2009, 10, 31, 0, 0)
```
- 格式化命令
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200317190745.png)
### `collections`
#### `namedtuple`
```python
from collections import namedtuple
Point = namedtuple('Point', ['x', 'y'])
p = Point(1, 2)
p.x
1
p.y
2
```
`namedtuple`是一个函数，它用来创建一个自定义的`tuple`对象，并且规定了`tuple`元素的个数，并可以用属性而不是索引来引用`tuple`的某个元素。
这样一来，我们用`namedtuple`可以很方便地定义一种数据类型，它具备`tuple`的不变性，又可以根据属性来引用，使用十分方便。
#### `deque`
`deque`是为了高效实现插入和删除操作的双向列表，适合用于队列和栈：
```python
from collections import deque
q = deque(['a', 'b', 'c'])
q.append('x')
q.appendleft('y')
q
deque(['y', 'a', 'b', 'c', 'x'])
```
`deque`除了实现`list`的`append()`和`pop()`外，还支持`appendleft()`和`popleft()`，这样就可以非常高效地往头部添加或删除元素。
#### `defaultdict`
使用`dict`时，如果引用的`Key`不存在，就会抛出`KeyError`。如果希望`key`不存在时，返回一个默认值，就可以用`defaultdict`：
```python
from collections import defaultdict
dd = defaultdict(lambda: 'N/A')
dd['key1'] = 'abc'
dd['key1'] # key1存在
'abc'
dd['key2'] # key2不存在，返回默认值
'N/A'
```
默认值是调用函数返回的，而函数在创建`defaultdict`对象时传入。除了在`Key`不存在时返回默认值，`defaultdict`的其他行为跟`dict`是完全一样的。
#### `OrderedDict`
使用`dict`时，`Key`是无序的。在对`dict`做迭代时，我们无法确定`Key`的顺序。
如果要保持`Key`的顺序，可以用`OrderedDict`：
```python
from collections import OrderedDict
d = dict([('a', 1), ('b', 2), ('c', 3)])
d # dict的Key是无序的
{'a': 1, 'c': 3, 'b': 2}
od = OrderedDict([('a', 1), ('b', 2), ('c', 3)])
od # OrderedDict的Key是有序的
OrderedDict([('a', 1), ('b', 2), ('c', 3)])
```
注意，`OrderedDict的Key`会按照插入的顺序排列，不是`Key`本身排序：
```python
od = OrderedDict()
od['z'] = 1
od['y'] = 2
od['x'] = 3
list(od.keys()) # 按照插入的Key的顺序返回
['z', 'y', 'x']
```
`OrderedDict`可以实现一个`FIFO`（先进先出）的`dict`，当容量超出限制时，先删除最早添加的`Key`：
```python
from collections import OrderedDict
class LastUpdatedOrderedDict(OrderedDict):
    def __init__(self, capacity):
        super(LastUpdatedOrderedDict, self).__init__()
        self._capacity = capacity
    def __setitem__(self, key, value):
        containsKey = 1 if key in self else 0
        if len(self) - containsKey >= self._capacity:
            last = self.popitem(last=False)
            print('remove:', last)
        if containsKey:
            del self[key]
            print('set:', (key, value))
        else:
            print('add:', (key, value))
        OrderedDict.__setitem__(self, key, value)
```
#### `ChainMap`
`ChainMap`可以把一组`dict`串起来并组成一个逻辑上的`dict`。`ChainMap`本身也是一个`dict`，但是查找的时候，会按照顺序在内部的`dict`依次查找。
什么时候使用`ChainMap`最合适？举个例子：应用程序往往都需要传入参数，参数可以通过命令行传入，可以通过环境变量传入，还可以有默认参数。我们可以用`ChainMap`实现参数的优先级查找，即先查命令行参数，如果没有传入，再查环境变量，如果没有，就使用默认参数。
下面的代码演示了如何查找user和color这两个参数：
``` python
from collections import ChainMap
import os, argparse
# 构造缺省参数:
defaults = {
    'color': 'red',
    'user': 'guest'
}
# 构造命令行参数:
parser = argparse.ArgumentParser()
parser.add_argument('-u', '--user')
parser.add_argument('-c', '--color')
namespace = parser.parse_args()
command_line_args = { k: v for k, v in vars(namespace).items() if v }
# 组合成ChainMap:
combined = ChainMap(command_line_args, os.environ, defaults)
# 打印参数:
print('color=%s' % combined['color'])
print('user=%s' % combined['user'])
```
没有任何参数时，打印出默认参数：
```shell
$ python3 use_chainmap.py 
color=red
user=guest
```
当传入命令行参数时，优先使用命令行参数：
```shell
$ python3 use_chainmap.py -u bob
color=red
user=bob
```
同时传入命令行参数和环境变量，命令行参数的优先级较高：
```shell
$ user=admin color=green python3 use_chainmap.py -u bob
color=green
user=bob
```
#### `Counter`
`Counter`是一个简单的计数器，例如，统计字符出现的个数：
```python
from collections import Counter
c = Counter()
for ch in 'programming':
c[ch] = c[ch] + 1
c
Counter({'g': 2, 'm': 2, 'r': 2, 'a': 1, 'i': 1, 'o': 1, 'n': 1, 'p': 1})
c.update('hello') # 也可以一次性update
c
Counter({'r': 2, 'o': 2, 'g': 2, 'm': 2, 'l': 2, 'p': 1, 'a': 1, 'i': 1, 'n': 1, 'h': 1, 'e': 1})
```
`Counter`实际上也是`dict`的一个子类，上面的结果可以看出每个字符出现的次数。


