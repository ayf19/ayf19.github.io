---
title: Python 速成
date: 2021-07-20 17:07:30
tags:
  - Python
  - 暑培
categories: 文档
---

### 0. 前言

#### 0.0. 前前言

这份讲义参考了许多rls的内容，因为让我单独写一份完善的入门讲义还是很困难的，毕竟我自己使用的时候难免有时候会有些ugly的做法，而rls的这份讲义中有大量的东西是我后来才学会的，最近才发现原来早就讲过。

在此感谢rls去年为科协暑培所做的贡献，去年参加暑培使我收获很大~~（尤其是小学期）~~。

<!--more-->

#### 0.1. 面向群体

建议同时满足以下条件的同学听本课程：

- 至少有一门语言基础，比如C++
- 没有Python基础或接触Python时长小于一星期

#### 0.2. 课程目标

通过本课程，希望你能达成以下目标：

- 具备将C++或其他类型语言的逻辑代码（非底层操作）翻译成Python的能力
- 能够用Pycharm创建新环境、新项目并进行配置
- 认识到Python解释型、强类型、动态类型语言的特点（对比C是编译型、弱类型、静态类型）
- 能够自行更深入地学习Python

本节课**不包含**以下要素：

- 爬虫（但是下一节课应该会有）
- 可视化
- 人(ke)工(xue)智(ji)能(suan)
- 以及其他经常被用作Python广告但并非Python基础的内容（亲眼见过有小孩学的Python讲如何做植物大战僵尸，我：？）

#### 0.3. 交互式窗口vs脚本

作为一个解释型语言，Python不需要像C系语言一样编译运行，可以“写一行跑一行”，因此它可以分为交互式窗口运行与脚本运行两种模式。（类似的，在后面讲到JavaScript时也会遇到这个问题）

- 交互式窗口：如在命令行输入`python`，即可进入交互式窗口，此时如同“问答”一般，输入一行代码后Python立刻执行并给出返回值；
- 脚本：经典运行方法`python xxx.py`，Python会将整个脚本依次执行。

学习阶段建议先从交互式窗口入手，因此在讲Pycharm之前，我们先从命令行开始学习。后续附代码的部分，如果出现了顶格>>>的情况，则暗示这是一个交互式窗口，有>>>的是代码，其他的是执行代码的输出。

比较特别的一个点是，上文提到的“输出”，除非是`print`这样的明确输出函数，否则一般情况下它会将执行语句的返回值转化成表示串，这和`print`的效果或许不同，后面讲字符串部分时会举例子。

#### 0.4. Hello World

放到哪一节都不太合适，还是放到前言意思意思吧。在命令行输入`python`，进入Python的交互式界面，然后：

```python
>>> print('Hello, world!')
Hello, world!
```

![hello_world](./hello_world.png)

rls的讲义中这个地方还涉及到虚拟环境的问题，这是conda配置的问题，我把它放在补充资料的环境配置中。

细心的话你会注意到这行代码中没有分号，可以直接运行（无需编译），字符串用了单引号，这些都算是它与C/C++不同之处。

~~好，你已经入门了Python！~~

退出界面的方法是按`ctrl+D`或者执行`exit()`（`ipython`的问题也会在环境配置文档中说明）。~~当然你要直接关闭窗口我和rls都没得说。~~但是在Windows Powershell里 Python 3.9.5版本似乎不可以用`ctrl+D`退出，我没有找到相关的说明（我的另一个3.8.8的版本是可以的，然而这个版本在cmd里又不行，可见这是一个玄学的字符转义问题）。

#### 0.5. Python2警告

如果你此前使用过Python2或看过Python2的教程，那么希望你能对于它和本教程（Python3）的不同之处做好心理准备。Python2从2020年1月1日开始就不再被支持，因此本课程不会如同一些较老的教程一般强调Python2与Python3的差异（况且我和rls都真的不会Python2）。

### 1. 变量类型&运算

#### 1.1. 简易基本类型

最基本类型与C系语言相似：

- `int`：**变长**整数，默认是4字节，有需要时自动增长。（这里需要单独说明的是，Python会存储所有的-5到256的整数，其他任何变量是这些值时，会被指向这个预先开好的内存，因此任何两个值为5的`int`变量都指向同一内存地址，这一点的影响将在后面看到）。与C系语言不同的是，整除是`//`，普通除法`/`会得到`float`。除了十进制外还支持十六进制、八进制和二进制的表示。

```python
>>> 0x11
17
>>> 11
11
>>> 0o11
9
>>> 0b11
3
```

- `complex`：Python中自带了复数类型，表示为`real + imag*1j`的形式，注意虚部为1的时候不可以省略1，可以用`j`也可以用`J`。复数的实虚部分别为一个float。
- `float`：8字节浮点数，相当于C系语言的`double`。
- `bool`：即众所周知的`True`和`False`，注意首字母大写，用作数值计算时与C系一样视作1与0。
- `NoneType`：`None`，空值，后面会介绍到用处。常用于返回值、特判。

#### 1.2. 运算符

运算符可以重载，此处先不讲重载，但是列出常见运算符与重载函数，后面再讲重载的方法。

