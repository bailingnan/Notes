# `Python`笔记
## 编码规范


![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200318181309.png)
## 对象
### 可变与不可变对象
- `Python`中的大多数对象，比如列表、字典、`NumPy`数组，和用户定义的类型（类），都是可变的。意味着这些对象或包含的值可以被修改。
- 字符串和元组，是不可变的。
```Python
a= 'abc'
b = a.replace('a', 'A')
print(b)
'Abc'
```
要始终牢记的是，`a`是变量，而`'abc'`才是字符串对象，有些时候，我们经常说，对象`a`的内容是`'abc'`，但其实是指，`a`本身是一个变量，它指向的对象的内容才是`'abc'`：
当我们调用`a.replace('a', 'A')`时，实际上调用方法`replace`是作用在字符串对象`'abc'`上的，而这个方法虽然名字叫`replace`，但却没有改变字符串`'abc'`的内容。相反，`replace`方法创建了一个新字符串`'Abc'`并返回，如果我们用变量`b`指向该新字符串，就容易理解了，变量`a`仍指向原有的字符串`'abc'`，但变量`b`却指向新字符串`'Abc'`了。
所以，对于不变对象来说，调用对象自身的任意方法，也不会改变该对象自身的内容。相反，这些方法会创建新的对象并返回，这样，就保证了不可变对象本身永远是不可变的。
- 变量可以连续赋值:
```Python
a=b=c=1
```
### 拷贝
- 简单的赋值只是将引用传给新对象，新旧对象除变量名外毫无区别
- 由于 `Python` 内部引用计数的特性，对于不可变对象，浅拷贝和深拷贝的作用是一致的，就相当于复制了一份副本，原对象内部的不可变对象的改变，不会影响到复制对象
- 浅拷贝的拷贝。其实是拷贝了原始元素的引用（内存地址），所以当拷贝可变对象时，原对象内可变对象的对应元素的改变，会在复制对象的对应元素上，有所体现
- 深拷贝在遇到可变对象时，又在内部做了新建了一个副本。所以，不管它内部的元素如何变化，都不会影响到原来副本的可变对象
- 如果对子对象修改，则浅拷贝后的结果也会跟着发生变化，而深拷贝则不会。他们的子对象还是指向统一对象（是引用）。`list`的`a=b[:]`相当于`copy()`
- 如果对父对象修改，则不管是浅拷贝还是深拷贝的结果，都不会跟着发生变化。
- 不管对什么对象修改，指针引用后的结果都会跟着发生相同变化。
标准库中的`copy`模块提供了两个方法来实现拷贝.一个方法是`copy`,它返回和参数包含内容一样的对象.
```Python
import copy
new_list = copy.copy(existing_list)
```
有些时候,你希望对象中的属性也被复制,可以使用`deepcopy`方法:
```Python
import copy
new_list_of_dicts = copy.deepcopy(existing_list_of_dicts)
```
当你对一个对象赋值的时候(做为参数传递,或者做为返回值),`Python`和`Java`一样,总是传递原始对象的引用,而不是一个副本.其它一些语言当赋值的时候总是传递副本,`Python`从不猜测用户的需求 ,如果你想要一个副本,你必须显式的要求.
`Python`的行为很简单,迅速,而且一致.然而,如果你需要一个对象拷贝而并没有显式的写出来,会出现问题的,比如:
```Python
a = [1, 2, 3]
b = a
print(id(a)==id(b))
True
b.append(5)
print(a,b) 
[1, 2, 3, 5] [1, 2, 3, 5]
```
在这里,变量`a`和`b`都指向同一个对象(一个列表),所以,一旦你修改了二者之一,另外一个也会受到影响.无论怎样,都会修改原来的对象。
```Python
import copy
c=copy.copy(a)
print(id(c)==id(a))
False
c[1]=222
print(c)
[1,222,3,5]
print(a)
[1,2,3,5]
a=[1,2,[3,4]]
d=copy.copy(a)
print(id(a)==id(d))
False
print(id(a[2])==id(d[2]))
True
a[0]=11
print(a)
[11,2,[3,4]]
print(d)
[1,2,[3,4]]
# 只会复制值的第一层，而不会复制往下的几层数据。
# 复杂的 object， 如 list 中套着 list 的情况，shallow copy 中的 子list，并未从原 object 真的「独立」出来。也就是说，如果你改变原 object 的子 list 中的一个元素，你的 copy 就会跟着一起变。这跟我们直觉上对「复制」的理解不同。
a[2][0]=333
print(d)
[1,2,[333,4]]
e=copy.deepcopy(a)
print(e[2]==a[2])
False
```
这种情况就不一样了，这是对`a`重新指向新的值那么其`id`就会变而此时`b`就不会变。
```Python
a = [1,2,3]
b = a
print(id(b))
a = {1:2}
print(id(a))
print(id(b))
print(b)
输出：
1998591409928
1998589307016
1998591409928
[1, 2, 3]
```
再举一个例子：
```Python
import copy
a=[1,[1,2],3]
b=a
b
[1, [1, 2], 3]
id(a)
4549388120
id(b)
4549388120
b[0]=3
a
[3, [1, 2], 3]
c=copy.copy(a)
id(c)
4549389992
id(a)
4549388120
c[0]=4
a
[3, [1, 2], 3]
c
[4, [1, 2], 3]
c[1].append(3)
a
[3, [1, 2, 3], 3]
c
[4, [1, 2, 3], 3]
id(a[2])
140345184649736
id(c[2])
140345184649736
id(c[1])
4549388192
id(a[1])
4549388192
d=copy.deepcopy(a)
id(d)
4549389632
id(a)
4549388120
d[1].append(4)
a
[3, [1, 2, 3], 3]
d
[3, [1, 2, 3, 4], 3]
```
`Python` 存储变量的方法跟其他 `OOP` 语言不同。它与其说是把值赋给变量，不如说是给变量建立了一个到具体值的 `reference`。
当在 `Python` 中 `a = something` 应该理解为给 `something` 贴上了一个标签 `a`。当再赋值给 `a` 的时候，就好像把 `a` 这个标签从原来的 `something` 上拿下来，贴到其他对象上，建立新的 `reference`。 这就解释了一些 `Python` 中可能遇到的诡异情况：
```Python
a = [1, 2, 3]
b = a
a = [4, 5, 6] # 赋新的值给 a
a
[4, 5, 6]
b
[1, 2, 3]
# a 的值改变后，b 并没有随着 a 变
a = [1, 2, 3]
b = a
a[0], a[1], a[2] = 4, 5, 6 # 改变原来 list 中的元素
a
[4, 5, 6]
b
[4, 5, 6]
# a 的值改变后，b 随着 a 变了
```
上面两段代码中，`a` 的值都发生了变化。区别在于，第一段代码中是直接赋给了 `a` 新的值(从 `[1, 2, 3]` 变为 `[4, 5, 6]`)；而第二段则是把 `list` 中每个元素分别改变。
首次把 `[1, 2, 3]` 看成一个物品。`a = [1, 2, 3]` 就相当于给这个物品上贴上 `a` 这个标签。而 `b = a` 就是给这个物品又贴上了一个 `b` 的标签。
`a = [4, 5, 6]` 就相当于把 `a` 标签从 `[1 ,2, 3]` 上撕下来，贴到了 `[4, 5, 6]` 上。
在这个过程中，`[1, 2, 3]` 这个物品并没有消失。 `b`自始至终都好好的贴在 `[1, 2, 3]` 上，既然这个 `reference` 也没有改变过。 `b` 的值自然不变。
第二种情况：
`a[0], a[1], a[2] = 4, 5, 6`则是直接改变了 `[1, 2, 3]` 这个物品本身。把它内部的每一部分都重新改装了一下。内部改装完毕后，`[1, 2, 3]` 本身变成了 `[4, 5, 6]`。
而在此过程当中，`a` 和 `b` 都没有动，他们还贴在那个物品上。因此自然 `a`,`b` 的值都变成了 `[4, 5, 6]`。
搞明白这个之后就要问了，对于一个复杂对象的浅`copy`，在`copy`的时候到底发生了什么？
再看一段代码：
```Python
import copy
origin = [1, 2, [3, 4]]
#origin 里边有三个元素：1， 2，[3, 4]
cop1 = copy.copy(origin)
cop2 = copy.deepcopy(origin)
cop1 == cop2
True
cop1 is cop2
False 
#cop1 和 cop2 看上去相同，但已不再是同一个object
origin[2][0] = "hey!" 
origin
[1, 2, ['hey!', 4]]
cop1
[1, 2, ['hey!', 4]]
cop2
[1, 2, [3, 4]]
#把origin内的子list [3, 4] 改掉了一个元素，观察 cop1 和 cop2
```
`copy`对于一个复杂对象的子对象并不会完全复制，什么是复杂对象的子对象呢？就比如序列里的嵌套序列，字典里的嵌套序列等都是复杂对象的子对象。对于子对象，`Python`会把它当作一个公共镜像存储起来，所有对他的复制都被当成一个引用，所以说当其中一个引用将镜像改变了之后另一个引用使用镜像的时候镜像已经被改变了。
所以说看这里的`origin[2]`，也就是 `[3, 4]` 这个 `list`。根据 `shallow copy` 的定义，在 `cop1[2]` 指向的是同一个 `list [3, 4]`。那么，如果这里我们改变了这个 `list`，就会导致 `origin` 和 `cop1` 同时改变。这就是为什么上边 `origin[2][0] = “hey!” `之后，cop1 也随之变成了 `[1, 2, [‘hey!’, 4]]`。
`deepcopy`的时候会将复杂对象的每一层复制一个单独的个体出来。
这时候的 `origin[2]` 和 `cop2[2]` 虽然值都等于 `[3, 4]`，但已经不是同一个 `list了`。即我们寻常意义上的复制。
总结：
```Python
lst = [10, ['A']]

# 指针引用: 不拷贝
a = lst
assert a is lst

# 浅拷贝: 只拷贝 父对象，不会拷贝 子对象
import copy
b = copy.copy(lst)
assert b is not lst and b == lst

# 深拷贝: 拷贝 父对象 及 子对象
c = copy.deepcopy(lst)
assert c is not lst and c == lst

# 修改 list 对象
lst.append(5)
lst[1].append('B')

print("原始的list对象:  lst =  [10, ['A']]")
print('修改后list对象:  lst = ', a, '\n')
print('指针引用:  a = ', a)
print('浅拷贝  :  b = ', b)
print('深拷贝  :  c = ', c)
原始的list对象:  lst =  [10, ['A']]
修改后list对象:  lst =  [10, ['A', 'B'], 5] 

指针引用:  a =  [10, ['A', 'B'], 5]
浅拷贝  :  b =  [10, ['A', 'B']]
深拷贝  :  c =  [10, ['A']]
```
即:
- 如果对子对象修改，则浅拷贝后的结果也会跟着发生变化，而深拷贝则不会。`list`的`a=b[:]`相当于`copy()`
- 如果对父对象修改，则不管是浅拷贝还是深拷贝的结果，都不会跟着发生变化。
- 不管对什么对象修改，指针引用后的结果都会跟着发生相同变化。
举例：
```Python
import copy
base = ['a', 'b', 'c', 'd', 'e']
# 切片
bak1 = base[:]
print("bak1: ", bak1)
# list工厂函数
bak2 = list(base)
print("bak2: ", bak2)
# Python list对象的copy方法
bak3 = base.copy()
print("bak3: ", bak3)
# copy模块的copy方法
bak4 = copy.copy(base)
print("bak4: ", bak4)
```
运行结果：
```Python
bak1:  ['a', 'b', 'c', 'd', 'e']
bak2:  ['a', 'b', 'c', 'd', 'e']
bak3:  ['a', 'b', 'c', 'd', 'e']
bak4:  ['a', 'b', 'c', 'd', 'e']
```
上面的代码使用了四种方式来对数据进行拷贝，这些方法都可以用来拷贝数据，结果都一样。
- 切片
需要拷贝的数据进行切片处理，返回的结果相当于拷贝了一份数据。
- 工厂方法
使用 `Python` 的工厂函数 `list` 来拷贝数据。(`Python`的工厂函数是比较特殊的，即是类也是函数，关于工厂函数的理解可以另行扩展一下)
拷贝列表时使用 `list`，如果拷贝字符串则将上面的 `list` 换成 `str` ，以此类推。
- list对象的copy方法
`Python` 中的 `list` 实现了 `copy` 方法，在拷贝列表时可以直接使用。这里需要注意，比如 `str` 没有实现 `copy` 方法，拷贝字符串时使用其他方法拷贝。
- `copy`模块的`copy`方法
在 `Python` 标准库中有一个 `copy` 模块，可以使用 `copy` 模块的 `copy()` 方法来拷贝数据，`copy` 模块可以拷贝所有类型的数据。
```Python
import copy
son = ['Python', 'copy']
base = ['a', 'b', 'c', 'd', 'e', son]
bak1 = base[:]
print("bak1: ", bak1)
bak2 = list(base)
print("bak2: ", bak2)
bak3 = base.copy()
print("bak3: ", bak3)
bak4 = copy.copy(base)
print("bak4: ", bak4)
print('-' * 20, '分割线', '-' * 20)
son[0] = 'PYTHON'
son[1] = 'COPY'
print('base: ', base)
print("bak1: ", bak1)
print("bak2: ", bak2)
print("bak3: ", bak3)
print("bak4: ", bak4)
```
运行结果：
```Python
bak1:  ['a', 'b', 'c', 'd', 'e', ['Python', 'copy']]
bak2:  ['a', 'b', 'c', 'd', 'e', ['Python', 'copy']]
bak3:  ['a', 'b', 'c', 'd', 'e', ['Python', 'copy']]
bak4:  ['a', 'b', 'c', 'd', 'e', ['Python', 'copy']]
-------------------- 分割线 --------------------
base:  ['a', 'b', 'c', 'd', 'e', ['PYTHON', 'COPY']]
bak1:  ['a', 'b', 'c', 'd', 'e', ['PYTHON', 'COPY']]
bak2:  ['a', 'b', 'c', 'd', 'e', ['PYTHON', 'COPY']]
bak3:  ['a', 'b', 'c', 'd', 'e', ['PYTHON', 'COPY']]
bak4:  ['a', 'b', 'c', 'd', 'e', ['PYTHON', 'COPY']]
```
在实际工作中，数据的嵌套层数是很多的，通常会嵌套好几层。上面就在 `base` 列表中嵌套了一个 `son` 子列表。
用上面的四种拷贝方法拷贝 `base` 列表，然后修改 `base` 列表中的子列表 `son` 。重新打印这几个列表，发现不仅 `base` 列表被修改了，拷贝的列表也全部被修改了。
现在的需求是拷贝一份数据，修改一份保留一份，如果两份数据都被修改，是不符合需求的。
上面的四种拷贝方法都被称为浅拷贝（相对深拷贝而言），浅拷贝 `Python` 中的可变对象，如果数据中嵌套了可变对象，修改嵌套的可变对象，所有拷贝的数据都会一起被修改。
在 `Python` 中，所有的数据都是对象，无论是数字，字符串，元组，列表，字典，还是函数，类，甚至是模块。
不可变对象：
`int`, `str`, `tuple` 等类型的数据是不可变对象，不可变对象的特性是数据不可被修改。
```Python
a = 'a'
print(id(a))
a = 'b'
print(id(a))
```
运行结果：
```
1543912659912
1543912658232
```
如果对不可变对象修改，其实不是修改变量对象，而是重新创建一个同名的变量对象。可以通过 `id` 函数来判断，`id` 不一样就证明已经不是同一个变量了。
可变对象：
`list`， `set`，`dict` 等类型的数据是可变对象，相对于不可变对象而言，可变对象的数据可以被修改，修改之后还是同一个`id`。
```Python
base = [1, 2, 3]
print(id(base))
base[0] = 100
print(base)
print(id(base))
```
运行结果：
```Python
2182371173000
[100, 2, 3]
2182371173000
```
对可变对象进行修改，修改后还是同一个对象，只是可变对象里面的元素指向了不同的数据，这种指向是通过引用的方式来实现的。
上面的代码是对列表进行修改，如果对元组这样修改，代码会报错，就是因为可变对象和不可变对象的区别。
在 `Python` 程序中，每个对象都会在内存中开辟一块空间来保存该对象，该对象在内存中所在位置的地址被称为引用。
在编写代码时，定义的变量名实际是定义指向对象的地址引用名。
我们定义一个列表时，变量名是列表的名字，这个名字指向内存中的一块空间。这个列表里有多个元素，表示这块内存空间中，保存着多个元素的引用。
1. 修改引用
当修改列表的元素时，其实是修改列表中的引用。
```Python
list_a = [1, 2, 3]
list_a[2] = 30
print('list_a: ', list_a)
```
运行结果：
```Python
list_a:  [1, 2, 30]
```
修改 `list_a` 中的第三个元素，其实是修改第三个元素的引用（这块内存指向的对象）。
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200323023647.png)
2. 引用传递（拷贝）
当拷贝列表时，其实是拷贝列表中的引用。
```Python
list_b = [1, 2, 3]
list_c = list_b.copy()
print('list_c: ', list_c)
```
运行结果：
```Python
list_c:  [1, 2, 3]
```
拷贝 `list_b` 到 `list_c`，其实是给 `list_c` 新开辟一块内存，然后拷贝一份 `list_b` 的引用给 `list_c` ，并不是将 `list_b`指向的对象拷贝一份。
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200323023828.png)
这里不是将 `list_b` 赋值给 `list_c`，那样的结果是 `list_b` 指向 `[1, 2, 3]` ，`list_c` 指向 `list_b`，是引用关系，而不是拷贝关系。上面列举拷贝的方法时，没有将赋值列为拷贝方法，因为赋值是引用的传递，而不是拷贝。
1. 拷贝后修改引用（数据无嵌套）
```Python
import copy
list_b = [1, 2, 3]
list_c = copy.copy(list_b)
list_b[2] = 30
print('list_b: ', list_b)
print('list_c: ', list_c)
```
运行结果：
```Python
list_b:  [1, 2, 30]
list_c:  [1, 2, 3]
```
使用 `copy.copy()` 方法拷贝 `list_b` 到 `list_c`，然后修改 `list_b` 中的引用关系，这样， `list_c` 不会被修改。
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200323024031.png)
2. 嵌套列表的拷贝
```Python
import copy
sub = [2, 3]
list_d = [1, sub]
list_e = copy.copy(list_d)
print('list_d: ', list_d)
print('list_e: ', list_e)
```
运行结果：
```Python
list_d:  [1, [2, 3]]
list_e:  [1, [2, 3]]
```
对于嵌套的列表，拷贝 `list_d` 到 `list_e`，也是拷贝一份 `list_d` 的引用给 `list_e` ，与不嵌套的相同。
这里需要特别注意，在浅拷贝嵌套的列表时，只会拷贝最上层的引用，对于子列表的引用，不会拷贝。
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200323024211.png)
3. 拷贝的列表随原列表一起被修改
```Python
import copy
sub = [2, 3]
list_d = [1, sub]
list_e = copy.copy(list_d)
list_d[1][1] = 30
print('list_d: ', list_d)
print('list_e: ', list_e)
```
运行结果：
```Python
list_d:  [1, [2, 30]]
list_e:  [1, [2, 30]]
```
拷贝 `list_d` 到 `list_e`，由于没有拷贝子列表的引用 ，当修改子列时， `list_d` 和 `list_e` 都引用了子列表 `sub`，所以 `list_d` 和 `list_e`都会被修改。如下图：
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200323024334.png)
拷贝数据后，修改其中一个，另一个也跟着被修改，原因就是浅拷贝中，只拷贝了最外层的引用。当修改内层的引用时，所有外层的引用不变，都会指向修改后的结果。
两份数据都被修改，这就是浅拷贝中存在的问题，需要使用深拷贝来解决。
4. 深拷贝保证数据不会被修改
```Python
import copy
sub = [2, 3]
list_d = [1, sub]
list_f = copy.deepcopy(list_d)
list_d[1][1] = 30
print('list_d: ', list_d)
print('list_e: ', list_f)
```
运行结果：
```Python
list_d:  [1, [2, 30]]
list_e:  [1, [2, 3]]
```
使用 `copy` 模块的 `deepcopy()` 方法，在拷贝数据时，会递归地拷贝数据中所有嵌套的引用。
使用 `deepcopy()` 拷贝 `list_d` 到 `list_f` ，然后修改 `list_d` 中子列表的引用，不会对 `list_f` 产生影响，所以 `list_f` 不会被修改。
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200323024515.png)
### 元组
- 如果要定义一个空的tuple，可以写成()：
```Python
t = ()
print(t)
()
```
- 但是，要定义一个只有1个元素的`tuple`，如果这么定义：
```Python
t = (1)
print(t)
1
```
定义的不是`tuple`，是`1`这个数！这是因为括号()既可以表示`tuple`，又可以表示数学公式中的小括号，这就产生了歧义，因此，`Python`规定，这种情况下，按小括号进行计算，计算结果自然是1。
所以，只有1个元素的`tuple`定义时必须加一个逗号,，来消除歧义：
```Python
t = (1,)
print(t）
(1,)
```
`Python`在显示只有1个元素的tuple时，也会加一个逗号,，以免你误解成数学计算意义上的括号。
- 如果元组中的某个对象是可变的，比如列表，可以在原位进行修改：
```Python
tup = tuple(['foo', [1, 2], True])
tup[1].append(3)
print(tup)
('foo', [1, 2, 3], True)
```
#### 拆分元组
- 使用特殊的语法`*rest`，函数签名中以抓取任意长度列表的位置参数：
```Python
values = 1, 2, 3, 4, 5
a, b, *rest = values
print(a, b)
(1, 2)
print(rest)
[3, 4, 5]
```
- `rest`的部分是想要舍弃的部分:
```Python
a, b, *_ = values
```
#### `tuple`方法
- 统计值出现频率：
```Python
a = (1, 2, 2, 2, 3, 4, 2)
print(a.count(2))
4
```
#### 常用函数
- `len(tuple)`:计算元组元素个数。
- `max(tuple)`:返回元组中元素最大值。
- `min(tuple)`:返回元组中元素最小值。
### 列表
#### 添加和删除元素
- `insert`在特定的位置插入元素：
```Python
b_list=['foo', 'bar', 'baz']
b_list.insert(1, 'red')
print(b_list)
['foo', 'red', 'peekaboo', 'baz', 'dwarf']
```
与`append`相比，`insert`耗费的计算量大，因为对后续元素的引用必须在内部迁移，以便为新元素提供空间。如果要在序列的头部和尾部插入元素，使用`collections.deque`，一个双尾部队列。
- `insert`的逆运算是`pop`，它移除并返回指定位置的元素,`pop()`默认删除最后一个元素：
```Python
print(b_list.pop(2))
'peekaboo'
print(b_list)
['foo', 'red', 'baz', 'dwarf']
```
- `remove`去除某个值，`remove`会先寻找第一个值并除去：
```Python
b_list.append('foo')
print(b_list)
['foo', 'red', 'baz', 'dwarf', 'foo']
b_list.remove('foo')
print(b_list)
['red', 'baz', 'dwarf', 'foo']
```
#### 串联和组合列表
- 可以用加号将两个列表串联起来,如果已经定义了一个列表，用`extend`方法可以追加多个元素：
```Python
x = [4, None, 'foo']
x.extend([7, 8, (2, 3)])
print(x)
[4, None, 'foo', 7, 8, (2, 3)]
```
- 通过加法将列表串联的计算量较大，因为要新建一个列表，并且要复制对象。用`extend`追加元素，尤其是到一个大列表中，更为可取。因此：
```Python
#快
everything = []
for chunk in list_of_lists:
    everything.extend(chunk)
#慢
everything = []
for chunk in list_of_lists:
    everything = everything + chunk
```
考虑下列代码片段：
```Python
list = [ [ ] ] * 5
list  # output?
list[0].append(10)
list  # output?
list[1].append(20)
list  # output?
list.append(30)
list  # output?
```
2,4,6,8行将输出什么结果？试解释。
输出的结果如下：
```Python
[[], [], [], [], []]
[[10], [10], [10], [10], [10]]
[[10, 20], [10, 20], [10, 20], [10, 20], [10, 20]]
[[10, 20], [10, 20], [10, 20], [10, 20], [10, 20], 30]
```
解释如下：
第一行的输出结果直觉上很容易理解，例如 `list = [ [ ] ] * 5` 就是简单的创造了5个空列表。然而，理解表达式`list=[ [ ] ] * 5`的关键一点是它不是创造一个包含五个独立列表的列表，而是它是一个创建了包含对同一个列表五次引用的列表。只有了解了这一点，我们才能更好的理解接下来的输出结果。
`list[0].append(10)` 将10附加在第一个列表上。
但由于所有5个列表是引用的同一个列表，所以这个结果将是：
```Python
[[10], [10], [10], [10], [10]]
```
同理，`list[1].append(20)`将20附加在第二个列表上。但同样由于5个列表是引用的同一个列表，所以输出结果现在是：
```Python
[[10, 20], [10, 20], [10, 20], [10, 20], [10, 20]]
```
作为对比， `list.append(30)`是将整个新的元素附加在外列表上，因此产生的结果是： `[[10, 20], [10, 20], [10, 20], [10, 20], [10, 20], 30]`。
#### 排序
- `sort`函数将一个列表原地排序（不创建新的对象）
```Python
a = [7, 2, 5, 1, 3]
a.sort()
print(a)
[1, 2, 3, 5, 7]
```
- `sort`有一些选项，有时会很好用。其中之一是二级排序`key`，可以用这个`key`进行排序。例如，我们可以按长度对字符串进行排序：
```Python
b = ['saw', 'small', 'He', 'foxes', 'six']
b.sort(key=len)
print(b)
['He', 'saw', 'six', 'small', 'foxes']
```
#### 二分搜索和维护已排序的列表
- `bisect`模块支持二分查找，和向已排序的列表插入值。`bisect.bisect`可以找到插入值后仍保证排序的位置，`bisect.insort`是向这个位置插入值:
```Python
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
- `zip`可以将多个列表、元组或其它序列成对组合成一个元组列表：
```Python
seq1 = ['foo', 'bar', 'baz']
seq2 = ['one', 'two', 'three']
zipped = zip(seq1, seq2)
print(list(zipped))
[('foo', 'one'), ('bar', 'two'), ('baz', 'three')]
```
- `zip`可以处理任意多的序列，元素的个数取决于最短的序列：
```Python
seq3 = [False, True]
print(list(zip(seq1, seq2, seq3)))
[('foo', 'one', False), ('bar', 'two', True)]
```
- `zip`的常见用法之一是同时迭代多个序列，可能结合`enumerate`使用：
```Python
for i, (a, b) in enumerate(zip(seq1, seq2)):
    print('{0}: {1}, {2}'.format(i, a, b))
