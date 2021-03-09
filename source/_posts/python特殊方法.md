---
title: python特殊方法
author: lihy
tags:
  - 2021春
  - python

top: false
cover: false
date: 2021-3-9 19:07:22
categories: python
summary: 介绍python中方法名左右为双下划线的方法
img:
coverImg:
---

<!-- TOC -->autoauto- [什么是Python的特殊方法？](#什么是python的特殊方法)auto- [几个常见特殊方法介绍](#几个常见特殊方法介绍)auto- [使用特殊方法的例子](#使用特殊方法的例子)auto    - [纸牌的例子](#纸牌的例子)auto- [复数例子](#复数例子)autoauto<!-- /TOC -->

本文参考自《流畅的 Python》第一章

## 什么是 Python 的特殊方法？

&emsp;&emsp;本文所指的特殊方法是指方法名左右为双下划线的方法，[点击这里查看 python 特殊方法文档](https://docs.python.org/3/reference/datamodel.html)

1. 需要明确的是，这些方法的存在是为被 Python 解释器调用的。例如`obj.__len__()`这种写法不该出现在代码中，应使用`len(obj)`
2. 此外许多时候特殊方法的调用是隐式的，如`for i in x`中实际用的是`iter(x)`，这背后则是`x.__iter__()`方法——当然 x 类中需要实现该方法。
3. 通过内置函数使用特殊方法，Python 解释器内置了许多函数以及类型，你可以在任意时候使用它们，[点击这里查看内置函数列表](https://docs.python.org/zh-cn/3/library/functions.html)

## 几个常见特殊方法介绍

1. `__getitem__`：这个方法用于在序列中提取片段，如列表、字典的取值操作`obj[i]`背后其实就是`obj.__getitem__`
2. `__len__`：该方法用于得到序列的长度，python 内置函数`len()`背后即是这一特殊方法
3. `__iter__()`：用于迭代序列，内置函数`iter()`的背后方法
4. `__abs__, __add__, __mul__`：实现求绝对值、加法、乘法等。

## 使用特殊方法的例子

### 纸牌的例子

&emsp;&emsp;这个例子实现了扑克牌对象的创建，实现了读取、长度测量、洗牌、排序等操作。
&emsp;&emsp;需要注意的是 python 的文档结构，经典的结构即下代码段所用的结构：起始行、模块文档、模块导入、变量定义、类定义、函数定义、主程序/测试程序

```python
import collections
import random
# 构建名为Card的namedtuple类，该类有两个属性
# namedtuple创建具名元组类型，顾名思义，具名元组与普通元组不同的是数据点有简单的属性
Card = collections.namedtuple('Card', ['suit', 'rank'])
class MyCard():
    # 在MyCard类中，suits和ranks是类变量，因此所有的self.suits, self.ranks与MyCard.suits, MyCard.ranks等价
    suits = "spades diamonds clubs hearts".split()
    ranks = [str(x) for x in range(2, 11)] + list('JQKA')

    def __init__(self):
        self._items = [Card(suit, rank) for suit in self.suits for rank in self.ranks]

    def __getitem__(self, position):
        return self._items[position]

    def __len__(self):
        return len(self._items)

    def __setitem__(self, position, card):
        self._items[position] = card

def card_value(card):
    按照黑桃、红桃、方块、梅花的顺序排序
    suit_values = dict(spades=3, hearts=2, diamonds=1, clubs=0)
    # 在计算卡牌的排序值时，index方法使用了列表元素查询下标的简洁方法
    rank_value = MyCard.ranks.index(card.rank) * len(MyCard.suits) + suit_values[card.suit]
    return rank_value

def test():
    cards = MyCard()
    print(len(cards), cards[:5], sep='\n') # 输出初始化后的前五张牌和扑克牌的顺序
    random.shuffle(cards) # shuffle为就地打乱序列，而非返回一个打乱的新序列
    print(cards[:5]) # 输出洗牌后的前五张牌
    print(sorted(cards, key=card_value)[:5])

if __name__ == '__main__':
    test()
```

## 复数例子

目标：构建负数类型，实现可打印、加法、乘法、绝对值运算

```python
import collections
import math

Vector = collections.namedtuple('Vector', ['x', 'y'])
class MyVector():
    def __init__(self, x, y):
        self._value = Vector(x, y)

    def __add__(self, num):
        return MyVector(self._value.x+num._value.x, self._value.y+num._value.y)

    def __repr__(self):
        return '{}+{}j'.format(self._value.x, self._value.y)

    def __abs__(self):
        return math.sqrt(self._value.x**2 + self._value.y**2)

    def __mul__(self, scalar):
        return MyVector(self._value.x*scalar, self._value.y*scalar)

def test():
    aVector = MyVector(1, 2)
    bVector = MyVector(1.1, 2.2)
    print(aVector, abs(aVector), aVector+bVector, aVector*3, sep='\n')

if __name__ == '__main__':
    test()
```

运行结果如下

<center>
<img src="https://img-blog.csdnimg.cn/20210309160015391.png" align="middle" width=60%>
