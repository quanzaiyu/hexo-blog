---
title: 高效使用ESlint进行开发
categories:
  - 前端开发
tags:
  - ECMAScript
---

> 在使用js编程的时候，常常会用到集合对象，集合对象其实是一种泛型，在js中没有明确的规定其内元素的类型，但在强类型语言譬如Java中泛型强制要求指定类型。

> ES6引入了iterable类型，Array，Map，Set都属于iterable类型，它们可以使用**for...of循环来遍历**，都内置**forEach**方法。



# 数组

## 遍历

### 普通遍历

最简单的一种，也是使用频率最高的一种。

```js
let arr = ['a', 'b', 'c', 'd', 'e']
for (let i = 0; i < arr.length; i++) {
  console.log(i, ' => ', arr[i])
}
```

优化: 缓存数组长度:

```js
let arr = ['a', 'b', 'c', 'd', 'e']
for (let i = 0, len = arr.length; i < len; i++) {
  console.log(i, ' => ', arr[i])
}
```

使用临时变量，将长度缓存起来，避免重复获取数组长度，当数组较大时优化效果才会比较明显。



### for-in

> 这个循环很多人爱用，但实际上，经分析测试，在众多的循环遍历方式中它的效率是最低的。

```js
let arr = ['a', 'b', 'c', 'd', 'e']
for (let i in arr) {
  console.log(i, ' => ', arr[i])
}
```



### for-of

> 这种方式是es6里面用到的，性能要好于forin，但仍然比不上普通for循环。

```js
let arr = ['a', 'b', 'c', 'd', 'e']
let index = 0
for (let item of arr) {
  console.log(index++, ' => ', item)
}
```



### forEach

> 数组自带的foreach循环，使用频率较高，实际上性能比普通for循环弱。

```js
let arr = ['a', 'b', 'c', 'd', 'e']
arr.forEach((v, k) => {
  console.log(k, ' => ', v)
})
```

> forEach接受第三个参数，指向原数组，没有返回值，对其进行操作会改变原数组对象

```js
let ary = [12, 23, 24, 42, 1]
let res = ary.forEach((item, index, input) => {
   input[index] = item * 10
})
console.log(res) //-->undefined
console.log(ary) //-->会对原来的数组产生改变
```

如果版本低的浏览器不兼容(IE8-)，可以自定义方法实现:

```js
/**
* forEach遍历数组
* @param callback [function] 回调函数；
* @param context [object] 上下文；
*/
Array.prototype.myForEach = function (callback,context) {
  context = context || window;
  if('forEach' in Array.prototype) {
    this.forEach(callback,context)
    return
  }
  // IE6-8下自己编写回调函数执行的逻辑
  for(let i = 0,len = this.length; i < len; i++) {
    callback && callback.call(context, this[i], i, this)
  }
}
let arr = [12, 23, 24, 42, 1]
arr.myForEach((v, k) => {
  console.log(k, ' => ', v)
})
```



### map

> map会返回一个全新的数组，同样接受第三个参数，如果对其进行操作会改变原数组。

```js
let ary = [12, 23, 24, 42, 1]
let res = ary.map((item, index, input) => {
   return item * 10
})
console.log(res) //-->[120,230,240,420,10]
console.log(ary) //-->[12,23,24,42,1]
```

如果版本低的浏览器不兼容(IE8-)，可以自定义方法实现:

```js
/**
* map遍历数组
* @param callback [function] 回调函数；
* @param context [object] 上下文；
*/
Array.prototype.myMap = function myMap(callback,context){
  context = context || window
  if('map' in Array.prototype) {
    return this.map(callback, context)
  }
  //IE6-8下自己编写回调函数执行的逻辑
  let newAry = []
  for(var i = 0,len = this.length; i < len; i++) {
    if(typeof callback === 'function') {
      var val = callback.call(context, this[i], i, this)
      newAry[newAry.length] = val
    }
  }
  return newAry
}
arr.myMap((v, k) => {
  console.log(k, ' => ', v)
})
```



## 过滤

### filter

> 对数组中的每个元素都执行一次指定的函数（callback），并且创建一个新的数组，该数组元素是所有回调函数执行时返回值为 true 的原数组元素。它只对数组中的非空元素执行指定的函数，没有赋值或者已经删除的元素将被忽略，同时，新创建的数组也不会包含这些元素。

```js
let arr = [12, 5, 8, 130, 44]
let ret = arr.filter((el, index, array) => {
  return el > 10
})
console.log(ret) // [12, 130, 44]
```



### map

> map也可以作为过滤器使用，不过返回的是对原数组每项元素进行操作变换后的数组，而不是每项元素返回为true的元素集合。