0: foo, one
1: bar, two
2: baz, three
```
- 给出一个“被压缩的”序列，`zip`可以被用来解压序列。也可以当作把行的列表转换为列的列表。
```Python
pitchers = [('Nolan', 'Ryan'), ('Roger', 'Clemens'),('Schilling', 'Curt')]
first_names, last_names = zip(*pitchers)
print(first_names)
('Nolan', 'Roger', 'Schilling')
print(last_names)
('Ryan', 'Clemens', 'Curt')
```
#### `reversed`函数
`reversed`是一个生成器（后面详细介绍），只有实体化（即列表或`for`循环）之后才能创建翻转的序列。
```Python
print(list(reversed(range(10))))
[9, 8, 7, 6, 5, 4, 3, 2, 1, 0]
```
#### 列表拷贝
- `b=a[:]`。
- `b=list(a)`。
- 使用`copy.copy()`函数，或`b=a.copy()`直接复制`list`，类似`a[:]`。
- 使用`copy.deepcopy()`。
使用`b=a`是完全引用，除了名字没区别
#### 常用函数
- `max(list)`:返回列表元素最大值
- `min(list)`:返回列表元素最小值
- `list.count(obj)`:统计某个元素在列表中出现的次数
- `list.extend(seq)`:在列表末尾一次性追加另一个序列中的多个值（用新列表扩展原来的列表）
- `list.index(obj)`:从列表中找出某个值第一个匹配项的索引位置
- `list.reverse()`:反向列表中元素
#### 串联函数
```Python
操作函数对象
def f():
    print('i\'m f')
def g():
    print('i\'m g')
[f,g][1]()
i'm g
```
### 字典
- 多种构造方法:
```Python
a = dict(one=1, two=2, three=3)
b = {'one':1, 'two':2, 'three':3}
c = dict(zip(['one', 'two', 'three'], [1, 2, 3]))
d = dict([('one', 1), ('two', 2), ('three', 3)])
e = dict({'three':3, 'one':1, 'two':2})
print(a)
print(b)
print(c)
print(d)
print(e)
>>>{'one': 1, 'two': 2, 'three': 3}
>>>{'one': 1, 'two': 2, 'three': 3}
>>>{'one': 1, 'two': 2, 'three': 3}
>>>{'one': 1, 'two': 2, 'three': 3}
>>>{'three': 3, 'one': 1, 'two': 2}
print(a==b==c==d==e)
>>>True
```
特别注意这种构造方法：
```Python
t = {x:y for x in range(10) for y in range(10)}
print(t)
{0: 9, 1: 9, 2: 9, 3: 9, 4: 9, 5: 9, 6: 9, 7: 9, 8: 9, 9: 9}
```
```Python
d = dict(name='Bob', age=20, score=88)
```
`pickle.dumps()`方法把任意对象序列化成一个`bytes`，然后，就可以把这个`bytes`写入文件。或者用另一个方法`pickle.dump()`直接把对象序列化后写入一个`file-like Object`：
```Python
f = open('dump.txt', 'wb')
pickle.dump(d, f)
f.close()
```
当我们要把对象从磁盘读到内存时，可以先把内容读到一个`bytes`，然后用`pickle.loads()`方法反序列化出对象，也可以直接用`pickle.load()`方法从一个`file-like Object`中直接反序列化出对象。我们打开另一个`Python`命令行来反序列化刚才保存的对象：
```Python
f = open('dump.txt', 'rb')
d = pickle.load(f)
f.close()
print(d)
{'age': 20, 'score': 88, 'name': 'Bob'}
```
```Python
import json
d = dict(name='Bob', age=20, score=88)
json.dumps(d)
'{"age": 20, "score": 88, "name": "Bob"}'
```
`dumps()`方法返回一个`str`，内容就是标准的`JSON`。类似的，`dump()`方法可以直接把`JSON`写入一个`file-like Object`。
要把`JSON`反序列化为`Python`对象，用`loads()`或者对应的`load()`方法，前者把`JSON`的字符串反序列化，后者从`file-like Object`中读取字符串并反序列化：
```Python
json_str = '{"age": 20, "score": 88, "name": "Bob"}'
json.loads(json_str)
{'age': 20, 'score': 88, 'name': 'Bob'}
```
#### 删除值
- 用`del`关键字或`pop`方法（返回值的同时删除键）删除值：
```Python
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
- `popitem():返回并删除字典中的最后一对键和值。
#### 更新字典
- 用`update`方法可以将一个字典与另一个融合,`update`方法是原地改变字典，因此任何传递给`update`的键的旧的值都会被舍弃。
```Python
d1.update({'b' : 'foo', 'c' : 12})
print(d1)
{'a': 'some value', 'b': 'foo', 7: 'an integer', 'c': 12}
```
#### 用序列创建字典
```Python
mapping = {}
for key, value in zip(key_list, value_list):
    mapping[key] = value
```
- 因为字典本质上是2元元组的集合，`dict`可以接受2元元组的列表：
```Python
mapping = dict(zip(range(5), reversed(range(5))))
print(mapping)
{0: 4, 1: 3, 2: 2, 3: 1, 4: 0}
```
#### 默认值
```Python
if key in some_dict:
    value = some_dict[key]
else:
    value = default_value
```
- `dict`的方法`get`和`pop`可以取默认值进行返回，上面的`if-else`语句可以简写成下面：
```Python
value = some_dict.get(key, default_value)
```
- `get`默认会返回`None`，如果不存在键，`pop`会抛出一个例外。关于设定值，常见的情况是在字典的值是属于其它集合，如列表。例如，你可以通过首字母，将一个列表中的单词分类：
```Python
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
```Python
for word in words:
    letter = word[0]
    by_letter.setdefault(letter, []).append(word)
```
- `collections`模块有一个很有用的类，`defaultdict`，它可以进一步简化上面。传递类型或函数以生成每个位置的默认值：
```Python
from collections import defaultdict
by_letter = defaultdict(list)
for word in words:
    by_letter[word[0]].append(word)
```
给定以下字典的子类，下面的代码能够运行么？为什么？
```Python
class DefaultDict(dict):
  def __missing__(self, key):
    return []
d = DefaultDict()
d['florp'] = 127
```
能够运行。
当`key`缺失时，执行`DefaultDict`类，字典的实例将自动实例化这个数列。
#### 有效的键类型
- 字典的值可以是任意`Python`对象，而键通常是不可变的标量类型（整数、浮点型、字符串）或元组（元组中的对象必须是不可变的）。这被称为“可哈希性”。可以用`hash`函数检测一个对象是否是可哈希的（可被用作字典的键）：
```Python
print(hash('string'))
5023931463650008331
print(hash((1, 2, (2, 3))))
1097636502276347782
print(hash((1, 2, [2, 3]))) # fails because lists are mutable
---------------------------------------------------------------------------
TypeError                                 
Traceback (most recent call last)
<iPython-input-129-800cd14ba8be> in <module>()
----> 1 hash((1, 2, [2, 3])) # fails because lists are mutable
TypeError: unhashable type: 'list'
```
#### 按键值排序
- 键：
```Python
sorted(dict.keys())
```
- 值：
```Python
sorted(dict.items(),key=lamda:item:item[1])
```
### 集合
- 集合是无序的不可重复的元素的集合。你可以把它当做字典，但是只有键没有值。可以用两种方式创建集合：通过`set`函数或使用尖括号`set`语句：
```Python
print(set([2, 2, 2, 1, 3, 3]))
{1, 2, 3}
print({2, 2, 2, 1, 3, 3})
{1, 2, 3}
```
- 通过`add(key)`方法可以添加元素到set中
- 通过`remove(key)`方法可以删除元素：
- 合并是取两个集合中不重复的元素。可以用`union`方法，或者`|`运算符：
```Python
a = {1, 2, 3, 4, 5}
b = {3, 4, 5, 6, 7, 8}
print(a.union(b))
{1, 2, 3, 4, 5, 6, 7, 8}
print(a | b)
{1, 2, 3, 4, 5, 6, 7, 8}
```
- 交集的元素包含在两个集合中。可以用`intersection`或`&`运算符：
```Python
print(a.intersection(b))
{3, 4, 5}
print(a & b)
{3, 4, 5}
```
- 常用集合方法


  ![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200317202703.png)
- 所有逻辑集合操作都有另外的原地实现方法，可以直接用结果替代集合的内容。对于大的集合，这么做效率更高：
```Python
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
```Python
a_set = {1, 2, 3, 4, 5}
print({1, 2, 3}.issubset(a_set))
True
print(a_set.issuperset({1, 2, 3}))
True
```
### 列表、集合和字典推导式
```Python
[expr for val in collection if condition]
```
等同于：
```Python
result = []
for val in collection:
    if condition:
        result.append(expr)
```
- 字典:
```Python
dict_comp = {key-expr : value-expr for value in collection if condition}
```
- 集合
```Python
set_comp = {expr for value in collection if condition}
```
- `map`函数可以进一步简化：
```Python
print(set(map(len, strings)))
{1, 2, 3, 4, 6}
```
#### 嵌套列表推导式
```Python
all_data = [['John', 'Emily', 'Michael', 'Mary', 'Steven'],['Maria', 'Juan', 'Javier', 'Natalia', 'Pilar']]
names_of_interest = []
for names in all_data:
    enough_es = [name for name in names if name.count('e') >= 2]
    names_of_interest.extend(enough_es)
```
嵌套列表推导式：
```Python
result = [name for names in all_data for name in names if name.count('e') >= 2]
print(result)
result=['Steven']
```
以下代码正常输出偶数：
```Python
[x for x in range(1, 11) if x % 2 == 0]
[2, 4, 6, 8, 10]
```
但是，我们不能在最后的`if`加上`else`：
```Python
[x for x in range(1, 11) if x % 2 == 0 else 0]
  File "<stdin>", line 1
    [x for x in range(1, 11) if x % 2 == 0 else 0]
                                              ^
SyntaxError: invalid syntax
```
这是因为跟在`for`后面的`if`是一个筛选条件，不能带`else`，否则如何筛选？
另一些童鞋发现把`if`写在`for`前面必须加`else`，否则报错：
```Python
[x if x % 2 == 0 for x in range(1, 11)]
  File "<stdin>", line 1
    [x if x % 2 == 0 for x in range(1, 11)]
                       ^
SyntaxError: invalid syntax
```
这是因为`for`前面的部分是一个表达式，它必须根据`x`计算出一个结果。因此，考察表达式：`x if x % 2 == 0`，它无法根据`x`计算出结果，因为缺少`else`，必须加上`else`:
```Python
[x if x % 2 == 0 else -x for x in range(1, 11)]
[-1, 2, -3, 4, -5, 6, -7, 8, -9, 10]
```
上述for前面的表达式`x if x % 2 == 0 else -x`才能根据`x`计算出确定的结果。
可见，在一个列表生成式中，`for`前面的`if ... else`是表达式，而`for`后面的`if`是过滤条件，不能带`else`。
```Python
print([[x for x in tup] for tup in some_tuples])
[[1, 2, 3], [4, 5, 6], [7, 8, 9]]
```
### 函数
#### 参数
- `Python`函数参数既不是传参也不是传引用。应该称其为传对象引用,如果是数字，字符串，元组则传值,如果是列表，字典则传址。对象作为参数传递给函数时，新的局域变量创建了对原始对象的引用，而不是复制。如果在函数里绑定一个新对象到一个变量，这个变动不会反映到上一层。因此可以改变可变参数的内容:
```Python
def append_element(some_list, element):
    some_list.append(element)
data = [1, 2, 3]
append_element(data, 4)
print(data)
[1, 2, 3, 4]
```
```Python
def func(d):
    d['a'] = 10
    d['b'] = 20            
    d = {'a': 1, 'b': 2}


d = {}                    # 1
func(d)                   # 2
print(d)
########打印结果########
{'a': 10, 'b': 20}
```
想一想, 最后的结果为什么还是`{'a': 10, 'b': 20}`?
首先在全局创建一个空字典,并将`d`贴上:
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/v2-5d4d7a9c04d8feac54b3350d8c5b1435_1440w.jpg)
将 `d` 传入到函数`func`中,在函数中局部的形参变量也为`d`,它同样贴在空字典对象上
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200323174500.png)
在函数中前两句,为字典赋值.因为字典是可变的,这一操作对全局的 `d` 也会产生同样的影响.因为此时全局的`d`与函数内部的`d`贴向的是同一个对象
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200323174540.png)
函数最后一句,本质上是将函数内部的`d`贴向另外一个字典对象,全局的`d`当然还是贴向原来的字典对象.
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200323174823.png)
函数结束,函数内部的`d`被回收,而且最后打印结果如下所示
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200323174908.png)

- 函数可以有一些位置参数(`positional`)和一些关键字参数(`keyword`)。关键字参数通常用于指定默认值或可选参数
#### 默认参数
```Python
i = 1
def test(a=i):
    print(a)

i = 2
test()  # 1
```
由于参数默认值是在函数定义时而不是函数执行时确定的，所以这段代码`test`方法的参数默认值时`1`而不是`2`。
```Python
def add_end(L=[]):
    L.append('END')
    return L
print(add_end())
['END']
print(add_end())
['END', 'END']
```
- Python函数在定义的时候，默认参数L的值就被计算出来了，即`[]`，因为默认参数L也是一个变量，它指向对象`[]`，每次调用该函数，如果改变了L的内容，则下次调用时，默认参数的内容就变了，不再是函数定义时的`[]`了。
**定义默认参数要牢记一点：默认参数必须指向不变对象！**
- 要修改上面的例子，我们可以用`None`这个不变对象来实现：
```Python
def add_end(L=None):
    if L is None:
        L = []
    L.append('END')
    return L
```
举例：
```Python
def extendList(val, list=[]):
    list.append(val)
    return list

list1 = extendList(10)
list2 = extendList(123,[])
list3 = extendList('a')

print "list1 = %s" % list1
print "list2 = %s" % list2
print "list3 = %s" % list3

list1 = [10, 'a']
list2 = [123]
list3 = [10, 'a']
```
很多人都会误认为`list1=[10]`，`list3=[‘a’]`,因为他们以为每次`extendList`被调用时，列表参数的默认值都将被设置为`[]`.但实际上的情况是，新的默认列表只在函数被定义的那一刻创建一次。
当`extendList`被没有指定特定参数`list`调用时，这组`list`的值随后将被使用。这是因为带有默认参数的表达式在函数被定义的时候被计算，不是在调用的时候被计算。因此`list1`和`list3`是在同一个默认列表上进行操作（计算）的。而`list2`是在一个分离的列表上进行操作（计算）的。（通过传递一个自有的空列表作为列表参数的数值）。
`extendList`的定义可以作如下修改。
尽管，创建一个新的列表，没有特定的列表参数。
下面这段代码可能能够产生想要的结果。
```Python
def extendList(val, list=None):
  if list is None:
    list = []
  list.append(val)
  return list
```
通过上面的修改，输出结果将变成：
```Python
list1 = [10]
list2 = [123]
list3 = ['a']
```
- 为什么要设计`str`、`None`这样的不变对象呢？因为不变对象一旦创建，对象内部的数据就不能修改，这样就减少了由于修改数据导致的错误。此外，由于对象不变，多任务环境下同时读取对象不需要加锁，同时读一点问题都没有。我们在编写程序时，如果可以设计一个不变对象，那就尽量设计成不变对象。
默认参数的值应该是不可变的对象，比如`None`、`True`、`False`、数字或字符串。 特别的，千万不要像下面这样写代码：
```python
def spam(a, b=[]): # NO!
    ...
```
如果你这么做了，当默认值在其他地方被修改后你将会遇到各种麻烦。这些修改会影响到下次调用这个函数时的默认值。比如：
```python
def spam(a, b=[]):
    print(b)
    return b
x = spam(1)
x
[]
x.append(99)
x.append('Yow!')
x
[99, 'Yow!']
spam(1) # Modified list gets returned!
[99, 'Yow!']
```
这种结果应该不是你想要的。为了避免这种情况的发生，最好是将默认值设为`None`， 然后在函数里面检查它，前面的例子就是这样做的。
在测试`None`值时使用 `is` 操作符是很重要的，也是这种方案的关键点。 有时候大家会犯下下面这样的错误：
```python
def spam(a, b=None):
    if not b: # NO! Use 'b is None' instead
        b = []
