---
title: python切片操作
author: lihy
tags:
  - 2020夏
  - second
top: false
cover: false
date: 2020-07-31 09:57:36
categories: python
summary: 完全理解python切片（使用切片操作符:或切片类slice）
img:
coverImg:
---

<!-- TOC -->

- [python 切片操作简介](#python-切片操作简介)
- [使用切片操作符:进行切片操作](#使用切片操作符进行切片操作)
    - [切片操作写法](#切片操作写法)
    - [切片操作读取方向和判断条件](#切片操作读取方向和判断条件)
    - [切片中的索引省略和负数索引](#切片中的索引省略和负数索引)
    - [多维切片](#多维切片)
- [使用 slice 类进行切片操作](#使用-slice-类进行切片操作)

<!-- /TOC -->

## python 切片操作简介

&emsp;&emsp;切片是从 python 对象中提取出部分值。  
&emsp;&emsp;python 切片操作可以使用切片操作符:和切片类 slice 来完成
python 列表|p|y|t|h|o|n|
--|--|--|--|--|--|--|
正索引|0|1|2|3|4|5|
负索引|-6|-5|-4|-3|-2|-1|
&emsp;&emsp;在下文中，我们将这个列表记为 aList，即`aList = ['p','y','t','h','o','n']`，一个简单的切片操作为 aList[1:5:2]=['y', 'h']

## 使用切片操作符:进行切片操作

&emsp;&emsp;python 的切片操作符为英文冒号:

### 切片操作写法

&emsp;&emsp;python 的切片语法简洁而美丽，我们主要以上图的一维对象来说明。主要有以下**五种写法**，第一种是全写，其他进行了省略,start，stop，step分别表示开始索引，停止索引和步长：

> aList[start:stop:step]  
> aList[start:stop]  
> aList[start:]  
> aList[:stop]  
> aList[:]

### 切片操作读取方向和判断条件

&emsp;&emsp;下面是切片读取规则

1. 首先看有无 step，step 隐含了切片读取的方向
   - step 为正，读取方向为正向
   - step 为负，读取方向为正向
2. 判断条件，step 的正负号决定了判断条件，注意判断时**根据 step 的正负将索引全部转化为正索引或者负索引，0 是正索引**
   - step 为正，判断条件为当前索引\<stop
   - step 为负，判断条件为当前索引>stop

第一种写法例子
python 列表|p|y|t|h|o|n|
--|--|--|--|--|--|--|
正索引|0|1|2|3|4|5|
负索引|-6|-5|-4|-3|-2|-1|

```python
>>>aList = ['p','y','t','h','o','n']
# aList[start:stop:step]
>>>aList[1:5:2]
'''
step为正，读取方向为正向，判断条件为当前索引<stop
从索引1开始，(1<5)==true, 读取y
步长为2，下一个读取索引(1+2<5)==true，读取h
下一个读取索引(1+2+2<5)==false，停止
'''
['y', 'h']

```

其他的写法主要是将第一种进行省略，省略索引在下一节展开

```python
# aList[start:stop]
>>>aList[1:5]
['y','t','h','o']
# aList[start:]
>>>aList[2:]
['t','h','o','n']
# aList[:stop]
>>>aList[:2]
['p','y']
# aList[:]
>>>aList[:]
['p','y','t','h','o','n']
```

### 切片中的索引省略和负数索引

几个事实：

1. python 的切片常常省略 start，stop，step 中的一个。省略 step 默认为 step=1，step的省略很彻底——占位符也丢掉了比如`aList[start:stop]`；而省略 start 或者 stop 必须保留占位符，上面五种切片操作的后面四种都在省略 step 的基础上对 start 和 stop 进行了省略
2. python 的**负索引需要 step 支持**，诸如`aList[-1:-2], aList[-1, 0]`都只能得到`[]`。上文已经说明省略 step 后默认其为 1，读取方向为正，第一步判断就直接停止读取，因此这些情况不难理解。
3. 省略 start 或者 stop 等价于使用 None 进行占位，如何停止上文已经给出了说明。主要是两点：**读取方向和判断条件**。
4. 对于正索引，省略 start 或者 stop 并没有什么难理解的。
5. 对于负索引，省略 start 或者 stop 后使用 None 进行占位，读取方向判断条件上文都已说明:
   - aList[-1:-3:-1]，读取方向为负，判断条件是当前索引>stop
   - aList[:-3:-1]\=\=aList[None:-3:-1]，读取方向为负，判断条件为当前索引>stop
   - aList[0:-3:-1]，读取方向为负，判断条件为当前索引>stop（需要先转化索引都为正或者负，这里将 0 转化为-6，第一步判断(-6>-3)==false，什么都没读到就停止了。

例子：
python 列表|p|y|t|h|o|n|
--|--|--|--|--|--|--|
正索引|0|1|2|3|4|5|
负索引|-6|-5|-4|-3|-2|-1|

```python
>>>aList[:-3:-1]
'''
省略了start，等价于使用None进行占位，即等价aList[None:-3:-1]，注意不等价aList[0:-3:-1]
step为负，读取方向为正向, 判断条件为当前索引>stop
start省略直接从负方向开始读，(-1>-3)==true，读取n
步长为-1，下一个读取索引(-1-1>-3)==true，读取o
下一个读取索引(-1-1-1>-3)==false，停止
'''
['n', 'o']
>>>aList[None:-3:-1]
['n', 'o']
>>>aList[0:-3:-1]
'''
step为负，读取方向为负方向, 判断条件为当前索引>stop
start为0，由于step为负先将start转化为负索引-6，(-6>-3)==false，停止
'''
[]
```

### 多维切片

多维切片只是一维的拓展，但是要注意切片操作写在一个方括号里面  
举个例子

```python
>>>import numpy as np
>>>aArray = np.array([[1, 2, 3], [4, 5, 6]])
>>>aArray
array([[1, 2, 3],
       [4, 5, 6]])
>>>aArray[-2:, -1:-3:-1]
'''
首先对第一维切片（python默认行优先），从-2即0开始，省略stop和step，step默认为1，stop等价None占位，两行都读取；
其次对第二维切片，读取方向为负，(-1>-3)==true，读取第三列；(-1-1>-3)==true，读取第二列；(-1-1-1>-3)==false，停止
'''
array([[5, 6],
       [2, 3]])
```

## 使用 slice 类进行切片操作

在 python 中，slice 是一个能实现切片操作的基本类（就是无需导入模块即可使用）。在 python 交互式环境中输入以下代码来理解

```python
>>>aSlice = slice(2, 5) # 创建一个slice类并初始化
>>>aList = [1, 2, 3, 4, 5, 6, 7]
>>>aSlice.__class__.__name__
'slice' # 这说明了aSlice是一个slice类
>>>aList[aSlice]
[3, 4, 5] # 等价于aList[2:5]
>>>bSlice = slice(4)
>>>aList[bSlice]
[1, 2, 3, 4] # 等价于aList[:4]
>>>cSlice = slice(1, 5, 2)
>>>aList[cSlice]
[2, 4] # 等价于aList[1:5:2]
```

这段代码已经基本说明了 slice 类的几种用法。与直接使用切片操作符相比，一点不同是 slice 可直接指定 stop 来切片。
