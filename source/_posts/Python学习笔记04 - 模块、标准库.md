---
title: Python学习笔记04 - 模块、标准库
categories:
  - Python
tags:
  - Python
---

# import 与 from...import

在 python 用 import 或者 from...import 来导入相应的模块。

- 将整个模块(somemodule)导入，格式为：` import somemodule`
- 从某个模块中导入某个函数,格式为：` from somemodule import somefunction`
- 从某个模块中导入多个函数,格式为：` from somemodule import firstfunc, secondfunc, thirdfunc`
- 将某个模块中的全部函数导入，格式为：` from somemodule import *`




# 使用模块

使用内建的`sys`模块为例

**hello.py**

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

' a test module '

__author__ = 'Quanzaiyu'

import sys

def test():
    args = sys.argv
    if len(args)==1:
        print('Hello, world!')
    elif len(args)==2:
        print('Hello, %s!' % args[1])
    else:
        print('Too many arguments!')

if __name__=='__main__':
    test()
```

运行

```python
$ python3 hello.py
Hello, world!
$ python hello.py Xiaoyu
Hello, Xiaoyu!
```

运行`python3 hello.py`获得的`sys.argv`就是`['hello.py']`。

运行`python3 hello.py Michael`获得的`sys.argv`就是`['hello.py', 'Michael]`。

因此可以看到两种运行结果。



# 自定义模块

## 作用域

在一个模块中，我们可能会定义很多函数和变量，但有的函数和变量我们希望给别人使用，有的函数和变量我们希望仅仅在模块内部使用。在Python中，是通过`_`前缀来实现的。

正常的函数和变量名是公开的（public），可以被直接引用，比如：`abc`，`x123`，`PI`等；

类似`__xxx__`这样的变量是特殊变量，可以被直接引用，但是有特殊用途，比如上面的`__author__`，`__name__`就是特殊变量，`hello`模块定义的文档注释也可以用特殊变量`__doc__`访问，我们自己的变量一般不要用这种变量名；

类似`_xxx`和`__xxx`这样的函数或变量就是非公开的（private），不应该被直接引用，比如`_abc`，`__abc`等。

**export.py**

```python
def _private_1(name):
    return 'Hello, %s' % name

def _private_2(name):
    return 'Hi, %s' % name

def greeting(name):
    if len(name) > 3:
        return _private_1(name)
    else:
        return _private_2(name)
```

我们在模块里公开`greeting()`函数，而把内部逻辑用private函数隐藏起来了，这样，调用`greeting()`函数不用关心内部的private函数细节，这也是一种非常有用的代码封装和抽象的方法，即：

外部不需要引用的函数全部定义成private，只有外部需要引用的函数才定义为public。

## 导出

导出模块不需要特定的语句，整个`export.py`文件就是一个模块，比如在上述文件同级目录下有一个叫`test.py`的文件:

```python
import export

hello = export.greeting('xiaoyu')
print(hello)
hello = export.greeting('a')
print(hello)
```

测试:

```python
$ python test.py
Hello, xiaoyu
Hi, a
```



# 安装第三方模块

在Python中，安装第三方模块，是通过包管理工具pip完成的。

第三方库都会在Python官方的[pypi.python.org](https://pypi.python.org/)网站注册，要安装一个第三方库，必须先知道该库的名称，可以在官网或者pypi上搜索，比如Pillow的名称叫[Pillow](https://pypi.python.org/pypi/Pillow/)，因此，安装Pillow的命令就是：

```
pip install Pillow
```

## Anaconda

在使用Python时，我们经常需要用到很多第三方库，例如，上面提到的Pillow，以及MySQL驱动程序，Web框架Flask，科学计算Numpy等。用pip一个一个安装费时费力，还需要考虑兼容性。我们推荐直接使用[Anaconda](https://www.anaconda.com/)，这是一个基于Python的数据处理和科学计算平台，它已经内置了许多非常有用的第三方库，我们装上Anaconda，就相当于把数十个第三方模块自动安装好了，非常简单易用。

可以从[Anaconda官网](https://www.anaconda.com/download/)下载GUI安装包，安装包有500~600M，所以需要耐心等待下载。下载后直接安装，Anaconda会把系统Path中的python指向自己自带的Python，并且，Anaconda安装的第三方模块会安装在Anaconda自己的路径下，不影响系统已安装的Python目录。



# python 内建模块

## sys

```python
import sys
```

> 系统模块。

```python
sys.argv # 命令行参数
sys.path # 模块搜索路径
sys.platform # 系统平台标识符
sys.version # 查看Python版本
sys.exit() # 退出解释器
sys.stdin、sys.stdout、sys.stderr # 标准输入、标准输出和错误输出
```

输入:

```python
sys.stdin.readline()
sys.stdin.readlines()
```

输出:

```python
sys.stdout.write("123456\n")
```



## os

```python
import os
```

> os模块封装了操作系统的目录和文件操作，要注意这些函数有的在`os`模块中，有的在`os.path`模块中。

```python
os.name # 返回操作系统类型，返回值是"posix"代表linux，"nt"代表windows
os.getcwd() # 获取当前路径
os.environ # 以字典形式返回系统变量
os.environ.get('PATH') # 获取指定环境变量
os.listdir(path) # 列表形式列出目录，path可选填，不填写为当前路径
os.chdir(path) # 改变当前工作目录到指定目录
os.mkdir(path [, mode=0777]) # 创建目录
os.makedirs(path [, mode=0777]) # 递归创建目录
os.rmdir(path) # 移除空目录
os.remove(path) # 移除文件
os.rename(old, new) # 重命名文件或目录
os.stat(path) # 获取文件或目录属性
os.chown(path, uid, gid) # 改变文件或目录所有者
os.chmod(path, mode) # 改变文件访问权限
os.symlink(src, dst) # 创建软链接
os.unlink(path) # 移除软链接
os.urandom(n) # 返回随机字节，适合加密使用
os.getuid() # 返回当前进程UID
os.getlogin() # 返回登录用户名
os.getpid() # 返回当前进程ID
os.kill(pid, sig) # 发送一个信号给进程
os.walk(path) # 目录树生成器，返回格式：(dirpath, [dirnames], [filenames])
os.system(command) # 执行shell命令，不能存储结果
os.popen(command [, mode='r' [, bufsize]]) # 打开管道来自shell命令，并返回一个文件对象
```

### os.path

>  os.path类用于获取文件属性

```python
os.path.basename(path) # 返回最后一个文件或目录名
os.path.dirname(path) # 返回最后一个文件前面目录
os.path.abspath(path) # 返回一个绝对路径
os.path.exists(path) # 判断路径是否存在，返回布尔值
os.path.isdir(path) # 判断是否是目录
os.path.isfile(path) # 判断是否是文件
os.path.islink(path) # 判断是否是链接
os.path.ismount(path) # 判断是否挂载
os.path.getatime(filename) # 返回文件访问时间戳
os.path.getctime(filename) # 返回文件变化时间戳
os.path.getmtime(filename) # 返回文件修改时间戳
os.path.getsize(filename) # 返回文件大小，单位字节
os.path.join(a, *p) # 加入两个或两个以上路径，以正斜杠"/"分隔。常用于拼接路径 
# 如: os.path.join('/home/user','test.py')，返回: '/home/user/test.py/a.py'
os.path.split(path) # 分隔路径名
os.path.splitext(path) # 分割扩展名
```

### 应用

列出当前目录下所有目录:

```python
[x for x in os.listdir('.') if os.path.isdir(x)]
```

列出当前目录下所有.py文件

```python
[x for x in os.listdir('.') if os.path.isfile(x) and os.path.splitext(x)[1]=='.py']
```



## glob

```python
import glob
```

> 文件查找，支持通配符（*、？、[]）

```python
# 查找目录中所有以.sh为后缀的文件
>>> glob.glob('/home/user/*.sh')
['/home/user/1.sh', '/home/user/b.sh', '/home/user/a.sh', '/home/user/sum.sh']
 
# 查找目录中出现单个字符并以.sh为后缀的文件
>>> glob.glob('/home/user/?.sh')
['/home/user/1.sh', '/home/user/b.sh', '/home/user/a.sh']
 
# 查找目录中出现a.sh或b.sh的文件
>>> glob.glob('/home/user/[a|b].sh')
['/home/user/b.sh', '/home/user/a.sh']
```



## math

```python
import math
```

> 数学处理

```python
math.pi、math.ceil(x)、math.floor(x)、math.fabs(x)、math.fmod(x,y)、math.pow(x,y)、math.sqrt(x)

math.trunc(x) # 将数字截尾取整
math.modf(x) # 返回x小数和整数
math.factorial(x) # 返回x的阶乘
```



## random

```python
import random
```

> 生成随机数

```python
random.randint(a,b) # 返回整数a和b范围内数字
random.random() # 返回0和1范围内的随机数
random.randrange(start, stop[, step]) # 返回整数范围的随机数，并可以设置只返回跳数
random.sample(array, x) # 从数组中返回随机x个元素
```



## platform

```python
import platform
```

> 获取操作系统详细信息

```python
platform.platform() # 返回操作系统平台
platform.uname() # 返回操作系统信息
platform.system() # 返回操作系统平台
platform.version() # 返回操作系统版本
platform.machine() # 返回计算机类型
platform.processor() # 返回计算机处理器类型
platform.node() # 返回计算机网络名
platform.python_version() # 返回Python版本号
```



## subprocess

> subprocess库会fork一个子进程去执行任务，连接到子进程的标准输入、输出、错误，并获得它们的返回代码。这个模块将取代os.system、os.spawn*、os.popen*、popen2.*和commands.*。



## Queue

> 队列，数据存放在内存中，一般用于交换数据。



## logging

> 记录日志库。

有几个主要的类：

| logging.Logger      | 应用程序记录日志的接口 |
| ------------------- | ----------- |
| logging.Filter      | 过滤哪条日志不记录   |
| logging.FileHandler | 日志写到磁盘文件    |
| logging.Formatter   | 定义最终日志格式    |

日志级别：

| 级别       | 数字值  | 描述   |
| -------- | ---- | ---- |
| critical | 50   | 危险   |
| error    | 40   | 错误   |
| warning  | 30   | 警告   |
| info     | 20   | 普通信息 |
| debug    | 10   | 调试   |
| noset    | 0    | 不设置  |

Formatter类可以自定义日志格式，默认时间格式是%Y-%m-%d %H:%M:%S，有以下这些属性：

| %(name)s            | 日志的名称                                 |
| ------------------- | ------------------------------------- |
| %(levelno)s         | 数字日志级别                                |
| %(levelname)s       | 文本日志级别                                |
| %(pathname)s        | 调用logging的完整路径（如果可用）                  |
| %(filename)s        | 文件名的路径名                               |
| %(module)s          | 模块名                                   |
| %(lineno)d          | 调用logging的源行号                         |
| %(funcName)s        | 函数名                                   |
| %(created)f         | 创建时间，返回time.time()值                   |
| %(asctime)s         | 字符串表示创建时间                             |
| %(msecs)d           | 毫秒表示创建时间                              |
| %(relativeCreated)d | 毫秒为单位表示创建时间，相对于logging模块被加载，通常应用程序启动。 |
| %(thread)d          | 线程ID（如果可用）                            |
| %(threadName)s      | 线程名字（如果可用）                            |
| %(process)d         | 进程ID（如果可用）                            |
| %(message)s         | 输出的消息                                 |

示例:

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-
#--------------------------------------------------
# 日志格式
#--------------------------------------------------
# %(asctime)s       年-月-日 时-分-秒,毫秒 2013-04-26 20:10:43,745
# %(filename)s      文件名，不含目录
# %(pathname)s      目录名，完整路径
# %(funcName)s      函数名
# %(levelname)s     级别名
# %(lineno)d        行号
# %(module)s        模块名
# %(message)s       消息体
# %(name)s          日志模块名
# %(process)d       进程id
# %(processName)s   进程名
# %(thread)d        线程id
# %(threadName)s    线程名
import logging
format = logging.Formatter('%(asctime)s - %(levelname)s %(filename)s [line:%(lineno)d] %(message)s')
# 创建日志记录器
info_logger = logging.getLogger('info')
# 设置日志级别,小于INFO的日志忽略
info_logger.setLevel(logging.INFO)
# 日志记录到磁盘文件
info_file = logging.FileHandler("info.log")
# info_file.setLevel(logging.INFO)
# 设置日志格式
info_file.setFormatter(format)
info_logger.addHandler(info_file)
error_logger = logging.getLogger('error')
error_logger.setLevel(logging.ERROR)
error_file = logging.FileHandler("error.log")
error_file.setFormatter(format)
error_logger.addHandler(error_file)
# 输出控制台（stdout）
console = logging.StreamHandler()
console.setLevel(logging.DEBUG)
console.setFormatter(format)
info_logger.addHandler(console)
error_logger.addHandler(console)
if __name__ == "__main__":
    # 写日志
    info_logger.warning("info message.")
    error_logger.error("error message!")
```



## ConfigParser

> 配置文件解析。

这个库我们主要用到ConfigParser.ConfigParser()类，对ini格式文件增删改查。

ini文件固定结构：有多个部分块组成，每个部分有一个[标识]，并有多个key，每个key对应每个值，以等号"="分隔。值的类型有三种：字符串、整数和布尔值。其中字符串可以不用双引号，布尔值为真用1表示，布尔值为假用0表示。注释以分号";"开头。




## urllib与urllib2

打开URL。urllib2是urllib的增强版，新增了一些功能，比如Request()用来修改Header信息。但是urllib2还去掉了一些好用的方法，比如urlencode()编码序列中的两个元素（元组或字典）为URL查询字符串。一般情况下这两个库结合着用。



## time

> 这个time库提供了各种操作时间值。

| **方法**                         | **描述**                                   | **示例**                                   |
| ------------------------------ | ---------------------------------------- | ---------------------------------------- |
| time.asctime([tuple])          | 将一个时间元组转换成一个可读的24个时间字符串                  | >>> time.asctime(time.localtime())'Sat Nov 12 01:19:00 2016' |
| time.ctime(seconds)            | 字符串类型返回当前时间                              | >>> time.ctime()'Sat Nov 12 01:19:32 2016' |
| time.localtime([seconds])      | 默认将当前时间转换成一个(struct_timetm_year,tm_mon,tm_mday,tm_hour,tm_min,                              tm_sec,tm_wday,tm_yday,tm_isdst) | >>> time.localtime()time.struct_time(tm_year=2016, tm_mon=11, tm_mday=12, tm_hour=1, tm_min=19, tm_sec=56, tm_wday=5, tm_yday=317, tm_isdst=0) |
| time.mktime(tuple)             | 将一个struct_time转换成时间戳                     | >>> time.mktime(time.localtime())1478942416.0 |
| time.sleep(seconds)            | 延迟执行给定的秒数                                | >>> time.sleep(1.5)                      |
| time.strftime(format[, tuple]) | 将元组时间转换成指定格式。[tuple]不指定默认以当前时间           | >>> time.strftime('%Y-%m-%d %H:%M:%S')'2016-11-12 01:20:54' |
| time.time()                    | 返回当前时间时间戳                                | >>> time.time()1478942466.45977          |

strftime():

| **指令** | **描述**              |
| ------ | ------------------- |
| %a     | 简化星期名称，如Sat         |
| %A     | 完整星期名称，如Saturday    |
| %b     | 简化月份名称，如Nov         |
| %B     | 完整月份名称，如November    |
| %c     | 当前时区日期和时间           |
| %d     | 天                   |
| %H     | 24小时制小时数（0-23）      |
| %I     | 12小时制小时数（01-12）     |
| %j     | 365天中第多少天           |
| %m     | 月                   |
| %M     | 分钟                  |
| %p     | AM或PM，AM表示上午，PM表示下午 |
| %S     | 秒                   |
| %U     | 一年中第几个星期            |
| %w     | 星期几                 |
| %W     | 一年中第几个星期            |
| %x     | 本地日期，如'11/12/16'    |
| %X     | 本地时间，如'17:46:20'    |
| %y     | 简写年名称，如16           |
| %Y     | 完整年名称，如2016         |
| %Z     | 当前时区名称（PST：太平洋标准时间） |
| %%     | 代表一个%号本身            |



## datetime

datetime库提供了以下几个类：

| **类**                | **描述**      |
| -------------------- | ----------- |
| datetime.date()      | 日期，年月日组成    |
| datetime.datetime()  | 包括日期和时间     |
| datetime.time()      | 时间，时分秒及微秒组成 |
| datetime.timedelta() | 时间间隔        |
| datetime.tzinfo()    |             |

### datetime.date()

| 方法                   | 描述                         | 描述                                       |
| -------------------- | -------------------------- | ---------------------------------------- |
| date.max             | 对象所能表示的最大日期                | datetime.date(9999, 12, 31)              |
| date.min             | 对象所能表示的最小日期                | datetime.date(1, 1, 1)                   |
| date.strftime()      | 根据datetime自定义时间格式          | >>> date.strftime(datetime.now(), '%Y-%m-%d %H:%M:%S')'2016-11-12 07:24:15 |
| date.today()         | 返回当前系统日期                   | >>> date.today()datetime.date(2016, 11, 12) |
| date.isoformat()     | 返回ISO 8601格式时间（YYYY-MM-DD） | >>> date.isoformat(date.today())'2016-11-12' |
| date.fromtimestamp() | 根据时间戳返回日期                  | >>> date.fromtimestamp(time.time())datetime.date(2016, 11, 12) |
| date.weekday()       | 根据日期返回星期几，周一是0，以此类推        | >>> date.weekday(date.today())5          |
| date.isoweekday()    | 根据日期返回星期几，周一是1，以此类推        | >>> date.isoweekday(date.today())6       |
| date.isocalendar()   | 根据日期返回日历（年，第几周，星期几）        | >>> date.isocalendar(date.today())(2016, 45, 6) |

### datetime.datetime()

| 方法                              | 描述               | 示例                                       |
| ------------------------------- | ---------------- | ---------------------------------------- |
| datetime.now()/datetime.today() | 获取当前系统时间         | >>> datetime.now()datetime.datetime(2016, 11, 12, 7, 39, 35, 106385) |
| date.isoformat()                | 返回ISO 8601格式时间   | >>> datetime.isoformat(datetime.now())'2016-11-12T07:42:14.250440' |
| datetime.date()                 | 返回时间日期对象，年月日     | >>> datetime.date(datetime.now())datetime.date(2016, 11, 12) |
| datetime.time()                 | 返回时间对象，时分秒       | >>> datetime.time(datetime.now())                   datetime.time(7, 46, 2, 594397) |
| datetime.utcnow()               | UTC时间，比中国时间快8个小时 | >>> datetime.utcnow()datetime.datetime(2016, 11, 12, 15, 47, 53, 514210) |

### datetime.time()

| 方法              | 描述         | 示例                                       |
| --------------- | ---------- | ---------------------------------------- |
| time.max        | 所能表示的最大时间  | >>> time.maxdatetime.time(23, 59, 59, 999999) |
| time.min        | 所能表示的最小时间  | >>> time.mindatetime.time(0, 0)          |
| time.resolution | 时间最小单位，1微妙 | >>> time.resolutiondatetime.timedelta(0, 0, 1) |

### datetime.timedelta()

```python
# 获取昨天日期
>>> date.today() - timedelta(days=1)         
datetime.date(2016, 11, 11)
>>> date.isoformat(date.today() - timedelta(days=1))
'2016-11-11'
# 获取明天日期
>>> date.today() + timedelta(days=1)               
datetime.date(2016, 11, 13)
>>> date.isoformat(date.today() + timedelta(days=1))
'2016-11-13'
```





# 参考资料

[廖雪峰的官方网站: Python教程](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/0014317568446245b3e1c8837414168bcd2d485e553779e000) 

[runoob: Python3 基础语法](http://www.runoob.com/python3/python3-basic-syntax.html) 

[Python 标准库一览（Python进阶学习）](http://blog.csdn.net/jurbo/article/details/52334345) 

[The Python Standard Library](https://docs.python.org/3.5/library/index.html) 

[Python常用标准库使用（必会）](http://blog.51cto.com/lizhenliang/1872538) 