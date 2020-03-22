## 列表
- 重复元素判定
以下方法可以检查给定列表是不是存在重复元素，它会使用 `set()` 函数来移除所有重复元素。
```python
def all_unique(lst):
    return len(lst) == len(set(lst))
x = [1,1,2,2,3,2,3,4,5,6]
y = [1,2,3,4,5]
all_unique(x) # False
all_unique(y) # True
```
- 取两个列表交集：
```python
def common_elements(list1, list2):
    common = set(list1).intersection(set(list2))
    return list(common)
```
- 压缩
这个方法可以将布尔型的值去掉，例如`(False，None，0，“”)`，它使用 `filter()` 函数。
```python
def compact(lst):
    return list(filter(bool, lst))
compact([0, 1, False, 2, '', 3, 'a', 's', 34])
# [ 1, 2, 3, 'a', 's', 34 ]
```
- 解包
如下代码段可以将打包好的成对列表解开成两组不同的元组。
```python
array = [['a', 'b'], ['c', 'd'], ['e', 'f']]
transposed = zip(*array)
print(transposed)
# [('a', 'c', 'e'), ('b', 'd', 'f')]
```
- 逗号连接
下面的代码可以将列表连接成单个字符串，且每一个元素间的分隔方式设置为了逗号。
```python
hobbies = ["basketball", "football", "swimming"]
print("My hobbies are: " + ", ".join(hobbies))
# My hobbies are: basketball, football, swimming
```
- 展开列表
该方法将通过递归的方式将列表的嵌套展开为单个列表。
```python
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
非递归：
```python
def spread(arg):
    ret = []
    for i in arg:
        if isinstance(i, list):
            ret.extend(i)
        else:
            ret.append(i)
    return ret
spread([1,2,3,[4,5,6],[7],8,9]) # [1,2,3,4,5,6,7,8,9]
```
- 列表的差
该方法将返回第一个列表的元素，其不在第二个列表内。如果同时要反馈第二个列表独有的元素，还需要加一句 `set_b.difference(set_a)`。
```python
def difference(a, b):
    set_a = set(a)
    set_b = set(b)
    comparison = set_a.difference(set_b)
    return list(comparison)
difference([1,2,3], [1,2,4]) # [3]
```
- 通过函数取差
如下方法首先会应用一个给定的函数，然后再返回应用函数后结果有差别的列表元素。
```python
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
```python
[x for x in list[::2] if x%2 == 0]
```
例如，给定列表如下：
```python
list = [ 1 , 3 , 5 , 8 , 10 , 13 , 18 , 36 , 78 ]
```
列表生成式`[x for x in list[::2] if x%2 == 0]` 的结果是，`[10, 18, 78]`
这个表达式工作的步骤是，第一步取出偶数切片的数字，
第二步剔除其中所有奇数。
- 更长列表
```python
def max_length(*lst):
    return max(*lst, key=lambda v: len(v))
r = max_length([1, 2, 3], [4, 5, 6, 7], [8])
print(f'更长的列表是{r}')  # [4, 5, 6, 7]
r = max_length([1, 2, 3], [4, 5, 6, 7], [8, 9])
print(f'更长的列表是{r}')  # [4, 5, 6, 7]
```
- 求众数
```
def top1(lst):
    return max(lst, default='列表为空', key=lambda v: lst.count(v))
lst = [1, 3, 3, 2, 1, 1, 2]
r = top1(lst)
print(f'{lst}中出现次数最多的元素为:{r}')  # [1, 3, 3, 2, 1, 1, 2]中出现次数最多的元素为:1
```
- 列表之最
```python
def max_lists(*lst):
    return max(max(*lst, key=lambda v: max(v)))
r = max_lists([1, 2, 3], [6, 7, 8], [4, 5])
print(r)  # 8
```
- 按条件分组
```python
def bif_by(lst, f):
    return [ [x for x in lst if f(x)],[x for x in lst if not f(x)]]
records = [25,89,31,34]
bif_by(records, lambda x: x<80) # [[25, 31, 34], [89]]
```

