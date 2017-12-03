---
title: Python学习笔记02 - 函数
categories:
  - Python
tags:
  - Python
---

# 内置函数

```python
>>> abs(100)
100
>>> abs(-20)
20
>>> abs(12.34)
12.34
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



# 递归函数

```python
def fact(n):
    if n==1:
        return 1
    return n * fact(n - 1)
```



# 参考资料

[廖雪峰的官方网站: Python教程](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/00143167832686474803d3d2b7d4d6499cfd093dc47efcd000) 

