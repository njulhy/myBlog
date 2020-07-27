---
title: 看论文Deep forest
author: lihy
tags:
  - 2020夏
  - deep forest
top: false
cover: false
date: 2020-07-27 07:57:11
categories: 看论文
summary: 一年前就应该看的gcForest论文
img:
coverImg:
---

## 写在开头

&emsp;&emsp;说来惭愧，这篇论文至少应该是一年前读懂的。研一倏然落下帷幕，论文没有看更不用说写了。看的是 Deep Forest 却连决策树也不是很清楚怎么一回事。今天去总结一下决策树另外看看能不能用一下常见的 DNN 网络。

## gcForest

老生常谈的话题：**浅层DNN为何不能成功？diversity的表征？**  
文章主体：

1. DNN:

   - 模型：可通过 bp 训练的 differentiable nonlinear modele
   - 成功原因：
     1. sufficienetmodel complexity：众所周知 DNN 复杂程度很高，而其他机器学习方法复杂度都有限
     2. layer-by-layer processing：即便是浅层的 DNN，其复杂度也可以无限增长，但是却不能取得成功，因此多层学习对于特征表征和集成是不可或缺的
     3. in model feature transformation：DNN 的级联结构被认为完成了这个功能，然而其他机器学习方法很难做到这一点
   - 缺点：
     1. hyper-parameters 非常多，这导致 DNN 训练仿佛成为艺术而非科学
     2. DNN 是个黑箱，理论分析很困难
     3. DNN 需要大量数据训练，对数据集要求太高
     4. 大数据时代，数据的标注很少，标注昂贵
   - 一个推测（这似乎是事实）
     1. DNN复杂度大幅超过了所需，一些技巧如adding shortcut connection, pruning, binarization都验证了这个猜测
2. gcForest：
   - 问题提出: Can deep learning be realized with non-differentiable modules?
   - 该工作
       1. 文献[65]的拓展
       2. 受到[63]的启发，该文献主要是将Ensemble learning，阐述了diversity的重要性。常见的enhance diversity方法有四种：
           - data sample
               1. Bagging: bootstrap
               2. AdaBoost: Squential important sampling
           - input feature: Random Subspace approach[24]
           - learning parameters
               1. [28]different initial weights
               2. [37]different split selections
           - output representation
               1. [10]EDOC approach
               2. [4]Flipping OUtput method
   - 特性：
       1. 级联结构
       2. 多粒度扫描——表征学习——aware，这使得复杂度自适应，适用于小规模应用，算力控制
       3. less hyper-parameters
       4. robust
   - meaning:
       1. DL = DNNs?或者说深度学习只能使用可微模型？
       2. 不用bp是否可行
       3. 相对于DNN，在特定种类的任务上深度森林是否得天独厚？一个事实是Kaggle比赛上XGBoost早已成为最流行的方法