```
这么写的问题在于尽管`None`值确实是被当成`False`， 但是还有其他的对象(比如长度为`0`的字符串、列表、元组、字典等)都会被当做`False`。 因此，上面的代码会误将一些其他输入也当成是没有输入。比如：
```python
spam(1) # OK
x = []
spam(1, x) # Silent error. x value overwritten by default
spam(1, 0) # Silent error. 0 ignored
spam(1, '') # Silent error. '' ignored
```
最后一个问题比较微妙，那就是一个函数需要测试某个可选参数是否被使用者传递进来。 这时候需要小心的是你不能用某个默认值比如`None`、 `0`或者`False`值来测试用户提供的值(因为这些值都是合法的值，是可能被用户传递进来的)。 因此，你需要其他的解决方案了。
为了解决这个问题，你可以创建一个独一无二的私有对象实例，就像上面的`_no_value`变量那样。 在函数里面，你可以通过检查被传递参数值跟这个实例是否一样来判断。 这里的思路是用户不可能去传递这个`_no_value`实例作为输入。 因此，这里通过检查这个值就能确定某个参数是否被传递进来了。
这里对 `object()` 的使用看上去有点不太常见。`object` 是`python`中所有类的基类。 你可以创建 `object` 类的实例，但是这些实例没什么实际用处，因为它并没有任何有用的方法， 也没有任何实例数据(因为它没有任何的实例字典，你甚至都不能设置任何属性值)。 你唯一能做的就是测试同一性。这个刚好符合我的要求，因为我在函数中就只是需要一个同一性的测试而已。
#### 可变参数
```Python
def calc(*numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum
```
定义可变参数和定义一个`list`或`tuple`参数相比，仅仅在参数前面加了一个`*`号。在函数内部，参数`numbers`接收到的是一个`tuple`，因此，函数代码完全不变。但是，调用该函数时，可以传入任意个参数，包括`0`个参数。
如果已经有一个`list`或者`tuple`：
```Python
nums = [1, 2, 3]
print(calc(*nums))
14
```
#### 关键字参数
- 关键字参数允许你传入`0`个或任意个含参数名的参数，这些关键字参数在函数内部自动组装为一个`dict`。
```Python
def person(name, age, **kw):
    print('name:', name, 'age:', age, 'other:', kw)
```
- 函数`person`除了必选参数`name`和`age`外，还接受关键字参数`kw`。在调用该函数时，可以只传入必选参数：
```Python
person('Michael', 30)
name: Michael age: 30 other: {}
```
- 也可以传入任意个数的关键字参数：
```Python
person('Bob', 35, city='Beijing')
name: Bob age: 35 other: {'city': 'Beijing'}
person('Adam', 45, gender='M', job='Engineer')
name: Adam age: 45 other: {'gender': 'M', 'job': 'Engineer'}
```
关键字参数可以扩展函数的功能。比如，在`person`函数里，我们保证能接收到`name`和`age`这两个参数，但是，如果调用者愿意提供更多的参数，我们也能收到。试想你正在做一个用户注册的功能，除了用户名和年龄是必填项外，其他都是可选项，利用关键字参数来定义这个函数就能满足注册的需求。
和可变参数类似，也可以先组装出一个`dict`，然后，把该`dict`转换为关键字参数传进去：
```Python
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
`**extra`表示把`extra`这个`dict`的所有`key-value`用关键字参数传入到函数的`**kw`参数，`kw`将获得一个`dict`，注意``kw`获得的`dict`是`extra`的一份拷贝，对`kw`的改动不会影响到函数外的`extra`。
#### 命名关键字参数
- 对于关键字参数，函数的调用者可以传入任意不受限制的关键字参数。至于到底传入了哪些，就需要在函数内部通过`kw`检查。
仍以`person()`函数为例，我们希望检查是否有`city`和`job`参数：
```Python
def person(name, age, **kw):
    if 'city' in kw:
        # 有city参数
        pass
    if 'job' in kw:
        # 有job参数
        pass
    print('name:', name, 'age:', age, 'other:', kw)
```
- 如果要限制关键字参数的名字，就可以用命名关键字参数，例如，只接收`city`和`job`作为关键字参数。这种方式定义的函数如下：
```Python
def person(name, age, *, city, job):
    print(name, age, city, job)
```
- 和关键字参数`**kw不同`，命名关键字参数需要一个特殊分隔符`*`，`*`后面的参数被视为命名关键字参数。
调用方式如下：
```Python
person('Jack', 24, city='Beijing', job='Engineer')
Jack 24 Beijing Engineer
```
- 如果函数定义中已经有了一个可变参数，后面跟着的命名关键字参数就不再需要一个特殊分隔符`*`了：
```Python
def person(name, age, *args, city, job):
    print(name, age, args, city, job)
```
- 命名关键字参数必须传入参数名，这和位置参数不同。如果没有传入参数名，调用将报错：
```Python
person('Jack', 24, 'Beijing', 'Engineer')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: person() takes 2 positional arguments but 4 were given
```
- 由于调用时缺少参数名`city`和`job`，`Python`解释器把这4个参数均视为位置参数，但`person()`函数仅接受2个位置参数。
命名关键字参数可以有缺省值，从而简化调用：
```Python
def person(name, age, *, city='Beijing', job):
    print(name, age, city, job)
```
- 由于命名关键字参数`city`具有默认值，调用时，可不传入`city`参数：
```Python
person('Jack', 24, job='Engineer')
Jack 24 Beijing Engineer
```
- 使用命名关键字参数时，要特别注意，如果没有可变参数，就必须加一个`*`作为特殊分隔符。如果缺少`*`，`Python`解释器将无法识别位置参数和命名关键字参数：
```Python
def person(name, age, city, job):
    # 缺少 *，city和job被视为位置参数
    pass
```
#### 强制位置参数
`Python3.8` 新增了一个函数形参语法`/`用来指明函数形参必须使用指定位置参数，不能使用关键字参数的形式。
在以下的例子中，形参 `a` 和 `b` 必须使用指定位置参数，`c` 或 `d` 可以是位置形参或关键字形参，而 `e` 或 `f` 要求为关键字形参:
```Python
def f(a, b, /, c, d, *, e, f):
    print(a, b, c, d, e, f)
```
以下使用方法是正确的:
```Python
f(10, 20, 30, d=40, e=50, f=60)
```
以下使用方法会发生错误:
```Python
f(10, b=20, c=30, d=40, e=50, f=60)   # b 不能使用关键字参数的形式
f(10, 20, 30, 40, 50, f=60)           # e 必须使用关键字参数的形式
```
#### 参数组合
- 在`Python`中定义函数，可以用**必选参数**、**默认参数**、**可变参数**、**关键字参数**和**命名关键字参数**，这5种参数都可以组合使用。但是请注意，参数定义的顺序必须是：**必选参数**、**默认参数**、**可变参数**、**命名关键字参数**和**关键字参数**。
```Python
def f1(a, b, c=0, *args, **kw):
    print('a =', a, 'b =', b, 'c =', c, 'args =', args, 'kw =', kw)

def f2(a, b, c=0, *, d, **kw):
    print('a =', a, 'b =', b, 'c =', c, 'd =', d, 'kw =', kw)
```
- 在函数调用的时候，`Python`解释器自动按照参数位置和参数名把对应的参数传进去。
```Python
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
- 通过一个`tuple`和`dict`，你也可以调用上述函数：
```Python
args = (1, 2, 3, 4)
kw = {'d': 99, 'x': '#'}
f1(*args, **kw)
a = 1 b = 2 c = 3 args = (4,) kw = {'d': 99, 'x': '#'}
args = (1, 2, 3)
kw = {'d': 88, 'x': '#'}
f2(*args, **kw)
a = 1 b = 2 c = 3 d = 88 kw = {'x': '#'}
```
- 所以，对于任意函数，都可以通过类似`func(*args, **kw)`的形式调用它，无论它的参数是如何定义的。
**虽然可以组合多达5种参数，但不要同时使用太多的组合，否则函数接口的可理解性很差。**
- 示例：
`Python`五类参数：位置参数，关键字参数，默认参数，可变位置或关键字参数的使用。
```Python
def f(a,*b,c=10,**d):
  print(f'a:{a},b:{b},c:{c},d:{d}')
```
默认参数`c`不能位于可变关键字参数`d`后.
调用`f`:
```Python
f(1,2,5,width=10,height=20)
a:1,b:(2, 5),c:10,d:{'width': 10, 'height': 20}
```
可变位置参数`b`实参后被解析为元组`(2,5)`;而`c`取得默认值10; `d`被解析为字典.

再次调用`f`:
```Python
f(a=1,c=12)
a:1,b:(),c:12,d:{}
```
`a=1`传入时`a`就是关键字参数，`b`,`d`都未传值，`c`被传入12，而非默认值。
注意观察参数`a`, 既可以`f(1)`,也可以`f(a=1)` 其可读性比第一种更好，建议使用`f(a=1)`。如果要强制使用`f(a=1)`，需要在前面添加一个星号:
```Python
def f(*,a,*b):
  print(f'a:{a},b:{b}')
```
此时`f(1)`调用，将会报错：`TypeError: f() takes 0 positional arguments but 1 was given`
只能`f(a=1)`才能`OK`.

说明前面的`*`发挥作用，它变为只能传入关键字参数，那么如何查看这个参数的类型呢？借助`Python`的`inspect`模块：
```Python
for name,val in signature(f).parameters.items():
    print(name,val.kind)
a KEYWORD_ONLY
b VAR_KEYWORD
```
可看到参数`a`的类型为`KEYWORD_ONLY`，也就是仅仅为关键字参数。
但是，如果`f`定义为：
```Python
def f(a,*b):
  print(f'a:{a},b:{b}')
```
查看参数类型：

In [24]: for name,val in signature(f).parameters.items():
    ...:     print(name,val.kind)
    ...:
a POSITIONAL_OR_KEYWORD
b VAR_POSITIONAL
可以看到参数a既可以是位置参数也可是关键字参数。
#### 匿名(lambda)函数
- `lambda` 函数拥有自己的命名空间，且不能访问自己参数列表之外或全局命名空间里的参数。
- 虽然`lambda`函数看起来只能写一行，却不等同于`C`或`C++`的内联函数，后者的目的是调用小函数时不占用栈内存从而增加运行效率。
```Python
strings = ['foo', 'card', 'bar', 'aaaa', 'abab']
strings.sort(key=lambda x: len(set(list(x))))
print(strings)
['aaaa', 'foo', 'abab', 'bar', 'card']
```
先看下下面代码的效果：
```python
x = 10
a = lambda y: x + y
x = 20
b = lambda y: x + y
print(a(10),b(10))
```
现在我问你，`a(10)`和`b(10)`返回的结果是什么？如果你认为结果是`20`和`30`，那么你就错了：
```python
a(10)
30
b(10)
30
```
这其中的奥妙在于`lambda`表达式中的`x`是一个自由变量， 在运行时绑定值，而不是定义时就绑定，这跟函数的默认值参数定义是不同的。 因此，在调用这个lambda表达式的时候，`x`的值是执行时的值。例如：
```python
x = 15
a(10)
25
x = 3
a(10)
13
```
如果你想让某个匿名函数在定义时就捕获到值，可以将那个参数值定义成默认参数即可，就像下面这样：
```python
x = 10
a = lambda y, x=x: x + y
x = 20
b = lambda y, x=x: x + y
a(10)
20
b(10)
30
```
在这里列出来的问题是新手很容易犯的错误，有些新手可能会不恰当的使用`lambda`表达式。 比如，通过在一个循环或列表推导中创建一个`lambda`表达式列表，并期望函数能在定义时就记住每次的迭代值。例如：
```python
funcs = [lambda x: x+n for n in range(5)]
for f in funcs:
    print(f(0))
4
4
4
4
4
```
但是实际效果是运行是`n`的值为迭代的最后一个值。现在我们用另一种方式修改一下：
```python
funcs = [lambda x, n=n: x+n for n in range(5)]
for f in funcs:
    print(f(0))
0
1
2
3
4
```
通过使用函数默认值参数形式，`lambda`函数在定义时就能绑定到值。
#### 柯里化：部分参数应用
- 柯里化（currying）是一个有趣的计算机科学术语，它指的是通过“部分参数应用”（partial argument application）从现有函数派生出新函数的技术。例如，假设我们有一个执行两数相加的简单函数:
```Python
def add_numbers(x, y):
    return x + y
add_five = lambda y: add_numbers(5, y)
```
`add_numbers`的第二个参数称为“柯里化的”（curried）。这里没什么特别花哨的东西，因为我们其实就只是定义了一个可以调用现有函数的新函数而已。内置的`functools`模块可以用`partial`函数将此过程简化：
```Python
from functools import partial
add_five = partial(add_numbers, 5)
```
#### 生成器
- 能以一种一致的方式对序列进行迭代（比如列表中的对象或文件中的行）是`Python`的一个重要特点。这是通过一种叫做迭代器协议(`iterator protocol`，它是一种使对象可迭代的通用方式)的方式实现的，一个原生的使对象可迭代的方法。
- 生成器(`generator`)是构造新的可迭代对象的一种简单方式。一般的函数执行之后只会返回单个值，而生成器则是以延迟的方式返回一个值序列，即每返回一个值之后暂停，直到下一个值被请求时再继续。要创建一个生成器，只需将函数中的`return`替换为`yeild`即可：
```Python
def squares(n=10):
    print('Generating squares from 1 to {0}'.format(n ** 2))
    for i in range(1, n + 1):
        yield i ** 2
```
- 调用该生成器时，没有任何代码会被立即执行：
```Python
gen = squares()
print(gen)
<generator object squares at 0x7fbbd5ab4570>
```
直到你从该生成器中请求元素时，它才会开始执行其代码：
```Python
for x in gen:
    print(x, end=' ')
Generating squares from 1 to 100
1 4 9 16 25 36 49 64 81 100
```
在一个对象上实现迭代最简单的方式是使用一个生成器函数。使用`Node`类来表示树形数据结构。你可能想实现一个以深度优先方式遍历树形节点的生成器。 下面是代码示例：
```python
class Node:
    def __init__(self, value):
        self._value = value
        self._children = []

    def __repr__(self):
        return 'Node({!r})'.format(self._value)

    def add_child(self, node):
        self._children.append(node)

    def __iter__(self):
        return iter(self._children)

    def depth_first(self):
        yield self
        for c in self:
            yield from c.depth_first()
# Example
if __name__ == '__main__':
    root = Node(0)
    child1 = Node(1)
    child2 = Node(2)
    root.add_child(child1)
    root.add_child(child2)
    child1.add_child(Node(3))
    child1.add_child(Node(4))
    child2.add_child(Node(5))

    for ch in root.depth_first():
        print(ch)
    # Outputs Node(0), Node(1), Node(3), Node(4), Node(2), Node(5)
```
在这段代码中，`depth_first()` 方法简单直观。 它首先返回自己本身并迭代每一个子节点并通过调用子节点的 `depth_first()` 方法(使用 `yield from` 语句)返回对应元素。
#### 生成器表达式
另一种更简洁的构造生成器的方法是使用生成器表达式(`generator expression`)。这是一种类似于列表、字典、集合推导式的生成器。其创建方式为，把列表推导式两端的方括号改成圆括号：
```Python
gen = (x ** 2 for x in range(100))
print(gen)
<generator object <genexpr> at 0x7fbbd5ab29e8>
```
它跟下面这个冗长得多的生成器是完全等价的：
```Python
def _make_gen():
    for x in range(100):
        yield x ** 2
gen = _make_gen()
```
生成器表达式也可以取代列表推导式，作为函数参数：
```Python
print(sum(x ** 2 for x in range(100)))
328350
print(dict((i, i **2) for i in range(5)))
{0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
```
#### 迭代器
- 可以被`next()`函数调用并不断返回下一个值的对象称为迭代器：`Iterator`。
可以使用`isinstance()`判断一个对象是否是`Iterator`对象：
```Python
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
```Python
isinstance(iter([]), Iterator)
True
isinstance(iter('abc'), Iterator)
True
```
为什么`list`、`dict`、`str`等数据类型不是`Iterator`？

这是因为`Python`的`Iterator`对象表示的是一个数据流，`Iterator`对象可以被`next()`函数调用并不断返回下一个数据，直到没有数据时抛出`StopIteration`错误。可以把这个数据流看做是一个有序序列，但我们却不能提前知道序列的长度，只能不断通过`next()`函数实现按需计算下一个数据，所以`Iterator`的计算是惰性的，只有在需要返回下一个数据时它才会计算。
`Iterator`甚至可以表示一个无限大的数据流，例如全体自然数。而使用`list`是永远不可能存储全体自然数的。
##### 类作为迭代器
把一个类作为一个迭代器使用需要在类中实现两个方法 `__iter__()` 与 `__next__()` 。
如果你已经了解的面向对象编程，就知道类都有一个构造函数，`Python` 的构造函数为 `__init__()`, 它会在对象初始化的时候执行。
`__iter__()` 方法返回一个特殊的迭代器对象， 这个迭代器对象实现了 `__next__()` 方法并通过 `StopIteration` 异常标识迭代的完成。
`__next__()` 方法(`Python 2` 里是 `next()`)会返回下一个迭代器对象。
创建一个返回数字的迭代器，初始值为 1，逐步递增 1：
```Python
class MyNumbers:
  def __iter__(self):
    self.a = 1
    return self
 
  def __next__(self):
    x = self.a
    self.a += 1
    return x
 
myclass = MyNumbers()
myiter = iter(myclass)
 
print(next(myiter))
print(next(myiter))
print(next(myiter))
print(next(myiter))
print(next(myiter))
1
2
3
4
5
```
`StopIteration` 异常用于标识迭代的完成，防止出现无限循环的情况，在 `__next__()` 方法中我们可以设置在完成指定循环次数后触发 `StopIteration` 异常来结束迭代。
在 20 次迭代后停止执行：
```
class MyNumbers:
  def __iter__(self):
    self.a = 1
    return self
 
  def __next__(self):
    if self.a <= 20:
      x = self.a
      self.a += 1
      return x
    else:
      raise StopIteration
 
myclass = MyNumbers()
myiter = iter(myclass)
for x in myiter:
  print(x)
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
```
##### 通过字符串调用对象方法
最简单的情况，可以使用 `getattr()` ：
```python
import math
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __repr__(self):
        return 'Point({!r:},{!r:})'.format(self.x, self.y)

    def distance(self, x, y):
        return math.hypot(self.x - x, self.y - y)

p = Point(2, 3)
d = getattr(p, 'distance')(0, 0)  # Calls p.distance(0, 0)
```
调用一个方法实际上是两部独立操作，第一步是查找属性，第二步是函数调用。 因此，为了调用某个方法，你可以首先通过 `getattr()` 来查找到这个属性，然后再去以函数方式调用它即可。
#### 函数式编程
##### 高阶函数
###### `map`函数
- `map()` 会根据提供的函数对指定序列做映射。第一个参数 `function` 以参数序列中的每一个元素调用 `function` 函数，返回包含每次 `function` 函数返回值的新列表。
```Python
map(function, iterable, ...)
```
- `function`:函数
- `iterable`:一个或多个序列
```Python
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
- `reduce`把一个函数作用在一个序列`[x1, x2, x3, ...]`上，这个函数必须接收两个参数，`reduce`把结果继续和序列的下一个元素做累积计算，其效果就是:
```Python
reduce(f, [x1, x2, x3, x4]) = f(f(f(x1, x2), x3), x4)
```
如果要把序列`[1, 3, 5, 7, 9]`变换成整数13579，reduce就可以派上用场：
```Python
from functools import reduce
def fn(x, y):
    return x * 10 + y
print(reduce(fn, [1, 3, 5, 7, 9]))
13579
```
考虑到字符串`str`也是一个序列，对上面的例子稍加改动，配合`map()`，我们就可以写出把`str`转换为`int`的函数：
```Python
from functools import reduce
DIGITS = {'0': 0, '1': 1, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9}
def char2num(s):
    return DIGITS[s]
def str2int(s):
    return reduce(lambda x, y: x * 10 + y, map(char2num, s))
```
###### `filter`函数
- `Python`内建的`filter()`函数用于过滤序列。
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
- `sorted`函数可以从任意序列的元素返回一个新的排好序的列表：
```Python
print(sorted([7, 1, 2, 6, 0, 3, 2]))
[0, 1, 2, 2, 3, 6, 7]
print(sorted('horse race'))
[' ', 'a', 'c', 'e', 'e', 'h', 'o', 'r', 'r', 's']
```
`sorted()`函数也是一个高阶函数，它还可以接收一个`key`函数来实现自定义的排序，例如按绝对值大小排序：
```Python
sorted([36, 5, -12, 9, -21], key=abs)
[5, 9, -12, -21, 36]
```
进行反向排序，不必改动`key`函数，可以传入第三个参数`reverse=True`：
```Python
sorted(['bob', 'about', 'Zoo', 'Credit'], key=str.lower, reverse=True)
['Zoo', 'Credit', 'bob', 'about']
```
##### 返回函数
```Python
def lazy_sum(*args):
    def sum():
        ax = 0
        for n in args:
            ax = ax + n
        return ax
    return sum
```
调用`lazy_sum()`时，返回的并不是求和结果，而是求和函数：
```Python
f = lazy_sum(1, 3, 5, 7, 9)
f
<function lazy_sum.<locals>.sum at 0x101c6ed90>
```
调用函数`f`时，才真正计算求和的结果：
```Python
f()
25
```
函数`lazy_sum`中又定义了函数`sum`，并且，内部函数`sum`可以引用外部函数`lazy_sum`的参数和局部变量，当`lazy_sum`返回函数`sum`时，相关参数和变量都保存在返回的函数中，这种称为“闭包(Closure)”的程序结构拥有极大的威力。
###### 闭包
- 返回的函数在其定义内部引用了局部变量`args`，所以，当一个函数返回了一个函数后，其内部的局部变量还被新函数引用，所以，闭包用起来简单，实现起来可不容易。
```Python
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
```Python
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
```Python
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
```Python
 f1, f2, f3 = count()
f1()
1
f2()
4
f3()
9
```
```Python
下面这段代码的输出结果将是什么？请解释。
```Python
def multipliers():
  return [lambda x : i * x for i in range(4)]
print [m(2) for m in multipliers()]
```
你如何修改上面的`multipliers`的定义产生想要的结果？
上面代码输出的结果是`[6, 6, 6, 6]`(不是我们想的`[0, 2, 4, 6]`)。
上述问题产生的原因是`Python`闭包的延迟绑定。这意味着内部函数被调用时，参数的值在闭包内进行查找。因此，当任何由`multipliers()`返回的函数被调用时，`i`的值将在附近的范围进行查找。那时，不管返回的函数是否被调用，for循环已经完成，`i`被赋予了最终的值3。
因此，每次返回的函数乘以传递过来的值3，因为上段代码传过来的值是2，它们最终返回的都是6(3*2)。碰巧的是，《The Hitchhiker’s Guide to Python》也指出，在与`lambdas`函数相关也有一个被广泛被误解的知识点，不过跟这个`case`不一样。由`lambda`表达式创造的函数没有什么特殊的地方，它其实是和def创造的函数式一样的。
下面是解决这一问题的一些方法。
一种解决方法就是用`Python`生成器。
```Python
def multipliers():
  for i in range(4): yield lambda x : i * x
```
另外一个解决方案就是创造一个闭包，利用默认函数立即绑定。
```Python
def multipliers():
  return [lambda x, i=i : i * x for i in range(4)]
```
还有种替代的方案是，使用偏函数：
```Python
from functools import partial
from operator import mul
def multipliers():
  return [partial(mul, i) for i in range(4)]
```
通常来讲，闭包的内部变量对于外界来讲是完全隐藏的。 但是，你可以通过编写访问函数并将其作为函数属性绑定到闭包上来实现这个目的。例如：
```python
def sample():
    n = 0
    # Closure function
    def func():
        print('n=', n)

    # Accessor methods for n
    def get_n():
        return n

    def set_n(value):
        nonlocal n
        n = value

    # Attach as function attributes
    func.get_n = get_n
    func.set_n = set_n
    return func
```
下面是使用的例子:
```python
f = sample()
f()
n= 0
f.set_n(10)
f()
n= 10
f.get_n()
10
```
讨论
为了说明清楚它如何工作的，有两点需要解释一下。首先，`nonlocal` 声明可以让我们编写函数来修改内部变量的值。
##### 装饰器
- 在代码运行期间动态增加功能的方式，称之为“装饰器”(`Decorator`)。
本质上，`decorator`就是一个返回函数的高阶函数。所以，我们要定义一个能打印日志的`decorator`，可以定义如下：
```Python
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
```Python
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
```Python
now = log('execute')(now)
```
首先执行`log('execute')`，返回的是`decorator`函数，再调用返回的函数，参数是`now`函数，返回值最终是`wrapper`函数。

以上两种`decorator`的定义都没有问题，但还差最后一步。因为我们讲了函数也是对象，它有`__name__`等属性，但你去看经过`decorator`装饰之后的函数，它们的`__name__`已经从原来的`'now'`变成了`'wrapper'`：
```Python
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
```Python
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
```Python
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
```Python
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
自定义某个属性的一种简单方法是将它定义为一个`property`。 例如，下面的代码定义了一个`property`，增加对一个属性简单的类型检查：
```python
class Person:
    def __init__(self, first_name):
        self.first_name = first_name

    # Getter function
    @property
    def first_name(self):
        return self._first_name

    # Setter function
    @first_name.setter
    def first_name(self, value):
        if not isinstance(value, str):
            raise TypeError('Expected a string')
        self._first_name = value

    # Deleter function (optional)
    @first_name.deleter
    def first_name(self):
        raise AttributeError("Can't delete attribute")
```
上述代码中有三个相关联的方法，这三个方法的名字都必须一样。 第一个方法是一个 `getter` 函数，它使得 `first_name` 成为一个属性。 其他两个方法给 `first_name` 属性添加了 `setter` 和 `deleter` 函数。 需要强调的是只有在 `first_name` 属性被创建后， 后面的两个装饰器 `@first_name.setter` 和 `@first_name.deleter` 才能被定义。
`property`的一个关键特征是它看上去跟普通的`attribute`没什么两样， 但是访问它的时候会自动触发 `getter` 、`setter` 和 `deleter` 方法。例如：
```python
a = Person('Guido')
a.first_name # Calls the getter
'Guido'
a.first_name = 42 # Calls the setter
Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
    File "prop.py", line 14, in first_name
        raise TypeError('Expected a string')
TypeError: Expected a string
del a.first_name
Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
AttributeError: can`t delete attribute
```
在实现一个`property`的时候，底层数据(如果有的话)仍然需要存储在某个地方。 因此，在`get`和`set`方法中，你会看到对 `_first_name` 属性的操作，这也是实际数据保存的地方。 另外，你可能还会问为什么 `__init__()` 方法中设置了 `self.first_name` 而不是 `self._first_name` 。 在这个例子中，我们创建一个`property`的目的就是在设置`attribute`的时候进行检查。 因此，你可能想在初始化的时候也进行这种类型检查。通过设置 `self.first_name` ，自动调用 `setter` 方法， 这个方法里面会进行参数的检查，否则就是直接访问 `self._first_name` 了。
还能在已存在的`get`和`set`方法基础上定义`property`。例如：
```python
class Person:
    def __init__(self, first_name):
        self.set_first_name(first_name)

    # Getter function
    def get_first_name(self):
        return self._first_name

    # Setter function
    def set_first_name(self, value):
        if not isinstance(value, str):
            raise TypeError('Expected a string')
        self._first_name = value

    # Deleter function (optional)
    def del_first_name(self):
        raise AttributeError("Can't delete attribute")

    # Make a property from existing get/set methods
    name = property(get_first_name, set_first_name, del_first_name)
```
一个`property`属性其实就是一系列相关绑定方法的集合。如果你去查看拥有`property`的类， 就会发现`property`本身的`fget`、`fset`和`fdel`属性就是类里面的普通方法。比如：
```python
Person.first_name.fget
<function Person.first_name at 0x1006a60e0>
Person.first_name.fset
<function Person.first_name at 0x1006a6170>
Person.first_name.fdel
<function Person.first_name at 0x1006a62e0>
```
通常来讲，你不会直接取调用`fget`或者`fset`，它们会在访问`property`的时候自动被触发。
只有当你确实需要对`attribute`执行其他额外的操作的时候才应该使用到`property`。 有时候一些从其他编程语言(比如`Java`)过来的程序员总认为所有访问都应该通过`getter`和`setter`， 所以他们认为代码应该像下面这样写：
```python
class Person:
    def __init__(self, first_name):
        self.first_name = first_name

    @property
    def first_name(self):
        return self._first_name

    @first_name.setter
    def first_name(self, value):
        self._first_name = value
```
不要写这种没有做任何其他额外操作的`property`。 首先，它会让你的代码变得很臃肿，并且还会迷惑阅读者。 其次，它还会让你的程序运行起来变慢很多。 最后，这样的设计并没有带来任何的好处。 特别是当你以后想给普通`attribute`访问添加额外的处理逻辑的时候， 你可以将它变成一个`property`而无需改变原来的代码。 因为访问`attribute`的代码还是保持原样。
`Properties`还是一种定义动态计算`attribute`的方法。 这种类型的`attributes`并不会被实际的存储，而是在需要的时候计算出来。比如：
```python
import math
class Circle:
    def __init__(self, radius):
        self.radius = radius

    @property
    def area(self):
        return math.pi * self.radius ** 2

    @property
    def diameter(self):
        return self.radius * 2

    @property
    def perimeter(self):
        return 2 * math.pi * self.radius
```
在这里，我们通过使用`properties`，将所有的访问接口形式统一起来， 对半径、直径、周长和面积的访问都是通过属性访问，就跟访问简单的`attribute`是一样的。 如果不这样做的话，那么就要在代码中混合使用简单属性访问和方法调用。 下面是使用的实例：
```python
c = Circle(4.0)
c.radius
4.0
c.area  # Notice lack of ()
50.26548245743669
c.perimeter  # Notice lack of ()
25.132741228718345
```
尽管`properties`可以实现优雅的编程接口，但有些时候你还是会想直接使用`getter`和`setter`函数。例如：
```python
p = Person('Guido')
p.get_first_name()
'Guido'
p.set_first_name('Larry')
```
这种情况的出现通常是因为`Python`代码被集成到一个大型基础平台架构或程序中。 例如，有可能是一个`Python`类准备加入到一个基于远程过程调用的大型分布式系统中。 这种情况下，直接使用`get/set`方法(普通方法调用)而不是`property`或许会更容易兼容。
最后一点，不要像下面这样写有大量重复代码的`property`定义：
```python
class Person:
    def __init__(self, first_name, last_name):
        self.first_name = first_name
        self.last_name = last_name

    @property
    def first_name(self):
        return self._first_name

    @first_name.setter
    def first_name(self, value):
        if not isinstance(value, str):
            raise TypeError('Expected a string')
        self._first_name = value

    # Repeated property code, but for a different name (bad!)
    @property
    def last_name(self):
        return self._last_name

    @last_name.setter
    def last_name(self, value):
        if not isinstance(value, str):
            raise TypeError('Expected a string')
        self._last_name = value
```
重复代码会导致臃肿、易出错和丑陋的程序。好消息是，通过使用装饰器或闭包，有很多种更好的方法来完成同样的事情。 
在子类中，扩展定义在父类中的`property`的功能。考虑如下的代码，它定义了一个`property`：
```python
class Person:
    def __init__(self, name):
        self.name = name

    # Getter function
    @property
    def name(self):
        return self._name

    # Setter function
    @name.setter
    def name(self, value):
        if not isinstance(value, str):
            raise TypeError('Expected a string')
        self._name = value

    # Deleter function
    @name.deleter
    def name(self):
        raise AttributeError("Can't delete attribute")
```
下面是一个示例类，它继承自`Person`并扩展了 `name` 属性的功能：
```python
class SubPerson(Person):
    @property
    def name(self):
        print('Getting name')
        return super().name

    @name.setter
    def name(self, value):
        print('Setting name to', value)
        super(SubPerson, SubPerson).name.__set__(self, value)

    @name.deleter
    def name(self):
        print('Deleting name')
        super(SubPerson, SubPerson).name.__delete__(self)
```
接下来使用这个新类：
```python
s = SubPerson('Guido')
Setting name to Guido
s.name
Getting name
'Guido'
s.name = 'Larry'
Setting name to Larry
s.name = 42
Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
    File "example.py", line 16, in name
        raise TypeError('Expected a string')
TypeError: Expected a string
```
如果你仅仅只想扩展`property`的某一个方法，那么可以像下面这样写：
```python
class SubPerson(Person):
    @Person.name.getter
    def name(self):
        print('Getting name')
        return super().name
```
或者，你只想修改`setter`方法，就这么写：
```python
class SubPerson(Person):
    @Person.name.setter
    def name(self, value):
        print('Setting name to', value)
        super(SubPerson, SubPerson).name.__set__(self, value)
```
在子类中扩展一个`property`可能会引起很多不易察觉的问题， 因为一个`property`其实是 `getter`、`setter` 和 `deleter` 方法的集合，而不是单个方法。 因此，当你扩展一个`property`的时候，你需要先确定你是否要重新定义所有的方法还是说只修改其中某一个。
在第一个例子中，所有的`property`方法都被重新定义。 在每一个方法中，使用了 `super()` 来调用父类的实现。 在 `setter` 函数中使用 `super(SubPerson, SubPerson).name.__set__(self, value)` 的语句是没有错的。 为了委托给之前定义的`setter`方法，需要将控制权传递给之前定义的`name`属性的 `__set__()` 方法。 不过，获取这个方法的唯一途径是使用类变量而不是实例变量来访问它。 这也是为什么我们要使用 `super(SubPerson, SubPerson)` 的原因。

如果你只想重定义其中一个方法，那只使用 `@property` 本身是不够的。比如，下面的代码就无法工作：
```python
class SubPerson(Person):
    @property  # Doesn't work
    def name(self):
        print('Getting name')
        return super().name
```
如果你试着运行会发现`setter`函数整个消失了：
```python
s = SubPerson('Guido')
Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
    File "example.py", line 5, in __init__
        self.name = name
AttributeError: can't set attribute
```
你应该像之前说过的那样修改代码：
```python
class SubPerson(Person):
    @Person.name.getter
    def name(self):
        print('Getting name')
        return super().name
```
这么写后，`property`之前已经定义过的方法会被复制过来，而`getter`函数被替换。然后它就能按照期望的工作了：
```python
s = SubPerson('Guido')
s.name
Getting name
'Guido'
s.name = 'Larry'
s.name
Getting name
'Larry'
s.name = 42
Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
    File "example.py", line 16, in name
        raise TypeError('Expected a string')
TypeError: Expected a string
```
在这个特别的解决方案中，我们没办法使用更加通用的方式去替换硬编码的 `Person` 类名。 如果你不知道到底是哪个基类定义了`property`， 那你只能通过重新定义所有`property`并使用 `super()` 来将控制权传递给前面的实现。
值得注意的是上面演示的第一种技术还可以被用来扩展一个描述器。比如：
```python
# A descriptor
class String:
    def __init__(self, name):
        self.name = name

    def __get__(self, instance, cls):
        if instance is None:
            return self
        return instance.__dict__[self.name]

    def __set__(self, instance, value):
        if not isinstance(value, str):
            raise TypeError('Expected a string')
        instance.__dict__[self.name] = value

# A class with a descriptor
class Person:
    name = String('name')

    def __init__(self, name):
        self.name = name

# Extending a descriptor with a property
class SubPerson(Person):
    @property
    def name(self):
        print('Getting name')
        return super().name

    @name.setter
    def name(self, value):
        print('Setting name to', value)
        super(SubPerson, SubPerson).name.__set__(self, value)

    @name.deleter
    def name(self):
        print('Deleting name')
        super(SubPerson, SubPerson).name.__delete__(self)
```
最后值得注意的是，读到这里时，你应该会发现子类化 `setter` 和 `deleter` 方法其实是很简单的。
###### `@classmethod`
`@classmethod`对应的函数不需要实例化，不需要 `self` 参数，但第一个参数需要是表示自身类的 `cls` 参数，可以来调用类的属性，类的方法，实例化对象等。
`@classmethod`因为持有`cls`参数，可以来调用类的属性，类的方法，实例化对象等，避免硬编码。
```Python
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
如果在`@staticmethod`中要调用到这个类的一些属性方法，只能直接`类名.属性名`或`类名.方法名`。
```Python
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
```Python
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
- `int()`函数可以把字符串转换为整数，当仅传入字符串时，`int()`函数默认按十进制转换。
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
- 简单总结`functools.partial`的作用就是，把一个函数的某些参数给固定住（也就是设置默认值），返回一个新的函数，调用这个新函数会更简单。
注意到上面的新的`int2`函数，仅仅是把`base`参数重新设定默认值为2，但也可以在函数调用时传入其他值：
```Python
int2('1000000', base=10)
1000000
```
- 最后，创建偏函数时，实际上可以接收函数对象、`*args`和`**kw`这3个参数，当传入：
```Python
int2 = functools.partial(int, base=2)
```
实际上固定了`int()`函数的关键字参数`base`，也就是：
```
int2('10010')
```
相当于：
```
kw = { 'base': 2 }
int('10010', **kw)
```
当传入：
```Python
max2 = functools.partial(max, 10)
```
实际上会把10作为*args的一部分自动加到左边，也就是：
```Python
max2(5, 6, 7)
```
相当于：
```Python
args = (10, 5, 6, 7)
max(*args)
```
结果为10。
当函数的参数个数太多，需要简化时，使用`functools.partial`可以创建一个新的函数，这个新函数可以固定住原函数的部分参数，从而在调用时更简单。
#### `itertools`模块
- 标准库`itertools`模块中有一组用于许多常见数据算法的生成器。例如，`groupby`可以接受任何序列和一个函数。它根据函数的返回值对序列中的连续元素进行分组。下面是一个例子：
```Python
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
### 字符串
```Python
 a= 'ABC'
```
在内存中创建了一个`'ABC'`的字符串；
在内存中创建了一个名为`a`的变量，并把它指向`'ABC'`。
也可以把一个变量`a`赋值给另一个变量`b`，这个操作实际上是把变量`b`指向变量`a`所指向的数据，例如下面的代码：
```Python
a = 'ABC'
b = a
a = 'XYZ'
print(b)
'ABC`
```
#### 分割
```Python
s = 'Python'
list(s)
['p', 'y', 't', 'h', 'o', 'n']
```
#### 模板化或格式化
```Python
template = '{0:.2f} {1:s} are worth US${2:d}'
```
- `{0:.2f}`表示格式化第一个参数为带有两位小数的浮点数。
- `{1:s}`表示格式化第二个参数为字符串。
- `{2:d}`表示格式化第三个参数为一个整数。
在括号中的数字用于指向传入对象在 `format()` 中的位置，如下所示：
```Python
print('{0} 和 {1}'.format('Google', 'Runoob'))
Google 和 Runoob
print('{1} 和 {0}'.format('Google', 'Runoob'))
Runoob 和 Google
```
如果在` format()` 中使用了关键字参数, 那么它们的值会指向使用该名字的参数。
```Python
print('{name}网址： {site}'.format(name='菜鸟教程', site='www.runoob.com'))
菜鸟教程网址： www.runoob.com
```
位置及关键字参数可以任意的结合:
```Python
print('站点列表 {0}, {1}, 和 {other}。'.format('Google', 'Runoob', other='Taobao'))
站点列表 Google, Runoob, 和 Taobao。
```
可选项 : 和格式标识符可以跟着字段名。 这就允许对值进行更好的格式化。 下面的例子将 `Pi` 保留到小数点后三位：
```Python
import math
print('常量 PI 的值近似为 {0:.3f}。'.format(math.pi))
常量 PI 的值近似为 3.142。
```
如果你有一个很长的格式化字符串, 而你不想将它们分开, 那么在格式化时通过变量名而非位置会是很好的事情。
最简单的就是传入一个字典, 然后使用方括号 `[]` 来访问键值 :
```Python
table = {'Google': 1, 'Runoob': 2, 'Taobao': 3}
print('Runoob: {0[Runoob]:d}; Google: {0[Google]:d}; Taobao: {0[Taobao]:d}'.format(table))
Runoob: 2; Google: 1; Taobao: 3
```
也可以通过在 `table` 变量前使用 `**` 来实现相同的功能：
```Python
table = {'Google': 1, 'Runoob': 2, 'Taobao': 3}
print('Runoob: {Runoob:d}; Google: {Google:d}; Taobao: {Taobao:d}'.format(**table))
Runoob: 2; Google: 1; Taobao: 3
```
#### 常用操作
- `str.join(iterable)`:返回一个由 `iterable` 中的字符串拼接而成的字符串。比`+`效率要高。
如果你想要合并的字符串是在一个序列或者 `iterable` 中，那么最快的方式就是使用 `join()` 方法。比如：
```python
parts = ['Is', 'Chicago', 'Not', 'Chicago?']
' '.join(parts)
'Is Chicago Not Chicago?'
','.join(parts)
'Is,Chicago,Not,Chicago?'
''.join(parts)
'IsChicagoNotChicago?'
```
初看起来，这种语法看上去会比较怪，但是 `join()` 被指定为字符串的一个方法。 这样做的部分原因是你想去连接的对象可能来自各种不同的数据序列(比如列表，元组，字典，文件，集合或生成器等)， 如果在所有这些对象上都定义一个 `join()` 方法明显是冗余的。 因此你只需要指定你想要的分割字符串并调用他的 `join()` 方法去将文本片段组合起来。
如果你仅仅只是合并少数几个字符串，使用加号`(+)`通常已经足够了：
```python
a = 'Is Chicago'
b = 'Not Chicago?'
a + ' ' + b
'Is Chicago Not Chicago?'
```
加号`(+)`操作符在作为一些复杂字符串格式化的替代方案的时候通常也工作的很好，比如：
```python
print('{} {}'.format(a,b))
Is Chicago Not Chicago?
print(a + ' ' + b)
Is Chicago Not Chicago?
```
如果你想在源码中将两个字面字符串合并起来，你只需要简单的将它们放到一起，不需要用加号`(+)`。比如：
```python
a = 'Hello' 'World'
a
'HelloWorld'
```
字符串合并可能看上去并不需要用一整节来讨论。 但是不应该小看这个问题，程序员通常在字符串格式化的时候因为选择不当而给应用程序带来严重性能损失。
最重要的需要引起注意的是，当我们使用加号`(+)`操作符去连接大量的字符串的时候是非常低效率的， 因为加号连接会引起内存复制以及垃圾回收操作。 特别的，你永远都不应像下面这样写字符串连接代码：
```python
s = ''
for p in parts:
    s += p
```
这种写法会比使用 `join()` 方法运行的要慢一些，因为每一次执行`+=`操作的时候会创建一个新的字符串对象。 你最好是先收集所有的字符串片段然后再将它们连接起来。
一个相对比较聪明的技巧是利用生成器表达式转换数据为字符串的同时合并字符串，比如：
```python
data = ['ACME', 50, 91.1]
','.join(str(d) for d in data)
'ACME,50,91.1'
```
同样还得注意不必要的字符串连接操作。有时候程序员在没有必要做连接操作的时候仍然多此一举。比如在打印的时候：
```python
print(a + ':' + b + ':' + c) # Ugly
print(':'.join([a, b, c])) # Still ugly
print(a, b, c, sep=':') # Better
```
当混合使用`I/O`操作和字符串连接操作的时候，有时候需要仔细研究你的程序。 比如，考虑下面的两端代码片段：
```python
# Version 1 (string concatenation)
f.write(chunk1 + chunk2)
# Version 2 (separate I/O operations)
f.write(chunk1)
f.write(chunk2)
```
如果两个字符串很小，那么第一个版本性能会更好些，因为`I/O`系统调用天生就慢。 另外一方面，如果两个字符串很大，那么第二个版本可能会更加高效， 因为它避免了创建一个很大的临时结果并且要复制大量的内存块数据。 还是那句话，有时候是需要根据你的应用程序特点来决定应该使用哪种方案。
最后谈一下，如果你准备编写构建大量小字符串的输出代码， 你最好考虑下使用生成器函数，利用yield语句产生输出片段。比如：
```python
def sample():
    yield 'Is'
    yield 'Chicago'
    yield 'Not'
    yield 'Chicago?'
```
这种方法一个有趣的方面是它并没有对输出片段到底要怎样组织做出假设。 例如，你可以简单的使用 `join()` 方法将这些片段合并起来：
```python
text = ''.join(sample())
```
或者你也可以将字符串片段重定向到`I/O`：
```python
for part in sample():
    f.write(part)
```
再或者你还可以写出一些结合`I/O`操作的混合方案：
```python
def combine(source, maxsize):
    parts = []
    size = 0
    for part in source:
        parts.append(part)
        size += len(part)
        if size > maxsize:
            yield ''.join(parts)
            parts = []
            size = 0
    yield ''.join(parts)
# 结合文件操作
with open('filename', 'w') as f:
    for part in combine(sample(), 32768):
        f.write(part)
```
这里的关键点在于原始的生成器函数并不需要知道使用细节，它只负责生成字符串片段就行了。
- `eval(str)`:用来计算在字符串中的有效`Python`表达式,并返回一个对象
- `str.center(width[, fillchar])`:返回长度为 `width` 的字符串，原字符串在其正中。 使用指定的 `fillchar` 填充两边的空位（默认使用 `ASCII` 空格符）。 如果 `width` 小于等于 `len(s)` 则返回原字符串的副本。
- `str.count(str, beg= 0,end=len(string))`:返回 `str` 在 `string` 里面出现的次数，如果 `beg` 或者 `end `指定则返回指定范围内 `str` 出现的次数
- `str.find(str, beg=0, end=len(string))`:检测 `str` 是否包含在字符串中，如果指定范围 `beg` 和 `end` ，则检查是否包含在指定范围内，如果包含返回开始的索引值，否则返回`-1`
- `str.upper()`:转换字符串中的小写字母为大写
- `str.lower()`:转换字符串中所有大写字符为小写
- `str.replace(old, new [, max])`:将字符串中的 `str1` 替换成 `str2`,如果`max`指定，则替换不超过`max`次。
想处理中间的空格,使用 `replace()` 方法。示例如下：
```python
s.replace(' ', '')
'helloworld'
```
- `str.split(str="", num=string.count(str))`:`num=string.count(str))` 以`str`为分隔符截取字符串，如果`num`有指定值，则仅截取`num+1`个子字符串
- `str.strip([chars])`:截掉字符串两边的空格或指定字符。
- 匹配的是字面字符串，那么你通常只需要调用基本字符串方法就行， 比如 `str.find()` , `str.endswith()` , `str.startswith()` 或者类似的方法：
```python
text = 'yeah, but no, but yeah, but no, but yeah'
# Exact match
text == 'yeah'
False
# Match at start or end
text.startswith('yeah')
True
text.endswith('no')
False
# Search for the location of the first occurrence
text.find('no')
10
```

### 运算符
#### `==`和`is`
- 要判断两个引用是否指向同一个对象，可以使用`is`方法:
```Python
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
`is` 用于判断两个变量引用对象是否为同一个， `==` 用于判断引用变量的值是否相等。
`a is b` 相当于 `id(a)==id(b)`.
如果 `a=10;b=a;` 则此时 `a` 和 `b` 的内存地址一样的;
但当 `a=[1,2,3]`; 另 `b=a[:]` 时，虽然 `a` 和 `b` 的值一样，但内存地址不一样。
#### `any()`和`all()`
`any()`, `all()`很好理解，就是字面意思，即参数中任何一个为 `true` 或者全部为 `true` 则返回 `true`。
#### 十进制转二进制
```Python
bin(10)
'0b1010'
```
## 模块
- 每一个包目录下面都会有一个`__init__.py`的文件，这个文件是必须存在的，否则，`Python`就把这个目录当成普通目录，而不是一个包。
- 每个模块有各自独立的符号表，在模块内部为所有的函数当作全局符号表来使用。所以，模块的作者可以放心大胆的在模块内部使用这些全局变量，而不用担心把其他用户的全局变量搞混。在当前目录下存在与要引入模块同名的文件，就会把要引入的模块屏蔽掉。
- 导入`sys`模块后，我们就有了变量`sys`指向该模块，利用`sys`这个变量，就可以访问`sys`模块的所有功能。
`sys`模块有一个`argv`变量，用`list`存储了命令行的所有参数。`argv`至少有一个元素，因为第一个参数永远是该``.py文件的名称，例如：
运行`Python3 hello.py`获得的`sys.argv`就是`['hello.py']`；
运行`Python3 hello.py Michael`获得的`sys.argv`就是`['hello.py', 'Michael]`。
注意当使用 `from package import item` 这种形式的时候，对应的 `item` 既可以是包里面的子模块（子包），或者包里面定义的其他名称，比如函数，类或者变量。
`import` 语法会首先把 `item` 当作一个包定义的名称，如果没找到，再试图按照一个模块去导入。如果还没找到，抛出一个 `:exc:ImportError` 异常。
反之，如果使用形如 `import item.subitem.subsubitem` 这种导入形式，除了最后一项，都必须是包，而最后一项则可以是模块或者是包，但是不可以是类，函数或者变量的名字。
- 内置的函数 `dir()` 可以找到模块内定义的所有名称。以一个字符串列表的形式返回。
- `sys.argv` 是一个包含命令行参数的列表。`sys.path` 包含了一个 `Python` 解释器自动查找所需模块的路径的列表。
- 搜索路径是由一系列目录名组成的，`Python`解释器就依次从这些目录中去寻找所引入的模块。
搜索路径是在`Python`编译或安装的时候确定的，安装新的库应该也会修改。搜索路径被存储在`sys`模块中的`path`变量，做一个简单的实验，在交互式解释器中，输入以下代码：
```Python
import sys
sys.path
['', '/usr/lib/Python3.4', '/usr/lib/Python3.4/plat-x86_64-linux-gnu', '/usr/lib/Python3.4/lib-dynload', '/usr/local/lib/Python3.4/dist-packages', '/usr/lib/Python3/dist-packages']
```
`sys.path` 输出是一个列表，其中第一项是空串`''`，代表当前目录（若是从一个脚本中打印出来的话，可以更清楚地看出是哪个目录），亦即我们执行`Python`解释器的目录（对于脚本的话就是运行的脚本所在的目录）。
因此若在当前目录下存在与要引入模块同名的文件，就会把要引入的模块屏蔽掉。
了解了搜索路径的概念，就可以在脚本中修改`sys.path`来引入一些不在搜索路径中的模块。
如果我们要添加自己的搜索目录，有两种方法：
一是直接修改`sys.path`，添加要搜索的目录：
```Python
import sys
sys.path.append('/Users/michael/my_py_scripts')
```
这种方法是在运行时修改，运行结束后失效。
第二种方法是设置环境变量`PYTHONPATH`，该环境变量的内容会被自动添加到模块搜索路径中。设置方式与设置`Path`环境变量类似。注意只需要添加你自己的搜索路径，`Python`自己本身的搜索路径不受影响。
现在，在解释器的当前目录或者 `sys.path `中的一个目录里面来创建一个`fibo.py`的文件，代码如下：
```Python
# 斐波那契(fibonacci)数列模块
def fib(n):    # 定义到 n 的斐波那契数列
    a, b = 0, 1
    while b < n:
        print(b, end=' ')
        a, b = b, a+b
    print()
 
def fib2(n): # 返回到 n 的斐波那契数列
    result = []
    a, b = 0, 1
    while b < n:
        result.append(b)
        a, b = b, a+b
    return result
```
然后进入`Python`解释器，使用下面的命令导入这个模块：
```Python
import fibo
```
这样做并没有把直接定义在`fibo`中的函数名称写入到当前符号表里，只是把模块`fibo`的名字写到了那里。
### `name`属性
一个模块被另一个程序第一次引入时，其主程序将运行。如果我们想在模块被引入时，模块中的某一程序块不执行，我们可以用`__name__`属性来使该程序块仅在该模块自身运行时执行。
```Python3
# Filename: using_name.py
if __name__ == '__main__':
   print('程序自身在运行')
else:
   print('我来自另一模块')
```
```shell
$ Python using_name.py
程序自身在运行
$ Python
>>> import using_name
我来自另一模块
```
说明： 每个模块都有一个`__name_`_属性，当其值是`'__main__'`时，表明该模块自身在运行，否则是被引入。
#### 包
- 如果包定义文件 `__init__.py` 存在一个叫做 `__all__` 的列表变量，那么在使用 `from package import *` 的时候就把这个列表中的所有名字作为包内容导入。
```Python
__all__ = ["echo", "surround", "reverse"]
```
如果 `__all__`真的没有定义，那么使用`from sound.effects import *`这种语法的时候，就不会导入包 `sound.effects` 里的任何子模块。他只是把包`sound.effects`和它里面定义的所有内容导入进来（可能运行`__init__.py`里定义的初始化代码）。
这会把 `__init__.py` 里面定义的所有名字导入进来。并且他不会破坏掉我们在这句话之前导入的所有明确指定的模块。看下这部分代码:
```Python
import sound.effects.echo
import sound.effects.surround
from sound.effects import *
```
这个例子中，在执行 `from...import` 前，包 `sound.effects` 中的 `echo` 和 `surround` 模块都被导入到当前的命名空间中了。（当然如果定义了 `__all__` 就更没问题了）
## OOP
### 访问限制
如果要让内部属性不被外部访问，可以把属性的名称前加上两个下划线`__`，在`Python`中，实例的变量名如果以`__`开头，就变成了一个私有变量(`private`)，只有内部可以访问，外部不能访问:
```Python
class Student(object):

    def __init__(self, name, score):
        self.__name = name
        self.__score = score

    def print_score(self):
        print('%s: %s' % (self.__name, self.__score))
```
改完后，对于外部代码来说，没什么变动，但是已经无法从外部访问`实例变量.__name`和`实例变量.__score`了：
```Python
bart = Student('Bart Simpson', 59)
bart.__name
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Student' object has no attribute '__name'
```
这样就确保了外部代码不能随意修改对象内部的状态，这样通过访问限制的保护，代码更加健壮。

