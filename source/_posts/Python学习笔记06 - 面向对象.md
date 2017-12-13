---
title: Python学习笔记06 - 面向对象
categories:
  - Python
  - Python学习笔记
tags:
  - Python
---



# 类的创建

```python
class Student(object):
    def __init__(self, name, score):
        self.name = name
        self.__score = score
    def print_score(self):
        print('%s: %s' % (self.name, self.__score))

bart = Student('Bart Simpson', 59)
lisa = Student('Lisa Simpson', 87)
bart.print_score() # Bart Simpson: 59
lisa.print_score() # Lisa Simpson: 87
```

`__init__`方法的第一个参数永远是`self`，表示创建的实例本身，因此，在`__init__`方法内部，就可以把各种属性绑定到`self`，因为`self`就指向创建的实例本身。

注意，python实例化一个对象不需要关键字`new`。



# 访问限制

在Class内部，可以有属性和方法，而外部代码可以通过直接调用实例变量的方法来操作数据，这样，就隐藏了内部的复杂逻辑。

如果要让内部属性不被外部访问，可以把属性的名称前加上两个下划线`__`，在Python中，实例的变量名如果以`__`开头，就变成了一个私有变量（private），只有内部可以访问，外部不能访问。

```python
print(bart.name) # Bart Simpson
print(bart.__score) # AttributeError: 'Student' object has no attribute '__score'
```

在Python中，变量名类似`__xxx__`的，也就是以双下划线开头，并且以双下划线结尾的，是特殊变量，特殊变量是可以直接访问的，不是private变量

如果看到以一个下划线开头的实例变量名，比如`_name`，这样的实例变量外部是可以访问的，但是，按照约定俗成的规定，当看到这样的变量时，意思就是，“虽然我可以被访问，但是，请把我视为私有变量，不要随意访问”。



# 继承和多态

在OOP程序设计中，当我们定义一个class的时候，可以从某个现有的class继承，新的class称为子类（Subclass），而被继承的class称为基类、父类或超类（Base class、Super class）。

继承什么类就在括号里写对应的类名，同其他编程语言一样，子类可以对父类的方法进行重写。

```python
class Animal(object):
    def run(self):
        print('Animal is running...')

class Dog(Animal):
    def run(self):
        print('Dog is running...')

    def eat(self):
        print('Eating meat...')

class Cat(Animal):
    def run(self):
        print('Cat is running...')

    def eat(self):
        print('Eating meat...')


dog = Dog()
dog.run()

cat = Cat()
cat.run()
```





# 对象相关操作

## isinstance

判断一个变量是否是某个类型可以用`isinstance()`判断：

```python
dog = Dog()
cat = Cat()
print(isinstance(dog, Dog)) # True
print(isinstance(dog, Cat)) # False
print(isinstance(dog, Animal)) # True
```



## type

用type来输出对象的类型。

```python
print(type(dog)) # <class '__main__.Dog'>
print(type('')) # <class 'str'>
print(type(1)) # <class 'int'>
```



## dir

获得一个对象的所有属性和方法，可以使用`dir()`函数，它返回一个包含字符串的list。

```python
print(dir(dog))
```



## hasattr、setattr、getattr

判断对象是否包含属性、属性设置与获取。

```python
print(hasattr(dog, 'x')) # False
print(hasattr(dog, 'eat')) # True
setattr(dog, 'y', 19) # 设置一个属性'y'
print(hasattr(dog, 'y')) # True
print(getattr(dog, 'y')) # 19
```



# 类属性

```python
class Student(object):
    name = 'Student'

s = Student()

print(s.name) # Student
```









# 参考资料

[廖雪峰的官方网站: Python教程](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/00143186739713011a09b63dcbd42cc87f907a778b3ac73000) 


