---
title: Python学习笔记05 - IO
categories:
  - Python
tags:
  - Python
---



> file 对象使用 open 函数来创建



# 文件操作

## 读文件

```python
f = open('./info.log', 'r') # 以只读的方式打开一个文件
t = f.read() # 读取文件内容，保存到一个str对象
print(t) # 如果文件打开成功，将打印文件内容
f.close() # 关闭文件
```

`open()` 方法，传入文件名和标示符，可以打开一个文件对象。

`read() ` 方法，可以一次读取文件的全部内容，Python把内容读到内存，用一个`str`对象表示。

`close()` 方法，关闭文件，文件使用完毕后必须关闭，因为文件对象会占用操作系统的资源，并且操作系统同一时间能打开的文件数量也是有限的。

如果文件不存在，将抛出一下错误信息:

```python
FileNotFoundError: [Errno 2] No such file or directory: '/Users/info.log'
```

由于文件读写时都有可能产生`IOError`，一旦出错，后面的`f.close()`就不会调用。所以，为了保证无论是否出错都能正确地关闭文件，我们可以使用`try ... finally`来实现：

```python
try:
    f = open('./info.log', 'r')
    print(f.read())
finally:
    if f:
        f.close()
```

但是每次都这么写实在太繁琐，所以，Python引入了`with`语句来自动帮我们调用`close()`方法：

```python
with open('./info.log', 'r') as f:
    print(f.read())
```

这和前面的`try ... finally`是一样的，但是代码更佳简洁，并且不必调用`f.close()`方法。

调用`read()`会一次性读取文件的全部内容，如果文件有10G，内存就爆了，所以，要保险起见，可以反复调用`read(size)`方法，每次最多读取size个字节的内容。另外，调用`readline()`可以每次读取一行内容，调用`readlines()`一次读取所有内容并按行返回`list`。因此，要根据需要决定怎么调用。

如果文件很小，`read()`一次性读取最方便；如果不能确定文件大小，反复调用`read(size)`比较保险；如果是配置文件，调用`readlines()`最方便：

```python
for line in f.readlines():
    print(line.strip()) # 把末尾的'\n'删掉
```

`read(size)` 方法，不带`size`读取整个文件内容，带`size`读取指定字节的内容。

`readlines()` 方法，一次性读取文件所有内容并按照行号返回为`list`。



### file-like Object

像`open()`函数返回的这种有个`read()`方法的对象，在Python中统称为file-like Object。除了file外，还可以是内存的字节流，网络流，自定义流等等。file-like Object不要求从特定类继承，只要写个`read()`方法就行。

`StringIO`就是在内存中创建的file-like Object，常用作临时缓冲。



### 二进制文件

前面默认都是读取文本文件，并且是UTF-8编码的文本文件。要读取二进制文件，比如图片、视频等等，用`'rb'`模式打开文件即可：

```python
with open('./100.jpg', 'rb') as f:
        print(f.read())
```

其中`b`表示以二进制文件形式打开，将输出类似以下的十六进制表示的字节内容:

```
b'\xff\xd8\xff\xe0\x00\x10JFIF\x00\x01\x01\x01\x00`\x00`\x00\x00\xff\xe1\x00\x08Exif\x00\x00\xff\xdb\x00C\x00\x08\x06\x06\x07\x06\x05\x08\x07\x07\x07\t\t\x08\n\x0c\x14\r\x0c\x0b\x0b\x0c\x19\x12\x13\x0f\x14\x1d\x1a\x1f\x1e\x1d\x1a\x1c\x1c $.\' ",#\x1c\x1c(7),
```



### 编码

只需要在第三个参数加上`encoding='xxx'` 即可

```python
with open('./info.log', 'r', encoding='gbk') as f:
        print(f.read())
```

遇到有些编码不规范的文件，你可能会遇到`UnicodeDecodeError`，因为在文本文件中可能夹杂了一些非法编码的字符。遇到这种情况，`open()`函数还接收一个`errors`参数，表示如果遇到编码错误后如何处理。最简单的方式是直接忽略：

```python
f = open('/Users/michael/gbk.txt', 'r', encoding='gbk', errors='ignore')
```



## 写文件

写文件和读文件是一样的，唯一区别是调用`open()`函数时，传入标识符`'w'`或者`'wb'`表示写文本文件或写二进制文件：

```python
with open('./info.log', 'w') as f:
    f.write('hello')
```

你可以反复调用`write()`来写入文件，但是务必要调用`f.close()`来关闭文件。当我们写文件时，操作系统往往不会立刻把数据写入磁盘，而是放到内存缓存起来，空闲的时候再慢慢写入。只有调用`close()`方法时，操作系统才保证把没有写入的数据全部写入磁盘。忘记调用`close()`的后果是数据可能只写了一部分到磁盘，剩下的丢失了。所以，还是用`with`语句来得保险。

要写入特定编码的文本文件，请给`open()`函数传入`encoding`参数，将字符串自动转换成指定编码。



