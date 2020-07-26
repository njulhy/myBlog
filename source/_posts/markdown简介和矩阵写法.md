---
title: markdown简介和矩阵写法
author: lihy
tags:
  - 2020夏
  - markdown语法
  - markdown矩阵
top: false
cover: false
mathjax: true
date: 2020-07-24 14:19:26
categories: 个人网站
summary: markdown简介和矩阵写法
img:
coverImg:
---

## github 文档

&emsp;&emsp;点击[here](https://guides.github.com/features/mastering-markdown/#syntax)

## markdown 中写矩阵

&emsp;&emsp;基本上是使用 latex 语法

### 基本矩阵

1. 普通矩阵，嵌入文本占用多行

   ```latex
   $$
     \begin{matrix}
     1 & 2 & 3 \\
     4 & 5 & 6 \\
     7 & 8 & 9
     \end{matrix}
   $$
   ```

   效果

   $$
     \begin{matrix}
     1 & 2 & 3 \\
     4 & 5 & 6 \\
     7 & 8 & 9
     \end{matrix}
   $$

2. 行间矩阵

   ```latex
   这是一个行间矩阵$\begin{matrix}
     l&l\\
     j&z
     \end{matrix}$的展示
   ```

   这是一个行间矩阵$\begin{matrix}
      l&l\\
      j&z
      \end{matrix}$的展示

3. 行间小矩阵

   ```latex
       这是一个行间小矩阵$\begin{smallmatrix}
         l&l\\
         j&z
         \end{smallmatrix}$的展示
   ```

   这是一个行间小矩阵$\begin{smallmatrix}
      l&l\\
      j&z
      \end{smallmatrix}$的展示

&emsp;&emsp;**当然您可以在矩阵前后加上括号**，见后文

### 带标号的矩阵

```latex
$$
\begin{matrix}
a&b&c\\
d&e&f\\
f&h&i\\
\end{matrix} \tag{1}
$$
```

效果

$$
  \begin{matrix}
   1 & 2 & 3 \\
   4 & 5 & 6 \\
   7 & 8 & 9
  \end{matrix} \tag{1}
$$

### 带括号的矩阵

&emsp;&emsp;注意这里的`left\{`, `right\}`是花括号矩阵，当然可以使用方括号以及竖线矩阵，将`\{`, `\}`对应修改为`[`, `|`等即可

```latex
$$
 \left\{
 \begin{matrix}
   1 & 2 & 3 \\
   4 & 5 & 6 \\
   7 & 8 & 9
  \end{matrix}
  \right\} \tag{2}
$$
```

效果

$$
 \left\{
 \begin{matrix}
   1 & 2 & 3 \\
   4 & 5 & 6 \\
   7 & 8 & 9
  \end{matrix}
  \right\} \tag{2}
$$

&emsp;&emsp;注意这里的`left\[`, `right\}`是花括号矩阵，当然可以使用方括号以及竖线矩阵，将`\{`, `\}`对应修改为`[`, `|`等即可

### 使用省略号填充矩阵

&emsp;&emsp;省略号和数字字母没什么不同，只需要你记住转义符号而已。

```latex
$$
\left[
\begin{matrix}
 1      & 2      & \cdots & 4      \\
 7      & 6      & \cdots & 5      \\
 \vdots & \vdots & \ddots & \vdots \\
 8      & 9      & \cdots & 0      \\
\end{matrix}
\right] \tag{3}
$$
```

效果

$$
\left[
\begin{matrix}
 1      & 2      & \cdots & 4      \\
 7      & 6      & \cdots & 5      \\
 \vdots & \vdots & \ddots & \vdots \\
 8      & 9      & \cdots & 0      \\
\end{matrix}
\right] \tag{3}
$$

### 带参数的矩阵

在矩阵中画出一条分割线，以强调最右侧一列的特殊性

```latex
$$
\left[
    \begin{array}{cc|c}
      1 & 2 & 3 \\
      4 & 5 & 6
    \end{array}
\right] \tag{4}
$$
```

效果
注：如果是我的博客网站则不显示这条竖线……应该是Katex和hexo-renderer有些字符冲突的原因)


$$
\left[
    \begin{array}{cc|c}
      1 & 2 & 3 \\
      4 & 5 & 6
    \end{array}
\right] \tag{4}
$$

[参考](https://blog.csdn.net/qq_38228254/article/details/79469727)
