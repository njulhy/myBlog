---
title: 看论文MLAL survey
author: lihy
top: false
cover: false
mathjax: true
tags:
  - 看论文
  - 2020夏
date: 2020-07-21 17:29:48
img:
coverImg:
categories: 看论文
summary: 总结一下MLAL综述论文的内容
keywords: 多标签主动学习综述
---

## 展开方式

&emsp;&emsp;主动学习示意图
<img src="https://www.researchgate.net/profile/Xinpeng_Xie/publication/326264220/figure/fig1/AS:646324261232642@1531107120662/The-process-of-active-learning-Ambiguous-uncertain-samples-are-selected-for-oracle-to_W640.jpg">

<center>图1</center>

&emsp;&emsp;文章主要从**Sampling Granularity, Informativeness,Measure, Annotation**三个层面对多标签主动学习算法展开讲解，通过总结 36 篇 paper 的内容完成，示意图如下
<img src="https://s1.ax1x.com/2020/07/21/UoOJtP.jpg">

<center>图2</center>

## 论文理解

&emsp;&emsp;可以从图 2 看到主要是 18 年之前的论文

| item                      | description                                                                                         |
| ------------------------- | --------------------------------------------------------------------------------------------------- |
| _Sampling Granularity_    | 现如今的主流是*Example-label-based*<br>*Batch-mode-based*值得探索，不知道和批量梯度下降联系如何 |
| _Informativeness Measure_ | *Uncertainty Metric*必选<br>*Label Correlation*已经成为主流，其他几种手段也十分常用       |
| _Annotation_              | 人类专家标记必选，图一没有细分非专家的人类，<br>*Voting committee*也值得探索                          |

&emsp;&emsp;下图是作者认为未来需要得到改进和发展的点
<img src="https://s1.ax1x.com/2020/07/21/UoOGkt.jpg">

<center>图3</center>

---

<h2>题外学到的东西</h2>
<div class="container">
  <h3>markdown表格画法</h3>
  <div class="card bg-dark text-white">
    <table>
        <tr>
          <td>问题描述</td>
          <td>markdown语法好像没有表格合并</td>
        </tr>
        <tr>
          <td>解决</td>
          <td><a herf="https://blog.csdn.net/loongshawn/article/details/72829090">使用html语法来制作表格</a></td>
        </tr>
        <tr>
          <td rowspan="3">具体</td>
          <td>table标签进入表格环境，tr标记描述一整行，嵌入td描述该行每一格子的内容</td>
        </tr>
        <tr>
          <td>在tr标记中使用rowspan和colspan描述合并行和列的数量
        </tr>
        <tr>
          <td>讲道理有点繁琐，markdown识别html语法时似乎对于标记技术不敏感，使用空行较好
        </tr>
    </table>
  </div>
</div>

<div class="container">
  <h3>在markdown中使用卡片</h3>
  <div class="card">
    <table>
      <tr>
        <td>问题描述</td>
        <td>如何在markdown中使用卡片来使得一些东西更突出
      </tr>
      <tr>
        <td>解决方法</td>
        <td>使用html语法
      </tr>
      <tr>
        <td rowspan="4">一些问题
        <td>似乎不能渲染卡片的颜色等操作
      </tr>
      <tr>
        <td>卡片中画表格使得右边留空白不美观
      </tr>
      <tr>
        <td>没有什么必要。。
      </tr>
    </table>
  </div>
</div>

<div class="container">
  <h3>html语法br和a标记的使用</h3>
  <div class="card">
    <table>
      <tr>
        <td>br标记表示换行
      </tr>
      <tr>
        <td>a标记表示引入链接
      </tr>
    </table>
  </div>
</div>

<div class="container">
  <h3>html语法br, a, code, pre标记的使用</h3>
  <div class="card">
    <table>
      <tr>
        <td>br标记表示换行
      </tr>
      <tr>
        <td>a标记表示引入链接
      </tr>
      <tr>
        <td>code和pre标记表示引入代码，pre引入多行代码
      </tr>
    </table>
  </div>
</div>

<div class="container">
  <h3>MathJax无法渲染多行公式</h3>
  <div class="card">
    <table>
      <tr>
        <td>问题描述：
        <td>矩阵、大括号等语法在matery主题中失效
      </tr>
      <tr>
        <td rowspan="2">解决方法：
        <td>在博客根目录下，找到node_modules/kramed/lib/rules/inline.js文件，修改inline变量
      </tr>
      <tr>
        <td>将原来的<code>em: /^\b_((?:__|[\s\S])+?)_\b|^\*((?:\*\*|[\s\S])+?)\*(?!\*)/,</code><br>修改为<code>em: /^\*((?:\*\*|[\s\S])+?)\*(?!\*)/,</code>
      </tr>
    </table>
  </div>
</div>