##### 1.2.1. 算术运算符

- `+`：`__add__`
- `-`：`__sub__`
- `*`：`__mul__`
- `/`：`__trudiv__`，真除
- `//`：`__floordiv__`，整除
- `%`：`__mod__`
- `**`：`__pow__`，幂运算

##### 1.2.2. 比较运算符

- `<`：`__lt__`
- `<=`：`__le__`
- `>`：`__gt__`
- `>=`：`__ge__`
- `==`：`__eq__`
- `!=`：`__ne__`

##### 1.2.3. 位运算符

- `&`：`__and__`
- `|`：`__or__`
- `^`：`__xor__`
- `~`：`__invert__`，注意这是一元运算符
- `<<`：`__lshift__`
- `>>`：`__rshift__`

##### 1.2.4. 赋值运算符

- `=`：赋值号，注意Python不能重载赋值号
- 自x赋值：如自加(`__iadd__`)、自减(`__isub__`)等，规则同C++

##### 1.2.5. 一元运算符

- `+`：`__pos__`
- `-`：`__neg__`

##### 1.2.6. 逻辑运算符

逻辑运算符也即布尔运算符，无法重载

- `and`
- `or`
- `not`

注意，对于`and`和`or`，得到的结果不会直接转换为`bool`，而是能够得出结果的最后一个变量，如：

```python
>>> 3 or 4
3
>>> 3 and 4
4
>>> 0 or 1 and 2
2
>>> 0 and 1 or 2
2
>>> 1 or 0 and 2
1
```

注意混用`and` `or`时需要考虑`and`的优先级更高，为了代码的可读性**建议不要混用**，或者显式地加上小括号。

##### 1.2.7. 三目运算符

三目运算符就一个

- `a if condition else b`：相当于C中的`condition ? a : b`

注意一下它的顺序，当然如果你没学过C/C++的话这个读起来还是很通顺的，可惜你学过了C/C++，那么请务必不要搞反了参数顺序。

##### 1.2.8. 其他运算符

下面运算符是C系没有的：

- `in`：`__contain__`，被包含于（见容器）
- `not in`：`in`的否定
- `is`：判断两个变量的地址是否相同，不可重载（不建议对临时变量使用，可能得到匪夷所思的结果）
- `is not`：判断两个变量地址是否不同

这里注意前面提到过-5到256的整数的问题，所以会得到以下结果

```python
>>> a = 256
>>> b = 256
>>> a is b
True
>>> c = 257
>>> d = 257
>>> c is d
False
```

这是由于分别构造的两个值相同的变量一般占据不同的内存空间，但-5到256的整数由于前面提过的原因，不会出现两次内存分配。

#### 1.3. 字符串与字节串

Python的一个便利点在于将字符串与字节串封装成了基本类型并处理了多种运算、引入了多种方法。

注意基本类型本身是不可修改的（注意区分赋值与修改），这与后面的容器不同（也与C系的`char *`不同）。

##### 1.3.1. 字符串

`str`即是Python中的字符串，与C系的`char *`的区别在于`str`类型的变量不可以修改（也即不可以修改某个位置的字符，只能重新构造整个字符串），且更重要的是它每个位置存的是一个unicode而非ascii。

此外，Python中没有单独的字符类型，单个字符将被视为长度为1的字符串。

构造字符串的方法是使用单引号或双引号，注意Python里单双引号构造字符串是没有任何差别的，可以根据个人习惯或实际场景使用，转义符的使用与C基本一致（尽管此处还没有讲到输出函数`print`，试着先接受这一点：`print(a)`是一个函数，它能打印变量`a`）：

```python
>>> print('你好世界')
你好世界
>>> print("你好世界")
你好世界
>>> print('"你好世界"')
"你好世界"
>>> print("'你好世界'")
'你好世界'
>>> print('\'你好世界\'')
'你好世界'
>>> print("\"你好世界\"")
"你好世界"
>>> '你好世界'
'你好世界'
>>> "你好世界"
'你好世界'
>>> '"你好世界"'
'"你好世界"'
>>> "'你好世界'"
"'你好世界'"
>>> '\'你好世界\''
"'你好世界'"
>>> "\"你好世界\""
'"你好世界"'
```

除此之外还有原始字符串（raw string）、跨行字符串等，可以自行了解。

此处说明几个重要函数：

- `len(obj)`：`__len__`，获取obj的参数，常用于获取字符串（注意是unicode，因此中文和英文字符一样长）、字节串或容器的长度
- `chr(i)`：将整型变量`i`转化成（长度为1的）的字符（串）
- `ord(c)`：获取（长度为1的）字符（串）的编号（Unicode）

```python
>>> len('你好世界')
4
>>> ord('你')
20320
>>> hex(ord('你'))
'0x4f60'
>>> chr(0x4f60)
'你'
>>> '\u4f60'
'你'
>>> ord('a')
97
>>> ord('你好')
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-28-b179c21de46c> in <module>
----> 1 ord('你好')

TypeError: ord() expected a character, but string of length 2 found
>>> chr(1234567)
---------------------------------------------------------------------------
ValueError                                Traceback (most recent call last)
<ipython-input-29-8215e040e74b> in <module>
----> 1 chr(1234567)

ValueError: chr() arg not in range(0x110000)
```