外部代码要获取`name`和`score`,修改属性,可以给`Student`类增加`get_name`和`get_score`这样的方法：
```Python
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
```Python
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
```Python
bart._Student__name
'Bart Simpson'
```
但是强烈建议你不要这么干，因为不同版本的`Python`解释器可能会把`__name`改成不同的变量名。
总的来说就是，`Python`本身没有任何机制阻止你干坏事，一切全靠自觉。
最后注意下面的这种错误写法：
```Python
bart = Student('Bart Simpson', 59)
bart.get_name()
'Bart Simpson'
bart.__name = 'New Name' # 设置__name变量！
bart.__name
'New Name'
```
表面上看，外部代码“成功”地设置了`__name`变量，但实际上这个`__name`变量和`class`内部的`__name`变量不是一个变量！内部的`__name`变量已经被`Python`解释器自动改成了`_Student__name`，而外部代码给`bart`新增了一个`__name`变量。不信试试：
```Python
bart.get_name() # get_name()内部返回self.__name
'Bart Simpson'
```
你还可能会遇到在类定义中使用两个下划线`(__)`开头的命名。比如：
```python
class B:
    def __init__(self):
        self.__private = 0

    def __private_method(self):
        pass

    def public_method(self):
        pass
        self.__private_method()
```
使用双下划线开始会导致访问名称变成其他形式。 比如，在前面的类`B`中，私有属性会被分别重命名为 `_B__private` 和 `_B__private_method` 。 这时候你可能会问这样重命名的目的是什么，答案就是继承——这种属性通过继承是无法被覆盖的。比如：
```python
class C(B):
    def __init__(self):
        super().__init__()
        self.__private = 1 # Does not override B.__private

    # Does not override B.__private_method()
    def __private_method(self):
        pass
```
这里，私有名称 `__private` 和 `__private_method` 被重命名为 `_C__private` 和 `_C__private_method` ，这个跟父类`B`中的名称是完全不同的。
大多数而言，你应该让你的非公共名称以单下划线开头。但是，如果你清楚你的代码会涉及到子类， 并且有些内部属性应该在子类中隐藏起来，那么才考虑使用双下划线方案。
### 继承和多态
```Python
#类定义
class people:
    #定义基本属性
    name = ''
    age = 0
    #定义私有属性,私有属性在类外部无法直接进行访问
    __weight = 0
    #定义构造方法
    def __init__(self,n,a,w):
        self.name = n
        self.age = a
        self.__weight = w
    def speak(self):
        print("%s 说: 我 %d 岁。" %(self.name,self.age))
 
#单继承示例
class student(people):
    grade = ''
    def __init__(self,n,a,w,g):
        #调用父类的构函
        people.__init__(self,n,a,w)
        self.grade = g
    #覆写父类的方法
    def speak(self):
        print("%s 说: 我 %d 岁了，我在读 %d 年级"%(self.name,self.age,self.grade))
```
```Python
class Animal(object):
    def run(self):
        print('Animal is running...')
class Dog(Animal):
    def run(self):
        print('Dog is running...')
```
子类和父类都存在相同的`run()`方法时，我们说，子类的`run()`覆盖了父类的`run()`，在代码运行的时候，总是会调用子类的`run()`。这样，我们就获得了继承的另一个好处：多态。]
```Python
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
```Python
isinstance(c, Animal)
True
```
看来`c`不仅仅是`Dog`，`c`还是`Animal`！因为`Dog`是从`Animal`继承下来的，当我们创建了一个`Dog`的实例`c`时，我们认为`c`的数据类型是`Dog`没错，但`c`同时也是`Animal`也没错，`Dog`本来就是`Animal`的一种！
所以，在继承关系中，如果一个实例的数据类型是某个子类，那它的数据类型也可以被看做是父类。
```Python
def run_twice(animal):
    animal.run()
    animal.run()