```js
let strings = ["hello", "Array", "WORLD"]
function makeUpperCase(v) {
  return v.toUpperCase()
}
let uppers = strings.map(makeUpperCase)
console.log(uppers) // ["HELLO", "ARRAY", "WORLD"]
```



### some

> 对数组中的每个元素都执行一次指定的函数（callback），直到此函数返回 true，如果发现这个元素，some 将返回 true，如果回调函数对每个元素执行后都返回 false ，some 将返回 false。它只对数组中的非空元素执行指定的函数，没有赋值或者已经删除的元素将被忽略。

```js
// 检查是否有数组元素大于等于10：
function isBigEnough(element, index, array) {
  return (element >= 10)
}
let passed1 = [2, 5, 8, 1, 4].some(isBigEnough) // passed1 is false
let passed2 = [12, 5, 8, 1, 4].some(isBigEnough) // passed2 is true
```



### every

> 对数组中的每个元素都执行一次指定的函数（callback），直到此函数返回 false，如果发现这个元素，every 将返回 false，如果回调函数对每个元素执行后都返回 true ，every 将返回 true。它只对数组中的非空元素执行指定的函数，没有赋值或者已经删除的元素将被忽略。

```js
// 测试是否所有数组元素都大于等于10：
function isBigEnough(element, index, array) {
  return (element >= 10)
}
let passed1 = [12, 5, 8, 1, 4].every(isBigEnough) // passed1 is false
let passed2 = [12, 15, 18, 11, 14].every(isBigEnough) // passed2 is true
```



## 排序

```js
let arr = ['a', 1, 'b', 3, 'c', 2, 'd', 'e']
console.log(arr) // ["a", 1, "b", 3, "c", 2, "d", "e"]
console.log(arr.reverse()) // 反转元素(改变原数组) // ["e", "d", 2, "c", 3, "b", 1, "a"]
console.log(arr.sort()) // 对数组元素排序(改变原数组) // [1, 2, 3, "a", "b", "c", "d", "e"]
console.log(arr) // [1, 2, 3, "a", "b", "c", "d", "e"]
```



### sort自定义排序

```js
let arr = [1, 100, 52, 6, 88, 99]
let arr1 = arr.sort((a, b) => a-b) // 从小到大排序
console.log(arr1) // [1, 6, 52, 88, 99, 100]
let arr2 = arr.sort((a, b) => b-a) // 从大到小排序
console.log(arr2) // [100, 99, 88, 52, 6, 1]
console.log(arr) // 原数组也发生改变
```



## 搜索

```js
let arr = [12, 5, 4, 8, 1, 4]
arr.indexOf(4) // 2，从前往后搜索，返回第一次搜索到的数组下标，搜索不到返回-1
arr.lastIndexOf(4) // 5，从后往前搜索，返回第一次搜索到的数组下标，搜索不到返回-1
arr.indexOf(0) // -1
```



## 增删、清空操作

### 添加元素

```js
let arr = ['a', 'b', 'c', 'd', 'e']
arr.push(10, 11) // 模仿栈进行操作，往数组末尾添加一个或多个元素(改变原数组)
arr.unshift(0, 1) // 模仿队列进行操作，往数组前端添加一个或多个元素(改变原数组)
console.log(arr) // [0, 1, "a", "b", 'c', "d", "e", 10, 11]
```



### 删除元素

```js
arr.pop() // 移除最后一个元素并返回该元素值(改变原数组)
arr.shift() // 移除最前一个元素并返回该元素值，数组中元素自动前移(改变原数组)
console.log(arr) // ["b", "c", "d"]
```



### 清空数组

> 将数组的length设置为0即可

```js
let arr = ['a', 1, 'b', 3, 'c', 2, 'd', 'e']
arr.length = 0
```

> length详解:
>
> - 因为数组的索引总是由0开始，所以一个数组的上下限分别是：0和length-1；
>
>
> - 当length属性被设置得更大时，整个数组的状态事实上不会发生变化，仅仅是length属性变大；
> - 当length属性被设置得比原来小时，则原先数组中索引大于或等于length的元素的值全部被丢失。



### splice

> 既可以删除也可以添加元素

```js
let arr = ['a', 'b', 'c', 'd', 'e']
arr.splice(2, 1, 1,2,3)
console.log(arr) // ["a", "b", 1, 2, 3, "d", "e"]
```

>**splice(start, len, elems)** : 删除并添加元素(改变原数组)
>
>- start: 起始位置
>- len: 删除元素的长度
>- elems: 添加的元素队列
>  - 几种形式:
>  - **splice(start, 0, elems)** : 从start位置添加元素
>  - **splice(start, len)** : 从start位置删除len个元素



