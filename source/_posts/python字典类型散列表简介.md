---
title: python字典类型散列表简介
author: lihy
tags:
  - 2021春
  - 散列表
  - 哈希表
top: false
cover: false
date: 2021-3-9 19:07:22
categories: python
summary: 简单介绍python字典类型背后的散列表，解释为什么不能在迭代字典的同时修改字典的内容
img:
coverImg:
---

<!-- TOC -->autoauto- [泛映射类型](#泛映射类型)auto- [dict 背后的散列表](#dict-背后的散列表)auto- [字典如何查询键值对](#字典如何查询键值对)auto- [字典如何添加键值对](#字典如何添加键值对)autoauto<!-- /TOC -->

## 泛映射类型

<center>
<img src="https://img-blog.csdnimg.cn/20210309202628739.png" width=60%>
</center>

&emsp;&emsp;Mapping 和 MutableMapping 作为两个基本的抽象基类，定义了构建一个映射类型所需要的最基本的接口。这两个基类位于 collections.abc 模块中。需要注意的是映射类型一般会直接对 dict 或者 collections.User.Dict 进行扩展，而非直接继承这两个抽象基类。
&emsp;&emsp;可以使用 Mapping 来判定数据是否为广义上的映射类型:

```python
>>> my_dict = {}
>>> isinstance(my_dict, abc.Mapping)
True
```

## dict 背后的散列表

&emsp;&emsp;dict 类型背后是散列表，散列表是一个稀疏数组。如果要把一个对象放入散列表，那么首先要计算这个元素键的散列值。python 中可以用`hash()`方法来做这件事。

> 散列值和相等性：python 内置函数`hash()`可以用于所有的内置类型对象。如果两个对象在比较的时候是相等的，那它们的散列值必须相等。

> 散列值和列表索引：为了让散列值能胜任散列表索引，它们必须在索引空间中尽量分散。即越相似的对象散列值差别应该越大，如`hash(1)`和`hash(1.0001)`的散列值应差距很大。

## 字典如何查询键值对

&emsp;&emsp;为获取 dict[search_key]背后的值，python 首先调用`hash(search_key)`来计算键的散列值，把这个值最低的几位数字当作偏移量，在散列表中查找表元。<b>若表元为空</b>，抛出 KeyError 异常；若表元中包含键值对，但该键与 search_key 不相等，此情况称为散列冲突。
&emsp;&emsp;使用散列表使得 dict 的查询时间不会随着 dict 大小而迅速增大，而是维持在一个稳定的水平——因为只需根据偏移量查找表元。但这样带来了一些限制：

1. 键必须可散列，这要求键支持 hash()函数、支持通过**eq**()方法检测相等性、a===b 与 hash(a)==hash(b)同真假
2. 内存开销巨大：散列表必须是稀疏的，这导致其空间效率低下。在某些情况下，将数据存于元组/具名元组中是更好的方法。

## 字典如何添加键值对

&emsp;&emsp;向 dict 中添加新键而又发生散列冲突时，新键可能会安排到另一个位置。因此对两个字典来说，当添加元素顺序不同时字典是相等的，但两个键可能顺序不同。
&emsp;&emsp;添加新键同时可能使 Python 解释器对字典进行扩容，这会导致散列表中键顺序改变。**因此不能对字典同时进行迭代和修改！**
