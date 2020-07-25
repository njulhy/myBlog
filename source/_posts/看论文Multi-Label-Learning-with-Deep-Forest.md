---
title: 看论文Multi-Label Learning with Deep Forest
author: lihy
tags:
  - 2020夏
  - Multi-Label
top: false
cover: false
mathjax: true
date: 2020-07-24 12:35:41
categories: 看论文
summary: 总结论文Multi-Label Learning with Deep Forest
img:
coverImg:
---

## 直接画图

&emsp;&emsp;多标签学习的 crucial task: how to leverage label correlations in building models. Deep neural netowrk methods usually jointly embed the feature and label information into a latent space to exploit label correlations.  
&emsp;&emsp;深度森林不依赖反向传播，参数少（如深度等各种超参数）
<img src="https://s1.ax1x.com/2020/07/25/UzPI81.jpg">
&emsp;&emsp;上图对于本文算法的灵感来源、优势、mechanisms、contributions、generating notes、forest block 等做了总结

## 名词解释

1. $F: X->[0, 1]^l$方程
2. $H: X->{0, 1}^l$带阈值的 F（后文 H 似乎引用了 F）
3. $\mathbf{X}$: sample matrix$\left[\begin{matrix}x_1\\
   x_2\\
   \vdots\\
   x_n\end{matrix}\right]$
4. $\mathbf{Y}$: 真实的 Label matrix$\left[\begin{matrix}y_1\\
   y_2\\
   \vdots\\
   y_n\end{matrix}\right]$
5. $\mathbf{Y}_i: \mathbf{Y}$的第 i 行
6. $f_{ij}$: i-th instance 在 j-th 标签的 confidence score
7. $h_{ij}$: i-th instance 的 j-th label 预测
8. $rank_F(x_i, j):x_i$在 j-th label 的排序
9. $Y_i^+, Y_i^-$: relevant/irrelevant note
   e.g., if $f(x_i)=[0.2, 0.8, 0.4]$, <br>then $h(x_i)=[0, 1, 0]$ with threshold=0.5, $Y_i^+={1, 3}, Y_i^-={2}, rank_F(x_i, 2)=1$
10. $\mathbf{H}^t$: representation of layer t, $\left[\begin{matrix}h (x_1)\\
   h(x_2)\\
   \vdots\\
   h(x_n)\end{matrix}\right]$
11. $\mathbf{G}^t: \mathbf{H}^t$结合$\mathbf{G}^{t-1}$形成的 representation，达到 feature resue, $\left[\begin{matrix}g(x_1)\\
    g(x_2)\\
    \vdots\\
    g(x_n)\end{matrix}\right]$
12. $\mathbf{P}:\mathbf{H}^t$的平均
13. $p_{ij}: Pr[\hat{y}_{ij}=1]$
14. $\alpha_i, \alpha_j$：表 confidence
    e.g., $p_{. j}=[0.9, 0.6, 0.4, 0.3],\alpha_j=\frac{1}{4}(0.9+0.6+0.6+0.7)=0.7$
15. M: measure
16. $\mathbf{m}_j^t$：度量 M 在 layer t 的值，j 表示 label-based 是一个列，i 则表示一行
17. S:confidence set，对 layer t，产生$\mathbf{H}^t$，取$\mathbf{H}_{. j}^t$计算$\alpha_j^t$, 根据$(\mathbf{H}_{. j}^t, \mathbf{H}_{. j})$计算$\mathbf{m}_j^t$, 若$\mathbf{m}_j^t$比$\mathbf{m}_j^{t-1}$差，$S=S\cup{\alpha_j^t}$，这记录了每一次度量下降？
18. $\theta_t$: compute threshold on S
19. $\mathbf{p}[t]$: performance of layer t on measure M with $\mathbf{G}^t$

## 文章图表

<img src="https://s1.ax1x.com/2020/07/25/UxN9IJ.jpg">
<center>算法一</center>

&emsp;&emsp;对每一层来根据行或者列（例相关或标签相关）的confidence来决定是否进行feature resue，这里的feature resue十分简单，如果表现不足直接用上一层该列/行来代替。想法：这肯定有改进的空间吧？比如借鉴残差网络
<img src="https://s1.ax1x.com/2020/07/25/UxNFR1.jpg">
<center>算法二</center>

&emsp;&emsp;对每一层根据行或者列（例相关或标签相关）先计算其confidence，然后计算对于某个度量M的表现，如果该度量在该列/行performance下降，那么将confidence并入集合S。如果是标签相关，对每一个performance下降的标签都进行了记录。想法：每个标签相关性肯定不会都较好，那这里阈值的统一感觉不是很恰当。能不能不同标签的阈值不同呢？比如记录上一层的confidence，还是说这里本身就是多层的平均，可是算法描述完全看不出来。
<img src="https://s1.ax1x.com/2020/07/25/UxN8QP.jpg">
<center>算法三</center>

1. 每一层先训练森林生成该层的分类器$h_t$;
2. 根据分类器得到分类结果$\mathbf{H}_t=h_t([\mathbf{X},\mathbf{G}_{t-1}])$
3. 根据算法二计算阈值(t>2时)
4. 根据算法一得到$\mathbf{G}^t$
5. 根据$\mathbf{G}^t$计算度量M在该层的表现$\mathbf{p}[t]$
6. 根据$\mathbf{p}[t]$来更新最好表现层和停止步骤。
7. 将$layer_t$加入model set: $S=S\cup layer_t$(这里S不是跟算法2混淆了吗)

<img src="https://s1.ax1x.com/2020/07/25/UxtOx0.jpg">
<center>图一</center>

&emsp;&emsp;想法：关于森林的选取，后文给出了一些说明，但是为什么不选更多种类的森林呢？那样不是更有多样性么？还有这里没有提到多粒度扫描是为什么
<img src="https://s1.ax1x.com/2020/07/25/Uxt5qS.jpg">
<center>表一</center>

&emsp;&emsp;六种多标签评估方法，其中hamming loss和macro-AUC是label-based, 其余是instance-based
<img src="https://s1.ax1x.com/2020/07/25/UxNSZF.jpg">
<center>表二</center>

&emsp;&emsp;这里说$p_{i.},p_{.j}$降序排列，只是为了好计算。开始没搞懂，但是人家只是拿来算一下度量的performance。
<img src="https://s1.ax1x.com/2020/07/25/UxNUoQ.jpg">
<center>表三</center>

&emsp;&emsp;文章使用的数据集

## 文章结果

<img src="https://s1.ax1x.com/2020/07/25/UxNyLT.jpg">
<center>表四</center>

&emsp;&emsp;将本文方法与三种ensemble methods, DNN method, 以及MLFE方法对比
<img src="https://s1.ax1x.com/2020/07/25/UxNoy6.jpg">
<center>表五</center>

&emsp;&emsp;选其他多标签分类树来做森林的block，同样取得良好效果，说明了方法的灵活性。
<img src="https://s1.ax1x.com/2020/07/25/UxN2o4.jpg">
<center>图二</center>

&emsp;&emsp;对比说明feature reuse的有效性
<img src="https://s1.ax1x.com/2020/07/25/UxNWFJ.jpg">
<center>图三</center>

&emsp;&emsp;将本文方法与RF-PCT对比说明feature reuse的有效性
<img src="https://s1.ax1x.com/2020/07/25/UxNIQx.jpg">
<center>图四</center>

&emsp;&emsp;考量了样本相关性。想法：直接使用删除一个标签来考量对另一个标签的影响似乎异常直观，别的方法是怎么做的呢？