## 截取、合并与拷贝

```js
let arr = ['a', 'b', 'c', 'd', 'e']
let arr1 = arr.slice(1, 2) // 以数组的形式返回数组的一部分，注意不包括 end 对应的元素，如果省略 end 将复制 start 之后的所有元素(返回新数组)
let arr2 = arr.concat([1,2,3]); // 将多个数组（也可以是字符串，或者是数组和字符串的混合）连接为一个数组(返回新数组)
console.log(arr) // ["a", "b", "c", "d", "e"]
console.log(arr1) // ["b"]
console.log(arr2) // ["a", "b", "c", "d", "e", 1, 2, 3]
```

> 其实`slice`和`concat`也可以作为数组的拷贝方法:

```js
arr.slice(0) // 返回数组的拷贝数组，注意是一个新的数组，不是指向
arr.concat() // 返回数组的拷贝数组，注意是一个新的数组，不是指向
```



# Map

> Map是一组键值对的结构，具有极快的查找速度。

## 创建

方法一: 创建的时候初始化

```js
let mapObj = new Map([
  ['a', 1],
  ['b', 2],
  ['c', 3]
])
console.log(mapObj.size) // 3
```

方法二: 创建空Map，之后添加元素

```js
let mapObj = new Map()
mapObj.set('a', 1)
mapObj.set('b', 2)
mapObj.set('c', 3)
console.log(mapObj.size) // 3
```

> 注意: Map对象的长度不是length，而是size





## 基础操作

Map对象的创建、添加元素、删除元素...

```js
mapObj.set('a', 1) // 添加元素
mapObj.delete('d') // 删除指定元素
mapObj.has('a') // true
mapObj.get('a') // 1
```



## 遍历

使用上面创建的Map进行操作

### forEach

> 同数组的forEach遍历，三个参数分别代表: value、key、map本身

```js
mapObj.forEach((e, index, self) => {
  console.log(index, ' => ', e)
})
```

打印出:

```js
a  =>  1
b  =>  2
c  =>  3
```



### for-of

```js
for (const e of mapObj) {
  console.log(e)
}
```

打印出:

```js
["a", 1]
["b", 2]
["c", 3]
```

> 注意: for-of遍历出来的是一个数组，其中e[0]为key，e[1]为value



# Set

> Set和Map类似，但set只存储key，且key不重复。

## 创建

方法一: 创建的时候初始化

```js
let setObj = new Set([1, 2, 3])
console.log(setObj.size)
```

方法二: 创建空Map，之后添加元素

```js
let setObj = new Set()
setObj.add(1)
setObj.add(2)
setObj.add(3)
console.log(setObj.size)
```

> 注意: Map对象的长度不是length，而是size



## 基础操作

```js
let s = new Set([1, 2, 3])
s.add(3) // 由于key重复，添加不进
s.delete(3) // 删除指定key
console.log(s) // 1 2
```



## 遍历

使用上面创建的Set进行操作

### forEach

> Set与Array类似，但Set没有索引，因此回调函数的前两个参数都是元素本身

```js
s1.forEach(function (element, sameElement, set) {
  console.log(element) // element === sameElement
})
// 打印 1 2 3
```



### for-of

```js
for (const item of s1) {
  console.log(item)
}
// 打印 1 2 3
```



# 类数组对象

JavaScript中，数组是一个特殊的对象，其property名为正整数，且其length属性会随着数组成员的增减而发生变化，同时又从Array构造函数中继承了一些用于进行数组操作的方法。而对于一个普通的对象来说:

> 如果它的所有property名均为正整数，同时也有相应的length属性，那么虽然该对象并不是由Array构造函数所创建的，它依然呈现出数组的行为，在这种情况下，这些对象被称为“类数组对象”。

形如:

```js
let obj = {
  0: 'qzy',
  1: 22,
  2: false,
  length: 3
}
```

类数组对象可以使用Array对象原生方法进行操作。



## 遍历

沿用上述对象进行操作

### forEach

```js
Array.prototype.forEach.call(obj, function(el, index){  
   console.log(index, ' => ', el)
})
```

### map

```js
Array.prototype.map.call(obj, function(el, index){  
   console.log(index, ' => ', el)
})
```



> 注意: 类数组对象不支持使用for-of进行遍历，否则会报错: [Symbol.iterator] is not a function



## 增删截取操作

沿用上述对象进行操作

```js
Array.prototype.join.call(obj, '-') // qzy-22-false
Array.prototype.slice.call(obj, 1, 2) // [22]
Array.prototype.push.call(obj, 5) // Object {0: "qzy", 1: 22, 2: false, 3: 5, length: 4}
```



## String也是一个类数组对象