```
传入`Dog`的实例时，`run_twice()`就打印出：
```Python
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
```Python
class Timer(object):
    def run(self):
        print('Start...')
```
这就是动态语言的“鸭子类型”，它并不要求严格的继承体系，一个对象只要“看起来像鸭子，走起路来像鸭子”，那它就可以被看做是鸭子。
`Python`的`“file-like object“`就是一种鸭子类型。对真正的文件对象，它有一个`read()`方法，返回其内容。但是，许多对象，只要有`read()`方法，都被视为`“file-like object“`。许多函数接收的参数就是`“file-like object“`，你不一定要传入真正的文件对象，完全可以传入任何实现了`read()`方法的对象。
下面这段代码的输出结果将是什么？请解释。
```Python
class Parent(object):
    x = 1

class Child1(Parent):
    pass

class Child2(Parent):
    pass
print Parent.x, Child1.x, Child2.x
Child1.x = 2
print Parent.x, Child1.x, Child2.x
Parent.x = 3
print Parent.x, Child1.x, Child2.x
1 1 1
1 2 1
3 2 3
```
让很多人困惑或惊讶的是最后一行输出为什么是3 2 3 而不是 3 2 1.为什么在改变`parent.x`的同时也改变了`child2.x`的值？但与此同时没有改变`Child1.x`的值？
此答案的关键是，在`Python`中，类变量在内部是以字典的形式进行传递。
如果一个变量名没有在当前类下的字典中发现。则在更高级的类（如它的父类）中尽心搜索直到引用的变量名被找到。（如果引用变量名在自身类和更高级类中没有找到，将会引发一个属性错误。）
因此,在父类中设定`x = 1`,让变量`x`类(带有值1)能够在其类和其子类中被引用到。这就是为什么第一个打印语句输出结果是1 1 1
因此，如果它的任何一个子类被覆写了值（例如说，当我们执行语句`Child1.x = 2`）,这个值只在子类中进行了修改。这就是为什么第二个打印语句输出结果是1 2 1
最终，如果这个值在父类中进行了修改，（例如说，当我们执行语句`Parent.x = 3`）,这个改变将会影响那些还没有覆写子类的值（在这个例子中就是`Child2`）这就是为什么第三打印语句输出结果是3 2 3
### 获取对象信息
#### 使用`type()`
判断对象类型，使用`type()`函数：
```Python
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
```Python
isinstance([1, 2, 3], (list, tuple))
True
isinstance((1, 2, 3), (list, tuple))
True
```
**总是优先使用isinstance()判断类型，可以将指定类型及其子类“一网打尽”。**
#### 使用dir()
如果要获得一个对象的所有属性和方法，可以使用`dir()`函数，它返回一个包含字符串的`list`，比如，获得一个`str`对象的所有属性和方法：
```Python
dir('ABC')
['__add__', '__class__',..., '__subclasshook__', 'capitalize', 'casefold',..., 'zfill']
```
类似`__xxx__`的属性和方法在`Python`中都是有特殊用途的，比如`__len__`方法返回长度。在`Python`中，如果你调用`len()`函数试图获取一个对象的长度，实际上，在`len()`函数内部，它自动去调用该对象的`__len__()`方法，所以，下面的代码是等价的：
```Python
len('ABC')
3
'ABC'.__len__()
3
```
我们自己写的类，如果也想用`len(myObj)`的话，就自己写一个`__len__()`方法：
```Python
class MyDog(object):
    def __len__(self):
        return 100
dog = MyDog()
len(dog)
100
```
剩下的都是普通属性或方法，比如`lower()`返回小写的字符串：
```Python
'ABC'.lower()
'abc'
```
仅仅把属性和方法列出来是不够的，配合`getattr()`、`setattr()`以及`hasattr()`，我们可以直接操作一个对象的状态：
```Python
class MyObject(object):
    def __init__(self):
        self.x = 9
    def power(self):
        return self.x * self.x
obj = MyObject()
```
紧接着，可以测试该对象的属性：
```Python
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
```Python
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
```Python
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
```Python
class Student(object):
    def __init__(self, name):
        self.name = name

s = Student('Bob')
s.score = 90
```
直接在`class`中定义属性，这种属性是类属性，归`Student`类所有：
```Python
class Student(object):
    name = 'Student'
```
当我们定义了一个类属性后，这个属性虽然归类所有，但类的所有实例都可以访问到。来测试一下：
```Python
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
```Python
def set_age(self, age): # 定义一个函数作为实例方法
    self.age = age
from types import MethodType
s.set_age = MethodType(set_age, s) # 给实例绑定一个方法
s.set_age(25) # 调用实例方法
s.age # 测试结果
25
```
但是，给一个实例绑定的方法，对另一个实例是不起作用的：
```Python
s2 = Student() # 创建新的实例
s2.set_age(25) # 尝试调用方法
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Student' object has no attribute 'set_age'
```
为了给所有实例都绑定方法，可以给class绑定方法：
```Python
def set_score(self, score):
    self.score = score
Student.set_score = set_score
```
给class绑定方法后，所有实例均可调用.
通常情况下，上面的`set_score`方法可以直接定义在`class`中，但动态绑定允许我们在程序运行的过程中动态给`class`加上功能，这在静态语言中很难实现。
限制实例的属性怎么办？比如，只允许对`Student`实例添加`name`和`age`属性。
为了达到限制的目的，`Python`允许在定义`class`的时候，定义一个特殊的`__slots__`变量，来限制该`class`实例能添加的属性：
```Python
class Student(object):
    __slots__ = ('name', 'age') # 用tuple定义允许绑定的属性名称
```Python
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
```Python
class GraduateStudent(Student):
    pass
g = GraduateStudent()
g.score = 9999
```
除非在子类中也定义`__slots__`，这样，子类实例允许定义的属性就是自身的`__slots__`加上父类的`__slots__`。
#### 多重继承
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200318023741.png)
```Python
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
```Python
class Runnable(object):
    def run(self):
        print('Running...')

class Flyable(object):
    def fly(self):
        print('Flying...')
```
对于需要`Runnable`功能的动物，就多继承一个`Runnable`，例如`Dog`：
```Python
class Dog(Mammal, Runnable):
    pass
```
对于需要`Flyable`功能的动物，就多继承一个`Flyable`，例如`Bat`：
```Python
class Bat(Mammal, Flyable):
    pass
```
通过多重继承，一个子类就可以同时获得多个父类的所有功能。
在设计类的继承关系时，通常，主线都是单一继承下来的，例如，`Ostrich`继承自`Bird`。但是，如果需要“混入”额外的功能，通过多重继承就可以实现，比如，让`Ostrich`除了继承自`Bird`外，再同时继承`Runnable`。这种设计通常称之为`MixIn`。

为了更好地看出继承关系，我们把`Runnable`和`Flyable`改为`RunnableMixIn`和`FlyableMixIn`。类似的，你还可以定义出肉食动物`CarnivorousMixIn`和植食动物`HerbivoresMixIn`，让某个动物同时拥有好几个`MixIn`：
```Python
class Dog(Mammal, RunnableMixIn, CarnivorousMixIn):
    pass
```
`MixIn`的目的就是给一个类增加多个功能，这样，在设计类的时候，我们优先考虑通过多重继承来组合多个`MixIn`的功能，而不是设计多层次的复杂的继承关系。

`Python`自带的很多库也使用了`MixIn`。举个例子，`Python`自带了`TCPServer`和`UDPServer`这两类网络服务，而要同时服务多个用户就必须使用多进程或多线程模型，这两种模型由`ForkingMixIn`和`ThreadingMixIn`提供。通过组合，我们就可以创造出合适的服务来。

比如，编写一个多进程模式的`TCP`服务，定义如下：
```Python
class MyTCPServer(TCPServer, ForkingMixIn):
    pass
```
编写一个多线程模式的`UDP`服务，定义如下：
```Python
class MyUDPServer(UDPServer, ThreadingMixIn):
    pass
```
如果你打算搞一个更先进的协程模型，可以编写一个`CoroutineMixIn`：
```Python
class MyTCPServer(TCPServer, CoroutineMixIn):
    pass
```
这样一来，我们不需要复杂而庞大的继承链，只要选择组合不同的类的功能，就可以快速构造出所需的子类。
由于`Python`允许使用多重继承，因此，`MixIn`就是一种常见的设计。
只允许单一继承的语言（如`Java`）不能使用`MixIn`的设计。
- 若是父类中有相同的方法名，而在子类使用时未指定，`Python`从左至右搜索 即方法在子类中未找到时，从左到右查找父类中是否包含方法。
```Python
#类定义
class people:
    #定义基本属性
    name = ''
    age = 0
    #定义私有属性,私有属性在类外部无法直接进行访问
    __weight = 0
    #定义构造方法
    def __init__(self,n,a,w):
        self.name = n
        self.age = a
        self.__weight = w
    def speak(self):
        print("%s 说: 我 %d 岁。" %(self.name,self.age))
 
#单继承示例
class student(people):
    grade = ''
    def __init__(self,n,a,w,g):
        #调用父类的构函
        people.__init__(self,n,a,w)
        self.grade = g
    #覆写父类的方法
    def speak(self):
        print("%s 说: 我 %d 岁了，我在读 %d 年级"%(self.name,self.age,self.grade))
 
#另一个类，多重继承之前的准备
class speaker():
    topic = ''
    name = ''
    def __init__(self,n,t):
        self.name = n
        self.topic = t
    def speak(self):
        print("我叫 %s，我是一个演说家，我演讲的主题是 %s"%(self.name,self.topic))
 
#多重继承
class sample(speaker,student):
    a =''
    def __init__(self,n,a,w,g,t):
        student.__init__(self,n,a,w,g)
        speaker.__init__(self,n,t)
 
test = sample("Tim",25,80,4,"Python")
test.speak()   #方法名同，默认调用的是在括号中排前地父类的方法
我叫 Tim，我是一个演说家，我演讲的主题是 Python
```
- `super()`
如果你的父类方法的功能不能满足你的需求，你可以在子类重写你父类的方法，实例如下：
```Python
class Parent:        # 定义父类
   def myMethod(self):
      print ('调用父类方法')
 
class Child(Parent): # 定义子类
   def myMethod(self):
      print ('调用子类方法')
 
c = Child()          # 子类实例
c.myMethod()         # 子类调用重写方法
super(Child,c).myMethod() #用子类对象调用父类已被覆盖的方法
调用子类方法
调用父类方法
```
如果重写了`__init__`时，要继承父类的构造方法，可以使用 `super` 关键字：
```Python
super(子类，self).__init__(参数1，参数2，....)
```
为了调用父类(超类)的一个方法，可以使用 `super()` 函数，比如：
```python
class A:
    def spam(self):
        print('A.spam')
class B(A):
    def spam(self):
        print('B.spam')
        super().spam()  # Call parent spam()
```
`super()` 函数的一个常见用法是在 `__init__()` 方法中确保父类被正确的初始化了：
```python
class A:
    def __init__(self):
        self.x = 0
class B(A):
    def __init__(self):
        super().__init__()
        self.y = 1
```
`super()` 的另外一个常见用法出现在覆盖`Python`特殊方法的代码中，比如：
```python
class Proxy:
    def __init__(self, obj):
        self._obj = obj

    # Delegate attribute lookup to internal obj
    def __getattr__(self, name):
        return getattr(self._obj, name)

    # Delegate attribute assignment
    def __setattr__(self, name, value):
        if name.startswith('_'):
            super().__setattr__(name, value) # Call original __setattr__
        else:
            setattr(self._obj, name, value)
```
在上面代码中，`__setattr__()` 的实现包含一个名字检查。 如果某个属性名以下划线`(_)`开头，就通过 `super()` 调用原始的 `__setattr__()` ， 否则的话就委派给内部的代理对象 `self._obj` 去处理。 这看上去有点意思，因为就算没有显式的指明某个类的父类， `super()` 仍然可以有效的工作。
实际上，大家对于在`Python`中如何正确使用 `super()` 函数普遍知之甚少。 你有时候会看到像下面这样直接调用父类的一个方法：
```python
class Base:
    def __init__(self):
        print('Base.__init__')

class A(Base):
    def __init__(self):
        Base.__init__(self)
        print('A.__init__')
```
尽管对于大部分代码而言这么做没什么问题，但是在更复杂的涉及到多继承的代码中就有可能导致很奇怪的问题发生。 比如，考虑如下的情况：
```python
class Base:
    def __init__(self):
        print('Base.__init__')
class A(Base):
    def __init__(self):
        Base.__init__(self)
        print('A.__init__')

class B(Base):
    def __init__(self):
        Base.__init__(self)
        print('B.__init__')

class C(A,B):
    def __init__(self):
        A.__init__(self)
        B.__init__(self)
        print('C.__init__')
```
如果你运行这段代码就会发现 `Base.__init__()` 被调用两次，如下所示：
```
c = C()
Base.__init__
A.__init__
Base.__init__
B.__init__
C.__init__
```
可能两次调用 `Base.__init__()` 没什么坏处，但有时候却不是。 另一方面，假设你在代码中换成使用 `super()` ，结果就很完美了：
```python
class Base:
    def __init__(self):
        print('Base.__init__')

class A(Base):
    def __init__(self):
        super().__init__()
        print('A.__init__')

class B(Base):
    def __init__(self):
        super().__init__()
        print('B.__init__')

class C(A,B):
    def __init__(self):
        super().__init__()  # Only one call to super() here
        print('C.__init__')
```
运行这个新版本后，你会发现每个 `__init__()` 方法只会被调用一次了：
```python
c = C()
Base.__init__
B.__init__
A.__init__
C.__init__
```
为了弄清它的原理，我们需要花点时间解释下`Python`是如何实现继承的。 对于你定义的每一个类，`Python`会计算出一个所谓的方法解析顺序(`MRO`)列表。 这个`MRO`列表就是一个简单的所有基类的线性顺序表。例如：
```python
C.__mro__
(<class '__main__.C'>, <class '__main__.A'>, <class '__main__.B'>,
<class '__main__.Base'>, <class 'object'>)
```
为了实现继承，`Python`会在`MRO`列表上从左到右开始查找基类，直到找到第一个匹配这个属性的类为止。

而这个`MRO`列表的构造是通过一个`C3`线性化算法来实现的。 我们不去深究这个算法的数学原理，它实际上就是合并所有父类的`MRO`列表并遵循如下三条准则：
1. 子类会先于父类被检查
2. 多个父类会根据它们在列表中的顺序被检查
3. 如果对下一个类存在两个合法的选择，选择第一个父类
4. 老实说，你所要知道的就是`MRO`列表中的类顺序会让你定义的任意类层级关系变得有意义。