## 读写模式

读写模式的类型有：

```
rU或Ua 以读方式打开, 同时提供通用换行符支持 (PEP 278)
w      以写方式打开，
a      以追加模式打开 (从 EOF 开始, 必要时创建新文件)
r+     以读写模式打开
w+     以读写模式打开 (参见 w )
a+     以读写模式打开 (参见 a )
rb     以二进制读模式打开
wb     以二进制写模式打开 (参见 w )
ab     以二进制追加模式打开 (参见 a )
rb+    以二进制读写模式打开 (参见 r+ )
wb+    以二进制读写模式打开 (参见 w+ )
ab+    以二进制读写模式打开 (参见 a+ )
```



# 内存操作

StringIO和BytesIO是在内存中操作str和bytes的方法，使得和读写文件具有一致的接口。

## StringIO

StringIO就是在内存中读写str。

```python
from io import StringIO
f = StringIO()
f.write('hello')
f.write(' ')
f.write('world!')
print(f.getvalue()) # hello world!
```

`getvalue()`方法用于获得写入后的str。

创建的时候初始化StringIO，然后可以像文件一样进行操作 :

```python
from io import StringIO
f = StringIO('Hello!\nHi!\nGoodbye!')
while True:
    s = f.readline()
    if s == '':
        break
    print(s.strip())
```



## BytesIO

BytesIO就是在内存中读写二进制数据。

```python
from io import BytesIO
f = BytesIO()
f.write('中文'.encode('utf-8'))
print(f.getvalue())
```

输出: `b'\xe4\xb8\xad\xe6\x96\x87'`



# pickle

我们把变量从内存中变成可存储或传输的过程称之为序列化，在Python中叫pickling，在其他语言中也被称之为serialization，marshalling，flattening等等，都是一个意思。

序列化之后，就可以把序列化后的内容写入磁盘，或者通过网络传输到别的机器上。

反过来，把变量内容从序列化的对象重新读到内存里称之为反序列化，即unpickling。

## 序列化

`pickle.dumps()`方法把任意对象序列化成一个`bytes`，然后，就可以把这个`bytes`写入文件了。

格式：`pickle.dumps(obj, protocol=None)`

```python
import pickle
d = dict(name='Bob', age=20, score=88)
p = pickle.dumps(d)
print(p)
```



## 存入文件

`pickle.dump()`方法可以直接把对象序列化后写入一个file-like Object。

格式: `pickle.dump(obj, file, protocol=None)`

```python
import pickle
d = dict(name='Bob', age=20, score=88)
f = open('dump.txt', 'wb')
pickle.dump(d, f)
f.close()

with open('./dump.txt', 'rb') as f:
    print(f.read())
# b'\x80\x03}q\x00(X\x04\x00\x00\x00nameq\x01X\x03\x00\x00\x00Bobq\x02X\x03\x00\x00\x00ageq\x03K\x14X\x05\x00\x00\x00scoreq\x04KXu.'
```



## 反序列化

`pickle.load()`方法反序列化出对象，也可以直接用`pickle.load()`方法从一个`file-like Object`中直接反序列化出对象。

格式：`pickle.load(file)`

```python
with open('./dump.txt', 'rb') as f:
    d = pickle.load(f)
    print(d)
# {'name': 'Bob', 'age': 20, 'score': 88}
```



# JSON

JSON表示的对象就是标准的JavaScript语言的对象，JSON和Python内置的数据类型对应如下：

| JSON类型     | Python类型   |
| ---------- | ---------- |
| {}         | dict       |
| []         | list       |
| "string"   | str        |
| 1234.56    | int或float  |
| true/false | True/False |
| null       | None       |

Python对json进行操作可以使用`json`模块

## 序列化

`dumps()`方法返回一个`str`，内容就是标准的JSON。

```python
import json
d = dict(name='Bob', age=20, score=88)
print(json.dumps(d))
# {"name": "Bob", "age": 20, "score": 88}
```



## 保存到文件

`dump()`方法可以直接把JSON写入一个`file-like Object`。

```python
f = open('./test.txt', 'w')
json.dump(d, f)
f.close()
```



## 讲一个对象转化为json

可选参数`default`就是把任意一个对象变成一个可序列为JSON的对象

```python
import json
class Student(object):
    def __init__(self, name, age, score):
        self.name = name
        self.age = age
        self.score = score
    
s = Student('Bob', 20, 88)
print(json.dumps(s, default=lambda obj: obj.__dict__))
```

通常`class`的实例都有一个`__dict__`属性，它就是一个`dict`，用来存储实例变量。也有少数例外，比如定义了`__slots__`的class。





# 参考资料

[廖雪峰的官方网站: Python教程](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/001431917715991ef1ebc19d15a4afdace1169a464eecc2000) 

[python:open/文件操作](http://www.cnblogs.com/dkblog/archive/2011/02/24/1980651.html) 