由于字符串对象也存在length，且序号从0开始增加，因此字符串也可以看做一个只读的类数组对象，这意味着String对象可以使用Array的所有原型方法。

```js
let str = 'hello world'
console.log(Array.prototype.slice.call(str, 0, 5)) // ["h", "e", "l", "l", "o"]
```



### String也可以使用for-of进行遍历

```js
let str = 'hello world'
for (const s of str) {
  console.log(s)
}
```



### String独有方法

除了使用Array原型对象的方法，String还包含其他一些自己独有的方法:

**与Array使用方法相同的方法** 

搜索: indexOf()、lastIndexOf()、concat()

### 转换

toLowerCase()、toUpperCase()

### 截取 

substr(start, len)

substring(start, end)

slice(start, end)

```js
let str = 'hello world'
let ret1 = str.substr(6, 5) // "world"
let ret2 = str.substring(6, 11) // "world"
let ret3 = str.slice(3, 8) // "lo wo"
```

> substring 是以两个参数中较小一个作为起始位置，较大的参数作为结束位置。
>
> slice 是第一参数为起始位置，第二参数为结束位置，如果结束位置小于起始位置返回空字符串

```js
console.log(str.substring(11, 6) === str.substring(6, 11)) // true
```

> 接收负数为参数时:
>
> - slice会将它字符串的长度与对应的负数相加，结果作为参数；
> - substr则仅仅是将第一个参数与字符串长度相加后的结果作为第一个参数；
> - substring则干脆将负参数都直接转换为0。

```js
let str = 'hello world'
let ret1 = str.substr(-5) // "world"
let ret2 = str.substr(-5, 3) // "wor"
let ret3 = str.substring(6, -1) // "hello"
let ret4 = str.slice(6, -1) // "worl"

console.log(ret1 === str.substr(str.length - 5)) // true
console.log(ret2 === str.substr(str.length - 5, 3)) // true
console.log(ret3 === str.substring(6, 0)) // true
console.log(ret4 === str.slice(6, str.length - 1)) // true
```

### 正则

- match()
- replace()
- search()

```js
let str = 'hello world'
let ret0 = str.match(/r/) // 非全局搜索，返回匹配的第一个字符串数组(length为1)，包括index和input
let ret1 = str.match(/o/g) // 全局搜索，返回匹配的字符串数组，length为搜索到的匹配正则表达式的长度
let ret2 = str.replace(/o/g, 'e') // 全局替换
let ret3 = str.replace(/O/i, 'e') // 不区分大小写，只替换搜索到的第一个字串
let ret4 = str.search(/l/) // 返回搜索到的第一个匹配字串的索引
let ret5 = str.search(/l/g) // 全局无效，同上

console.log(ret0) // ["r", index: 8, input: "hello world"]
console.log(ret1) // ["o", "o"]
console.log(ret2) // "helle werld"
console.log(ret3) // "helle world"
console.log(ret4) // 2
console.log(ret5) // 2
console.log(str) // 不改变源字符串 'hello world'
```



# 转化

## Map => Object

```js
let mapObj = new Map([ ['a', 1], ['b', 2], ['c', 3] ])

let obj = {}
for (const item of mapObj) {
  obj[item[0]] = item[1]
}

console.log(obj)
```



## Set => Array

```js
let setObj = new Set([1, 2, 3])

let arr = []
for (const item of setObj) {
  arr.push(item)
}

console.log(arr)
```



## Array => String

> arr.join(separator)

```js
['a', 'b', 'c', 'd', 'e'].join('')
// toLocaleString 、toString 、valueOf：可以看作是join的特殊用法，不常用
```



## String => Array

> str.split(separator)

```js
'hello world'.split(' ') // ["hello", "world"]
```



# 参考资料

[JavaScript中的数组遍历forEach()与map()方法以及兼容写法介绍](http://www.jb51.net/article/84609.htm) 

[JS几种数组遍历方式以及性能分析对比](http://www.cnblogs.com/lvmh/p/6104397.html) 

[JavaScript中的类数组对象介绍](http://www.jb51.net/article/59169.htm) 

[数组对象和类数组对象区别](http://blog.csdn.net/ailongyang/article/details/60466654) 

[js数组的操作指南](http://www.jb51.net/article/59084.htm) 

[详谈js遍历集合(Array,Map,Set)](http://www.jb51.net/article/110487.htm) 

[JavaScript中的Map、Set及其遍历](http://www.cnblogs.com/weilan/p/7002088.html) 

[JavaScript创建Map对象](http://blog.csdn.net/mr__fang/article/details/42024535) 

[slice,substr和substring的区别](http://www.cnblogs.com/littledu/archive/2011/04/18/2019475.html) 