当你使用 `super()` 函数时，`Python`会在`MRO`列表上继续搜索下一个类。 只要每个重定义的方法统一使用 `super()` 并只调用它一次， 那么控制流最终会遍历完整个`MRO`列表，每个方法也只会被调用一次。 这也是为什么在第二个例子中你不会调用两次 `Base.__init__()` 的原因。

`super()` 有个令人吃惊的地方是它并不一定去查找某个类在`MRO`中下一个直接父类， 你甚至可以在一个没有直接父类的类中使用它。例如，考虑如下这个类：
```python
class A:
    def spam(self):
        print('A.spam')
        super().spam()
```
如果你试着直接使用这个类就会出错：
```python
a = A()
a.spam()
Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
    File "<stdin>", line 4, in spam
AttributeError: 'super' object has no attribute 'spam'
```
但是，如果你使用多继承的话看看会发生什么：
```python
class B:
    def spam(self):
        print('B.spam')
class C(A,B):
    pass
c = C()
c.spam()
A.spam
B.spam
```
你可以看到在类A中使用 `super().spam()` 实际上调用的是跟类`A`毫无关系的类`B`中的 `spam()` 方法。 这个用类`C`的`MRO`列表就可以完全解释清楚了：
```python
C.__mro__
(<class '__main__.C'>, <class '__main__.A'>, <class '__main__.B'>,
<class 'object'>)
```
在定义混入类的时候这样使用 `super()` 是很普遍的。

然而，由于 `super()` 可能会调用不是你想要的方法，你应该遵循一些通用原则。 首先，确保在继承体系中所有相同名字的方法拥有可兼容的参数签名(比如相同的参数个数和参数名称)。 这样可以确保 `super()` 调用一个非直接父类方法时不会出错。 其次，最好确保最顶层的类提供了这个方法的实现，这样的话在`MRO`上面的查找链肯定可以找到某个确定的方法。
#### 定制类
##### `__str__`
我们先定义一个`Student`类，打印一个实例：
```Python
class Student(object):
    def __init__(self, name):
        self.name = name
print(Student('Michael'))
<__main__.Student object at 0x109afb190>
```
打印出一堆`<__main__.Student object at 0x109afb190>`，不好看。
怎么才能打印得好看呢？只需要定义好`__str__()`方法，返回一个好看的字符串就可以了：
```Python
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
```Python
s = Student('Michael')
s
<__main__.Student object at 0x109afb310>
```
这是因为直接显示变量调用的不是`__str__()`，而是`__repr__()`，两者的区别是`__str__()`返回用户看到的字符串，而`__repr__()`返回程序开发者看到的字符串，也就是说，`__repr__()`是为调试服务的。

解决办法是再定义一个`__repr__()`。但是通常`__str__()`和`__repr__()`代码都是一样的，所以，有个偷懒的写法：
```Python
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
```Python
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
```Python
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
```Python
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
```Python
class Student(object):
    def __init__(self):
        self.name = 'Michael'
```
调用`name`属性，没问题，但是，调用不存在的`score`属性，就有问题了：
```Python
s = Student()
print(s.name)
Michael
print(s.score)
Traceback (most recent call last):
AttributeError: 'Student' object has no attribute 'score'
```
错误信息很清楚地告诉我们，没有找到`score`这个`attribute`。

要避免这个错误，除了可以加上一个`score`属性外，`Python`还有另一个机制，那就是写一个`__getattr__()`方法，动态返回一个属性。修改如下：
```Python
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
```Python
s.age()
25
```
注意，只有在没有找到属性的情况下，才调用`__getattr__`，已有的属性，比如`name`，不会在_`_getattr__`中查找。
此外，注意到任意调用如`s.abc`都会返回`None`，这是因为我们定义的`__getattr__`默认返回就是`None`。要让`class`只响应特定的几个属性，我们就要按照约定，抛出`AttributeError`的错误：
```Python
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
```Python
class Student(object):
    def __init__(self, name):
        self.name = name

    def __call__(self):
        print('My name is %s.' % self.name)
调用方式如下：
```Python
s = Student('Michael')
s() # self参数不要传入
My name is Michael.
```
`__call__()`还可以定义参数。对实例进行直接调用就好比对一个函数进行调用一样，所以你完全可以把对象看成函数，把函数看成对象，因为这两者之间本来就没啥根本的区别。
如果你把对象看成函数，那么函数本身其实也可以在运行期动态创建出来，因为类的实例都是运行期创建出来的，这么一来，我们就模糊了对象和函数的界限。
那么，怎么判断一个变量是对象还是函数呢？其实，更多的时候，我们需要判断一个对象是否能被调用，能被调用的对象就是一个`Callable`对象，比如函数和我们上面定义的带有`__call__()`的类实例。
通过`callable()`函数，我们就可以判断一个对象是否是“可调用”对象。
##### 实现比较操作
`Python`类对每个比较操作都需要实现一个特殊方法来支持。 例如为了支持`>=`操作符，你需要定义一个 `__ge__()` 方法。 尽管定义一个方法没什么问题，但如果要你实现所有可能的比较方法那就有点烦人了。