注意到a的Unicode就是它的ASCII码，这是符合期望的。另外如果函数输入有问题，则会有几行异常提示，最下方会说明异常原因~~（相信你高考英语阅读理解分数不错）~~

此外字符串也会支持一些运算符，通常来说这些运算符的含义是很符合人的直觉的~~（否则你可以去暴揍一顿重载这些运算符的人然后提pr改掉它们）~~，这里课上举几个例子即可，不单独列出。

此外字符串本身包含非常多的函数，下面编码一节会提到`str.encode`，其他函数后面会提及学习方法。在此重申字符串是不可修改的基本类型，因此字符串函数一定不会是原地修改，有需要的话会生成一个新的字符串（如`str.replace`）。

格式化字符串有多种模式，这里简单介绍一种最快也最符合直觉的方式（尽管我是后来才学会的所以并不习惯于这样做），即f-string：

```python
>>> name = 'SAST'
>>> date = '2021-07-19'
>>> print(f'today is {date} and {name} is teaching python.')
today is 2021-07-19 and SAST is teaching python.
```

f-string是由f开头的字符串，其中被`{}`包裹的表达式会被计算，也即在使用的时候才会生成

此外还有两种方式，列举如下，不再展开

C风格的格式化字符串（注意后面需要一个元组，元组的内容在后面讲到）：

```python
>>> name = 'SAST'
>>> date = '2021-07-19'
>>> print('today is %s and %s is teaching python.' % (date, name))
today is 2021-07-19 and SAST is teaching python.
```

format方法格式化字符串：

```python
>>> name = 'SAST'
>>> date = '2021-07-19'
>>> print('today is {} and {} is teaching python.'.format(date, name))
today is 2021-07-19 and SAST is teaching python.
```

以上几种方式都支持C风格的格式化，具体方法可以自行学习

##### 1.3.2. 字节串

`bytes`即是Python中的字节串，它表示最纯粹的“二进制”数据，非常像C里的`unsigned char *`，但是在显示上它仅支持ASCII显示，因此用肉眼看显得有些不伦不类，通常它只存在于数据的处理过程中。

`bytes`的构造与字符串类似，但是要加一个b做前导：

```python
>>> print(b'123')
b'123'
>>> print(b'\1')
b'\x01'
>>> print(b'\x41')
b'A'
>>> print(b'\x61\x62')
b'ab'
>>> print(len(b'\x61\x62'))
2
```

`bytes`本身的运算符显得并不重要，因为我们很少自己编辑`bytes`，真正重要的是它与`str`之间的转换以及随之而来的编码问题。

##### 1.3.3. 编码与转换

