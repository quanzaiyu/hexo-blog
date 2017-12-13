---
title: Python学习笔记01 - 基础
categories:
  - Python
  - Python学习笔记
tags:
  - Python
---

# 简介

Python是一门动态语言。变量本身类型不固定的语言称之为动态语言，与之对应的是静态语言。静态语言在定义变量时必须指定变量类型，如果赋值的时候类型不匹配，就会报错。



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







# 参考资料

[廖雪峰的官方网站: Python教程](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000) 

[welcome to python.org](https://www.python.org/) 