装饰器 `functools.total_ordering` 就是用来简化这个处理的。 使用它来装饰一个类，你只需定义一个 `__eq__()` 方法， 外加其他方法(`__lt__`, `__le__`, `__gt__`, or `__ge__`)中的一个即可。 然后装饰器会自动为你填充其它比较方法。
作为例子，我们构建一些房子，然后给它们增加一些房间，最后通过房子大小来比较它们：
```python
from functools import total_ordering

class Room:
    def __init__(self, name, length, width):
        self.name = name
        self.length = length
        self.width = width
        self.square_feet = self.length * self.width

@total_ordering
class House:
    def __init__(self, name, style):
        self.name = name
        self.style = style
        self.rooms = list()

    @property
    def living_space_footage(self):
        return sum(r.square_feet for r in self.rooms)

    def add_room(self, room):
        self.rooms.append(room)

    def __str__(self):
        return '{}: {} square foot {}'.format(self.name,
                self.living_space_footage,
                self.style)

    def __eq__(self, other):
        return self.living_space_footage == other.living_space_footage

    def __lt__(self, other):
        return self.living_space_footage < other.living_space_footage
```
这里我们只是给`House`类定义了两个方法：`__eq__()` 和 `__lt__()` ，它就能支持所有的比较操作：
```python
# Build a few houses, and add rooms to them
h1 = House('h1', 'Cape')
h1.add_room(Room('Master Bedroom', 14, 21))
h1.add_room(Room('Living Room', 18, 20))
h1.add_room(Room('Kitchen', 12, 16))
h1.add_room(Room('Office', 12, 12))
h2 = House('h2', 'Ranch')
h2.add_room(Room('Master Bedroom', 14, 21))
h2.add_room(Room('Living Room', 18, 20))
h2.add_room(Room('Kitchen', 12, 16))
h3 = House('h3', 'Split')
h3.add_room(Room('Master Bedroom', 14, 21))
h3.add_room(Room('Living Room', 18, 20))
h3.add_room(Room('Office', 12, 16))
h3.add_room(Room('Kitchen', 15, 17))
houses = [h1, h2, h3]
print('Is h1 bigger than h2?', h1 > h2) # prints True
print('Is h2 smaller than h3?', h2 < h3) # prints True
print('Is h2 greater than or equal to h1?', h2 >= h1) # Prints False
print('Which one is biggest?', max(houses)) # Prints 'h3: 1101-square-foot Split'
print('Which is smallest?', min(houses)) # Prints 'h2: 846-square-foot Ranch'
```
其实 `total_ordering` 装饰器也没那么神秘。 它就是定义了一个从每个比较支持方法到所有需要定义的其他方法的一个映射而已。 比如你定义了 `__le__()` 方法，那么它就被用来构建所有其他的需要定义的那些特殊方法。 实际上就是在类里面像下面这样定义了一些特殊方法：
```python
class House:
    def __eq__(self, other):
        pass
    def __lt__(self, other):
        pass
    # Methods created by @total_ordering
    __le__ = lambda self, other: self < other or self == other
    __gt__ = lambda self, other: not (self < other or self == other)
    __ge__ = lambda self, other: not (self < other)
    __ne__ = lambda self, other: not self == other
```
当然，你自己去写也很容易，但是使用 `@total_ordering` 可以简化代码，何乐而不为呢。
#### 枚举类
```Python
from enum import Enum
Month = Enum('Month', ('Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'))
```
这样我们就获得了`Month`类型的枚举类，可以直接使用`Month.Jan`来引用一个常量，或者枚举它的所有成员：
```Python
for name, member in Month.__members__.items():
    print(name, '=>', member, ',', member.value)
```
`value`属性则是自动赋给成员的`int`常量，默认从1开始计数。
如果需要更精确地控制枚举类型，可以从`Enum`派生出自定义类：
```Python
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
```Python
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
```Python
class Hello(object):
    def hello(self, name='world'):
        print('Hello, %s.' % name)
```
当`Python`解释器载入`hello`模块时，就会依次执行该模块的所有语句，执行结果就是动态创建出一个`Hello`的`class`对象，测试如下：
```Python
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
```Python
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
```Python
# metaclass是类的模板，所以必须从`type`类型派生：
class ListMetaclass(type):
    def __new__(cls, name, bases, attrs):
        attrs['add'] = lambda self, value: self.append(value)
        return type.__new__(cls, name, bases, attrs)
有了`ListMetaclass`，我们在定义类的时候还要指示使用`ListMetaclass`来定制类，传入关键字参数`metaclass`：
```Python
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
```Python
L = MyList()
L.add(1)
L
[1]
```
而普通的list没有`add()`方法：
```Python
L2 = list()
L2.add(1)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'list' object has no attribute 'add'
```
动态修改有什么意义？直接在`MyList`定义中写上`add()`方法不是更简单吗？正常情况下，确实应该直接写，通过`metaclass`修改纯属变态。
## 命名空间和作用域
### 三种命名空间：
- 内置名称(`built-in names`)， `Python` 语言内置的名称，比如函数名 `abs`、`char` 和异常名称 `BaseException`、`Exception` 等等。
- 全局名称(`global names`)，模块中定义的名称，记录了模块的变量，包括函数、类、其它导入的模块、模块级的变量和常量。
- 局部名称(`local names`)，函数中定义的名称，记录了函数的变量，包括函数的参数和局部定义的变量。（类中定义的也是）
`Python` 的查找顺序为：**局部的命名空间** -> **全局命名空间** -> **内置命名空间**。
#### 四种作用域：
- L(Local)：最内层，包含局部变量，比如一个函数/方法内部。
- E(Enclosing)：包含了非局部(`non-local`)也非全局(`non-global`)的变量。比如两个嵌套函数，一个函数（或类） `A` 里面又包含了一个函数 `B` ，那么对于`B` 中的名称来说 `A` 中的作用域就为 `nonlocal`。
- G(`Global`)：当前脚本的最外层，比如当前模块的全局变量。
- B(`Built-in`)： 包含了内建的变量/关键字等。最后被搜索。
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200320015425.png)
`Python` 中只有模块(`module`)，类(`class`)以及函数(`def`、`lambda`)才会引入新的作用域，其它的代码块(如 `if`/`elif`/`else`/、`try`/`except`、`for`/`while`等）是不会引入新的作用域的，也就是说这些语句内定义的变量，外部也可以访问，如下代码：
```Python
if True:
    msg = 'I am from Runoob'
msg
'I am from Runoob'
``` 
实例中 `msg` 变量定义在 `if` 语句块中，但外部还是可以访问的。
如果将 msg 定义在函数中，则它就是局部变量，外部不能访问：
```Python
def test():
    msg_inner = 'I am from Runoob'
msg_inner
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'msg_inner' is not defined
``` 
从报错的信息上看，说明了 `msg_inner` 未定义，无法使用，因为它是局部变量，只有在函数内可以使用。
#### `global` 和 `nonlocal`关键字
```Python
num = 1
def fun1():
    global num  # 需要使用 global 关键字声明
    print(num) 
    num = 123
    print(num)
fun1()
print(num)
1
123
123
```
如果要修改嵌套作用域(`enclosing` 作用域，外层非全局作用域)中的变量则需要 `nonlocal` 关键字了，如下实例：
```Python
def outer():
    num = 10
    def inner():
        nonlocal num   # nonlocal关键字声明
        num = 100
        print(num)
    inner()
    print(num)
outer()
100
100
```
另外有一种特殊情况，假设下面这段代码被运行：
```Python
a = 10
def test():
    a = a + 1
    print(a)
test()
Traceback (most recent call last):
  File "test.py", line 7, in <module>
    test()
  File "test.py", line 5, in test
    a = a + 1
UnboundLocalError: local variable 'a' referenced before assignment
```
错误信息为局部作用域引用错误，因为 `test` 函数中的 `a` 使用的是局部，未定义，无法修改。
修改 `a` 为全局变量，通过函数参数传递，可以正常执行输出结果为：
```Python
a = 10
def test(a):
    a = a + 1
    print(a)
test(a)
11
```
## 错误和异常处理
```Python
def attempt_float(x):
    try:
        return float(x)
    except ValueError:
        return x
```
某些情况下，你可能不想抑制异常，你想无论`try`部分的代码是否成功，都执行一段代码。可以使用`finally`：
```Python
f = open(path, 'w')

try:
    write_to_file(f)
finally:
    f.close()
```
这里，文件处理`f`总会被关闭。相似的，你可以用`else`让只在`try`部分成功的情况下，才执行代码：
```Python
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
- 可以有多个`except`来捕获不同类型的错误：
```Python
try:
    print('try...')
    r = 10 / int('a')
    print('result:', r)
except ValueError as e:
    print('ValueError:', e)
except ZeroDivisionError as e:
    print('ZeroDivisionError:', e)
finally:
    print('finally...')
print('END')
```
- Python的错误其实也是`class`，所有的错误类型都继承自`BaseException`，所以在使用`except`时需要注意的是，它不但捕获该类型的错误，还把其子类也“一网打尽”。比如：
```Python
try:
    foo()
except ValueError as e:
    print('ValueError')
except UnicodeError as e:
    print('UnicodeError')
第二个`except`永远也捕获不到`UnicodeError`，因为`UnicodeError`是`ValueError`的子类，如果有，也被第一个`except`给捕获了。
```
使用`try...except`捕获错误还有一个巨大的好处，就是可以跨越多层调用，比如函数`main()`调用`foo()`，`foo()`调用`bar()`，结果`bar()`出错了，这时，只要`main()`捕获到了，就可以处理：
```Python
def foo(s):
    return 10 / int(s)

def bar(s):
    return foo(s) * 2

def main():
    try:
        bar('0')
    except Exception as e:
        print('Error:', e)
    finally:
        print('finally...')
```
也就是说，不需要在每个可能出错的地方去捕获错误，只要在合适的层次去捕获错误就可以了。这样一来，就大大减少了写`try...except...finally`的麻烦。
```Python
def foo(s):
    n = int(s)
    if n==0:
        raise ValueError('invalid value: %s' % s)
    return 10 / n

def bar():
    try:
        foo('0')
    except ValueError as e:
        print('ValueError!')
        raise
bar()
```
在`bar()`函数中，我们明明已经捕获了错误，但是，打印一个`ValueError!`后，又把错误通过`raise`语句抛出去。
这种错误处理方式相当常见。捕获错误目的只是记录一下，便于后续追踪。但是，由于当前函数不知道应该怎么处理该错误，所以，最恰当的方式是继续往上抛，让顶层调用者去处理。

`raise`语句如果不带参数，就会把当前错误原样抛出。此外，在`except`中`raise`一个`Error`，还可以把一种类型的错误转化成另一种类型：
```Python
try:
    10 / 0
except ZeroDivisionError:
    raise ValueError('input error!')
```
只要是合理的转换逻辑就可以，但是，决不应该把一个`IOError`转换成毫不相干的`ValueError`。
- `Python` 使用 `raise` 语句抛出一个指定的异常。
`raise`语法格式如下：
```Python
raise [Exception [, args [, traceback]]]
```
以下实例如果 x 大于 5 就触发异常:
```
x = 10
if x > 5:
    raise Exception('x 不能大于 5。x 的值为: {}'.format(x))
```
执行以上代码会触发异常：
```Python
Traceback (most recent call last):
  File "test.py", line 3, in <module>
    raise Exception('x 不能大于 5。x 的值为: {}'.format(x))
Exception: x 不能大于 5。x 的值为: 10
```
`raise` 唯一的一个参数指定了要被抛出的异常。它必须是一个异常的实例或者是异常的类（也就是 `Exception` 的子类）。
如果你只想知道这是否抛出了一个异常，并不想去处理它，那么一个简单的 `raise` 语句就可以再次把它抛出。
```Python
try:
    raise NameError('HiThere')
except NameError:
    print('An exception flew by!')
    raise
An exception flew by!
Traceback (most recent call last):
  File "<stdin>", line 2, in ?
NameError: HiThere
```
### 调试
- 凡是用`print()`来辅助查看的地方，都可以用断言`(assert)`来替代：
```Python
def foo(s):
    n = int(s)
    assert n != 0, 'n is zero!'
    return 10 / n
def main():
    foo('0')
```
`assert`的意思是，表达式`n != 0`应该是`True`，否则，根据程序运行的逻辑，后面的代码肯定会出错。
如果断言失败，`assert`语句本身就会抛出`AssertionError`：
```shell
$ Python err.py
Traceback (most recent call last):
  ...
AssertionError: n is zero!
```
- `logging`
把`print()`替换为`logging`是第3种方式，和`assert`比，`logging`不会抛出错误，而且可以输出到文件：
```Python
import logging
logging.basicConfig(level=logging.INFO)
s = '0'
n = int(s)
logging.info('n = %d' % n)
print(10 / n)
```
```shell
$ Python err.py
INFO:root:n = 0
Traceback (most recent call last):
  File "err.py", line 8, in <module>
    print(10 / n)
ZeroDivisionError: division by zero
```
这就是`logging`的好处，它允许你指定记录信息的级别，有`debug`，`info`，`warning`，`error`等几个级别，当我们指定`level=INFO`时，`logging.debug`就不起作用了。同理，指定`level=WARNING`后，`debug`和`info`就不起作用了。这样一来，你可以放心地输出不同级别的信息，也不用删除，最后统一控制输出哪个级别的信息。
`logging`的另一个好处是通过简单的配置，一条语句可以同时输出到不同的地方，比如`console`和文件。
## 文件和操作系统
- 为了打开一个文件以便读写，可以使用内置的`open`函数以及一个相对或绝对的文件路径：
```Python
path = 'examples/segismundo.txt'
f = open(path)
```
- 默认情况下，文件是以只读模式`('r')`打开的。然后，我们就可以像处理列表那样来处理这个文件句柄`f`了，比如对行进行迭代：
```Python
for line in f:
    pass
```
- 如果使用`open`创建文件对象，一定要用`close`关闭它。关闭文件可以返回操作系统资源：
```Python
f.close()
```
- 用`with`语句可以可以更容易地清理打开的文件,这样可以在退出代码块时，自动关闭文件：
```
with open(path) as f:
    lines = [x.rstrip() for x in f]
```
- 读写模式：
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200317211525.png)
- 向文件写入，可以使用文件的`write`或`writelines`方法。例如，我们可以创建一个无空行版的`prof_mod.py`：
```Python
with open('tmp.txt', 'w') as handle:
    handle.writelines(x for x in open(path) if len(x) > 1)
with open('tmp.txt') as f:
    lines = f.readlines()
```
```Python
with open('/Users/michael/test.txt', 'w') as f:
    f.write('Hello, world!')
```
要写入特定编码的文本文件，请给`open()`函数传入`encoding`参数，将字符串自动转换成指定编码。
以`'w'`模式写入文件时，如果文件已存在，会直接覆盖（相当于删掉后新写入一个文件）。如果我们希望追加到文件末尾怎么办？可以传入`'a'`以追加`(append)`模式写入。
- 调用`readline()`可以每次读取一行内容，调用`readlines()`一次读取所有内容并按行返回list。
- 常用文件方法
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200317211726.png)
### 操作文件和目录
操作文件和目录的函数一部分放在`os`模块中，一部分放在`os.path`模块中，这一点要注意一下。查看、创建和删除目录可以这么调用：
- 查看当前目录的绝对路径:
```Python
os.path.abspath('.')
'/Users/michael'
- 在某个目录下创建一个新目录，首先把新目录的完整路径表示出来:
````Python
os.path.join('/Users/michael', 'testdir')
'/Users/michael/testdir'
```
- 创建一个目录:
``` Python
os.mkdir('/Users/michael/testdir')
```
- 删掉一个目录:
>>> os.rmdir('/Users/michael/testdir')
把两个路径合成一个时，不要直接拼字符串，而要通过`os.path.join()`函数，这样可以正确处理不同操作系统的路径分隔符.
同样的道理，要拆分路径时，也不要直接去拆字符串，而要通过`os.path.split()`函数，这样可以把一个路径拆分为两部分，后一部分总是最后级别的目录或文件名：
```Python
os.path.split('/Users/michael/testdir/file.txt')

('/Users/michael/testdir', 'file.txt')
```
`os.path.splitext()`可以直接让你得到文件扩展名，很多时候非常方便：
```Python
os.path.splitext('/path/to/file.txt')
('/path/to/file', '.txt')
```
这些合并、拆分路径的函数并不要求目录和文件要真实存在，它们只对字符串进行操作。
文件操作使用下面的函数。假定当前目录下有一个`test.txt`文件：
- 对文件重命名:
```
os.rename('test.txt', 'test.py')
```
- 删掉文件:
```Python
os.remove('test.py')
```
`shutil`模块提供了`copyfile()`的函数，你还可以在`shutil`模块中找到很多实用函数，它们可以看做是`os`模块的补充。
### 序列化
`pickle.dumps()`方法把任意对象序列化成一个`bytes`，然后，就可以把这个`bytes`写入文件。或者用另一个方法`pickle.dump()`直接把对象序列化后写入一个`file-like Object`：
```Python
f = open('dump.txt', 'wb')
pickle.dump(d, f)
f.close()
```
当我们要把对象从磁盘读到内存时，可以先把内容读到一个`bytes`，然后用`pickle.loads()`方法反序列化出对象，也可以直接用`pickle.load()`方法从一个`file-like Object`中直接反序列化出对象。我们打开另一个Python命令行来反序列化刚才保存的对象：
```Python
f = open('dump.txt', 'rb')
d = pickle.load(f)
f.close()
d
{'age': 20, 'score': 88, 'name': 'Bob'}
```
```Python
import json
d = dict(name='Bob', age=20, score=88)
json.dumps(d)
'{"age": 20, "score": 88, "name": "Bob"}'
```
`dumps()`方法返回一个`str`，内容就是标准的`JSON`。类似的，`dump()`方法可以直接把`JSON`写入一个`file-like Object`。

要把`JSON`反序列化为`Python`对象，用`loads()`或者对应的`load()`方法，前者把`JSON`的字符串反序列化，后者从`file-like Object`中读取字符串并反序列化：
```Python
 json_str = '{"age": 20, "score": 88, "name": "Bob"}'
>>> json.loads(json_str)
{'age': 20, 'score': 88, 'name': 'Bob'}
```
`Python`的`dict`对象可以直接序列化为`JSON`的`{}`，不过，很多时候，我们更喜欢用`class`表示对象，比如定义`Student`类，然后序列化：
```Python
import json
class Student(object):
    def __init__(self, name, age, score):
        self.name = name
        self.age = age
        self.score = score
s = Student('Bob', 20, 88)
print(json.dumps(s))
```
运行代码，毫不留情地得到一个`TypeError`：
```Python
Traceback (most recent call last):
  ...
TypeError: <__main__.Student object at 0x10603cc50> is not JSON serializable
```
错误的原因是`Student`对象不是一个可序列化为`JSON`的对象。
前面的代码之所以无法把`Student`类实例序列化为`JSON`，是因为默认情况下，`dumps()`方法不知道如何将`Student`实例变为一个`JSON`的`{}`对象。
可选参数`default`就是把任意一个对象变成一个可序列为`JSON`的对象，我们只需要为`Student`专门写一个转换函数，再把函数传进去即可：
```Python
def student2dict(std):
    return {
        'name': std.name,
        'age': std.age,
        'score': std.score
    }
```
这样，`Student`实例首先被`student2dict()`函数转换成`dict`，然后再被顺利序列化为`JSON`：
```Python
print(json.dumps(s, default=student2dict))
{"age": 20, "name": "Bob", "score": 88}
```
不过，下次如果遇到一个`Teacher`类的实例，照样无法序列化为`JSON`。我们可以偷个懒，把任意`class`的实例变为`dict`：
```Python
print(json.dumps(s, default=lambda obj: obj.__dict__))
```
因为通常`class`的实例都有一个`__dict__`属性，它就是一个`dict`，用来存储实例变量。也有少数例外，比如定义了`__slots__`的`class`。
同样的道理，如果我们要把`JSON`反序列化为一个`Student`对象实例，`loads()`方法首先转换出一个`dict`对象，然后，我们传入的`object_hook`函数负责把`dict`转换为`Student`实例：
```Python
def dict2student(d):
    return Student(d['name'], d['age'], d['score'])
```
运行结果如下：
```Python
json_str = '{"age": 20, "score": 88, "name": "Bob"}'
print(json.loads(json_str, object_hook=dict2student))
<__main__.Student object at 0x10cd3c190>
```
打印出的是反序列化的`Student`实例对象。
## 标准库
### `collections`
#### `namedtuple`
```Python
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
```Python
from collections import deque
q = deque(['a', 'b', 'c'])
q.append('x')
q.appendleft('y')
q
deque(['y', 'a', 'b', 'c', 'x'])
# 清除所有元素
q.clear()
# 计算x的个数
q.count(x)
#移除找到的第一个 value。
q.remove(value)
#逆序排列
q.reverse()
```
`deque`除了实现`list`的`append()`和`pop()`外，还支持`appendleft()`和`popleft()`，这样就可以非常高效地往头部添加或删除元素。
#### `defaultdict`
使用`dict`时，如果引用的`Key`不存在，就会抛出`KeyError`。如果希望`key`不存在时，返回一个默认值，就可以用`defaultdict`：
```Python
from collections import defaultdict
dd = defaultdict(lambda: 'N/A')
dd['key1'] = 'abc'
dd['key1'] # key1存在
'abc'
dd['key2'] # key2不存在，返回默认值
'N/A'
```
```Python
from collections import deque

dlist=deque([1,'a'])
dlist.append('b') # 在末尾加数据
dlist.appendleft(0) # 在最前端插入数据
print(dlist)
# 输出 :  deque([0, 1, 'a', 'b'])

dlist.pop() # 删除末尾的数据
dlist.popleft() # 删除最前端的数据
print(dlist)
# 输出 :  deque([1, 'a'])

dlist.extend(['b','c']) # 在末尾追加list 数据
dlist.extendleft([-1,0])# 在前端插入list 数据
print(dlist)
# 输出 : deque([0, -1, 1, 'a', 'b', 'c'])

print(dlist.index('a')) # 找出 a 的索引位置
# 输出 :  3

dlist.insert(2, 555) # 在索引2 的位置插入555
print(dlist)
# 输出 :  deque([0, -1, 555, 1, 'a', 'b', 'c'])

print(dlist.count('a')) # 查找 ‘a’ 的数量

dlist.remove(1) # 删除第一个匹配值
dlist.reverse()  # 反向
print(dlist)
# 输出 :  deque(['c', 'b', 'a', 555, -1, 0])


dlist.rotate(-2) # 将左端的元素移动到右端
print(dlist)
# 输出 :  deque(['a', 555, -1, 0, 'c', 'b'])

dlist.rotate(2) # 将右端的元素移动到左端
print(dlist)
# 输出 :  deque(['c', 'b', 'a', 555, -1, 0])

dl1=dlist # 赋值 dlist 值变化，dl1的值也会修改
dl2=dlist.copy() # 拷贝 dlist, 拷贝后对dl修改不影响dlist的值
dlist.pop() # 删除最后一个数据, dl1的值也被修改
print(dl1) # 输出： deque(['c', 'b', 'a', 555, -1])
print(dl2) # 输出： deque(['c', 'b', 'a', 555, -1, 0])
```
- 合并字典
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
默认值是调用函数返回的，而函数在创建`defaultdict`对象时传入。除了在`Key`不存在时返回默认值，`defaultdict`的其他行为跟`dict`是完全一样的。
#### `OrderedDict`
使用`dict`时，`Key`是无序的。在对`dict`做迭代时，我们无法确定`Key`的顺序。
如果要保持`Key`的顺序，可以用`OrderedDict`：
```Python
from collections import OrderedDict
d = dict([('a', 1), ('b', 2), ('c', 3)])
d # dict的Key是无序的
{'a': 1, 'c': 3, 'b': 2}
od = OrderedDict([('a', 1), ('b', 2), ('c', 3)])
od # OrderedDict的Key是有序的
OrderedDict([('a', 1), ('b', 2), ('c', 3)])
```
注意，`OrderedDict的Key`会按照插入的顺序排列，不是`Key`本身排序：
```Python
od = OrderedDict()
od['z'] = 1
od['y'] = 2
od['x'] = 3
list(od.keys()) # 按照插入的Key的顺序返回
['z', 'y', 'x']
```
`OrderedDict`可以实现一个`FIFO`（先进先出）的`dict`，当容量超出限制时，先删除最早添加的`Key`：
```Python
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
``` Python
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
$ Python3 use_chainmap.py 
color=red
user=guest
```
当传入命令行参数时，优先使用命令行参数：
```shell
$ Python3 use_chainmap.py -u bob
color=red
user=bob
```
同时传入命令行参数和环境变量，命令行参数的优先级较高：
```shell
$ user=admin color=green Python3 use_chainmap.py -u bob
color=green
user=bob
```
```Python
from collections import ChainMap
m1 = {'Type': 'admin', 'codeID': '00001'}
m2 = {'name': 'woodname','codeID': '00002'}
m = ChainMap(m1, m2)
print(m)
# 输出：
# ChainMap({'Type': 'admin', 'codeID': '00001'}, {'name': 'woodname', 'codeID': '00002'})
print(m.maps)
# 输出：[{'Type': 'admin', 'codeID': '00001'}, {'name': 'woodname', 'codeID': '00002'}]
for i in m.items():
    print(i)
# 输出：
# ('name', 'woodname')
# ('codeID', '00001')
# ('Type', 'admin')
print(m['name']) # 读取元素的值
print(m['codeID']) # 注意，当key重复时以最前一个为准
print(m.get('Type'))
# 输出：
# woodname
# 00001
# admin
# 新增map
m3 = {'data': '888'}
m=m.new_child(m3) # 将 m3 加入m
print(m)
# 输出：
# ChainMap({'data': '888'}, {'Type': 'admin', 'codeID': '00001'}, {'name': 'woodname', 'codeID': '00002'})
print(m.parents) # m 的父亲
# 输出：ChainMap({'Type': 'admin', 'codeID': '00001'}, {'name': 'woodname', 'codeID': '00002'})
print(m.parents.parents)
# 输出 ： ChainMap({'name': 'woodname', 'codeID': '00002'})
```
```Python
a = {'a': 'A', 'c': 'C'}
b = {'b': 'B', 'c': 'D'}
m = collections.ChainMap(a, b)
print('c = {}'.format(m['c']))
c = C
```
可以通过 `maps` 属性将结果以列表形式返回。由于列表是可变的，所以可以对这个列表重新排序，或者添加新的值。
```Python
a = {'a': 'A', 'c': 'C'}
b = {'b': 'B', 'c': 'D'}
m = collections.ChainMap(a, b)
print(m.maps)    # [{'a': 'A', 'c': 'C'}, {'b': 'B', 'c': 'D'}]
print('c = {}\n'.format(m['c']))    # c = C
# reverse the list
m.maps = list(reversed(m.maps))
print(m.maps)    # [{'b': 'B', 'c': 'D'}, {'a': 'A', 'c': 'C'}]
print('c = {}'.format(m['c']))    # c = D
```
`ChainMap` 不会给子映射创建一个单独的空间，所以对子映射修改时，结果也会反馈到 `ChainMap` 上。
```Python
a = {'a': 'A', 'c': 'C'}
b = {'b': 'B', 'c': 'D'}
m = collections.ChainMap(a, b)
print('Before: {}'.format(m['c']))    # Before: C
a['c'] = 'E'
print('After : {}'.format(m['c']))    # After : E
```
也可以通过 `ChainMap` 直接设置值，实际上只修改了第一个字典中的值。
```Python
a = {'a': 'A', 'c': 'C'}
b = {'b': 'B', 'c': 'D'}
m = collections.ChainMap(a, b)
print('Before:', m)    # Before: ChainMap({'a': 'A', 'c': 'C'}, {'b': 'B', 'c': 'D'})
m['c'] = 'E'
print('After :', m)    # After : ChainMap({'a': 'A', 'c': 'E'}, {'b': 'B', 'c': 'D'})
print('a:', a)    # a: {'a': 'A', 'c': 'E'}
```
`ChainMap`提供了一个简单的方法，用于在`maps`列表的前面创建一个新实例，这样做的好处是可以避免修改现有的底层数据结构。
```Python
a = {'a': 'A', 'c': 'C'}
b = {'b': 'B', 'c': 'D'}
m1 = collections.ChainMap(a, b)
m2 = m1.new_child()
print(m1)    # ChainMap({'a': 'A', 'c': 'C'}, {'b': 'B', 'c': 'D'})
print(m2)    # ChainMap({}, {'a': 'A', 'c': 'C'}, {'b': 'B', 'c': 'D'})
m2['c'] = 'E'
print(m1)    # ChainMap({'a': 'A', 'c': 'C'}, {'b': 'B', 'c': 'D'})
print(m2)    # ChainMap({'c': 'E'}, {'a': 'A', 'c': 'C'}, {'b': 'B', 'c': 'D'})
```
这种堆叠行为使得将`ChainMap` 实例用作模板或应用程序上下文变得非常方便。具体来说，在一次迭代中很容易添加或更新值，然后丢弃下一次迭代的更改。
对于新上下文已知或预先构建的情况，也可以将映射传递给`new_child()`。
```Python
a = {'a': 'A', 'c': 'C'}
b = {'b': 'B', 'c': 'D'}
c = {'c': 'E'}
m1 = collections.ChainMap(a, b)
m2 = m1.new_child(c)
print('m1["c"] = {}'.format(m1['c']))    # m1["c"] = C
print('m2["c"] = {}'.format(m2['c']))    # m2["c"] = E
```
这相当于：
```Python
m2 = collections.ChainMap(c, *m1.maps)
```
#### `Counter`
`Counter`是一个简单的计数器，例如，统计字符出现的个数：
```Python
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
- `elements()`:返回一个迭代器，其中每个元素将重复出现计数值所指定次。 元素会按首次出现的顺序返回。 如果一个元素的计数值小于一，`elements()` 将会忽略它。
```Python
c = Counter(a=4, b=2, c=0, d=-2)
sorted(c.elements())
['a', 'a', 'a', 'a', 'b', 'b']
```
- `most_common([n])`:返回一个列表，其中包含`n` 个最常见的元素及出现次数，按常见程度由高到低排序。 如果 `n` 被省略或为 `None`，`most_common()` 将返回计数器中的 所有 元素。 计数值相等的元素按首次出现的顺序排序：
```Python
Counter('abracadabra').most_common(3)
[('a', 5), ('b', 2), ('r', 2)]
```
```Python
c = Counter(a=3, b=1)
d = Counter(a=1, b=2)
c + d                       # add two counters together:  c[x] + d[x]
Counter({'a': 4, 'b': 3})
c - d                       # subtract (keeping only positive counts)
Counter({'a': 2})
c & d                       # intersection:  min(c[x], d[x]) 
Counter({'a': 1, 'b': 1})
c | d                       # union:  max(c[x], d[x])
Counter({'a': 3, 'b': 2})
```
`Counter` 支持三种初始化形式：
```Python
print(collections.Counter(['a', 'b', 'c', 'a', 'b', 'b']))
print(collections.Counter({'a': 2, 'b': 3, 'c': 1}))
print(collections.Counter(a=2, b=3, c=1))
# output
# Counter({'b': 3, 'a': 2, 'c': 1})
# Counter({'b': 3, 'a': 2, 'c': 1})
# Counter({'b': 3, 'a': 2, 'c': 1})
```
`Counter` 初始化时也可以不传参数，然后通过`update()`方法更新。
```Python
c = collections.Counter()
print('Initial :', c)    # Initial : Counter()
c.update('abcdaab')
print('Sequence:', c)    # Sequence: Counter({'a': 3, 'b': 2, 'c': 1, 'd': 1})
c.update({'a': 1, 'd': 5})
print('Dict    :', c)    # Dict    : Counter({'d': 6, 'a': 4, 'b': 2, 'c': 1})
```
计数值基于新数据而不是替换而增加。在上例中，计数`a`从3到 4。
`Counter` 中的值，可以使用字典 `API` 获取它的值。
```Python
c = collections.Counter('abcdaab')
for letter in 'abcde':
    print('{} : {}'.format(letter, c[letter]))
# output
# a : 3
# b : 2
# c : 1
# d : 1
# e : 0
```
对于 `Counter` 中没有的键，不会报 `KeyError`。如本例中的 `e`，将其计数为0。
`elements()`方法返回一个迭代器，遍历它可以获得 `Counter` 中的值。
```Python
c = collections.Counter('extremely')
c['z'] = 0
print(c)    # Counter({'e': 3, 'x': 1, 't': 1, 'r': 1, 'm': 1, 'l': 1, 'y': 1, 'z': 0})
print(list(c.elements()))    # ['e', 'e', 'e', 'x', 't', 'r', 'm', 'l', 'y']
```
不保证元素的顺序，并且不包括计数小于或等于零的值。
使用`most_common()`产生序列最常遇到的输入值和它们各自的计数。
```Python
c = collections.Counter()
with open('/usr/share/dict/words', 'rt') as f:
    for line in f:
        c.update(line.rstrip().lower())
print('Most common:')
for letter, count in c.most_common(3):
    print('{}: {:>7}'.format(letter, count))

# output
# Most common:
# e:  235331
# i:  201032
# a:  199554
```
此示例计算在系统字典所有单词中的字母生成频率分布，然后打印三个最常见的字母。如果没有参数的话，会按频率顺序生成所有项目的列表。
`Counter`实例支持算术和聚合结果。这个例子显示了标准的操作符计算新的`Counter`实例，就地操作符 `+=`，`-=`，`&=`，和`|=`也支持。
```Python
c1 = collections.Counter(['a', 'b', 'c', 'a', 'b', 'b'])
c2 = collections.Counter('alphabet')

print('C1:', c1)
print('C2:', c2)

print('\nCombined counts:')
print(c1 + c2)

print('\nSubtraction:')
print(c1 - c2)

print('\nIntersection (taking positive minimums):')
print(c1 & c2)

print('\nUnion (taking maximums):')
print(c1 | c2)

# output
# C1: Counter({'b': 3, 'a': 2, 'c': 1})
# C2: Counter({'a': 2, 'l': 1, 'p': 1, 'h': 1, 'b': 1, 'e': 1, 't': 1})
# 
# Combined counts:
# Counter({'a': 4, 'b': 4, 'c': 1, 'l': 1, 'p': 1, 'h': 1, 'e': 1, 't': 1})
# 
# Subtraction:
# Counter({'b': 2, 'c': 1})
# 
# Intersection (taking positive minimums):
# Counter({'a': 2, 'b': 1})
# 
# Union (taking maximums):
# Counter({'b': 3, 'a': 2, 'c': 1, 'l': 1, 'p': 1, 'h': 1, 'e': 1, 't': 1})
```
每次`Counter`通过操作产生新的时，任何具有零或负计数的项目都将被丢弃。计数`a`在`c1`和`c2`中是相同的，因此相减之后变为零。
### `heapq`
这个模块提供了堆队列算法的实现，也称为优先队列算法。
堆是一个二叉树，它的每个父节点的值都只会小于或大于所有孩子节点（的值）。它使用了数组来实现：从零开始计数，对于所有的 `k` ，都有 `heap[k]` <= `heap[2*k+1]` 和 `heap[k] <= heap[2*k+2]`。 为了便于比较，不存在的元素被认为是无限大。 堆最有趣的特性在于最小的元素总是在根结点：`heap[0]`。
这个`API`与教材的堆算法实现有所不同，具体区别有两方面：
1. 我们使用了从零开始的索引。这使得节点和其孩子节点索引之间的关系不太直观但更加适合，因为 `Python` 使用从零开始的索引。 
2. 我们的 `pop` 方法返回最小的项而不是最大的项（这在教材中称为“最小堆”；而“最大堆”在教材中更为常见，因为它更适用于原地排序）。但是可以通过取反来实现最大堆。
要创建一个堆，可以使用`list`来初始化为 `[]` ，或者你可以通过一个函数 `heapify()` ，来把一个`list`转换成堆。
- `heapq.heappush(heap, item)`:将 `item` 的值加入 `heap` 中，保持堆的不变性。
- `heapq.heappop(heap)`:弹出并返回 `heap` 的最小的元素，保持堆的不变性。如果堆为空，抛出 `IndexError` 。使用 `heap[0]` ，可以只访问最小的元素而不弹出它。
- `heapq.heappushpop(heap, item)`:将 `item` 放入堆中，然后弹出并返回 `heap` 的最小元素。该组合操作比先调用  `heappush()` 再调用 `heappop()` 运行起来更有效率。
- `heapq.heapify(x)`:将`list x` 转换成堆，原地，线性时间内。
- `heapq.heapreplace(heap, item)`:弹出并返回 `heap` 中最小的一项，同时推入新的 `item`。 堆的大小不变。 如果堆为空则引发 `IndexError`。
这个单步骤操作比 `heappop()` 加 `heappush()` 更高效，并且在使用固定大小的堆时更为适宜。 `pop/push` 组合总是会从堆中返回一个元素并将其替换为 `item`。
返回的值可能会比添加的 `item` 更大。 如果不希望如此，可考虑改用 `heappushpop()`。 它的 `push/pop` 组合会返回两个值中较小的一个，将较大的值留在堆中。
-`heapq.merge(*iterables, key=None, reverse=False)`
将多个已排序的输入合并为一个已排序的输出。返回已排序值的`iterator`。
类似于 `sorted(itertools.chain(*iterables))` 但返回一个可迭代对象，不会一次性地将数据全部放入内存，并假定每个输入流都是已排序的（从小到大）。
- `heapq.nlargest(n, iterable, key=None)`:从 `iterable` 所定义的数据集中返回前 `n`个最大元素组成的列表。 如果提供了 `key` 则其应指定一个单参数的函数，用于从 `iterable` 的每个元素中提取比较键 (例如 `key=str.lower`)。 等价于: `sorted(iterable, key=key, reverse=True)[:n]`。
- `heapq.nsmallest(n, iterable, key=None)`:从 `iterable` 所定义的数据集中返回前 `n` 个最小元素组成的列表。 如果提供了 `key` 则其应指定一个单参数的函数，用于从 `iterable` 的每个元素中提取比较键 (例如 `key=str.lower`)。 等价于: `sorted(iterable, key=key)[:n]`。
两个函数在 `n` 值较小时性能最好。 对于更大的值，使用 `sorted()` 函数会更有效率。 此外，当 `n==1` 时，使用内置的 `min()` 和 `max()` 函数会更有效率。 如果需要重复使用这些函数，请考虑将可迭代对象转为真正的堆。
堆排序:
堆排序 可以通过将所有值推入堆中然后每次弹出一个最小值项来实现。
```Python
def heapsort(iterable):
    h = []
    for value in iterable:
        heappush(h, value)
    return [heappop(h) for i in range(len(h))]
heapsort([1, 3, 5, 7, 9, 2, 4, 6, 8, 0])
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```
如果我们想要`heapq`排序的是一个对象。那么heapq并不知道应该依据对象当中的哪个参数来作为排序的衡量标准，所以这个时候，需要我们自己定义一个获取关键字的函数，传递给`heapq`，这样才可以完成排序。
比如说，我们现在有一批电脑，我们希望`heapq`能够根据电脑的价格排序：
```Python
laptops = [
    {'name': 'ThinkPad', 'amount': 100, 'price': 91.1},
    {'name': 'Mac', 'amount': 50, 'price': 543.22},
    {'name': 'Surface', 'amount': 200, 'price': 21.09},
    {'name': 'Alienware', 'amount': 35, 'price': 31.75},
    {'name': 'Lenovo', 'amount': 45, 'price': 16.35},
    {'name': 'Huawei', 'amount': 75, 'price': 115.65}
]
cheap = heapq.nsmallest(3, portfolio, key=lambda s: s['price'])
expensive = heapq.nlargest(3, portfolio, key=lambda s: s['price'])
```
在调用`nlargest`和`nsmallest`的时候，我们额外传递了一个参数`key`，我们传入的是一个匿名函数，它返回的结果是这个对象的`price`，也就是说我们希望`heapq`根据对象的`price`来进行排序。
这类似于 `sorted(iterable)`，但与 `sorted()` 不同的是这个实现是不稳定的。
优先队列 是堆的常用场合，并且它的实现包含了多个挑战：
1. 排序稳定性：你该如何令相同优先级的两个任务按它们最初被加入时的顺序返回？
2. 如果优先级相同且任务没有默认比较顺序，则 (priority, task) 对的元组比较将会中断。
针对前两项挑战的一种解决方案是将条目保存为包含优先级、条目计数和任务对象 3 个元素的列表。 条目计数可用来打破平局，这样具有相同优先级的任务将按它们的添加顺序返回。 并且由于没有哪两个条目计数是相同的，元组比较将永远不会直接比较两个任务。
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
### `bisect`
- `bisect.bisect_left(a, x, lo=0, hi=len(a))`
在 `a` 中找到 `x` 合适的插入点以维持有序。参数 `lo` 和 `hi` 可以被用于确定需要考虑的子集；默认情况下整个列表都会被使用。如果 `x` 已经在 `a` 里存在，那么插入点会在已存在元素之前（也就是左边）。如果 `a` 是列表（`list`）的话，返回值是可以被放在 `list.insert()` 的第一个参数的。
返回的插入点 `i` 可以将数组 `a` 分成两部分。左侧是 `all(val < x for val in a[lo:i]) `，右侧是 `all(val >= x for val in a[i:hi])` 。
- `bisect.bisect_right(a, x, lo=0, hi=len(a))`
`bisect.bisect(a, x, lo=0, hi=len(a))`
类似于 `bisect_left()`，但是返回的插入点是 `a` 中已存在元素 `x` 的右侧。
返回的插入点 `i` 可以将数组 `a` 分成两部分。左侧是 `all(val <= x for val in a[lo:i])`，右侧是 `all(val > x for val in a[i:hi]) for the right side`。
- `bisect.insort_left(a, x, lo=0, hi=len(a))`
将 `x` 插入到一个有序序列 `a` 里，并维持其有序。如果 `a` 有序的话，这相当于 `a.insert(bisect.bisect_left(a, x, lo, hi), x)`。要注意搜索是 `O(log n)` 的，插入却是 `O(n)` 的。
- `bisect.insort_right(a, x, lo=0, hi=len(a))`
`bisect.insort(a, x, lo=0, hi=len(a))`
类似于 `insort_left()`，但是把 `x` 插入到 `a` 中已存在元素 `x `的右侧。
函数 `bisect()` 还可以用于数字表查询。这个例子是使用 `bisect()` 从一个给定的考试成绩集合里，通过一个有序数字表，查出其对应的字母等级：`90` 分及以上是 `'A'`，`80` 到 `89` 是 `'B'`，以此类推
```Python
def grade(score, breakpoints=[60, 70, 80, 90], grades='FDCBA'):
    i = bisect(breakpoints, score)
    return grades[i]
[grade(score) for score in [33, 99, 77, 70, 89, 90, 100]]
['F', 'A', 'C', 'C', 'B', 'A', 'A']
```
### `itertools`
- `itertools.count(start=0,step=1)`:创建一个迭代器，生成从 `n` 开始的连续整数，如果忽略 `n`，则从 `0` 开始计算。
```Python
for n in itertools.count():
    if 100000 < n < 100010:
        print n
    if n > 1000000:
        break
100001
100002
100003
100004
100005
100006
100007
100008
100009
```
- `itertools.cycle(iterable)`:把传入的一个序列无限重复下去。
```Python
for c in itertools.cycle("AB"):
    if count > 4:
        break
    print c
   count += 1     
A
B
A
B
A
```
- `itertools.repeat(object [,times])`:创建一个迭代器，重复生成 `object`，`times`（如果已提供）指定重复计数，如果未提供 `times`，将无止尽返回该对象。
```Python
for x in itertools.repeat("hello world", 5):
    print x    
hello world
hello world
hello world
hello world
hello world
```
- `itertools.chain(*iterables)`:把一组迭代对象串联起来，形成一个更大的迭代器。
```Python
for c in itertools.chain('ABC', 'XYZ'):
    print c    
A
B
C
X
Y
Z
```
- `itertools.permutations(iterable[, r])：返回 `iterable` 中任意取 `r` 个元素做排列的元组的迭代器，如果不指定 `r`，那么序列的长度与 `iterable` 中的项目数量相同。
```Python
for elem in itertools.permutations('abc', 2):
    print elem
('a', 'b')
('a', 'c')
('b', 'a')
('b', 'c')
('c', 'a')
('c', 'b')
for elem in itertools.permutations('abc'):
    print elem
('a', 'b', 'c')
('a', 'c', 'b')
('b', 'a', 'c')
('b', 'c', 'a')
('c', 'a', 'b')
('c', 'b', 'a')
```
- `itertools.combinations(iterable, r)`:组合，如果 `iterable` 为 `"abc"`，`r` 为 2 时，`ab` 和 `ba` 则视为重复，此时只放回 `ab`. 示例：
```Python
for elem in itertools.combinations('abc', 2):
    print elem   
('a', 'b')
('a', 'c')
('b', 'c')
```
- `itertools.combinations_with_replacement(iterable, r)`:与 `combinations` 类似，但允许重复值，即如果 `iterable` 为 `"abc"`，`r` 为 2 时，会多出 `aa`, `bb`, `cc`。
- `itertools.compress(data, selectors)`:
相当于 `bool` 选取，只有当 `selectors` 对应位置的元素为 `true` 时，才保留 `data` 中相应位置的元素，否则去除。
```Python
list(itertools.compress('abcdef', [1, 1, 0, 1, 0, 1]))
['a', 'b', 'd', 'f']
list(itertools.compress('abcdef', [True, False, True]))
['a', 'c']
```
- `itertools.groupby(iterable[, keyfunc])`:对`iterable`中的元素进行分组。`keyfunc` 是分组函数，用于对 `iterable` 的连续项进行分组，如果不指定，则默认对 `iterable` 中的连续相同项进行分组，返回一个 `(key, sub-iterator)` 的迭代器。
```Python
for key, value_iter in itertools.groupby('aaabbbaaccd'):
    print key, list(value_iter)  
a ['a', 'a', 'a']
b ['b', 'b', 'b']
a ['a', 'a']
c ['c', 'c']
d ['d']
data = ['a', 'bb', 'cc', 'ddd', 'eee', 'f']
for key, value_iter in itertools.groupby(data, len):
    print key, list(value_iter)   
1 ['a']
2 ['bb', 'cc']
3 ['ddd', 'eee']
1 ['f']
```
注意，注意，注意：必须先排序后才能分组，因为`groupby`是通过比较相邻元素来分组的。可以看第二个例子，因为 `a` 和`f` 没有排在一起，所以最后没有分组到同一个列表中。
- `itertools.islice(iterable, [start,] stop [, step])`:切片选择，`start` 是开始索引，`stop` 是结束索引，`step` 是步长，`start`和 `step` 可选。
```Python
list(itertools.islice([10, 6, 2, 8, 1, 3, 9], 5))
[10, 6, 2, 8, 1]
list(itertools.islice(itertools.count(), 6))
[0, 1, 2, 3, 4, 5]
list(itertools.islice(itertools.count(), 3, 10))
[3, 4, 5, 6, 7, 8, 9]
list(itertools.islice(itertools.count(), 3, 10, 2))
[3, 5, 7, 9]
```
- `itertools.tee(iterable, n=2)`:从 `iterable` 创建 `n` 个独立的迭代器，以元组的形式返回。
```Python
itertools.tee("abcedf")
(<itertools.tee at 0x7fed7b8f59e0>, <itertools.tee at 0x7fed7b8f56c8>)
iter1, iter2 = itertools.tee("abcedf")
list(iter1)
['a', 'b', 'c', 'e', 'd', 'f']
list(iter2)
['a', 'b', 'c', 'e', 'd', 'f']
```
- `itertools.product(*iterables, repeat=1)`:
大致相当于生成器表达式中的嵌套循环。例如， `product(A, B)` 和 `((x,y) for x in A for y in B)` 返回结果一样。
嵌套循环像里程表那样循环变动，每次迭代时将最右侧的元素向后迭代。这种模式形成了一种字典序，因此如果输入的可迭代对象是已排序的，笛卡尔积元组依次序发出。
要计算可迭代对象自身的笛卡尔积，将可选参数 `repeat` 设定为要重复的次数。例如，`product(A, repeat=4)` 和 `product(A, A, A, A)` 是一样的。
- `itertools.dropwhile`
```Python
 x = itertools.dropwhile(lambda e: e < 5, range(10))
print(list(x))
[5, 6, 7, 8, 9]
```
- `itertools.filterfalse`:
保留对应真值为`False`的元素
```Python
x = itertools.filterfalse(lambda e: e < 5, (1, 5, 3, 6, 9, 4))
print(list(x))
[5, 6, 9]
```
### `math`
- `math.ceil(x)`:返回 `x` 的上限，即大于或者等于 `x` 的最小整数。
- `math.comb(n, k)`:返回不重复且无顺序地从 `n` 项中选择 `k` 项的方式总数。
- `math.fabs(x)`:返回 `x` 的绝对值。
- `math.floor(x)`:返回 `x` 的向下取整，小于或等于 `x` 的最大整数。
- `math.gcd(a, b)`:最大公约数
- `math.exp(x)`:返回 `e` 次 `x` 幂
- `math.pow(x, y)`:将返回 `x` 的 `y` 次幂。
- `math.sqrt(x)`:返回 `x` 的平方根。
- `math.pi`:数学常数 π = 3.141592...，精确到可用精度。
- `math.e`:数学常数 e = 2.718281...，精确到可用精度。
- `math.inf`:浮点正无穷大。 
### `time`
在 `Python` 中，用三种方式来表示时间，分别是时间戳、格式化时间字符串和结构化时间
- 时间戳（`timestamp`）：也就是 1970 年 1 月 1 日之后的秒，例如 1506388236.216345，可以通过`time.time()`获得。时间戳是一个浮点数，可以进行加减运算，但请注意不要让结果超出取值范围。
- 格式化的时间字符串（`string_time`）：也就是年月日时分秒这样的我们常见的时间字符串，例如2017-09-26 09:12:48，可以通过`time.strftime('%Y-%m-%d')`获得;
结构化时间（`struct_time`）：一个包含了年月日时分秒的多元元组，例如`time.struct_time(tm_year=2017, tm_mon=9, tm_mday=26, tm_hour=9, tm_min=14, tm_sec=50, tm_wday=1, tm_yday=269, tm_isdst=0)`，可以通过`time.localtime()`获得。
- 利用`time.strftime('%Y-%m-%d %H:%M:%S')`等方法可以获得一个格式化时间字符串。
```Python
time.strftime('%Y-%m-%d %H:%M:%S')
'2017-09-26 10:04:28'
```
`time.strptime(string[,format])`
将格式化时间字符串转化成结构化时间。该方法是`time.strftime()`方法的逆操作。`time.strptime()`方法根据指定的格式把一个时间字符串解析为时间元组。要注意的是，你提供的字符串要和 `format` 参数的格式一一对应，如果 `string` 中日期间使用`“-”`分隔，`format` 中也必须使用`“-”`分隔，时间中使用冒号`“:”`分隔，后面也必须使用冒号分隔，否则会报格式不匹配的错误。并且值也要在合法的区间范围内。
```Python
stime = "2017-09-26 12:11:30"
st = time.strptime(stime,"%Y-%m-%d %H:%M:%S")
```
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200321211914.png)
- `time.time()`:返回当前系统时间戳。时间戳可以做算术运算。
```Python
time.time()
1506391907.020303
该方法经常用于计算程序运行时间：
```Python
import time
def func():
    time.sleep(1.14)
    pass

t1 = time.time()
func()
t2 = time.time()
print(t2 - t1)
```
### `datetime`
```Python
from datetime import datetime, date, time
dt = datetime(2011, 10, 29, 20, 30, 21)
dt.day
29
dt.minute
30
```
- 根据`datetime`实例，你可以用`date`和`time`提取出各自的对象：
```Python
dt.date()
datetime.date(2011, 10, 29)
dt.time()
datetime.time(20, 30, 21)
```
- `strftime`方法可以将`datetime`格式化为字符串：
```Python
dt.strftime('%m/%d/%Y %H:%M')
'10/29/2011 20:30'
```
- `strptime`可以将字符串转换成`datetime`对象：
```Python
datetime.strptime('20091031', '%Y%m%d')
datetime.datetime(2009, 10, 31, 0, 0)
```
- 计算差值
```Python
now = date.today()
birthday = date(1964, 7, 31)
age = now - birthday
age.days
14368
```
- 格式化命令


![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200317190745.png)
## `os`
- `os.path.exists(path)`:路径存在则返回`True`,路径损坏返回`False`
- `os.path.join(path1[, path2[, ...]])`:把目录和文件名合成一个路径
- `os.system('mkdir today')`:命令行命令
- `os.environ`:`os.environ["CUDA_VISIBLE_DEVICES"] = "7"`
## `Path`
在过去，文件的路径是纯字符串，现在它会是一个`pathlib.Path`对象:
```Python
from pathlib import Path
p = Path('/home/ubuntu')
PosixPath('/home/ubuntu')
str(p)
'/home/ubuntu'
```
过去路径拼接最正确的方法是用`os.path.join`:
```Python
os.path.join('/', 'home', 'dongwm/code')
'/home/dongwm/code'
os.path.join('/home', 'dongwm/code')
'/home/dongwm/code'
现在可以用`pathlib.Path`提供的`joinpath`来拼接:
```Python
Path('/').joinpath('home', 'dongwm/code')
PosixPath('/home/dongwm/code')
但是更简单和方便的方法是用`/`运算符:
```Python
Path('/') / 'home' / 'dongwm/code'
PosixPath('/home/dongwm/code')
Path('/') / Path('home') / 'dongwm/code'
PosixPath('/home/dongwm/code')
'/' / Path('home') / 'dongwm/code'
PosixPath('/home/dongwm/code')
```
使用`Path`对象的`parents`属性可以拿到各级目录列表(索引值越大越接近`root`)，而`parent`就表示父级目录:
```Python
p = Path('/Users/dongweiming/test')
p.parents[0]
PosixPath('/Users/dongweiming')
p.parents[1]
PosixPath('/Users')
p.parents[2]
PosixPath('/')
p.parent
PosixPath('/Users/dongweiming')
p.parent.parent
PosixPath('/Users')
```
获得文件后缀名:
```Python
p = Path('/usr/local/etc/my.cnf')
p.suffix, p.stem
('.cnf', 'my')
```
当文件有多个后缀，可以用`suffixes`返回文件所有后缀列表:
```Python
Path('my.tar.bz2').suffixes
['.tar', '.bz2']
Path('my.tar').suffixes
['.tar']
Path('my').suffixes
[]
```
Python语言没有内置创建文件的方法(`linux`下的`touch`命令)，过去这么做:
```Python
with open('new.txt', 'a') as f:
```
现在可以直接用`Path`的`touch`方法:
```Python
Path('new.txt').touch()
```
`touch`接受`mode`参数，能够在创建时确认文件权限，还能通过`exist_ok`参数方式确认是否可以重复`touch`(默认可以重复创建，会更新文件的`mtime`)
- `filename.exists()`；路径是否存在
- 打开文件：
```Python
data_folder = Path("source_data/text_files/")
file_to_open = data_folder / "raw_data.txt"
print(file_to_open.read_text())
```
- 与`os`模块对比
![](https://raw.githubusercontent.com/bailingnan/PicGo/master/20200321202526.png)
## 第三方库
### `h5py`
h5py文件是存放两类对象的容器，数据集(`dataset`)和组(`group`)，`dataset`类似数组类的数据集合，和`numpy`的数组差不多。`group`是像文件夹一样的容器，它好比`Python`中的字典，有键(`key`)和值(`value`)。`group`中可以存放`dataset`或者其他的`group`。”键”就是组成员的名称，”值”就是组成员对象本身(组或者数据集)
```Python
import h5py
```
#### 创建
```Python
with h5py.File('test.h5','w') as f:
```
#### 读取
```Python
with h5py.File('test.h5','r') as f:
```
`h5py`文件就像一个 `Python` 字典，因此我们可以检查`key`,
```Python
list(f.keys())
['mydataset']
```
文件中有一个数据集，即`mydataset`:
```Python
dset = f['mydataset']
dset.shape
(100,)
dset.dtype
dtype('int32')
dset[...] = np.arange(100)
dset[0]
0
dset[10]
10
dset[0:100:10]
array([ 0, 10, 20, 30, 40, 50, 60, 70, 80, 90])
data = f['mydataset'][:]
```
#### 创建数据集：
```Python
d1=f.create_dataset("dset1", (20,), 'i')
for key in f.keys():
    print(key)
    print(f[key].name)
    print(f[key].shape)
    print(f[key].value)
dset1
/dset1
(20,)
[0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0]
```
```Python
dset3 = f.create_dataset('subgroup2/dataset_three', (10,), dtype='i')
```
#### 赋值
```Python
d1=f.create_dataset("dset1",(20,),'i')
d1[...]=np.arange(20)
#或者我们可以直接按照下面的方式创建数据集并赋值
f["dset2"]=np.arange(15)
for key in f.keys():
    print(f[key].name)
    print(f[key].value)
/dset1
[ 0  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19]
/dset2
[ 0  1  2  3  4  5  6  7  8  9 10 11 12 13 14]
```
如果我们有现成的`numpy`数组，那么可以在创建数据集的时候就赋值，这个时候就不必指定数据的类型和形状了，只需要把数组名传给参数`data`。
```Python
a=np.arange(20)
d1=f.create_dataset("dset1",data=a)
for key in f.keys():
    print(f[key].name)
    print(f[key].value)
/dset1
[ 0  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19]
```
#### 综合示例1
```Python
#分别创建dset1,dset2,dset3这三个数据集
a=np.arange(20)
d1=f.create_dataset("dset1",data=a)

d2=f.create_dataset("dset2",(3,4),'i')
d2[...]=np.arange(12).reshape((3,4))

f["dset3"]=np.arange(15)

for key in f.keys():
    print(f[key].name)
    print(f[key].value)
/dset1
[ 0  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19]
/dset2
[[ 0  1  2  3]
 [ 4  5  6  7]
 [ 8  9 10 11]]
/dset3
[ 0  1  2  3  4  5  6  7  8  9 10 11 12 13 14]
```
#### 创建`group`,需要首先以`append`模式打开文件
```Python
f = h5py.File('mydataset.hdf5', 'a')
grp = f.create_group("subgroup")
```
```Python
g1=f.create_group("bar")
#在bar这个组里面分别创建name为dset1,dset2的数据集并赋值。
g1["dset1"]=np.arange(10)
g1["dset2"]=np.arange(12).reshape((3,4))

for key in g1.keys():
    print(g1[key].name)
    print(g1[key].value)
/bar/dset1
[0 1 2 3 4 5 6 7 8 9]
/bar/dset2
[[ 0  1  2  3]
 [ 4  5  6  7]
 [ 8  9 10 11]]
```
注意观察数据集`dset1`和`dset2`的名字是不是有点和前面的不一样，如果是直接创建的数据集，不在任何组里面，那么它的名字就是`/+名字`，现在这两个数据集都在`bar`这个`group`(组)里面，名字就变成了`/bar+/`名字，是不是有点文件夹的感觉！继续看下面的代码，你会对`group`和`dataset`的关系进一步了解。
```Python
#创建组bar1,组bar2，数据集dset
g1=f.create_group("bar1")
g2=f.create_group("bar2")
d=f.create_dataset("dset",data=np.arange(10))

#在bar1组里面创建一个组car1和一个数据集dset1。
c1=g1.create_group("car1")
d1=g1.create_dataset("dset1",data=np.arange(10))

#在bar2组里面创建一个组car2和一个数据集dset2
c2=g2.create_group("car2")
d2=g2.create_dataset("dset2",data=np.arange(10))

#根目录下的组和数据集
for key in f.keys():
    print(f[key].name)
/bar1
/bar2
/dset

#bar1这个组下面的组和数据集
for key in g1.keys():
    print(g1[key].name)
/bar1/car1
/bar1/dset1
#bar2这个组下面的组和数据集
for key in g2.keys():
    print(g2[key].name)
/bar2/car2
/bar2/dset2
#顺便看下car1组和car2组下面都有什么，估计你都猜到了为空。
print(c1.keys())
print(c2.keys())
[]
[]
```
- 综合示例2
```Python
#遍历文件中的一级组
for group in f.keys():
    print (group)
    #根据一级组名获得其下面的组
    group_read = f[group]
    #遍历该一级组下面的子组
    for subgroup in group_read.keys():
        print subgroup     
        #根据一级组和二级组名获取其下面的dataset          
        dset_read = f[group+'/'+subgroup]                           
        #遍历该子组下所有的dataset
        for dset in dset_read.keys():
            #获取dataset数据
            dset1 = f[group+'/'+subgroup+'/'+dset]
            print dset1.name
            data = np.array(dset1)
            print data.shape
            x = data[...,0]
            y = data[...,1]        
```
#### `Pandas`对`h5py`的操作
##### 写出
- `path`：字符型输入，用于指定`h5`文件的名称（不在当前工作目录时需要带上完整路径信息）
- `mode`：用于指定`IO`操作的模式，与`Python`内建的`open()`中的参数一致，默认为`'a'`，即当指定文件已存在时不影响原有数据写入，指定文件不存在时则新建文件；`'r'`，只读模式；`'w'`，创建新文件（会覆盖同名旧文件）；`'r+'`，与`'a'`作用相似，但要求文件必须已经存在；
- `complevel`：`int`型，用于控制`h5`文件的压缩水平，取值范围在0-9之间，越大则文件的压缩程度越大，占用的空间越小，但相对应的在读取文件时需要付出更多解压缩的时间成本，默认为`0`，代表不压缩
创建一个`HDF5 IO`对象`store`：
```Python
store = pd.HDFStore('demo.h5')
s = pd.Series(np.random.randn(5), index=['a', 'b', 'c', 'd', 'e'])
df = pd.DataFrame(np.random.randn(8, 3),
                 columns=['A', 'B', 'C'])
store['s'],store['df'] = s,df
```
从`pandas`中的数据结构直接导出到本地`h5`文件中：
```Python
#创建新的数据框
df_ = pd.DataFrame(np.random.randn(5,5))
#导出到已存在的h5文件中，这里需要指定key
df_.to_hdf(path_or_buf='demo.h5',key='df_')
#创建于本地demo.h5进行IO连接的store对象
store = pd.HDFStore('demo.h5')
#查看指定h5对象中的所有键
print(store.keys())
```
利用store对象的`put()`方法，其主要参数如下：
- `key`：指定`h5`文件中待写入数据的`key`
- `value`：指定与`key`对应的待写入的数据
- `format`：字符型输入，用于指定写出的模式，`'fixed'`对应的模式速度快，但是不支持追加也不支持检索；`'table'`对应的模式以表格的模式写出，速度稍慢，但是支持直接通过`store`对象进行追加和表格查询操作
使用`put()`方法将数据存入`store`对象中：
```Python
store.put(key='s',value=s);store.put(key='df',value=df)
```
既然是键值对的格式，那么可以查看`store`的`items`属性（注意这里`store`对象只有`items`和`keys`属性，没有`values`属性）：
```Python
store.items
```
调用`store`对象中的数据直接用对应的键名来索引即可：
```Python
store['df']
```
删除`store`对象中指定数据的方法有两种，一是使用`remove()`方法，传入要删除数据对应的键：
```Python
store.remove('s')
print(store.keys())
```
二是使用`Python`中的关键词`del`来删除指定数据：
```Python
del store['s']
```
##### 读取
```Python
store = pd.HDFStore('demo.h5')
'''方式1'''
df1 = store['df']
'''方式2'''
df2 = store.get('df')
```
```Python
df = pd.read_hdf('demo.h5',key='df')
```
- 删除对象
```Python
del subgroup["MyDataset"]
```