Unicode是通用的编码，它在世界范围内提供了统一的字符集编码。但是传输过程中裸传 Unicode 可能引起一些问题 （如鲁棒性问题、空间浪费等），因此有了对 Unicode 进行实现的编码，如 UTF-8、UTF-16、GBK 等等，它们统称为 Uncode 转换格式 (Unicode Transform Format）。对于我们日常见到的文件，一个汉字两个字节的常是 GBK 编码，一个汉宇三个字节的常是 UTF-8编码，部分同学曾经或将来在写C++时或许会处理这样的问题。

字符串本身是代表的 Unicode，但是在文件存储时常常存储为UTF-8或GBK编码，字节串本没有编码信息，需要人为指定或用一些算法去推导。另外GBK 是 GB2312 的扩展，有时候打开一些文件编辑器会提示编码方式是 GB2312，方便起见大家写代码时直接用 GBK 即可，下文同。

`str`自身有函数`str.encode`可以编码成`bytes`，而`bytes`也有函数`bytes.decode`可以解码成 `str`。（我所用的版本）Linux、Mac、Windows下默认是UTF-8，但如果我没记错的话有些Windows会用ANSI之类的编码，因此最好显式地写出编码格式。下面是几个例子（注：`＃`引导的是单行注释，和C中的`//`效果基本一样，但是`#`左边必须有空格符号隔开）：

```python
>>> 'Hello world'.encode()
b'Hello world'
>>> b'Hello world'.decode()
'Hello world'
>>> '你好'.encode()        # 在我的Mac上不加编码方式默认UTF-8，你可以试试其他操作系统默认编码方式是什么
b'\xe4\xbd\xa0\xe5\xa5\xbd'
>>> '你好'.encode('utf-8') # 相当于'你好'.encode(encoding='utf-8')
b'\xe4\xbd\xa0\xe5\xa5\xbd'
>>> '你好'.encode('gbk')
b'\xc4\xe3\xba\xc3'
>>> b'\xe4\xbd\xa0\xe5\xa5\xbd'.decode('gbk')
'浣犲ソ'                    # 这种情况俗称“中文乱码”
>>> b'\xe4\xbd\xa0\xe5\xa5\xbd'.decode('utf-8')
'你好'
>>> b'\xc4\xe3\xba\xc3'.decode('utf-8')  # 用gbk解utf-8或许是乱码，反过来则可能直接报错
---------------------------------------------------------------------------
UnicodeDecodeError                        Traceback (most recent call last)
<ipython-input-52-81396d56f1d0> in <module>
----> 1 b'\xc4\xe3\xba\xc3'.decode('utf-8')

UnicodeDecodeError: 'utf-8' codec can't decode byte 0xc4 in position 0: invalid continuation byte
>>> b'\xe4\xbd\xa0\xe5\xa5\xbd'.decode() # 不加编码方式默认为utf-8
'你好'
>>> b'\xcc\xcc\xcc\xcc\xcc\xcc\xcc\xcc\xcc\xcc'.decode('gbk') # ？
'烫烫烫烫烫'
```

#### 1.4. 容器

Python提供了一系列内置容器，它们如同C++的STL一样，不过比STL的用法灵活得多。

##### 1.4.0. 预备知识

为了便于学习这部分内容，先补充几个函数：

- `type(obj)`：可以获取参数obj的类型
- `isinstance(obj, class_or_tuple)`：可以判断obj是不是一个/些类或子类的实例
- `id(obj)`：获取obj的地址，`a is b`等价于`id(a) == id(b)`

```python
>>> type((1, 2, 3))
<class 'tuple'>
>>> type(int)
<class 'type'>
>>> isinstance(1, float)
False
>>> isinstance(1, (int, list))
True
>>> isinstance((1, 2, 3), (int, float, list))
False
>>> isinstance((1, 2, 3), (float, tuple))
True
```

##### 1.4.1. list

列表（list）是很常用的容器，常被看作Python中的数组，但一定要类比的话或许类比成向量比较好。

列表的构造是用中括号完成，按索引访问、修改数据与C中数组基本一样，如：

```python
>>> [1, [2], '3']
[1, [2], '3']
>>> a = [1, 2, 3]
>>> a[2]
3
>>> a[1] = 10
>>> a
[1, 10, 3]
>>> id(a)
139715443892352
>>> a.append(9)
>>> id(a)
139715443892352
>>> a
[1, 10, 3, 9]
>>> a = [1, 10, 3, 9]
>>> id(a)
139715443838848
```

与C不同的是，Python中的容器支持反向索引和切片的操作，如：

```python
>>> a = [1, 2, 3, 4]
>>> a[-1]
4
>>> a[-2]
3
>>> a[1: 3]    # 注意区间
[2, 3]
>>> a[1: 3: -1]
[3, 2]
>>> a[::2]
[1, 3]
```

列表有一系列的函数，如与栈、队列类似的接口，这部分内容自学，后面会提到如何学习。

这里强调一下注意 赋值 与 修改 的差别，id是有变化的，认识到这一点对于学习Python的引用特性颇有意义。

此外同字符串一样，列表也会支持一些运算符操作，同样自学。如果你需要构造空列表，直接写`[]`即可，或者`list()`有等价效果。

##### 1.4.2. tuple

元组（tuple）是一种常被新手忽略，但是高效有用的容器。它像是“列表常量”，用小括号生成，不可修改（弱意义上的）：

```python
>>> (1, (2, 3, ), 4)
(1, (2, 3), 4)
>>> a = (1, 2, 3)
>>> a[1]
2
>>> a[0] = 1
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-56-c8548dbc0a7e> in <module>
----> 1 a[0] = 1

TypeError: 'tuple' object does not support item assignment
```

元组的小括号有时候是可以省略的（省略规则自学~~，我也不知道怎么总结~~，总之就是不引起歧义），因此在Python中没有逗号运算符的说法，逗号会构造元组，也正因此可以写出一个经典的Python交换赋值代码：

```python
>>> a = 1
>>> b = 2
>>> a, b
(1, 2)
>>> a, b = b, a    # 这里发生了元组的拆包
>>> a
2
>>> b
1
```

另外元组本身不可修改，但是如果元组的元素是可以修改的，那么可以修改这些元素：

```python
>>> a = 1, [2]
>>> a[1].append(3) # a[1]是list的引用，因此修改list的时候并没有修改tuple中存储的内容
>>> a
(1, [2, 3])
```

注意，如果你希望构造一个单元素元组或者空元组，操作有点特殊：

```python
>>> (1)            # 这是括号里有个1，算完还是1
1
>>> (1,)           # 这是有单元素1的元组
(1,)
>>> ()             # 这是空元组
()
>>> tuple()        # 这也是空元组
()
>>> (,)            # 这是bug
  File "<ipython-input-62-d79fbbe8be60>", line 1
    (,)
     ^
SyntaxError: invalid syntax
```

##### 1.4.3. set

集合（set）对于一般程序员来说用得就更少了，但是对于我们而言应当认识到它的高效之处，至少从数学意义上讲集合由一组无序、互不重复的元素构成，这已经暗含了很多信息。集合一般用`{}`构造，或者直接对列表等类型进行转换，（至少在现在的Python版本里）集合内部用hash实现，不保序，核心函数是`set.add`与`set.remove`，操作实例如下：

```python
>>> {1, 2, 3}
{1, 2, 3}
>>> a = {1, 2, 3}
>>> a.add(4)
>>> a.remove(2)
>>> a
{1, 3, 4}
>>> 3 in a
True
>>> set([1, 1, 1, 2, 2, 3])
{1, 2, 3}
```

注意空集合的构造比较特殊，你只能使用`set()`，因为很不幸`{}`的表达被空字典占用了。

##### 1.4.4. dict

字典（dict）相信对于oier并不陌生~~（说的好像我是oier/oier用我教）~~，它是一个键值对（Key-Value）容器，用于存储一组有穷映射，常用`{key: value}`的形式表示，调用与C++ STL中的map类似，底层实现基于hash但是略复杂，可以自行学习。它的基本操作如下：

```python
>>> d = {'name': '张三', 'age': 18}
>>> d['name'] = '李四'
>>> d
{'name': '李四', 'age': 18}
>>> d['age'] += 1
>>> d
{'name': '李四', 'age': 19}
```

注意字典的键（包括集合的元素）必须是可以hash的，这通过对应类（型）的`__hash__`函数实现。而对于Python内置**可变容器**是没有实现该函数的，因此无法作为字典的键或者集合元素：

```python
>>> d[[1, 2, 3]] = 1
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-64-43aceb8d1e57> in <module>
----> 1 d[[1, 2, 3]] = 1

TypeError: unhashable type: 'list'
>>> {0, (1, 2, 3), {4, 5, 6}}
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-66-a8806a59920b> in <module>
----> 1 {0, (1, 2, 3), {4, 5, 6}}

TypeError: unhashable type: 'set'
>>> d[(1, 2, 3)] = {(4, 5, 6), (7, 8, 9)}    # tuple是可hash的类型，因此可以作为键
>>> d
{'name': '李四', 'age': 19, (1, 2, 3): {(7, 8, 9), (4, 5, 6)}}
```

##### 1.4.5. 补充

前面提到了元组解包的操作，但解包的作用远不止多个变量赋值，请自行学习~~（实际上我好像是在JS里面第一次了解这些）~~。

#### 1.5. 迭代器

##### 1.5.1. 可迭代对象

迭代器属于高级特性之一，前文提到的容器外加字符串、字节串都是可迭代的，它常常与生成器进行比较。通常来说我们自己写生成器、迭代器（造轮子）的情况比较少，这里重点讲它的应用，关于它们的构造可以自行根据文末资料学习。

实际上我们用到比较多的是可迭代对象，这意味着它们可以用作`for`循环、直接构造出`list`、`tuple`等，这将在生成器一节提到。

##### 1.5.2. range

`range`是一个特殊的类型，先让我们用一点简单的例子来见证`range`的作用：

```python
>>> list(range(10))
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
>>> [i for i in range(10)]
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

实际上这可能有一点点超纲，至少你可以看到`range`实例是可以转换成list的，此外`range`的参数与C的`for`循环颇有相似之处。这里介绍一个查看帮助的函数`help(obj)`。执行

```python
help(range)
```

你会看到

```
Help on class range in module builtins:

class range(object)
 |  range(stop) -> range object
 |  range(start, stop[, step]) -> range object
 |
 |  Return an object that produces a sequence of integers from start (inclusive)
 |  to stop (exclusive) by step.  range(i, j) produces i, i+1, i+2, ..., j-1.
 |  start defaults to 0, and stop is omitted!  range(4) produces 0, 1, 2, 3.
 |  These are exactly the valid indices for a list of 4 elements.
 |  When step is given, it specifies the increment (or decrement).
 |
 |  Methods defined here:
 |
 |  __bool__(self, /)
 |      self != 0
 |
 |  __contains__(self, key, /)
 |      Return key in self.
# 下略
```

如果你执行下述内容：

```python
>>> range(10)
range(0, 10)
>>> type(range(10))
<class 'range'>
```

你会发现`range`本身即是一个类型，它不是迭代器的子类，但它却是可以迭代、且**节省空间**（比如说对于巨大的`for`循环，真的生成一个很长的列表显然是错误的，此时`range`起到与C系`for`循环变量类似的作用）的存在。

#### 1.6. 生成式

使用生成式构造容器是非常常见、高效的操作，下面举几个例子：

```python
>>> a = list(range(10))                # 直接构造列表为基本列表
>>> [i ** 2 for i in a]                # 列表生成式构造平方数列表
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
>>> {x % 7 for x in a if x % 3 != 1}   # 集合生成式筛选出模3不余1的数模7后的集合
{0, 1, 2, 3, 5, 6}
>>> {x ** 2: x for x in a}             # 字典生成式构造平方数到原数的字典
{0: 0, 1: 1, 4: 2, 9: 3, 16: 4, 25: 5, 36: 6, 49: 7, 64: 8, 81: 9}
>>> (x + 1 for x in a)                 # 试图构造一个1到10的元组……哪里不太对？
<generator object <genexpr> at 0x104fc72e0>
>>> tuple(x + 1 for x in a)            # 这次构造元组成功了
(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
```

生成式的三段式：1. 表达式  2. for 循环变量 in 迭代对象 3. （可选）筛选条件

注意生成式中表达式是集合时不可省略小括号：

```python
>>> [x, x * x for x in a]
  File "<ipython-input-70-63780f60f420>", line 1
    [x, x * x for x in a]
              ^
SyntaxError: invalid syntax
>>> [(x, x * x) for x in a]
[(0, 0), (1, 1), (2, 4), (3, 9), (4, 16), (5, 25), (6, 36), (7, 49), (8, 64), (9, 81)]
```

当然，上面代码的前提条件是有了个a当跳板，实际上不必这么麻烦，直接使用迭代器即可。

试着将上述代码中的a替换成`range(10)`或者其他你喜欢的范围，并且试着修改左边的表达式、为右边添加筛选条件，看看结果与你想的是否一致。

### 2. 流程控制

#### 2.0. 代码块

代码块（code block）是个比较重要的层级概念，~~众所周知~~Python是no brace的，没有打括号进行显式地分块，因此它分块需要特别的手段：冒号配合缩进。

缩进的使用有一套很完善的规则，但我不准备在这里讲解（混用缩进既不符合工程规范也不能显得作者水平很高），在这里记住一个原则，缩进统一使用tab或者四个空格（如果你是双空格党，那是你的自由）作为一级，每当多一层层级嵌套时便会多一个tab/四个空格。

我们不妨先写出如下伪码（下划线表示空格，注意冒号是认真的）：

```python
定义一个函数：
____写点代码
____开始一个循环：
________循环体代码第一行
________循环体代码第二行
____这与“写点代码”同级
____这里也可以定义一个函数：
________函数体代码的唯一一行

这行代码与“定义一个函数”同级
```

老段子：据说Python程序员工作时都带着一把游标卡尺。

注意在交互式窗口运行（非IPython）时，写完一个代码块要多敲一个空行，否则会报错，例如：

```python
if 1 == 1:
    print(1)
print(2)
```

就是错误的，在工程文件中同样应该注意适当空行，虽然不是语法要求，但涉及到美观的问题（主要是指定义函数、类等代码块之后）。

#### 2.1. 条件分支

注意Python中对小括号的依赖减少了，但是对空格的依赖大大增长：

```python
a = 1
if a == 1:
    print('a is 1')
elif a == 0:
    print('a is 0')
else:
    print('wtf is a?')
```

当然，`else`和`elif`都是可选的，`elif`可以拆成`else:`和`if`，但这太蠢了，它会长得像这个样子：

```python
a = 1
if a == 1:
    print('a is 1')
else:
    if a == 0:
        print('a is 0')
    else:
        print('wtf is a?')
```

多层的嵌套会导致左边出现大量的缩进，所幸在 IDE/编辑器插件 的帮助下这不会为编程带来太大困难，但是仍然观感很差，这种情况下建议使用函数“转移矛盾”。

但是话说回来，任何一种语言多层嵌套总不能完全不缩进吧，多层嵌套无论如何都是会影响阅读的，其实并不只是因为Python层级关系依赖缩进的锅。

#### 2.2. 循环

Python同样支持`while`循环与`for`循环：

```python
current, then = 0, 1
for _ in range(1000):
    current, then = then, current + then

print(current)   # 计算斐波那契数列第1000项
```

此外关于数组等均可以用于`for`循环迭代，比较特别的是字符串也可以迭代：

```python
a = 'hello world'
for c in a:
    print(c)
```

Tip：如果你在交互式命令行里写带层次的代码，那么写完最后一个层次后要连按两次回车（即留出一个空行），否则会报错。

```python
a = 10
while 2 <= a < 20:    # 语法糖，这种写法是被建议使用的
    print(a)
    a -= 1            # 注意Python中没有自增1和自减1运算符
```

### 3. 函数

函数的概念想必大家都了解了，这里不再赘述，直接上例子见证语法：

```python
def fib(n):
    current, then = 0, 1
    for _ in range(n):
        current, then = then, current + then
    return current
```

注意第一行固定格式`def <func_name>(params,...):`，其他和C/C++差别不大。作为动态类型语言，函数返回值不固定，可以多个return在不同情况下返回不同值，或者没有return（等价于`return None`）

```python
def fib(n):
    # 这个代码逻辑很差劲，但是能说明关于return None的问题
    if not isinstance(n, int):
        return None
    if n < 0:
        return None
    if n < 1000:
        current, then = 0, 1
        for _ in range(n):
            current, then = then, current + then
        return current
    # n >= 1000 的情况没有显式的处理，其实是return None了
```

此外函数传参本质上和赋值号是一样的，他们都是浅复制，结合基本类型章节的内容考虑下这是为什么。

可以按照如下的方式指定参数和返回值的类型，但是并不起到约束作用，只是给程序员自己看的：

```python
def fib(n: int) -> int:
    current, then = 0, 1
    for _ in range(n):
        current, then = then, current + then
    return current
```

扩展阅读：自学参数中`*`, `**`等字符的作用

当然，输入输出函数绝对应当拥有姓名：

- `input`：从标准输入读入**一行**字符串，可以提供一个字符串当作提示语。在工程中几乎用不到（当然C++的cin在工程中也几乎用不到）
- `print`：打印一句话到指定文件，文件缺省值为`sys.stdout`，表示到标准输出。参数自行了解，打印原理是调用对象的`__str__`函数将它转成字符串

#### 3.1. lambda表达式

和很多语言一样，Python中可以使用lambda表达式完成一些简单的逻辑，但比较特殊的地方在于，Python中lambda表达式必须只由一个表达式构成，这个表达式的值也就是lambda表达式的返回值。

lambda表达式实际上构造了一个函数对象，因此可以作为变量或者参数、返回值等传递：

```python
add = lambda a, b: a + b
a = add(1, 2)    # a = 3
```

```python
nums = [1, 2, 3, 4, 5, 6, 7, 8, 9]
nums.sort(key=lambda a: abs(5-a))    # nums = [5, 4, 6, 3, 7, 2, 8, 1, 9]
```

```python
def lambda_test():
    return lambda x: x + 5
a = lambda_test()(5) # a = 10
```

### 4. OOP

#### 4.1. 作用域

Python中的类基础用法比C/C++更简洁一些，首先介绍一点没什么卵用但是可以预防一些可能的bug的作用域的知识：

```python
class A:
    pass  # pass用于一个块必须存在，但是你又不想写任何语句的时候，作为占位符

A.b = 1   # 为类型 A 动态添加公有成员变量 b
print(A.b)
a = A()   # 实例化 A
print(A.b)
a.b = 3   # 为对象 a 添加了公有成员 b
print(a.b)
print(A.b)
del a.b   # del 可以删除一个变量，这里删掉了 a 的成员 b
print(a.b)
```

#### 4.2. 类成员函数

```python
class A:
    def __init__(self, a, b):
        # 这里是构造函数
        self.a = a
        self.b = b
    def print(self):
        # 注意，这里没有任何递归的意思
        print(self.a)
        print(self.b)

a = A(1, 2)
print(a.a)
print(a.b)
a.print()
```

注意类的所有成员函数第一个参数必须是self，原则上你可以给它起任何名字，它将起到和C++中this类似的作用。不加self的变量、函数会被视作局部或全局的变量、函数，但是不会视作类成员，你需要显示地指出self。

另外Python不支持函数重载（支持虚函数，除了特殊前缀外默认都是虚函数），构造函数必然只有一个，但是你可以提供动态的参数列表，见函数章节的扩展阅读部分。

（不要问析构函数，我们很少在Python中用到析构，就像你很少在Python代码里见到`del`，有需要的话我们会用上下文管理器替代析构函数的职能，这涉及`with`部分，自学）

#### 4.3. 类继承

Python的类继承默认都是公有继承，迫于时间这部分直接跳过。

```python
class A(parent_class_1, parent_class_2):
    pass
```

如果你对于构造函数的初始化有所疑问，学习一下`super().__init()`或许能解决你的一些疑问。

#### 4.4. 运算符重载

Python的运算符重载很特别，都是写成函数的成员函数的形式，函数名比较特别，下面介绍一个例子，大家触类旁通即可（还记得前面介绍运算符里的一堆`__xxx__`吗？可供查阅）：

```python
class A():
    def __init__(self, a):
        self.a = a
    def __add__(self, other):
        if isinstance(other, A):
            return A(self.a + other.a)
        elif isinstance(other, int):
            return A(self.a + other)
        else:
            return None    # 非标准处理手段，标准方法是抛出异常，但是我们没空讲异常了
    def __str__(self):     # 重载 str(obj) 函数
        return 'A: %d' % self.a

a = A(1)
b = A(2)
c = a + b
d = c + 10
print(c.a)
print(d)
```

### 5. 文件操作

文件操作也是比较重要的基础，何况网上充斥着一键读入全文件的**代码，因此有必要强调一下。

首先相信大家都有了C文件操作基础（以及之前讲Linux应该也有说），因此这里略过相对路径、打开方式等不谈，首先在终端中执行指令构造一个简单文件：

```bash
echo -e "123\n456\n789" > temp.txt
```

（或者手动创建，第一行123，第二行456，第三行789）

然后再到Python中进行操作：

```python
file = open('temp.txt')     # 注意此处相当于 open('temp.txt', 'r')，其中'r'表示读取
line1 = file.readline()
lines = file.readlines()
file.close()
print(line1)
print(lines)
```

注意前面提到上下文管理器，在读写文件时就非常有用，可以避免由于忘记关闭文件导致丢失数据：

```python
with open('temp.txt') as file:
    line1 = file.readline()
    lines = file.readlines()
    # 这里 file 会被自动 close 掉
print(line1)
print(lines)
```

不过这还是太简单了点，下面用bash加点料：

```bash
echo -e "你好世界" >> temp.txt
```

再到Python操作：

```python
data = open('temp.txt').read()   # 此时有没有显式关闭并不重要，因为临时变量被析构时文件数据也被清除了
print(data)
```

它执行成功了吗？我不知道，因为我无法确定你的终端编码与你的Python默认编码是否一致（事实上在Windows的终端中执行`echo -e`是错误的）。在我的电脑上它可能等价于：

```python
data = open('temp.txt', encoding='utf-8').read()
print(data)
```

但是在Windows电脑上，它或许等价于

```python
data = open('temp.txt', encoding='gbk').read()
print(data)
```

Anyway，你应当自己保证或让提供文件的人保证文件的编码是什么，否则你将需要一些手段（库）来推测编码。

此外，介绍一点比较高效的操作（这部分代码可比CSDN上通篇的全部读取有价值多了）：

```python
with open('temp.txt') as file:
    for line in file:    # 使用迭代器按行读取，效率比全文读取高得多，想象一下如果这文件几个G
        print(line)
```

二进制文件的操作略去（注意二进制文件显然不应该指定编码），文件的写入略去。

### 6. 库

Python的一大优势在于它的第三方库真的很多，使得很多事情很容易完成。实际上你也很容易将自己写的代码封装成库。~~比如说我自己写了访问树洞的库（然而API更新了我还没改）~~这样的例子有很多，下一节Python应用中会举几个例子来说明。下面先讲库的使用。

这里涉及的几个概念分别是script、module、package，这些概念自行学习。

所谓库，或者也叫包（但至少日常聊天时我们不会提模块），是指Python内部提供或其他开源社区贡献者提供的一些代码，可以用来直接调用，譬如C++有内置库\<iostream\>，C有\<stdio.h\>，Python有json、http这样的内置库，此外Python的第三方库安装非常简单（见参考资料）。

下面以json为例：

考虑一下如何把一个字典保存在文件里？

```python
d = {'姓名': '张三', '年龄': 18, '生日': None}
```

这就是一个字典，直接写到文件里是行不通的，文件只能写字节串或者字符串，你需要转化它，一个可能的方法是用`str(d)`，如果你想到了这个操作，那很好，但是还不够，因为我们缺少一个手段将字符串转化回字典（这被称作序列化和反序列化）。json库正为我们提供了这样的手段

```python
# 引入整个库
import json
json.dump(d, open('data.json', 'w'))
data = json.load(open('data.json'))
```

或许你会很好奇data.json里面是些什么：

```json
{"\u59d3\u540d": "\u5f20\u4e09", "\u5e74\u9f84": 18, "\u751f\u65e5": null}
```

这是什么鬼？哦这是Unicode，真不是给人看的东西……不过没关系，我们很容易把它改成给人看的：

```python
json.dump(d, open('data.json', 'w'), ensure_ascii=False)
```

再来看看文件里的东西吧：

```json
{"姓名": "张三", "年龄": 18, "生日": null}
```

这次看上去是不是好多了？注意那个null，这是JavaScript的写法，毕竟json的全称是JavaScript Object Notation。

还有一些额外的语法

```python
# 引入库并起别名
import json as js
js.dump(d, open('data.json', 'w'), ensure_ascii=False)
```

```python
# 引入一部分内容（当然也可以别名）
from json import dump
dump(d, open('data.json', 'w'), ensure_ascii=False)
```

```python
# 可以引入多个部分
from json import dump, load
# 也可以使用通配符，但是一般不要这样做
from json import *
```

### 7. 脚本化

当然，在交互式窗口里写东西只能是玩具级别的手段，你终究和C/C++一样写成文件才有足够的复用性。

执行Python脚本的方法非常简单：

```bash
python filename.py [here maybe some params]
```

### 8. 深入学习

#### 8.1. 入门级

廖雪峰教程：[https://www.liaoxuefeng.com/wiki/1016959663602400]()

此处可以看到我有意或无意间忽略的一些基础知识。

#### 8.2. 进阶级

Python Doc（简体中文版）：[https://docs.python.org/zh-cn/3/contents.html]()

此处可以看到关于Python的内置库、新特性的详细介绍。此外它同样包含一份新手教程，链接为：[https://docs.python.org/zh-cn/3/tutorial/index.html]()

#### 8.3. 一些知识点

下面是一些应当拥有姓名，我备课时想到了但完全没时间在文中提及的零碎知识或库：

- ~~f-string~~（rls想到了没讲但是这次讲了）
- @ 装饰器 或 运算符
- os与sys库
- pathlib
- datetime
- argparser~~（梦回oop）~~
- sorted
- \_\_getattribute\_\_
- 并发与并行（多线程、多进程、协程）
- 正则
- \_\_main\_\_
- 异常抛出与处理

无论到文档里检索这些东西，还是直接百度，都不失为一种学习手段。Python的热度决定了你在学习它时能够轻松获取大量的材料——但是不要忘了辨别它们。

#### 8.4. 就地取材

在交互式窗口中你可以很方便地就近获取一些东西，包括：

- 使用`help(obj)`函数，查看函数接口，比方说看看基本类型都有些什么函数？
- 使用`dir(obj_or_type)`可以快捷查看一个对象的类（或直接看类）有些什么成员函数。

如果你使用了Ipython（或Jupyter-notebook），你可以用name? 或name?? 的形式查看接口/源码。

在pycharm中你可以用查看定义的方法（快捷键不确定，我的是Ctrl+鼠标左键）很方便的查看一些库的源码。
