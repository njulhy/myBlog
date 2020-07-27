---
title: 西瓜书decision tree
author: lihy
tags:
  - 2020夏
  - decisionTree
  - machineLearing
top: false
cover: false
date: 2020-07-27 22:03:17
categories: 笔记
summary: 西瓜书第四章
img:
coverImg:
---

## 整体结构

&emsp;&emsp;不断根据选择属性划分节点，划分过程是递归的。  
&emsp;&emsp;四个点：

1. **划分选择**
2. **过拟合**和**欠拟合**
3. **连续值处理**
4. **和缺失处理**

## 划分选择

&emsp;&emsp;西瓜书列出了三种划分选择，分别为：

1. information gain（ID3 算法采用）
   - 定义 information entropy 度量 D 的纯度（k 为类别，$p_k$表示 k 类样本的比例，常见的熵定义）:$$Ent(D)=-\sum_{k=1}^{|y|}p_k\log_2p_k$$
   - informatiokanqilain gain 拓展了上式（V 表示当前划分节点的属性的取值：$$Gain(D,a)=Ent(D)-\sum_{v=1}^V \frac{|D|^v}{|D|} Ent(D^v)$$
   - 选择 information gain 最大的属性划分节点，但其对可取值数目较多的属性有所偏好
2. gain ratio（C4.5 算法采用）
   - 因为 information gain 对可取值数目较多的属性有所偏好，其原作者做出改进后诞生了 gain ratio 算法
   - $$Gain\_ ratio(D,a)= \frac{Gain(D,a)}{IV(a)}$$
      while$IV(a)=-\sum_{v=1}^V \frac{|D|^v}{|D|} \log_2 \frac{|D|^v}{|D|}$, `IV(a)`为属性 a 的 intrinsic value.
   - gain ratio 对可去植树木较少的属性有所偏好，因此 C4.5 并非直接使用 gain ratio，而是选用启发式的策略：先找出信息增益高于平均的再选增益率最高的。
3. Gini index(CART(Classification and Regression Tree)使用)
   - 定义 Gini value 度量 D 的纯度$$Gini(D)=\sum_{k=1}^{|y|}\sum_{k'\neq k}p_kp_{k'}=1-\sum_{k=1}{|y|}p_k^2$$直观上是 D 中随机抽取两个样本，类别标记不一致的概率
   - $$Gini\_ index(D,a)=\sum_{v=1}^{V} \frac{|D|^v}{|D|} Gini(D^v)$$
   - 选择 Gini index 最小的属性划分节点

## 过拟合、欠拟合

&emsp;&emsp;由于节点划分直至叶节点，因此欠拟合的风险不会直接出现（训练集太小带来的欠拟合和算法无关），主要处理过拟合。主要采取剪枝措施：
pruning:

1. prepruning：在decision tree生成过程中，对每个节点在划分前估计，若划分不能带来决策树泛化性能提升，当前节点不划分直接标记为叶节点
   - 优势：减小过拟合风险；显著减小训练时间和测试时间开销
   - 劣势：一步判断节点是否提升泛化性能的“贪心”本质可能带来欠拟合风险
2. post-pruning：在decision tree生成后，自底向上地对非叶节点进行考察，若节点对应的子树替换为叶节点提升泛化能力，将子树替换为叶节点
   - 优势：欠拟合风险很小，泛化性能好
   - 劣势：需要对每个非叶节点逐一考察，时间开销比预剪枝和非剪枝decision tree都大很多

## 连续值处理

&emsp;&emsp;**连续属性离散化**，最简单的策略是使用二分法，C4.5使用了这个机制

$$Gain(D,a)=\max_{t\in T_a} Gain(D,a,t)=\max_{t\in T_a} Ent(D)-\sum_{\lambda\in\{-,+\}} \frac{|D|_t^\lambda}{|D|} Ent(D_t^\lambda)$$
&emsp;&emsp;与离散属性不同，若当前节点划分属性为连续属性，该属性还可作为其后代的划分属性。

## 缺失值处理

&emsp;&emsp;**连续属性离散化**，最简单的策略是使用二分法，C4.5使用了这个机制

$$Gain(D,a)=\max_{t\in T_a} Gain(D,a,t)=\max_{t\in T_a} Ent(D)-\sum_{\lambda\in\{-,+\}} \frac{|D|_t^\lambda}{|D|} Ent(D_t^\lambda)$$
&emsp;&emsp;为每个样本$\mathbf{x}$赋予权重，对属性a来说仅根据没有缺失值的样本子集判断属性，然后将缺失值样本以不同的概率划分到不同的子节点中去。公式打起来麻烦不打了。

## 多变量决策树

&emsp;&emsp;上述决策树的决策边界都与某个坐标轴平行，然而真实分类边界比较复杂是，必须使用多段划分才能得到较好的近似。
&emsp;&emsp;对于多变量决策树，非叶节点不在针对某个属性，而是对属性的线性集合进行测试。每个非叶节点是一个形如$$\sum_{i=1}^d\omega_ia_i=t$$的线性分类器。

## 其他

&emsp;&emsp;一些决策树学习可以实现增量学习（incremental learning），如ID4、ITI等，但降低开销的同时模型会和基于全部数据训练得到的有较大差别。