## 元组
- 取元组元素
```python
a,b,c = ('cat','dog','tiger')
# 提取首、尾两个元素：
first,*_,end = (1,2,3,4,5,6)
# 提取首、中、尾三部分：
first,*middle,end = (1,2,3,4,5,6)
```
## 字符串
- 字符元素组成判定
检查两个字符串的组成元素是不是一样的。
```python
from collections import Counter
def anagram(first, second):
    return Counter(first) == Counter(second)
anagram("abcd3", "3acdb") # True
```
- N 次字符串
该代码块不需要循环语句
```python
n = 2;
s ="Programming"
print(s * n)
# ProgrammingProgramming  
```
## 运算符
链式对比,我们可以在一行代码中使用不同的运算符对比多个不同的元素。
```python
a = 3
print( 2 < a < 8) # True
print(1 == a < 2) # False
```
## 函数
- 链式函数调用
你可以在一行代码内调用多个函数。
```python
def add(a, b):
    return a + b
def subtract(a, b):
    return a - b
a, b = 4, 5
print((subtract if a > b else add)(a, b)) # 9 
```
## 字典
- 合并字典
```python
def merge_two_dicts(a, b):
    c = a.copy()   # make a copy of a 
    c.update(b)    # modify keys and values of a with the ones from b
    return c
a = { 'x': 1, 'y': 2}
b = { 'y': 3, 'z': 4}
print(merge_two_dicts(a, b))
# {'y': 3, 'x': 1, 'z': 4}
```
在 Python 3.5 或更高版本中，我们也可以用以下方式合并字典：
```python
def merge_dictionaries(a, b)
   return {**a, **b}
a = { 'x': 1, 'y': 2}
b = { 'y': 3, 'z': 4}
print(merge_dictionaries(a, b))
# {'y': 3, 'x': 1, 'z': 4}
```
这是一般的字典合并写法
```python
dic1 = {'x': 1, 'y': 2 }
dic2 = {'y': 3, 'z': 4 }
merged1 = {**dic1, **dic2} # {'x': 1, 'y': 3, 'z': 4}
```
修改`merged[‘x’]=10`，`dic1`中的`x`值不变，`merged`是重新生成的一个新字典。
但是，`ChainMap`却不同，它在内部创建了一个容纳这些字典的列表。因此使用`ChainMap`合并字典，修改`merged[‘x’]=10`后，`dic1`中的`x`值改变，如下所示：
```python
from collections import ChainMap
merged2 = ChainMap(dic1,dic2)
print(merged2) # ChainMap({'x': 1, 'y': 2}, {'y': 3, 'z': 4})
```
- 将两个列表转化为字典
如下方法将会把两个列表转化为单个字典。
```python
def to_dictionary(keys, values):
    return dict(zip(keys, values))
keys = ["a", "b", "c"]    
values = [2, 3, 4]
print(to_dictionary(keys, values))
# {'a': 2, 'c': 4, 'b': 3}
```
- 值最大的字典
```python
def max_pairs(dic):
    if len(dic) == 0:
        return dic
    max_val = max(map(lambda v: v[1], dic.items()))
    return [item for item in dic.items() if item[1] == max_val]
r = max_pairs({'a': -10, 'b': 5, 'c': 3, 'd': 5})
print(r)  # [('b', 5), ('d', 5)]
```
- `topn`字典
```python
from heapq import nlargest
# 返回字典d前n个最大值对应的键
def topn_dict(d, n):
    return nlargest(n, d, key=lambda k: d[k])
topn_dict({'a': 10, 'b': 8, 'c': 9, 'd': 10}, 3)  # ['a', 'd', 'c']
```
## 集合
- 元素频率
下面的方法会根据元素频率取列表中最常见的元素。
```python
def most_frequent(list):
    return max(set(list), key = list.count)
list = [1,2,1,2,3,2,1,4,2]
most_frequent(list)
```
