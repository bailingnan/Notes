# Python代码段
- [Python代码段](#python代码段)
  - [列表](#列表)
  - [元组](#元组)
  - [字符串](#字符串)
  - [运算符](#运算符)
  - [函数](#函数)
  - [字典](#字典)
  - [集合](#集合)
  - [`collections`](#collections)
    - [`deque`](#deque)
    - [`Counter`](#counter)
  - [`heapq`](#heapq)
  - [`itertools`](#itertools)
    - [`groupby`](#groupby)
  - [迭代器与生成器](#迭代器与生成器)
## 列表
- 重复元素判定
以下方法可以检查给定列表是不是存在重复元素，它会使用 `set()` 函数来移除所有重复元素。
```Python
def all_unique(lst):
    return len(lst) == len(set(lst))
x = [1,1,2,2,3,2,3,4,5,6]
y = [1,2,3,4,5]
all_unique(x) # False
all_unique(y) # True
```
- 取两个列表交集：
```Python
def common_elements(list1, list2):
    common = set(list1).intersection(set(list2))
    return list(common)
```
- 压缩

这个方法可以将布尔型的值去掉，例如`(False，None，0，“”)`，它使用 `filter()` 函数。
```Python
def compact(lst):
    return list(filter(bool, lst))
compact([0, 1, False, 2, '', 3, 'a', 's', 34])
# [ 1, 2, 3, 'a', 's', 34 ]
```
- 解包

如下代码段可以将打包好的成对列表解开成两组不同的元组。
```Python
array = [['a', 'b'], ['c', 'd'], ['e', 'f']]
transposed = zip(*array)
print(transposed)
# [('a', 'c', 'e'), ('b', 'd', 'f')]
```
- 逗号连接

下面的代码可以将列表连接成单个字符串，且每一个元素间的分隔方式设置为了逗号。
```Python
hobbies = ["basketball", "football", "swimming"]
print("My hobbies are: " + ", ".join(hobbies))
# My hobbies are: basketball, football, swimming
```
- 展开列表

该方法将通过递归的方式将列表的嵌套展开为单个列表。
```Python
def spread(arg):
    ret = []
    for i in arg:
        if isinstance(i, list):
            ret.extend(i)
        else:
            ret.append(i)
    return ret
def deep_flatten(lst):
    result = []
    result.extend(
        spread(list(map(lambda x: deep_flatten(x) if type(x) == list else x, lst))))
    return result
deep_flatten([1, [2], [[3], 4], 5]) # [1,2,3,4,5]
```
可以写一个包含 `yield from` 语句的递归生成器来轻松解决这个问题。比如：
```python
from collections import Iterable
def flatten(items, ignore_types=(str, bytes)):
    for x in items:
        if isinstance(x, Iterable) and not isinstance(x, ignore_types):
            yield from flatten(x)
        else:
            yield x

items = [1, 2, [3, 4, [5, 6], 7], 8]
# Produces 1 2 3 4 5 6 7 8
for x in flatten(items):
    print(x)
```
在上面代码中， `isinstance(x, Iterable)` 检查某个元素是否是可迭代的。 如果是的话， `yield from` 就会返回所有子例程的值。最终返回结果就是一个没有嵌套的简单序列了。
额外的参数 `ignore_types` 和检测语句 `isinstance(x, ignore_types)` 用来将字符串和字节排除在可迭代对象外，防止将它们再展开成单个的字符。 这样的话字符串数组就能最终返回我们所期望的结果了。比如：
```python
items = ['Dave', 'Paula', ['Thomas', 'Lewis']]
for x in flatten(items):
    print(x)
Dave
Paula
Thomas
Lewis
```
语句 `yield from` 在你想在生成器中调用其他生成器作为子例程的时候非常有用。 如果你不使用它的话，那么就必须写额外的 `for` 循环了。比如：
```python
def flatten(items, ignore_types=(str, bytes)):
    for x in items:
        if isinstance(x, Iterable) and not isinstance(x, ignore_types):
            for i in flatten(x):
                yield i
        else:
            yield x
```
尽管只改了一点点，但是 `yield from` 语句看上去感觉更好，并且也使得代码更简洁清爽。
之前提到的对于字符串和字节的额外检查是为了防止将它们再展开成单个字符。 如果还有其他你不想展开的类型，修改参数 `ignore_types` 即可。
- 列表的差

该方法将返回第一个列表的元素，其不在第二个列表内。如果同时要反馈第二个列表独有的元素，还需要加一句 `set_b.difference(set_a)`。
```Python
def difference(a, b):
    set_a = set(a)
    set_b = set(b)
    comparison = set_a.difference(set_b)
    return list(comparison)
difference([1,2,3], [1,2,4]) # [3]
```
- 通过函数取差

如下方法首先会应用一个给定的函数，然后再返回应用函数后结果有差别的列表元素。
```Python
def difference_by(a, b, fn):
    b = set(map(fn, b))
    return [item for item in a if fn(item) not in b]
from math import floor
difference_by([2.1, 1.2], [2.3, 3.4],floor) # [1.2]
difference_by([{ 'x': 2 }, { 'x': 1 }], [{ 'x': 1 }], lambda v : v['x'])
# [ { x: 2 } ]
```
- Given a list of N numbers。

给定一个含有`N`个数字的列表。
使用单一的列表生成式来产生一个新的列表，该列表只包含满足以下条件的值：
(a)偶数值
(b)元素为原始列表中偶数切片。
例如，如果`list[2]`包含的值是偶数。那么这个值应该被包含在新的列表当中。因为这个数字同时在原始列表的偶数序列（2为偶数）上。然而，如果`list[3]`包含一个偶数，
那个数字不应该被包含在新的列表当中，因为它在原始列表的奇数序列上。
对此问题的简单解决方法如下：
```Python
[x for x in list[::2] if x%2 == 0]
```
例如，给定列表如下：
```Python
list = [ 1 , 3 , 5 , 8 , 10 , 13 , 18 , 36 , 78 ]
```
列表生成式`[x for x in list[::2] if x%2 == 0]` 的结果是，`[10, 18, 78]`
这个表达式工作的步骤是，第一步取出偶数切片的数字，
第二步剔除其中所有奇数。
- 更长列表
```Python
def max_length(*lst):
    return max(*lst, key=lambda v: len(v))
r = max_length([1, 2, 3], [4, 5, 6, 7], [8])
print(f'更长的列表是{r}')  # [4, 5, 6, 7]
r = max_length([1, 2, 3], [4, 5, 6, 7], [8, 9])
print(f'更长的列表是{r}')  # [4, 5, 6, 7]
```
- 求众数
```Python
def top1(lst):
    return max(lst, default='列表为空', key=lambda v: lst.count(v))
lst = [1, 3, 3, 2, 1, 1, 2]
r = top1(lst)
print(f'{lst}中出现次数最多的元素为:{r}')  # [1, 3, 3, 2, 1, 1, 2]中出现次数最多的元素为:1
```
- 列表之最
```Python
def max_lists(*lst):
    return max(max(*lst, key=lambda v: max(v)))
r = max_lists([1, 2, 3], [6, 7, 8], [4, 5])
print(r)  # 8
```
- 按条件分组
```Python
def bif_by(lst, f):
    return [ [x for x in lst if f(x)],[x for x in lst if not f(x)]]
records = [25,89,31,34]
bif_by(records, lambda x: x<80) # [[25, 31, 34], [89]]
```
- 解压可迭代对象赋值给多个变量

`Python` 的星号表达式可以用来解决这个问题。比如，你在学习一门课程，在学期末的时候， 你想统计下家庭作业的平均成绩，但是排除掉第一个和最后一个分数。如果只有四个分数，你可能就直接去简单的手动赋值， 但如果有 24 个呢？这时候星号表达式就派上用场了：
```Python
def drop_first_last(grades):
    first, *middle, last = grades
    return avg(middle)
```
另外一种情况，假设你现在有一些用户的记录列表，每条记录包含一个名字、邮件，接着就是不确定数量的电话号码。 你可以像下面这样分解这些记录：
```Python
record = ('Dave', 'dave@example.com', '773-555-1212', '847-555-1212')
name, email, *phone_numbers = record
name
'Dave'
email
'dave@example.com'
phone_numbers
['773-555-1212', '847-555-1212']
```
值得注意的是上面解压出的 `phone_numbers` 变量永远都是列表类型，不管解压的电话号码数量是多少（包括 `0` 个）。 所以，任何使用到 `phone_numbers` 变量的代码就不需要做多余的类型检查去确认它是否是列表类型了。
星号表达式也能用在列表的开始部分。比如，你有一个公司前 8 个月销售数据的序列， 但是你想看下最近一个月数据和前面 7 个月的平均值的对比。你可以这样做：
```Python
*trailing_qtrs, current_qtr = sales_record
trailing_avg = sum(trailing_qtrs) / len(trailing_qtrs)
return avg_comparison(trailing_avg, current_qtr)
```
下面是在 `Python` 解释器中执行的结果：
```Python
*trailing, current = [10, 8, 7, 1, 9, 5, 10, 3]
trailing
[10, 8, 7, 1, 9, 5, 10]
current
3
```
扩展的迭代解压语法是专门为解压不确定个数或任意个数元素的可迭代对象而设计的。 通常，这些可迭代对象的元素结构有确定的规则（比如第1个元素后面都是电话号码）， 星号表达式让开发人员可以很容易的利用这些规则来解压出元素来。 而不是通过一些比较复杂的手段去获取这些关联的元素值。
值得注意的是，星号表达式在迭代元素为可变长元组的序列时是很有用的。 比如，下面是一个带有标签的元组序列：
```Python
records = [
    ('foo', 1, 2),
    ('bar', 'hello'),
    ('foo', 3, 4),
]

def do_foo(x, y):
    print('foo', x, y)

def do_bar(s):
    print('bar', s)

for tag, *args in records:
    if tag == 'foo':
        do_foo(*args)
    elif tag == 'bar':
        do_bar(*args)
```
星号解压语法在字符串操作的时候也会很有用，比如字符串的分割。
代码示例：
```Python
line = 'nobody:*:-2:-2:Unprivileged User:/var/empty:/usr/bin/false'
uname, *fields, homedir, sh = line.split(':')
uname
'nobody'
homedir
'/var/empty'
sh
'/usr/bin/false'
```
有时候，你想解压一些元素后丢弃它们，你不能简单就使用 `*` ， 但是你可以使用一个普通的废弃名称，比如 `_` 或者 `ign(ignore)`。
```Python
record = ('ACME', 50, 123.45, (12, 18, 2012))
name, *_, (*_, year) = record
name
'ACME'
year
2012
```
在很多函数式语言中，星号解压语法跟列表处理有许多相似之处。比如，如果你有一个列表， 你可以很容易的将它分割成前后两部分：
```Python
items = [1, 10, 7, 4, 5, 9]
head, *tail = items
head
1
tail
[10, 7, 4, 5, 9]
```
如果你够聪明的话，还能用这种分割语法去巧妙的实现递归算法。比如：
```Python
def sum(items):
    head, *tail = items
    return head + sum(tail) if tail else head
sum(items)
36
```
然后，由于语言层面的限制，递归并不是 `Python` 擅长的。 因此，最后那个递归演示仅仅是个好奇的探索罢了，对这个不要太认真了。
- 删除序列相同元素并保持顺序

如果序列上的值都是 `hashable` 类型，那么可以很简单的利用集合或者生成器来解决这个问题。比如：
```python
def dedupe(items):
    seen = set()
    for item in items:
        if item not in seen:
            yield item
            seen.add(item)
```
下面是使用上述函数的例子：
```python
a = [1, 5, 2, 1, 9, 1, 5, 10]
list(dedupe(a))
[1, 5, 2, 9, 10]
```
这个方法仅仅在序列中元素为 `hashable` 的时候才管用。 如果你想消除元素不可哈希（比如 `dict` 类型）的序列中重复元素的话，你需要将上述代码稍微改变一下，就像这样：
```python
def dedupe(items, key=None):
    seen = set()
    for item in items:
        val = item if key is None else key(item)
        if val not in seen:
            yield item
            seen.add(val)
```
这里的`key`参数指定了一个函数，将序列元素转换成 `hashable` 类型。下面是它的用法示例：
```python
a = [ {'x':1, 'y':2}, {'x':1, 'y':3}, {'x':1, 'y':2}, {'x':2, 'y':4}]
list(dedupe(a, key=lambda d: (d['x'],d['y'])))
[{'x': 1, 'y': 2}, {'x': 1, 'y': 3}, {'x': 2, 'y': 4}]
list(dedupe(a, key=lambda d: d['x']))
[{'x': 1, 'y': 2}, {'x': 2, 'y': 4}]
```
如果你想基于单个字段、属性或者某个更大的数据结构来消除重复元素，第二种方案同样可以胜任。
如果你仅仅就是想消除重复元素，通常可以简单的构造一个集合。比如：
```python
a
[1, 5, 2, 1, 9, 1, 5, 10]
set(a)
{1, 2, 10, 5, 9}
```
然而，这种方法不能维护元素的顺序，生成的结果中的元素位置被打乱。而上面的方法可以避免这种情况。
在本节中我们使用了生成器函数让我们的函数更加通用，不仅仅是局限于列表处理。 比如，如果如果你想读取一个文件，消除重复行，你可以很容易像这样做：
```python
with open(somefile,'r') as f:
    for line in dedupe(f):
        ...
```
上述key函数参数模仿了 `sorted()` , `min()` 和 `max()` 等内置函数的相似功能。 
- 命名切片

假定你要从一个记录（比如文件或其他类似格式）中的某些固定位置提取字段：
```python
######    0123456789012345678901234567890123456789012345678901234567890'
record = '....................100 .......513.25 ..........'
cost = int(record[20:23]) * float(record[31:37])
```
与其那样写，为什么不想这样命名切片呢：
```python
SHARES = slice(20, 23)
PRICE = slice(31, 37)
cost = int(record[SHARES]) * float(record[PRICE])
```
在这个版本中，你避免了使用大量难以理解的硬编码下标。这使得你的代码更加清晰可读。
一般来讲，代码中如果出现大量的硬编码下标会使得代码的可读性和可维护性大大降低。 比如，如果你回过来看看一年前你写的代码，你会摸着脑袋想那时候自己到底想干嘛啊。 这是一个很简单的解决方案，它让你更加清晰的表达代码的目的。
内置的 `slice()` 函数创建了一个切片对象。所有使用切片的地方都可以使用切片对象。比如：
```python
items = [0, 1, 2, 3, 4, 5, 6]
a = slice(2, 4)
items[2:4]
[2, 3]
items[a]
[2, 3]
items[a] = [10,11]
items
[0, 1, 10, 11, 4, 5, 6]
del items[a]
items
[0, 1, 4, 5, 6]
```
如果你有一个切片对象`a`，你可以分别调用它的 `a.start` , `a.stop` , `a.step` 属性来获取更多的信息。比如：
```python
a = slice(5, 50, 2)
a.start
5
a.stop
50
a.step
2
```
另外，你还可以通过调用切片的 `indices(size)` 方法将它映射到一个已知大小的序列上。 这个方法返回一个三元组 `(start, stop, step)` ，所有的值都会被缩小，直到适合这个已知序列的边界为止。 这样，使用的时就不会出现 `IndexError` 异常。比如：
```python
s = 'HelloWorld'
a.indices(len(s))
(5, 10, 2)
for i in range(*a.indices(len(s))):
    print(s[i])
W
r
d
```
- 排序
```python
s=['ab','abc','a','djkj']
b=sorted(s,key=lambda x:len(x))# 有返回值
s.sort(key=len)# 原地排序
```
- 排序不支持原生比较的对象

内置的 `sorted()` 函数有一个关键字参数 `key` ，可以传入一个 `callable` 对象给它， 这个 `callable` 对象对每个传入的对象返回一个值，这个值会被 `sorted` 用来排序这些对象。 比如，如果你在应用程序里面有一个 `User` 实例序列，并且你希望通过他们的 `user_id` 属性进行排序， 你可以提供一个以 `User` 实例作为输入并输出对应 `user_id` 值的 `callable` 对象。比如：
```python
class User:
    def __init__(self, user_id):
        self.user_id = user_id

    def __repr__(self):
        return 'User({})'.format(self.user_id)
def sort_notcompare():
    users = [User(23), User(3), User(99)]
    print(users)
    print(sorted(users, key=lambda u: u.user_id))
```
另外一种方式是使用 `operator.attrgetter()` 来代替 `lambda` 函数：
```python
from operator import attrgetter
sorted(users, key=attrgetter('user_id'))
[User(3), User(23), User(99)]
```
选择使用 `lambda` 函数或者是 `attrgetter()` 可能取决于个人喜好。 但是， `attrgetter()` 函数通常会运行的快点，并且还能同时允许多个字段进行比较。 这个跟 `operator.itemgetter()` 函数作用于字典类型很类似。 例如，如果 `User` 实例还有一个 `first_name` 和 `last_name` 属性，那么可以向下面这样排序：
```python
by_name = sorted(users, key=attrgetter('last_name', 'first_name'))
```
同样需要注意的是，这一小节用到的技术同样适用于像 `min()` 和 `max()` 之类的函数。比如：
```python
min(users, key=attrgetter('user_id'))
User(3)
max(users, key=attrgetter('user_id'))
User(99)
```
## 元组
- 取元组元素
```Python
a,b,c = ('cat','dog','tiger')
# 提取首、尾两个元素：
first,*_,end = (1,2,3,4,5,6)
# 提取首、中、尾三部分：
first,*middle,end = (1,2,3,4,5,6)
```
## 字符串
- 字符元素组成判定

检查两个字符串的组成元素是不是一样的。
```Python
from collections import Counter
def anagram(first, second):
    return Counter(first) == Counter(second)
anagram("abcd3", "3acdb") # True
```
- N 次字符串

该代码块不需要循环语句
```Python
n = 2;
s ="Programming"
print(s * n)
# ProgrammingProgramming  
```
- 列表转字符串 `list -> str`
```Python
name_list = ['Zarten_1', 'Zarten_2', 'Zarten_3']
name_str = '&'.join(name_list) # &为列表元素之间分隔符
print(type(name_str), name_str)
```
- 映射名称到序列元素

`collections.namedtuple()` 函数通过使用一个普通的元组对象来帮你解决这个问题。 这个函数实际上是一个返回 `Python` 中标准元组类型子类的一个工厂方法。 你需要传递一个类型名和你需要的字段给它，然后它就会返回一个类，你可以初始化这个类，为你定义的字段传递值等。 代码示例：
```python
from collections import namedtuple
Subscriber = namedtuple('Subscriber', ['addr', 'joined'])
sub = Subscriber('jonesy@example.com', '2012-10-19')
sub
Subscriber(addr='jonesy@example.com', joined='2012-10-19')
sub.addr
'jonesy@example.com'
sub.joined
'2012-10-19'
```
尽管 `namedtuple` 的实例看起来像一个普通的类实例，但是它跟元组类型是可交换的，支持所有的普通元组操作，比如索引和解压。 比如：
```python
len(sub)
2
addr, joined = sub
addr
'jonesy@example.com'
joined
'2012-10-19'
```
命名元组的一个主要用途是将你的代码从下标操作中解脱出来。 因此，如果你从数据库调用中返回了一个很大的元组列表，通过下标去操作其中的元素， 当你在表中添加了新的列的时候你的代码可能就会出错了。但是如果你使用了命名元组，那么就不会有这样的顾虑。
为了说明清楚，下面是使用普通元组的代码：
```python
def compute_cost(records):
    total = 0.0
    for rec in records:
        total += rec[1] * rec[2]
    return total
```
下标操作通常会让代码表意不清晰，并且非常依赖记录的结构。 下面是使用命名元组的版本：
```python
from collections import namedtuple
Stock = namedtuple('Stock', ['name', 'shares', 'price'])
def compute_cost(records):
    total = 0.0
    for rec in records:
        s = Stock(*rec)
        total += s.shares * s.price
    return total
```
命名元组另一个用途就是作为字典的替代，因为字典存储需要更多的内存空间。 如果你需要构建一个非常大的包含字典的数据结构，那么使用命名元组会更加高效。 但是需要注意的是，不像字典那样，一个命名元组是不可更改的。比如：
```python
s = Stock('ACME', 100, 123.45)
s
Stock(name='ACME', shares=100, price=123.45)
s.shares = 75
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
AttributeError: can't set attribute
```
如果你真的需要改变属性的值，那么可以使用命名元组实例的 `_replace()` 方法， 它会创建一个全新的命名元组并将对应的字段用新的值取代。比如：
```python
s = s._replace(shares=75)
s
Stock(name='ACME', shares=75, price=123.45)
```
`_replace()` 方法还有一个很有用的特性就是当你的命名元组拥有可选或者缺失字段时候， 它是一个非常方便的填充数据的方法。 你可以先创建一个包含缺省值的原型元组，然后使用 `_replace()` 方法创建新的值被更新过的实例。比如：
```python
from collections import namedtuple
Stock = namedtuple('Stock', ['name', 'shares', 'price', 'date', 'time'])
# Create a prototype instance
stock_prototype = Stock('', 0, 0.0, None, None)
# Function to convert a dictionary to a Stock
def dict_to_stock(s):
    return stock_prototype._replace(**s)
```
下面是它的使用方法：
```python
a = {'name': 'ACME', 'shares': 100, 'price': 123.45}
dict_to_stock(a)
Stock(name='ACME', shares=100, price=123.45, date=None, time=None)
b = {'name': 'ACME', 'shares': 100, 'price': 123.45, 'date': '12/17/2012'}
dict_to_stock(b)
Stock(name='ACME', shares=100, price=123.45, date='12/17/2012', time=None)
```
最后要说的是，如果你的目标是定义一个需要更新很多实例属性的高效数据结构，那么命名元组并不是你的最佳选择。 这时候你应该考虑定义一个包含 `__slots__` 方法的类。
- `x="abc"`,`y="def"`,`z=["d","e","f"]`,分别求出`x.join(y)`和`x.join(z)`返回的结果
```python
x="abc"
y="def"
z=["d","e","f"]
print(x.join(y),x.join(z))
dabceabcf dabceabcf
```
`join()`括号里面的是可迭代对象，`x`插入可迭代对象中间，形成字符串，结果一致。
- 去空格的两种方法
```python
str="hello world ha ha"
res=str.replace(" ","")
list=str.split(" ")
res="".join(list)
```
## 运算符
- 链式对比,我们可以在一行代码中使用不同的运算符对比多个不同的元素。
```Python
a = 3
print( 2 < a < 8) # True
print(1 == a < 2) # False
```
- 反向迭代
```Python
for i in reversed(range(1, 10)):
    print(i, end=',')
```
## 函数
- 链式函数调用

你可以在一行代码内调用多个函数。
```Python
def add(a, b):
    return a + b
def subtract(a, b):
    return a - b
a, b = 4, 5
print((subtract if a > b else add)(a, b)) # 9 
```
## 字典
- 合并字典
```Python
def merge_two_dicts(a, b):
    c = a.copy()   # make a copy of a 
    c.update(b)    # modify keys and values of a with the ones from b
    return c
a = { 'x': 1, 'y': 2}
b = { 'y': 3, 'z': 4}
print(merge_two_dicts(a, b))
# {'y': 3, 'x': 1, 'z': 4}
```
在 `Python 3.5` 或更高版本中，我们也可以用以下方式合并字典：
```Python
def merge_dictionaries(a, b)
   return {**a, **b}
a = { 'x': 1, 'y': 2}
b = { 'y': 3, 'z': 4}
print(merge_dictionaries(a, b))
# {'y': 3, 'x': 1, 'z': 4}
```
这是一般的字典合并写法
```Python
dic1 = {'x': 1, 'y': 2 }
dic2 = {'y': 3, 'z': 4 }
merged1 = {**dic1, **dic2} # {'x': 1, 'y': 3, 'z': 4}
```
修改`merged[‘x’]=10`，`dic1`中的`x`值不变，`merged`是重新生成的一个新字典。
但是，`ChainMap`却不同，它在内部创建了一个容纳这些字典的列表。因此使用`ChainMap`合并字典，修改`merged[‘x’]=10`后，`dic1`中的`x`值改变，如下所示：
```Python
from collections import ChainMap
merged2 = ChainMap(dic1,dic2)
print(merged2) # ChainMap({'x': 1, 'y': 2}, {'y': 3, 'z': 4})
```
**Python Cookbook**:
假如你有如下两个字典:
```python
a = {'x': 1, 'z': 3 }
b = {'y': 2, 'z': 4 }
```
现在假设你必须在两个字典中执行查找操作（比如先从 `a` 中找，如果找不到再在 `b` 中找）。 一个非常简单的解决方案就是使用 `collections` 模块中的 `ChainMap` 类。比如：
```python
from collections import ChainMap
c = ChainMap(a,b)
print(c['x']) # Outputs 1 (from a)
print(c['y']) # Outputs 2 (from b)
print(c['z']) # Outputs 3 (from a)
```
一个 `ChainMap` 接受多个字典并将它们在逻辑上变为一个字典。 然后，这些字典并不是真的合并在一起了， `ChainMap` 类只是在内部创建了一个容纳这些字典的列表 并重新定义了一些常见的字典操作来遍历这个列表。大部分字典操作都是可以正常使用的，比如：
```python
len(c)
3
list(c.keys())
['x', 'y', 'z']
list(c.values())
[1, 2, 3]
```
如果出现重复键，那么第一次出现的映射值会被返回。 因此，例子程序中的 `c['z']` 总是会返回字典 `a` 中对应的值，而不是 `b` 中对应的值。
对于字典的更新或删除操作总是影响的是列表中第一个字典。比如：
```python
c['z'] = 10
c['w'] = 40
del c['x']
a
{'w': 40, 'z': 10}
del c['y']
Traceback (most recent call last):
...
KeyError: "Key not found in the first mapping: 'y'"
```
`ChainMap` 对于编程语言中的作用范围变量（比如 `globals` , `locals` 等）是非常有用的。 事实上，有一些方法可以使它变得简单：
```python
values = ChainMap()
values['x'] = 1
# Add a new mapping
values = values.new_child()
values['x'] = 2
# Add a new mapping
values = values.new_child()
values['x'] = 3
values
ChainMap({'x': 3}, {'x': 2}, {'x': 1})
values['x']
3
# Discard last mapping
values = values.parents
values['x']
2
# Discard last mapping
values = values.parents
values['x']
1
values
ChainMap({'x': 1})
```
作为 `ChainMap` 的替代，你可能会考虑使用 `update()` 方法将两个字典合并。比如：
```python
a = {'x': 1, 'z': 3 }
b = {'y': 2, 'z': 4 }
merged = dict(b)
merged.update(a)
merged['x']
1
merged['y']
2
merged['z']
3
```
这样也能行得通，但是它需要你创建一个完全不同的字典对象（或者是破坏现有字典结构）。 同时，如果原字典做了更新，这种改变不会反应到新的合并字典中去。比如：
```python
a['x'] = 13
merged['x']
1
```
`ChainMap` 使用原来的字典，它自己不创建新的字典。所以它并不会产生上面所说的结果，比如：
```python
a = {'x': 1, 'z': 3 }
b = {'y': 2, 'z': 4 }
merged = ChainMap(a, b)
merged['x']
1
a['x'] = 42
merged['x'] # Notice change to merged dicts
42
```
- 将两个列表转化为字典

如下方法将会把两个列表转化为单个字典。
```Python
def to_dictionary(keys, values):
    return dict(zip(keys, values))
keys = ["a", "b", "c"]    
values = [2, 3, 4]
print(to_dictionary(keys, values))
# {'a': 2, 'c': 4, 'b': 3}
```
- 值最大的字典
```Python
def max_pairs(dic):
    if len(dic) == 0:
        return dic
    max_val = max(map(lambda v: v[1], dic.items()))
    return [item for item in dic.items() if item[1] == max_val]
r = max_pairs({'a': -10, 'b': 5, 'c': 3, 'd': 5})
print(r)  # [('b', 5), ('d', 5)]
```
- `topn`字典
```Python
from heapq import nlargest
# 返回字典d前n个最大值对应的键
def topn_dict(d, n):
    return nlargest(n, d, key=lambda k: d[k])
topn_dict({'a': 10, 'b': 8, 'c': 9, 'd': 10}, 3)  # ['a', 'd', 'c']
```
- 查找两字典的相同点
```python
a = {
    'x' : 1,
    'y' : 2,
    'z' : 3
}

b = {
    'w' : 10,
    'x' : 11,
    'y' : 2
}
```
为了寻找两个字典的相同点，可以简单的在两字典的 `keys()` 或者 `items()` 方法返回结果上执行集合操作。比如：
```python
# Find keys in common
a.keys() & b.keys() # { 'x', 'y' }
# Find keys in a that are not in b
a.keys() - b.keys() # { 'z' }
# Find (key,value) pairs in common
a.items() & b.items() # { ('y', 2) }
```
这些操作也可以用于修改或者过滤字典元素。 比如，假如你想以现有字典构造一个排除几个指定键的新字典。 下面利用字典推导来实现这样的需求：
```python
# Make a new dictionary with certain keys removed
c = {key:a[key] for key in a.keys() - {'z', 'w'}}
# c is {'x': 1, 'y': 2}
```
一个字典就是一个键集合与值集合的映射关系。 字典的 `keys()` 方法返回一个展现键集合的键视图对象。 键视图的一个很少被了解的特性就是它们也支持集合操作，比如集合并、交、差运算。 所以，如果你想对集合的键执行一些普通的集合操作，可以直接使用键视图对象而不用先将它们转换成一个 `set`。
字典的 `items()` 方法返回一个包含 `(键，值)` 对的元素视图对象。 这个对象同样也支持集合操作，并且可以被用来查找两个字典有哪些相同的键值对。
尽管字典的 `values()` 方法也是类似，但是它并不支持这里介绍的集合操作。 某种程度上是因为值视图不能保证所有的值互不相同，这样会导致某些集合操作会出现问题。 不过，如果你硬要在值上面执行这些集合操作的话，你可以先将值集合转换成`set`，然后再执行集合运算就行了。
- 字典的运算

考虑下面的股票名和价格映射字典：
```python
prices = {
    'ACME': 45.23,
    'AAPL': 612.78,
    'IBM': 205.55,
    'HPQ': 37.20,
    'FB': 10.75
}
```
为了对字典值执行计算操作，通常需要使用 `zip()` 函数先将键和值反转过来。 比如，下面是查找最小和最大股票价格和股票值的代码：
```python
min_price = min(zip(prices.values(), prices.keys()))
# min_price is (10.75, 'FB')
max_price = max(zip(prices.values(), prices.keys()))
# max_price is (612.78, 'AAPL')
```
类似的，可以使用 `zip()` 和 `sorted()` 函数来排列字典数据：
```python
prices_sorted = sorted(zip(prices.values(), prices.keys()))
# prices_sorted is [(10.75, 'FB'), (37.2, 'HPQ'),
#                   (45.23, 'ACME'), (205.55, 'IBM'),
#                   (612.78, 'AAPL')]
```
执行这些计算的时候，需要注意的是 `zip()` 函数创建的是一个只能访问一次的迭代器。 比如，下面的代码就会产生错误：
```python
prices_and_names = zip(prices.values(), prices.keys())
print(min(prices_and_names)) # OK
print(max(prices_and_names)) # ValueError: max() arg is an empty sequence
```
如果你在一个字典上执行普通的数学运算，你会发现它们仅仅作用于键，而不是值。比如：
```python
min(prices) # Returns 'AAPL'
max(prices) # Returns 'IBM'
```
这个结果并不是你想要的，因为你想要在字典的值集合上执行这些计算。 或许你会尝试着使用字典的 `values()` 方法来解决这个问题：
```python
min(prices.values()) # Returns 10.75
max(prices.values()) # Returns 612.78
```
不幸的是，通常这个结果同样也不是你想要的。 你可能还想要知道对应的键的信息（比如那种股票价格是最低的？）。
你可以在 `min()` 和 `max()` 函数中提供 `key` 函数参数来获取最小值或最大值对应的键的信息。比如：
```python
min(prices, key=lambda k: prices[k]) # Returns 'FB'
max(prices, key=lambda k: prices[k]) # Returns 'AAPL'
```
但是，如果还想要得到最小值，你又得执行一次查找操作。比如：
```python
min_value = prices[min(prices, key=lambda k: prices[k])]
```
前面的 `zip()` 函数方案通过将字典”反转”为 (值，键) 元组序列来解决了上述问题。 当比较两个元组的时候，值会先进行比较，然后才是键。 这样的话你就能通过一条简单的语句就能很轻松的实现在字典上的求最值和排序操作了。
需要注意的是在计算操作中使用到了 (值，键) 对。当多个实体拥有相同的值的时候，键会决定返回结果。 比如，在执行 `min()` 和 `max()` 操作的时候，如果恰巧最小或最大值有重复的，那么拥有最小或最大键的实体会返回：
```python
prices = { 'AAA' : 45.23, 'ZZZ': 45.23 }
min(zip(prices.values(), prices.keys()))
(45.23, 'AAA')
max(zip(prices.values(), prices.keys()))
(45.23, 'ZZZ')
```
- 通过某个关键字排序一个字典列表

通过使用 `operator` 模块的 `itemgetter` 函数，可以非常容易的排序这样的数据结构。 假设你从数据库中检索出来网站会员信息列表，并且以下列的数据结构返回：
```python
rows = [
    {'fname': 'Brian', 'lname': 'Jones', 'uid': 1003},
    {'fname': 'David', 'lname': 'Beazley', 'uid': 1002},
    {'fname': 'John', 'lname': 'Cleese', 'uid': 1001},
    {'fname': 'Big', 'lname': 'Jones', 'uid': 1004}
]
```
根据任意的字典字段来排序输入结果行是很容易实现的，代码示例：
```python
from operator import itemgetter
rows_by_fname = sorted(rows, key=itemgetter('fname'))
rows_by_uid = sorted(rows, key=itemgetter('uid'))
print(rows_by_fname)
print(rows_by_uid)
```
代码的输出如下：
```python
[{'fname': 'Big', 'uid': 1004, 'lname': 'Jones'},
{'fname': 'Brian', 'uid': 1003, 'lname': 'Jones'},
{'fname': 'David', 'uid': 1002, 'lname': 'Beazley'},
{'fname': 'John', 'uid': 1001, 'lname': 'Cleese'}]
[{'fname': 'John', 'uid': 1001, 'lname': 'Cleese'},
{'fname': 'David', 'uid': 1002, 'lname': 'Beazley'},
{'fname': 'Brian', 'uid': 1003, 'lname': 'Jones'},
{'fname': 'Big', 'uid': 1004, 'lname': 'Jones'}]
```
`itemgetter()` 函数也支持多个 `keys`，比如下面的代码
```python
rows_by_lfname = sorted(rows, key=itemgetter('lname','fname'))
print(rows_by_lfname)
```
会产生如下的输出：
```python
[{'fname': 'David', 'uid': 1002, 'lname': 'Beazley'},
{'fname': 'John', 'uid': 1001, 'lname': 'Cleese'},
{'fname': 'Big', 'uid': 1004, 'lname': 'Jones'},
{'fname': 'Brian', 'uid': 1003, 'lname': 'Jones'}]
```
在上面例子中， `rows` 被传递给接受一个关键字参数的 `sorted()` 内置函数。 这个参数是 `callable` 类型，并且从 `rows` 中接受一个单一元素，然后返回被用来排序的值。 `itemgetter()` 函数就是负责创建这个 `callable` 对象的。

`operator.itemgetter()` 函数有一个被 `rows` 中的记录用来查找值的索引参数。可以是一个字典键名称， 一个整形值或者任何能够传入一个对象的 `__getitem__()` 方法的值。 如果你传入多个索引参数给 `itemgetter()` ，它生成的 `callable` 对象会返回一个包含所有元素值的元组， 并且 `sorted()` 函数会根据这个元组中元素顺序去排序。 但你想要同时在几个字段上面进行排序（比如通过姓和名来排序，也就是例子中的那样）的时候这种方法是很有用的。
`itemgetter()` 有时候也可以用 `lambda` 表达式代替，比如：
```python
rows_by_fname = sorted(rows, key=lambda r: r['fname'])
rows_by_lfname = sorted(rows, key=lambda r: (r['lname'],r['fname']))
```
这种方案也不错。但是，使用 `itemgetter()` 方式会运行的稍微快点。因此，如果你对性能要求比较高的话就使用 `itemgetter()` 方式。
最后，不要忘了这节中展示的技术也同样适用于 `min()` 和 `max()` 等函数。比如：
```python
min(rows, key=itemgetter('uid'))
{'fname': 'John', 'lname': 'Cleese', 'uid': 1001}
max(rows, key=itemgetter('uid'))
{'fname': 'Big', 'lname': 'Jones', 'uid': 1004}
```
- 字典中提取子集

最简单的方式是使用字典推导。比如：
```python
prices = {
    'ACME': 45.23,
    'AAPL': 612.78,
    'IBM': 205.55,
    'HPQ': 37.20,
    'FB': 10.75
}
# Make a dictionary of all prices over 200
p1 = {key: value for key, value in prices.items() if value > 200}
# Make a dictionary of tech stocks
tech_names = {'AAPL', 'IBM', 'HPQ', 'MSFT'}
p2 = {key: value for key, value in prices.items() if key in tech_names}
```
大多数情况下字典推导能做到的，通过创建一个元组序列然后把它传给 `dict()` 函数也能实现。比如：
```python
p1 = dict((key, value) for key, value in prices.items() if value > 200)
```
但是，字典推导方式表意更清晰，并且实际上也会运行的更快些 （在这个例子中，实际测试几乎比 `dict()` 函数方式快整整一倍）。
有时候完成同一件事会有多种方式。比如，第二个例子程序也可以像这样重写：
```python
# Make a dictionary of tech stocks
tech_names = { 'AAPL', 'IBM', 'HPQ', 'MSFT' }
p2 = { key:prices[key] for key in prices.keys() & tech_names }
```
但是，运行时间测试结果显示这种方案大概比第一种方案慢 1.6 倍。 如果对程序运行性能要求比较高的话，需要花点时间去做计时测试。
## 集合
- 元素频率

下面的方法会根据元素频率取列表中最常见的元素。
```Python
def most_frequent(list):
    return max(set(list), key = list.count)
list = [1,2,1,2,3,2,1,4,2]
most_frequent(list)
```
## `collections`
### `deque`
- 保留最后 `N` 个元素

使用 `deque(maxlen=N)` 构造函数会新建一个固定大小的队列。当新的元素加入并且这个队列已满的时候， 最老的元素会自动被移除掉。
```Python
q = deque(maxlen=3)
q.append(1)
q.append(2)
q.append(3)
q
deque([1, 2, 3], maxlen=3)
q.append(4)
q
deque([2, 3, 4], maxlen=3)
q.append(5)
q
deque([3, 4, 5], maxlen=3)
```
尽管你也可以手动在一个列表上实现这一的操作（比如增加、删除等等）。但是这里的队列方案会更加优雅并且运行得更快些。
更一般的， `deque` 类可以被用在任何你只需要一个简单队列数据结构的场合。 如果你不设置最大队列大小，那么就会得到一个无限大小队列，你可以在队列的两端执行添加和弹出元素的操作。
```python
q = deque()
q.append(1)
q.append(2)
q.append(3)
q
deque([1, 2, 3])
q.appendleft(4)
q
deque([4, 1, 2, 3])
q.pop()
3
q
deque([4, 1, 2])
q.popleft()
4
```
在队列两端插入或删除元素时间复杂度都是 `O(1)` ，区别于列表，在列表的开头插入或删除元素的时间复杂度为 `O(N)` 。
### `Counter`
- 序列中出现次数最多的元素

`collections.Counter` 类就是专门为这类问题而设计的， 它甚至有一个有用的 `most_common()` 方法直接给了你答案。
为了演示，先假设你有一个单词列表并且想找出哪个单词出现频率最高。你可以这样做：
```python
words = [
    'look', 'into', 'my', 'eyes', 'look', 'into', 'my', 'eyes',
    'the', 'eyes', 'the', 'eyes', 'the', 'eyes', 'not', 'around', 'the',
    'eyes', "don't", 'look', 'around', 'the', 'eyes', 'look', 'into',
    'my', 'eyes', "you're", 'under'
]
from collections import Counter
word_counts = Counter(words)
# 出现频率最高的3个单词
top_three = word_counts.most_common(3)
print(top_three)
# Outputs [('eyes', 8), ('the', 5), ('look', 4)]
```
作为输入， `Counter` 对象可以接受任意的由可哈希(`hashable`)元素构成的序列对象。 在底层实现上，一个 `Counter`对象就是一个字典，将元素映射到它出现的次数上。比如：
```python
word_counts['not']
1
word_counts['eyes']
8
```
如果你想手动增加计数，可以简单的用加法：
```python
morewords = ['why','are','you','not','looking','in','my','eyes']
for word in morewords:
    word_counts[word] += 1
word_counts['eyes']
9
```
或者你可以使用 `update()` 方法：
```python
word_counts.update(morewords)
```
`Counter` 实例一个鲜为人知的特性是它们可以很容易的跟数学运算操作相结合。比如：
```python
a = Counter(words)
b = Counter(morewords)
a
Counter({'eyes': 8, 'the': 5, 'look': 4, 'into': 3, 'my': 3, 'around': 2,
"you're": 1, "don't": 1, 'under': 1, 'not': 1})
b
Counter({'eyes': 1, 'looking': 1, 'are': 1, 'in': 1, 'not': 1, 'you': 1,
'my': 1, 'why': 1})
# Combine counts
c = a + b
c
Counter({'eyes': 9, 'the': 5, 'look': 4, 'my': 4, 'into': 3, 'not': 2,
'around': 2, "you're": 1, "don't": 1, 'in': 1, 'why': 1,
'looking': 1, 'are': 1, 'under': 1, 'you': 1})
# Subtract counts
d = a - b
d
Counter({'eyes': 7, 'the': 5, 'look': 4, 'into': 3, 'my': 2, 'around': 2,
"you're": 1, "don't": 1, 'under': 1})
```
毫无疑问， `Counter` 对象在几乎所有需要制表或者计数数据的场合是非常有用的工具。 在解决这类问题的时候你应该优先选择它，而不是手动的利用字典去实现。
## `heapq`
- 查找最大或最小的 `N` 个元素

`heapq` 模块有两个函数：`nlargest()` 和 `nsmallest()` 可以完美解决这个问题。
```python
import heapq
nums = [1, 8, 2, 23, 7, -4, 18, 23, 42, 37, 2]
print(heapq.nlargest(3, nums)) # Prints [42, 37, 23]
print(heapq.nsmallest(3, nums)) # Prints [-4, 1, 2]
```
两个函数都能接受一个关键字参数，用于更复杂的数据结构中：
```python
portfolio = [
    {'name': 'IBM', 'shares': 100, 'price': 91.1},
    {'name': 'AAPL', 'shares': 50, 'price': 543.22},
    {'name': 'FB', 'shares': 200, 'price': 21.09},
    {'name': 'HPQ', 'shares': 35, 'price': 31.75},
    {'name': 'YHOO', 'shares': 45, 'price': 16.35},
    {'name': 'ACME', 'shares': 75, 'price': 115.65}
]
cheap = heapq.nsmallest(3, portfolio, key=lambda s: s['price'])
expensive = heapq.nlargest(3, portfolio, key=lambda s: s['price'])
```
上面代码在对每个元素进行对比的时候，会以 `price` 的值进行比较。
如果你想在一个集合中查找最小或最大的 `N` 个元素，并且 `N` 小于集合元素数量，那么这些函数提供了很好的性能。 因为在底层实现里面，首先会先将集合数据进行堆排序后放入一个列表中：
```python
nums = [1, 8, 2, 23, 7, -4, 18, 23, 42, 37, 2]
import heapq
heap = list(nums)
heapq.heapify(heap)
heap
[-4, 2, 1, 23, 7, 2, 18, 23, 42, 37, 8]
```
堆数据结构最重要的特征是 `heap[0]` 永远是最小的元素。并且剩余的元素可以很容易的通过调用 `heapq.heappop()` 方法得到， 该方法会先将第一个元素弹出来，然后用下一个最小的元素来取代被弹出元素（这种操作时间复杂度仅仅是 `O(log N)`，`N` 是堆大小）。 比如，如果想要查找最小的 3 个元素，你可以这样做：
```python
heapq.heappop(heap)
-4
heapq.heappop(heap)
1
heapq.heappop(heap)
2
```
当要查找的元素个数相对比较小的时候，函数 `nlargest()` 和 `nsmallest()` 是很合适的。 如果你仅仅想查找唯一的最小或最大（`N=1`）的元素的话，那么使用 `min()` 和 `max()` 函数会更快些。 类似的，如果 `N` 的大小和集合大小接近的时候，通常先排序这个集合然后再使用切片操作会更快点 （ `sorted(items)[:N]` 或者是 `sorted(items)[-N:]` ）。 需要在正确场合使用函数 `nlargest()` 和 `nsmallest()` 才能发挥它们的优势 （如果 `N` 快接近集合大小了，那么使用排序操作会更好些）。
- 实现优先级队列

下面的类利用 `heapq` 模块实现了一个简单的优先级队列：
```python
import heapq
class PriorityQueue:
    def __init__(self):
        self._queue = []
        self._index = 0

    def push(self, item, priority):
        heapq.heappush(self._queue, (-priority, self._index, item))
        self._index += 1

    def pop(self):
        return heapq.heappop(self._queue)[-1]
```
下面是它的使用方式：
```python
class Item:
    def __init__(self, name):
        self.name = name
    def __repr__(self):
        return 'Item({!r})'.format(self.name)
q = PriorityQueue()
q.push(Item('foo'), 1)
q.push(Item('bar'), 5)
q.push(Item('spam'), 4)
q.push(Item('grok'), 1)
q.pop()
Item('bar')
q.pop()
('spam')
q.pop()
Item('foo')
q.pop()
Item('grok')
```
仔细观察可以发现，第一个 `pop()` 操作返回优先级最高的元素。 另外注意到如果两个有着相同优先级的元素`(foo 和 grok)`，`pop` 操作按照它们被插入到队列的顺序返回的。
这一小节我们主要关注 `heapq` 模块的使用。 函数 `heapq.heappush()` 和 `heapq.heappop()` 分别在队列 `_queue` 上插入和删除第一个元素， 并且队列 `_queue` 保证第一个元素拥有最高优先级。 `heappop()` 函数总是返回”最小的”的元素，这就是保证队列`pop`操作返回正确元素的关键。 另外，由于 `push` 和 `pop` 操作时间复杂度为 `O(log N)`，其中 `N` 是堆的大小，因此就算是 `N` 很大的时候它们运行速度也依旧很快。

在上面代码中，队列包含了一个 `(-priority, index, item)` 的元组。 优先级为负数的目的是使得元素按照优先级从高到低排序。 这个跟普通的按优先级从低到高排序的堆排序恰巧相反。

`index` 变量的作用是保证同等优先级元素的正确排序。 通过保存一个不断增加的 `index` 下标变量，可以确保元素按照它们插入的顺序排序。 而且， `index` 变量也在相同优先级元素比较的时候起到重要作用。

为了阐明这些，先假定 `Item` 实例是不支持排序的：
```python
a = Item('foo')
b = Item('bar')
a < b
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
TypeError: unorderable types: Item() < Item()
```
如果你使用元组 `(priority, item)` ，只要两个元素的优先级不同就能比较。 但是如果两个元素优先级一样的话，那么比较操作就会跟之前一样出错：
```python
a = (1, Item('foo'))
b = (5, Item('bar'))
a < b
True
c = (1, Item('grok'))
a < c
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
TypeError: unorderable types: Item() < Item()
```
通过引入另外的 `index` 变量组成三元组 `(priority, index, item)` ，就能很好的避免上面的错误， 因为不可能有两个元素有相同的 `index` 值。`Python` 在做元组比较时候，如果前面的比较已经可以确定结果了， 后面的比较操作就不会发生了：
```python
a = (1, 0, Item('foo'))
b = (5, 1, Item('bar'))
c = (1, 2, Item('grok'))
a < b
True
a < c
True
```
如果你想在多个线程中使用同一个队列，那么你需要增加适当的锁和信号量机制。
## `itertools`
### `groupby`
- 通过某个字段将记录分组

`itertools.groupby()` 函数对于这样的数据分组操作非常实用。 为了演示，假设你已经有了下列的字典列表：
```python
rows = [
    {'address': '5412 N CLARK', 'date': '07/01/2012'},
    {'address': '5148 N CLARK', 'date': '07/04/2012'},
    {'address': '5800 E 58TH', 'date': '07/02/2012'},
    {'address': '2122 N CLARK', 'date': '07/03/2012'},
    {'address': '5645 N RAVENSWOOD', 'date': '07/02/2012'},
    {'address': '1060 W ADDISON', 'date': '07/02/2012'},
    {'address': '4801 N BROADWAY', 'date': '07/01/2012'},
    {'address': '1039 W GRANVILLE', 'date': '07/04/2012'},
]
```
现在假设你想在按 `date` 分组后的数据块上进行迭代。为了这样做，你首先需要按照指定的字段(这里就是 `date` )排序， 然后调用 `itertools.groupby()` 函数：
```python
from operator import itemgetter
from itertools import groupby

# Sort by the desired field first
rows.sort(key=itemgetter('date'))
# Iterate in groups
for date, items in groupby(rows, key=itemgetter('date')):
    print(date)
    for i in items:
        print(' ', i)
```
运行结果：
```python
07/01/2012
  {'date': '07/01/2012', 'address': '5412 N CLARK'}
  {'date': '07/01/2012', 'address': '4801 N BROADWAY'}
07/02/2012
  {'date': '07/02/2012', 'address': '5800 E 58TH'}
  {'date': '07/02/2012', 'address': '5645 N RAVENSWOOD'}
  {'date': '07/02/2012', 'address': '1060 W ADDISON'}
07/03/2012
  {'date': '07/03/2012', 'address': '2122 N CLARK'}
07/04/2012
  {'date': '07/04/2012', 'address': '5148 N CLARK'}
  {'date': '07/04/2012', 'address': '1039 W GRANVILLE'}
```
`groupby()` 函数扫描整个序列并且查找连续相同值（或者根据指定 `key` 函数返回值相同）的元素序列。 在每次迭代的时候，它会返回一个值和一个迭代器对象， 这个迭代器对象可以生成元素值全部等于上面那个值的组中所有对象。
一个非常重要的准备步骤是要根据指定的字段将数据排序。 因为 `groupby()` 仅仅检查连续的元素，如果事先并没有排序完成的话，分组函数将得不到想要的结果。
如果你仅仅只是想根据 `date` 字段将数据分组到一个大的数据结构中去，并且允许随机访问， 那么你最好使用 `defaultdict()` 来构建一个多值字典，
```python
from collections import defaultdict
rows_by_date = defaultdict(list)
for row in rows:
    rows_by_date[row['date']].append(row)
```
这样的话你可以很轻松的就能对每个指定日期访问对应的记录：
```python
for r in rows_by_date['07/01/2012']:
    print(r)
{'date': '07/01/2012', 'address': '5412 N CLARK'}
{'date': '07/01/2012', 'address': '4801 N BROADWAY'}
```
在上面这个例子中，我们没有必要先将记录排序。因此，如果对内存占用不是很关心， 这种方式会比先排序然后再通过 `groupby()` 函数迭代的方式运行得快一些。
## 迭代器与生成器
- 过滤序列元素

最简单的过滤序列元素的方法就是使用列表推导。比如：
```python 
mylist = [1, 4, -5, 10, -7, 2, 3, -1]
[n for n in mylist if n > 0]
[1, 4, 10, 2, 3]
[n for n in mylist if n < 0]
[-5, -7, -1]
```
使用列表推导的一个潜在缺陷就是如果输入非常大的时候会产生一个非常大的结果集，占用大量内存。 如果你对内存比较敏感，那么你可以使用生成器表达式迭代产生过滤的元素。比如：
```python
pos = (n for n in mylist if n > 0)
pos
<generator object <genexpr> at 0x1006a0eb0>
for x in pos:
    print(x)
1
4
10
2
3
```
有时候，过滤规则比较复杂，不能简单的在列表推导或者生成器表达式中表达出来。 比如，假设过滤的时候需要处理一些异常或者其他复杂情况。这时候你可以将过滤代码放到一个函数中， 然后使用内建的 `filter()` 函数。示例如下：
```python
values = ['1', '2', '-3', '-', '4', 'N/A', '5']
def is_int(val):
    try:
        x = int(val)
        return True
    except ValueError:
        return False
ivals = list(filter(is_int, values))
print(ivals)
# Outputs ['1', '2', '-3', '4', '5']
```
`filter()` 函数创建了一个迭代器，因此如果你想得到一个列表的话，就得像示例那样使用 `list()` 去转换。
列表推导和生成器表达式通常情况下是过滤数据最简单的方式。 其实它们还能在过滤的时候转换数据。比如：
```python
mylist = [1, 4, -5, 10, -7, 2, 3, -1]
import math
[math.sqrt(n) for n in mylist if n > 0]
[1.0, 2.0, 3.1622776601683795, 1.4142135623730951, 1.7320508075688772]
```
过滤操作的一个变种就是将不符合条件的值用新的值代替，而不是丢弃它们。 比如，在一列数据中你可能不仅想找到正数，而且还想将不是正数的数替换成指定的数。 通过将过滤条件放到条件表达式中去，可以很容易的解决这个问题，就像这样：
```python
clip_neg = [n if n > 0 else 0 for n in mylist]
clip_neg
[1, 4, 0, 10, 0, 2, 3, 0]
clip_pos = [n if n < 0 else 0 for n in mylist]
clip_pos
[0, 0, -5, 0, -7, 0, 0, -1]
```
另外一个值得关注的过滤工具就是 `itertools.compress()` ， 它以一个 `iterable` 对象和一个相对应的 `Boolean` 选择器序列作为输入参数。 然后输出 `iterable` 对象中对应选择器为 `True` 的元素。 当你需要用另外一个相关联的序列来过滤某个序列的时候，这个函数是非常有用的。 比如，假如现在你有下面两列数据：
```python
addresses = [
    '5412 N CLARK',
    '5148 N CLARK',
    '5800 E 58TH',
    '2122 N CLARK',
    '5645 N RAVENSWOOD',
    '1060 W ADDISON',
    '4801 N BROADWAY',
    '1039 W GRANVILLE',
]
counts = [ 0, 3, 10, 4, 1, 7, 6, 1]
```
现在你想将那些对应 `count` 值大于5的地址全部输出，那么你可以这样做：
```python
from itertools import compress
more5 = [n > 5 for n in counts]
more5
[False, False, True, False, False, True, True, False]
list(compress(addresses, more5))
['5800 E 58TH', '1060 W ADDISON', '4801 N BROADWAY']
```
这里的关键点在于先创建一个 `Boolean` 序列，指示哪些元素符合条件。 然后 `compress()` 函数根据这个序列去选择输出对应位置为 `True` 的元素。
和 `filter()` 函数类似， `compress()` 也是返回的一个迭代器。因此，如果你需要得到一个列表， 那么你需要使用 `list()` 来将结果转换为列表类型。
- 转换并同时计算数据

一个非常优雅的方式去结合数据计算与转换就是使用一个生成器表达式参数。 比如，如果你想计算平方和，可以像下面这样做：
```python
nums = [1, 2, 3, 4, 5]
s = sum(x * x for x in nums)
```
下面是更多的例子：
```python
# Determine if any .py files exist in a directory
import os
files = os.listdir('dirname')
if any(name.endswith('.py') for name in files):
    print('There be python!')
else:
    print('Sorry, no python.')
# Output a tuple as CSV
s = ('ACME', 50, 123.45)
print(','.join(str(x) for x in s))
# Data reduction across fields of a data structure
portfolio = [
    {'name':'GOOG', 'shares': 50},
    {'name':'YHOO', 'shares': 75},
    {'name':'AOL', 'shares': 20},
    {'name':'SCOX', 'shares': 65}
]
min_shares = min(s['shares'] for s in portfolio)
```
上面的示例向你演示了当生成器表达式作为一个单独参数传递给函数时候的巧妙语法（你并不需要多加一个括号）。 比如，下面这些语句是等效的：
```python
s = sum((x * x for x in nums)) # 显式的传递一个生成器表达式对象
s = sum(x * x for x in nums) # 更加优雅的实现方式，省略了括号
```
使用一个生成器表达式作为参数会比先创建一个临时列表更加高效和优雅。 比如，如果你不使用生成器表达式的话，你可能会考虑使用下面的实现方式：
```python
nums = [1, 2, 3, 4, 5]
s = sum([x * x for x in nums])
```
这种方式同样可以达到想要的效果，但是它会多一个步骤，先创建一个额外的列表。 对于小型列表可能没什么关系，但是如果元素数量非常大的时候， 它会创建一个巨大的仅仅被使用一次就被丢弃的临时数据结构。而生成器方案会以迭代的方式转换数据，因此更省内存。
在使用一些聚集函数比如 `min()` 和 `max()` 的时候你可能更加倾向于使用生成器版本， 它们接受的一个 `key` 关键字参数或许对你很有帮助。 比如，在上面的证券例子中，你可能会考虑下面的实现版本：
```python
# Original: Returns 20
min_shares = min(s['shares'] for s in portfolio)
# Alternative: Returns {'name': 'AOL', 'shares': 20}
min_shares = min(portfolio, key=lambda s: s['shares'])
```