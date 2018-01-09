---
title: 我的开发之路系列 - python3学习指南
categories:
  - Python
tags:
  - Python
---



> 本文已制作成完整电子书发布，这里只是简单介绍python3的常用操作，更多详情请查看《<u>[我的开发之路系列 - python3学习指南](http://book.python3.xiaoyulive.top)</u>》

<div style='text-align: center;'>
<a href='http://book.python3.xiaoyulive.top'>
  <img src='http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/python/cover.jpg?x-oss-process=style/thumb'/>
</a>
</div>



# python简介

 Python是一门动态语言。变量本身类型不固定的语言称之为动态语言，与之对应的是静态语言。静态语言在定义变量时必须指定变量类型，如果赋值的时候类型不匹配，就会报错。

Python的3.0版本，常被称为Python 3000，或简称Py3k。相对于Python的早期版本，这是一个较大的升级。相对于Python2 (最新版本为2.7) ，有了更多的特性，很多语法规则也有了改变，在设计的时候没有考虑向下兼容。



# 安装与使用

下载链接: [Python Download Python.org](https://www.python.org/downloads/)

安装并添加到环境变量

测试

```
python
# 输出
Python 3.7.0a2 (v3.7.0a2:f7ac4fe, Oct 17 2017, 17:06:29) [MSC v.1900 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

安装 pylint (代码检测)

```
python -m pip install pylint
```

执行.py文件

```
python test.py
```

退出Python交互模式

```
exit() 
```



## UnicodeDecodeError的解决方案

如果出现错误:

```
UnicodeDecodeError: 'utf-8' codec can't decode byte 0xa1 in position 43: invalid start byte
```

的报错，解决方法如下:

打开`c:\program files\python36\lib\site-packages\pip\compat\__init__.py`约75行

```
return s.decode('utf_8') 
```

改为

```
return s.decode('cp936')
```

参考资料: http://blog.csdn.net/qq_25191257/article/details/56488662 



# 输入输出语句

打印语句

```
print('hello world')
```

输入语句

```python
name = input()
print('hello', name)
```

可使用`sys.stdout`命名空间下的输出语句

```python
import sys;
x = 'hello world';
sys.stdout.write(x + '\n')
```



# 基础语法

## 注释

使用`#`开始

通常在文件开头写上这两行：

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
```

第一行注释是为了告诉`Linux/OS X`系统，这是一个`Python`可执行程序，`Windows`系统会忽略这个注释；

第二行注释是为了告诉`Python`解释器，按照`UTF-8`编码读取源代码，否则，你在源代码中写的中文输出可能会有乱码。

申明了`UTF-8`编码并不意味着你的`.py`文件就是`UTF-8`编码的，必须并且要确保文本编辑器正在使用`UTF-8 without BOM`编码。

如果`.py`文件本身使用`UTF-8`编码，并且也申明了`# -*- coding: utf-8 -*-`，打开命令提示符测试就可以正常显示中文。

### 三引号注释

```python
'''
这是多行注释，用三个单引号
这是多行注释，用三个单引号 
这是多行注释，用三个单引号
'''
"""
这是多行注释，用三个单引号
这是多行注释，用三个单引号 
这是多行注释，用三个单引号
"""
```



## 保留字

```python
import keyword
keyword.kwlist
# 输出
['False', 'None', 'True', 'and', 'as', 'assert', 'async', 'await', 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return', 'try', 'while', 'with', 'yield']
```



## 数据类型

- Number（数字num）
  - 整数(int): 1，100，-8080，0，0xff00，0xa5b4c3d2
  - 浮点数(float): 1.23，3.14，-9.01，1.23e9，1.2e-5
  - 复数(complex): 1 + 2j、 1.1 + 2.2j
- String（字符串str）: 'abc'，"xyz"，'I'm "OK"!'
- Boolean （布尔值bool）: True、False（注意首字母是大写）
- None（空值）: None
- List（列表list）
- Tuple（元组tuple）
- Sets（集合set）
- Dictionary（字典dict）

### 数据类型转换

| 函数                                       | 描述                              |
| ---------------------------------------- | ------------------------------- |
| [int](http://www.runoob.com/python3/python-func-int.html)(x [,base]) | 将x转换为一个整数                       |
| [float(x)](http://www.runoob.com/python3/python-func-float.html) | 将x转换到一个浮点数                      |
| [complex](http://www.runoob.com/python3/python-func-complex.html)(real [,imag]) | 创建一个复数                          |
| [str(x)](http://www.runoob.com/python3/python-func-str.html) | 将对象 x 转换为字符串                    |
| [repr(x)](http://www.runoob.com/python3/python-func-repr.html) | 将对象 x 转换为表达式字符串                 |
| [eval(str)](http://www.runoob.com/python3/python-func-eval.html) | 用来计算在字符串中的有效Python表达式,并返回一个对象   |
| [tuple(s)](http://www.runoob.com/python3/python3-func-tuple.html) | 将序列 s 转换为一个元组                   |
| [list(s)](http://www.runoob.com/python3/python3-att-list-list.html) | 将序列 s 转换为一个列表                   |
| [set(s)](http://www.runoob.com/python3/python-func-set.html) | 转换为可变集合                         |
| [dict(d)](http://www.runoob.com/python3/python-func-dict.html) | 创建一个字典。d 必须是一个序列 (key,value)元组。 |
| [frozenset(s)](http://www.runoob.com/python3/python-func-frozenset.html) | 转换为不可变集合                        |
| [chr(x)](http://www.runoob.com/python3/python-func-chr.html) | 将一个整数转换为一个字符                    |
| [unichr(x)](http://www.runoob.com/python3/python-func-unichr.html) | 将一个整数转换为Unicode字符               |
| [ord(x)](http://www.runoob.com/python3/python-func-ord.html) | 将一个字符转换为它的整数值                   |
| [hex(x)](http://www.runoob.com/python3/python-func-hex.html) | 将一个整数转换为一个十六进制字符串               |
| [oct(x)](http://www.runoob.com/python3/python-func-oct.html) | 将一个整数转换为一个八进制字符串                |

### 字符串的处理

```python
>>> a = 'abc'
>>> a.replace('a', 'A')
'Abc'
>>> a
'abc'
```



## 运算符

### 算术运算符

`+`、`-`、`*`、`/`、`%`、`**`(幂)、`//`(取整除)

### 比较运算符

`==`、`!=`、`>`、`<`、`>=`、`<=`

### 赋值运算符

`=`、`+=`、`-=`、`*=`、`/=`、`%=`、`**=`、`//=`

### 位运算符

`&`、`|`、`~`、`^`、`<<`、`>>`

### 逻辑运算符

`and`、`or`、`not`

### 成员运算符

- in : 如果在指定的序列中找到值返回 True，否则返回 False。
- not in : 如果在指定的序列中没有找到值返回 True，否则返回 False。

如

```python
a = 10
b = 1
list = [1, 2, 3, 4, 5 ]
print(a in list) # False
print(b in list) # True
print(a not in list) # True
print(b not in list) # False
```

### 身份运算符

- is : 判断两个标识符是不是引用自一个对象
- is not : 是判断两个标识符是不是引用自不同对象

```python
a = 20
b = 20
print(a is b) # True
print(a is not b) # False
a = 10
print(a is b) # False
print(a is not b) # True
```

### 运算符优先级

## Python运算符优先级

| 运算符                      | 描述                                |
| ------------------------ | --------------------------------- |
| **                       | 指数 (最高优先级)                        |
| ~ + -                    | 按位翻转, 一元加号和减号 (最后两个的方法名为 +@ 和 -@) |
| * / % //                 | 乘，除，取模和取整除                        |
| + -                      | 加法减法                              |
| >> <<                    | 右移，左移运算符                          |
| &                        | 位 'AND'                           |
| ^ \|                     | 位运算符                              |
| <= < > >=                | 比较运算符                             |
| <> == !=                 | 等于运算符                             |
| = %= /= //= -= += *= **= | 赋值运算符                             |
| is is not                | 身份运算符                             |
| in not in                | 成员运算符                             |
| not or and               | 逻辑运算符                             |



## 字符串的特殊用法

**多行语句**

```python
total = item_one + \
        item_two + \
        item_three
```

**在 [], {}, 或 () 中的多行语句，不需要使用反斜杠(\)**

```python
total = ['item_one', 'item_two', 'item_three',
        'item_four', 'item_five']
```

**自然字符串，使用 r'' 表示引号内部的字符串默认不转义**

```python
>>> print('\\\t\\')
\       \
>>> print(r'\\\t\\')
\\\t\\
```

**使用三引号 '''...''' 表示多行内容** 

```python
>>> print('''line1
    line2
    line3''')
line1
line2
line3
```

**十六进制**

```python
>>> '\u4e2d\u6587'
'中文'
```

**bytes字节码**

Python的字符串类型是str，在内存中以Unicode表示，一个字符对应若干个字节。如果要在网络上传输，或者保存到磁盘上，就需要把str变为以字节为单位的bytes。

在bytes中，无法显示为ASCII字符的字节，用\x##显示。

```python
b'ABC'
b'\xe4\xb8\xad\xe6\x96\x87'
```



## 编码转化

`ord()` 函数获取字符的整数表示

`chr()` 函数把编码转换为对应的字符

```python
>>> ord('A')
65
>>> ord('中')
20013
>>> chr(66)
'B'
>>> chr(25991)
'文'
```

`encode()` 将以Unicode表示的str编码为指定的bytes

`decode()` 把bytes转化为为str

```python
>>> 'ABC'.encode('ascii')
b'ABC'
>>> '中文'.encode('utf-8')
b'\xe4\xb8\xad\xe6\x96\x87'
>>> '中文'.encode('gb2312')
b'\xd6\xd0\xce\xc4'
>>> '中文'.encode('ascii')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-1: ordinal not in range(128)
>>> b'ABC'.decode('ascii')
'ABC'
>>> b'\xe4\xb8\xad\xe6\x96\x87'.decode('utf-8')
'中文'
```
如果bytes中包含无法解码的字节，decode()方法会报错：

```python
>>> b'\xe4\xb8\xad\xff'.decode('utf-8')
Traceback (most recent call last):
  ...
UnicodeDecodeError: 'utf-8' codec can't decode byte 0xff in position 3: invalid start byte
```

如果bytes中只有一小部分无效的字节，可以传入errors='ignore'忽略错误的字节:

```python
>>> b'\xe4\xb8\xad\xff'.decode('utf-8', errors='ignore')
'中'
```

`len()` 计算str的字符数，或者bytes的字节数

```python
>>> len('ABC')
3
>>> len('中文')
2
>>> len(b'ABC')
3
>>> len(b'\xe4\xb8\xad\xe6\x96\x87')
6
>>> len('中文'.encode('utf-8'))
6
```



## 变量、常量声明

```
a = 1
PI = 3.14159265359
```



## 字符串格式化

| 占位符  | 替换内容   |
| ---- | ------ |
| %d   | 整数     |
| %f   | 浮点数    |
| %s   | 字符串    |
| %x   | 十六进制整数 |

```python
>>> 'Hello, %s' % 'world'
'Hello, world'
>>> 'Hi, %s, you have $%d.' % ('Michael', 1000000)
'Hi, Michael, you have $1000000.'
>>> '%2d-%02d' % (3, 1)
' 3-01'
>>> '%.2f' % 3.1415926
'3.14'
```
如果字符串里面的%是一个普通字符，这个时候就需要转义，用%%来表示一个%：

```python
>>> 'growth rate: %d%%' % 7
'growth rate: 7%'
```


**format()**

另一种格式化字符串的方法是使用字符串的format()方法，它会用传入的参数依次替换字符串内的占位符{0}、{1}……，不过这种方式写起来比%要麻烦得多：

```python
>>> 'Hello, {0}, 成绩提升了 {1:.1f}%'.format('小明', 17.125)
'Hello, 小明, 成绩提升了 17.1%'
```



# 集合类型

## list (列表)

Python内置的一种数据类型是列表：list。list是一种有序的集合，可以随时添加和删除其中的元素。

```python
>>> classmates = ['Michael', 'Bob', 'Tracy']
>>> classmates
['Michael', 'Bob', 'Tracy']
>>> len(classmates)
3
>>> classmates[0]
'Michael'
>>> classmates[-1]
'Tracy'
>>> classmates[3]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: list index out of range
>>> classmates.append('Adam')
>>> classmates
['Michael', 'Bob', 'Tracy', 'Adam']
>>> classmates.insert(1, 'Jack')
>>> classmates
['Michael', 'Jack', 'Bob', 'Tracy', 'Adam']
>>> classmates.pop()
'Adam'
>>> classmates
['Michael', 'Jack', 'Bob', 'Tracy']
>>> classmates.pop(1)
'Jack'
>>> classmates
['Michael', 'Bob', 'Tracy']
>>> s = ['python', 'java', ['asp', 'php'], 'scheme', 123, True] # 多维数组
>>> a = ['c', 'b', 'a']
>>> a.sort() # 排序
>>> a
['a', 'b', 'c']
```

当索引超出了范围时，`Python`会报一个`IndexError`错误，要确保索引不要越界，记得最后一个元素的索引是`len(classmates) - 1`

相关方法:

`append(ele)` 往末尾添加元素

`insert(index, ele)` 往指定位置添加元素

`pop(index)` 出栈操作，带上index参数则删除指定位置元素，返回删除的元素

`sort()` 排序



## tuple (元组)

另一种有序列表叫元组：tuple。tuple和list非常类似，但是tuple一旦初始化就不能修改。

```python
>>> classmates = ('Michael', 'Bob', 'Tracy')
>>> t = (1,) # 定义只有一个元素的元组，如果不加都好则括号被解析为数学计算意义上的括号
```

多维元组，如果内部包含list，则list里面的元素可更改

```python
>>> t = ('a', 'b', ['A', 'B'])
>>> t[2][0] = 'X'
>>> t[2][1] = 'Y'
>>> t
('a', 'b', ['X', 'Y'])
```



## dict (字典)

以key-value的方式存储

```python
>>> d = {'Michael': 95, 'Bob': 75, 'Tracy': 85}
>>> d['Michael']
95
>>> d['Adam'] = 67
>>> d['Adam']
67
>>> 'Thomas' in d # 通过in判断key是否存在
False
>>> d.get('Thomas') # 使用get()方法，如果key不存在，返回None
>>> d.get('Thomas', -1) # 返回自己指定的value
-1
>>> d.get('Bob', -1) # 如果存在，则返回对应的value
75
```



## set (无序无重复元素集合)

一组key的集合，key不能重复，但不存储value。

```python
>>> s = set([1, 2, 3])
>>> s
{1, 2, 3}
>>> s.add(4)
>>> s
{1, 2, 3, 4}
>>> s.add(4)
>>> s
{1, 2, 3, 4}
>>> s.remove(4)
>>> s
{1, 2, 3}
```



# 控制流程

## 分支

### if...elif...else

```python
age = 3
if age >= 18:
    print('adult')
elif age >= 6:
    print('teenager')
else:
    print('kid')
```

```python
x = True
if x:
    print('True')
```

与input一起使用

```python
s = input('birth: ')
birth = int(s)
if birth < 2000:
    print('00前')
else:
    print('00后')
```

因为`input()`返回的数据类型是`str`，`str`不能直接和整数比较，必须先把`str`转换成整数。Python提供了`int()`函数将str转化为int。



## 循环

### for...in

可以直接作用于`for`循环的数据类型有以下两种:

一类是集合数据类型，如`list`、`tuple`、`dict`、`set`、`str`等；

一类是`generator`，包括生成器和带`yield`的generator function。

这些可以直接作用于`for`循环的对象统称为可迭代对象：`Iterable`。

```python
names = ['Michael', 'Bob', 'Tracy']
for name in names:
    print(name)
```

```python
sum = 0
for x in [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]:
    sum = sum + x
print(sum)
```



### Range()

带一个参数，表示0-max

```python
sum = 0
for x in range(100):
    sum = sum + x
print(sum)
```

带两个参数，表示min-max

```python
sum = 0
for x in range(10, 20):
    sum = sum + x
print(sum)
```

可以使用list()将range()转化为list

```python
>>> list(range(5))
[0, 1, 2, 3, 4]
```



### while

```python
sum = 0
n = 99
while n > 0:
    sum = sum + n
    n = n - 2
print(sum)
```



### break

```python
n = 1
while n <= 100:
    if n > 10: # 当n = 11时，条件满足，执行break语句
        break # break语句会结束当前循环
    print(n)
    n = n + 1
print('END')
```



### continue

```python
n = 0
while n < 10:
    n = n + 1
    if n % 2 == 0: # 如果n是偶数，执行continue语句
        continue # continue语句会直接继续下一轮循环，后续的print()语句不会执行
    print(n)
```



# 内置函数

## 数学函数

不在`math` 下的数学函数:

abs()、min()、max()、round()

```python
divmod(100,10)  # 返回一个元组（10,0），第一个元素的100/10的商，第二个元素的100/10的余数 
pow(x,y)  # 求次方，返回x**y的结果
pow(x,y,z) # 返回 x**y%z 的结果
range()  # 获取随机数或随机字符 eg. range(10) 从0到10的随机数
```

## 转换函数

#### 类型转换

int()、bool()、str()、list()、set()、tuple()

#### 进制转换

bin()、oct()、hex()

#### 其它转换

ascii()、bytes()、chr()、ord()

## 集合运算

all()、any()、filter()、map()

```python
enumerate()  # 接收序列化类型的数据，返回一个迭代器（对象). e.g. for i,item in enumerate(['one','two','three']): print(i,item)  打印1 'one' 换行2 'two'换行 3 'three'
frozenset()  #转换为不可变的集合
globals()  # 返回一个字典，包括所有的全局变量与它的值所组成的键值对
locals()  # 返回一个字典，包括所有的局部变量与它的值所组成的键值对
reversed()  #对序列化类型数据反向排序，返回一个新的对象。注意与对象的reverse方法区别，后者是就地改变对象
sorted() # 对序列化类型数据正向排序，返回一个新的对象。注意与对象的sort方法区别，后者是就地改变对象
slice()  #对序列化类型数据切片，返回一个新的对象。eg. slice(起始下标，终止下标，步长)，步长默认为1
zip() # 接收多个序列化类型的数据，对各序列化数据中的元素，按索引位置分类成一个个元组。
```

## 其他函数

next() 、iter() 、object() 

```python
compile() # 接收.py文件或字符串作为传入参数，将其编译成python字节码
eval()    # 执行python代码，并返回其执行结果。 e.g. eval("1+2+3")   eval("print(123)").   在接收用户输入时应避免使用eval，因为别有用心的用户可能借此注入恶意代码
exec()    #执行python代码（可以是编译过的，也可以是未编译的），没有返回结果（返回None） e.g. exec(compile("print(123)","<string>","exec"))   exec("print(123)")
dir()  # 接收对象作为参数，返回该对象的所有属性和方法
help()  # 接收对象作为参数，更详细地返回该对象的所有属性和方法
isinstance(object, class)  # 判断对象是否是某个类的实例. e.g. isinstance([1,2,3],list)
format()  #字符串格式化
hash()  # 对传入参数取哈希值并返回
id() # 返回内存地址，可用于查看两个变量是否指向相同一块内存地址
input('please input:')  # 提示用户输入，返回用户输入的内容（不论输入什么，都转换成字符串类型）
issubclass(subclass,class) #查看这个类是否是另一个类的派生类，如果是返回True，否则返回False
len('string')  # 返回字符串长度，在python3中以字符为单位，在python2中以字节为单位
memoryview()  # 查看内存地址
repr()  # 执行传入对象中的_repr_方法
type() # 返回对象类型
staticmethod() # 返回静态方法
super()  # 返回基类
vars() #返回当前模块中的所有变量
```



# 函数定义

```python
def my_abs(x):
    if x >= 0:
        return x
    else:
        return -x
```

调用函数时，如果参数个数不对，Python解释器会自动检查出来，并抛出`TypeError`：

```python
>>> my_abs(1, 2)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: my_abs() takes 1 positional argument but 2 were given
```

如果参数类型不对，Python解释器就无法帮我们检查。

```python
>>> my_abs('A')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 2, in my_abs
TypeError: unorderable types: str() >= int()
>>> abs('A')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: bad operand type for abs(): 'str'
```

让我们修改一下`my_abs`的定义，对参数类型做检查，只允许整数和浮点数类型的参数。数据类型检查可以用内置函数`isinstance()`实现：

```python
def my_abs(x):
    if not isinstance(x, (int, float)):
        raise TypeError('bad operand type')
    if x >= 0:
        return x
    else:
        return -x
```

添加了参数检查后，如果传入错误的参数类型，函数就可以抛出一个错误：

```python
>>> my_abs('A')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 3, in my_abs
TypeError: bad operand type
```



# 空函数

```python
def nop():
    pass
```

`pass`语句什么都不做。实际上`pass`可以用来作为占位符，比如现在还没想好怎么写函数的代码，就可以先放一个`pass`，让代码能运行起来。

`pass`还可以用在其他语句里，比如：

```python
if age >= 18:
    pass
```

缺少了`pass`，代码运行就会有语法错误。



# 多值返回

```python
import math

def move(x, y, step, angle=0):
    nx = x + step * math.cos(angle)
    ny = y - step * math.sin(angle)
    return nx, ny

x, y = move(100, 100, 60, math.pi / 6)
print(x, y) # (151.96152422706632, 70.0)
```

多值返回值其实是一个tuple，只是可以省略括号



# 函数的参数

## 默认参数

有多个默认参数时，调用的时候，既可以按顺序提供默认参数，也可以不按顺序提供部分默认参数。当不按顺序提供部分默认参数时，需要把参数名写上。

```python
def my_print(name, age=18, sex='male'):
  print(name, age, sex)

my_print('xiaoyu') # xiaoyu 18 male
my_print('xiaozhang', 22) # xiaozhang 22 male
my_print('xiaoqiao', sex='female') # xiaoqiao 18 female
```

> 注意: 默认参数必须为不可变对象，否则坑了会出现意想不到的副作用

比如:

```python
def add_end(L=[]):
    L.append('END')
    return L
```

```python
>>> add_end()
['END']
>>> add_end()
['END', 'END']
```

解决方案:

```python
def add_end(L=None):
    if L is None:
        L = []
    L.append('END')
    return L
```

```python
>>> add_end()
['END']
>>> add_end()
['END']
```



## 可变参数

可以使用确定的list或tuple传值，如:

```python
def calc(numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum

calc([1, 2, 3]) # 14
calc((1, 3, 5, 7)) # 84
```

也可使用可变参数的形式，仅仅在可变参数前面添加一个`*`号，如:

```python
def calc(*numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum

calc(1, 2, 3) # 14
```

Python允许你在list或tuple前面加一个`*`号，把list或tuple的元素变成可变参数传进去：

```python
nums = [1, 2, 3]
calc(*nums) # 14
```



## 关键字参数

python支持关键字参数，实际上是一个dict对象，使用两个`**`表示:

```python
def person(name, age, **kw):
    print('name:', name, 'age:', age, 'other:', kw)

person('Adam', 45, gender='M', job='Engineer')
# name: Adam age: 45 other: {'gender': 'M', 'job': 'Engineer'}
```

Python允许你在list或tuple前面加两个`**`号，把dict元素变成可变参数传进去：

```python
extra = {'city': 'Beijing', 'job': 'Engineer'}
person('Jack', 24, **extra)
# name: Jack age: 24 other: {'city': 'Beijing', 'job': 'Engineer'}
```

使用dict类型传值写法同下，不过较繁琐:

```python
def person(name, age, kw):
    print('name:', name, 'age:', age, 'other:', kw)

person('Adam', 45, {'gender':'M', 'job': 'Engineer'})
```



## 关键字参数名检查

对于关键字参数，函数的调用者可以传入任意不受限制的关键字参数。至于到底传入了哪些，就需要在函数内部通过`in`检查。

```python
def person(name, age, **kw):
    if 'city' in kw:
        # 有city参数的情况 do something
        pass
    if 'job' in kw:
        # 有job参数的情况 do something
        pass
    print('name:', name, 'age:', age, 'other:', kw)
```



## 命名关键字参数

```python
def person(name, age, *, city, job):
    print(name, age, city, job)
    
person('Jack', 24, city='Beijing', job='Engineer')
# Jack 24 Beijing Engineer
```

如果少传指定关键字则会报错

```python
person('Jack', 24, city='Beijing')
# TypeError: person() missing 1 required positional argument: 'job'
```

如果传入关键字之外的参数则会报错

```python
person('Jack', 24, city='Beijing', sex='famale')
# person() got an unexpected keyword argument 'sex'
```

如果函数定义中已经有了一个可变参数，后面跟着的命名关键字参数就不再需要一个特殊分隔符`*`了:

```python
def person(name, age, *args, city, job):
    print(name, age, args, city, job)
    
person('Jack', 24, 'hello', 'my', 'friend', city='Beijing', job='Engineer')
# Jack 24 ('hello', 'my', 'friend') Beijing Engineer
```



## 参数组合

在Python中定义函数，可以用必选参数、默认参数、可变参数、关键字参数和命名关键字参数，这5种参数都可以组合使用。但是参数定义的顺序必须是：必选参数、默认参数、可变参数、命名关键字参数和关键字参数。

```python
def f1(a, b, c=0, *args, d, **kw):
    print('a =', a, 'b =', b, 'c =', c, 'args =', args, 'd=', d, 'kw =', kw)

args = (1, 2, 3, 4)
kw = {'d': 99, 'x': '#'}
f1(*args, **kw)
# a = 1 b = 2 c = 3 args = (4,) d= 99 kw = {'x': '#'}
```



# 闭包

## 以函数作为返回值

```python
def lazy_sum(*args):
    def sum():
        ax = 0
        for n in args:
            ax = ax + n
        return ax
    return sum

sum = lazy_sum(1,2,3,4,5)
print(sum) # <function lazy_sum.<locals>.sum at 0x0000029DF5E930D0>
print(sum()) # 15
```

以上代码，我们在函数`lazy_sum`中又定义了函数`sum`，并且，内部函数`sum`可以引用外部函数`lazy_sum`的参数和局部变量，当`lazy_sum`返回函数`sum`时，相关参数和变量都保存在返回的函数中，这种程序结构被称为“闭包（`Closure`）”。



## 未立即执行的函数

```python
def count():
    fs = []
    for i in range(1, 4):
        def f():
             return i*i
        fs.append(f)
    return fs

f1, f2, f3 = count()

print(f1()) # 9
print(f2()) # 9
print(f3()) # 9
```

以上代码，本期待获得的值为`1 4 9`，却由于未立即执行，最后获得了`9 9 9`。

原因就在于返回的函数引用了变量`i`，但它并非立刻执行。等到3个函数都返回时，它们所引用的变量`i`已经变成了`3`，因此最终结果为`9`。



## 使用立即执行函数实现变量暂存

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

fs = count()

print(fs[0]()) # 1
print(fs[1]()) # 4
print(fs[2]()) # 9
```



# Lambda表达式

即匿名函数，`lambda x: x * x`相当于:

```python
def f(x):
    return x * x
```

## lambda表达式的一些应用:

在map中进行迭代:

```python
list(map(lambda x: x * x, [1, 2, 3, 4, 5, 6, 7, 8, 9]))
[1, 4, 9, 16, 25, 36, 49, 64, 81]
```

在filter中进行过滤:

```python
def is_odd(n):
    return n % 2 == 1

list(filter(lambda n: n%2==1, range(1,20)))
# [1, 3, 5, 7, 9, 11, 13, 15, 17, 19] 
```

闭包:

```python
def build(x, y):
    return lambda: x * x + y * y

print(build(1,2)())
```



# 高阶函数

## map

`map()`传入的第一个参数是`f`，即函数对象本身。返回的结果是一个`Iterator`，`Iterator`是惰性序列，需要通过`list()`函数让它把整个序列都计算出来并返回一个list。

### map的应用

将所有list为数值的元素转化为字符串

```python
list(map(str, [1, 2, 3, 4, 5, 6, 7, 8, 9]))
# ['1', '2', '3', '4', '5', '6', '7', '8', '9']
```

求立方

```python
l = list(map(lambda x: x**3 , [1, 2, 3, 4, 5, 6, 7, 8, 9]))
# [1, 8, 27, 64, 125, 216, 343, 512, 729]
```

格式化不规范的英文名，使之首字母大写，后面的字母小写

```python
def normalize(name):
    return name[0:1].upper() + name[1:].lower()
L1 = ['adam', 'LISA', 'barT']
L2 = list(map(normalize, L1)) # ['Adam', 'Lisa', 'Bart'] 
```







## reduce

这个函数必须接收两个参数，`reduce`把结果继续和序列的下一个元素做累积计算。

### reduce的应用

求和

```python
from functools import reduce
s = reduce(lambda x,y: x+y, [1, 2, 3, 4, 5, 6, 7, 8, 9]) # 45
```

最大值

```python
from functools import reduce
def max(x, y):
  if (x > y):
    return x
  return y

s = reduce(max, [1, 2, 3, 4, 5, 6, 7, 8, 9])
print(s)
```



## fliter

过滤器，筛选出满足条件(返回值为True)的序列

### filter的应用

过滤奇数

```python
l = list(filter(lambda x: x%2==1, [1, 2, 4, 5, 6, 9, 10, 15]))
# [1, 5, 9, 15]
```

取出空字符

```python
def not_empty(s):
    return s and s.strip()

l = list(filter(not_empty, ['A', '', 'B', None, 'C', '  ']))
# ['A', 'B', 'C']
```

求素数

```python
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

# 打印1000以内的素数:
for n in primes():
    if n < 1000:
        print(n)
    else:
        break
```



## sorted

字典排序

```python
sorted([36, 5, -12, 9, -21]) # [-21, -12, 5, 9, 36]
sorted(['bob', 'about', 'Zoo', 'Credit']) # ['Credit', 'Zoo', 'about', 'bob']
```

sorted可以携带第二参数，用于对list进行处理后输出排序

```python
sorted([36, 5, -12, 9, -21], key=abs) # [5, 9, -12, -21, 36]
```

忽略英文大小写进行排序

```python
sorted(['bob', 'about', 'Zoo', 'Credit'], key=str.lower) # ['about', 'bob', 'Credit', 'Zoo']
```

可以传入第三个参数`reverse=True`进行反转排序

```python
sorted(['bob', 'about', 'Zoo', 'Credit'], key=str.lower, reverse=True)
# ['Zoo', 'Credit', 'bob', 'about']
```



# 装饰器

本质上，decorator就是一个返回函数的高阶函数。所以，我们要定义一个能打印日志的decorator，可以定义如下：

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
# call now():
# 2015-3-25
```

上面的`log`，因为它是一个decorator，所以接受一个函数作为参数，并返回一个函数。我们要借助Python的@语法，把decorator置于函数的定义处即可。

调用`now()`函数，不仅会运行`now()`函数本身，还会在运行`now()`函数前打印一行日志。



如果decorator本身需要传入参数，那就需要编写一个返回decorator的高阶函数。比如，要自定义log的文本：

```python
def log(text):
    def decorator(func):
        def wrapper(*args, **kw):
            print('%s %s():' % (text, func.__name__))
            return func(*args, **kw)
        return wrapper
    return decorator

@log('print')
def now():
    print('2015-3-25')

now()
# print now():
# 2015-3-25
```

## 使用functools.wraps

普通装饰器:

```python
import functools

def log(func):
    @functools.wraps(func)
    def wrapper(*args, **kw):
        print('call %s():' % func.__name__)
        return func(*args, **kw)
    return wrapper
```

带参数的装饰器:

```python
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



# 偏函数

比如使用int将字符串转化为数值类型，可以增加第二参数，代表转化为n进制数

```python
int('12345', base=8) # 5349
```

将之封装为一个将字符串转化为n进制的函数，方法如下:

```python
def int2(x, base=2):
    return int(x, base)

int2('1000000') # 64
```

可以使用偏函数进一步简化:

```python
import functools
int2 = functools.partial(int, base=2)

int2('1000000') # 64
```

所以，`functools.partial`的作用就是，把一个函数的某些参数给固定住（也就是设置默认值），返回一个新的函数，调用这个新函数会更简单。



# 递归函数的应用

**求阶乘** 

```python
def fact(n):
    if n==1:
        return 1
    return n * fact(n - 1)
```



# 切片

list和tuple都可以进行切片操作，操作的结果仍是list或tuple。

```python
L = ['Michael', 'Sarah', 'Tracy', 'Bob', 'Jack']
print(L[0:3]) # ['Michael', 'Sarah', 'Tracy']
```

如果省略起始索引，那么默认从0开始

```python
print(L[:3]) # ['Michael', 'Sarah', 'Tracy']
```

同样的，省略结束索引，将切片到最后一个元素

```python
print(L[1:]) # ['Sarah', 'Tracy', 'Bob', 'Jack']
```

如果传入负数，则从list最后一个元素开始取

```python
print(L[-2:]) # ['Bob', 'Jack']
```

切片可以有第三个参数，表示步长，每隔多少位元素取一个

```python
print(L[:4:2]) # ['Michael', 'Tracy'] 前4位元素每隔2位取一位
print(L[::2]) # ['Michael', 'Tracy', 'Jack'] 所有元素每隔2位取一位
```

如果什么都不写，只写`[:]`就可以原样复制

```python
print(L[:])
```

字符串也可以进行切片操作

```python
'ABCDEFG'[:3] # 'ABC'
```

其实其他语言中的字符串截取操作，如`substring`就是一种切片操作



# 迭代

## list及tuple迭代

```python
l = [1, 2, 3, 4, 5]
for item in l:
  print(item)
```

带下标迭代，使用Python内置的`enumerate`函数可以把一个list变成索引-元素对

```python
for i, value in enumerate(['A', 'B', 'C']):
	print(i, value)
```

多变量迭代

```python
for x, y in [(1, 1), (2, 4), (3, 9)]:
  print(x, y)
```



## dict迭代

默认字典迭代只遍历其中的key

```python
d = {'a': 1, 'b': 2, 'c': 3}
for key in d:
  print(key)
```

如果要迭代其中的value，则:

```python
for value in d.values():
  print(value)
```

如果要同时迭代其中的key和value，则:

```python
for value in d.items():
  print(value)
```

或者

```python
d = {'x': 'A', 'y': 'B', 'z': 'C' }
for k, v in d.items():
  print(k, '=', v)
```



## str迭代

```python
for ch in 'ABCD':
  print(ch)
```



## 判断一个对象是可迭代对象

通过`collections`模块的`Iterable`类型判断

```python
from collections import Iterable
print(isinstance('abc', Iterable)) # True
print(isinstance([1,2,3], Iterable)) # True
print(isinstance(123, Iterable)) # False
```



# 列表生成式

普通生成列表的方式:

```python
list(range(1, 11))
# [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

使用列表生成式生成列表

```python
[x * x for x in range(1, 11)]
# [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```

两层循环生成

```python
[m + n for m in 'ABC' for n in 'XYZ']
# ['AX', 'AY', 'AZ', 'BX', 'BY', 'BZ', 'CX', 'CY', 'CZ']
```

遍历当前目录下所有文件

```python
[d for d in os.listdir('.')]
# ['.vscode', '01_test.py', 'learning.py', 'test.md']
```

迭代对象

```python
d = {'x': 'A', 'y': 'B', 'z': 'C' }
ret = [k + '=' + v for k, v in d.items()]
print(ret) # ['y=B', 'x=A', 'z=C']
```

列表元素转化

```python
L = ['Hello', 'World', 'IBM', 'Apple']
ret = [s.lower() for s in L]
print(ret) # ['hello', 'world', 'ibm', 'apple']
```



# 生成器

通过列表生成式，我们可以直接创建一个列表。但是，受到内存限制，列表容量肯定是有限的。而且，创建一个包含100万个元素的列表，不仅占用很大的存储空间，如果我们仅仅需要访问前面几个元素，那后面绝大多数元素占用的空间都白白浪费了。

所以，如果列表元素可以按照某种算法推算出来，那我们是否可以在循环的过程中不断推算出后续的元素呢？这样就不必创建完整的list，从而节省大量的空间。在Python中，这种一边循环一边计算的机制，称为生成器：generator。

要创建一个generator，有很多种方法。第一种方法很简单，只要把一个列表生成式的`[]`改成`()`，就创建了一个generator：

```python
g = (x * x for x in range(10))
print(next(g)) # 0
print(next(g)) # 1
print(next(g)) # 4
print(next(g)) # 9
print(next(g)) # 16
```

generator保存的是算法，每次调用`next(g)`，就计算出`g`的下一个元素的值，直到计算到最后一个元素，没有更多的元素时，抛出`StopIteration`的错误。

generator也是一个可迭代对象:

```python
for n in g:
	print(n)
```

## 斐波那契数列

```python
def fib(max):
    n, a, b = 0, 0, 1
    while n < max:
        print(b)
        a, b = b, a + b
        n = n + 1
    return 'done'
```

将之转化为生成器`generator`，使用`yield`即可

```python
def fib(max):
    n, a, b = 0, 0, 1
    while n < max:
        yield b
        a, b = b, a + b
        n = n + 1
    return 'done'
  
a = fib(10)
print(a) # <generator object fib at 0x00000269D81DA7D8>
for i in a:
  print(i) # 1 1 2 3 5 8 13 21 34 55
```

但是用`for`循环调用generator时，发现拿不到generator的`return`语句的返回值。如果想要拿到返回值，必须捕获`StopIteration`错误，返回值包含在`StopIteration`的`value`中：

```python
g = fib(6)
while True:
  try:
    x = next(g)
    print('g:', x)
    except StopIteration as e:
      print('Generator return value:', e.value)
      break
      
# g: 1 g: 1 g: 2 g: 3 g: 5 g: 8
# Generator return value: done
```



# 迭代器

可以直接作用于`for`循环的数据类型有以下两种:

一类是集合数据类型，如`list`、`tuple`、`dict`、`set`、`str`等；

一类是`generator`，包括生成器和带`yield`的generator function。

这些可以直接作用于`for`循环的对象统称为可迭代对象：`Iterable`。

可以使用`isinstance()`判断一个对象是否是`Iterable`对象：

```python
>>> from collections import Iterable
>>> isinstance([], Iterable)
True
>>> isinstance({}, Iterable)
True
>>> isinstance('abc', Iterable)
True
>>> isinstance((x for x in range(10)), Iterable)
True
>>> isinstance(100, Iterable)
False
```

而生成器不但可以作用于`for`循环，还可以被`next()`函数不断调用并返回下一个值，直到最后抛出`StopIteration`错误表示无法继续返回下一个值了。

可以被`next()`函数调用并不断返回下一个值的对象称为迭代器：`Iterator`。

可以使用`isinstance()`判断一个对象是否是`Iterator`对象：

```python
>>> from collections import Iterator
>>> isinstance((x for x in range(10)), Iterator)
True
>>> isinstance([], Iterator)
False
>>> isinstance({}, Iterator)
False
>>> isinstance('abc', Iterator)
False
```

生成器都是`Iterator`对象，但`list`、`dict`、`str`虽然是`Iterable`，却不是`Iterator`。

把`list`、`dict`、`str`等`Iterable`变成`Iterator`可以使用`iter()`函数：

```python
>>> isinstance(iter([]), Iterator)
True
>>> isinstance(iter('abc'), Iterator)
True
```



# 参考资料

[welcome to python.org](https://www.python.org/) 

[The Python Standard Library](https://docs.python.org/3.5/library/index.html) 

[廖雪峰的官方网站: Python教程](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000) 

[Runoob: Python 3 教程](http://www.runoob.com/python3/python3-tutorial.html) 

[Python 标准库一览（Python进阶学习）](http://blog.csdn.net/jurbo/article/details/52334345) 

[Python常用标准库使用（必会）](http://blog.51cto.com/lizhenliang/1872538) 