---
title: hexo-matery多行公式不换行
author: lihy
tags:
  - 2020夏
  - hexo-renderer
  - matery
top: false
cover: false
mathjax: false
date: 2020-07-24 14:11:48
categories: 笔记
summary: hexo-matery矩阵不换行
img:
coverImg:
---

### matery矩阵不换行

&emsp;&emsp;这是因为渲染引擎和mathjax部分字符冲突。可以选择更换渲染引擎。hexo自带的renderer engine是hexo-renderer-marked, 后面有人修改了一下自带的renderer engine命名为[hexo-renderer-kramed](https://www.npmjs.com/package/@neilsustc/markdown-it-katex)，但是只是做出了[小小的改进：](https://github.com/hsfzxjy/hexo-renderer-kramed/blob/master/lib/renderer.js)，主要改进如下：

```javascript
// Change inline math rule
function formatText(text) {
  // Fit kramed's rule: $$ + \1 + $$
  return text;
  // return text.replace(/`\$(.*?)\$`/g, '$$$$$1$$$$');
  // return text;
}
```

&emsp;&emsp;对于一些hexo主题似乎解决了问题。但是至少对本文使用的matery主题还是存在冲突问题。然后本人在主题的一个[issue这里](https://github.com/blinkfox/hexo-theme-matery/issues/119)找到了正常显示公式的办法，只可惜我使用了netlify来自动deploy，修改本地的inline规则没有帮助。

&emsp;&emsp;这之后，又在github上面寻找其他的渲染引擎。直到[hexo-renderer-markdown-it-plus](https://github.com/CHENXCHEN/hexo-renderer-markdown-it-plus)出现，看见它支持Katex，便想试一试。首先更换renderer engine，一般操作是：

```bash
npm un hexo-renderer-marked --save
npm i hexo-renderer-markdown-it-plus --save
```

&emsp;&emsp;当然本人使用的是yarn包管理器，操作大同小异。在安装了新的renderer后还需要开启Katex，在开发者口中Katex不能正确显示数学公式：
Katex plugin is enabled by default. However, this renderer alone does not work out of the box for mathematical formulas to display correctly on your website. Therefore, you do not need to do anything if you do not want to use Katex. Otherwise, if you want to use katex, you must add this css style to your website:
`https://cdnjs.cloudflare.com/ajax/libs/KaTeX/0.9.0/katex.min.css`
&emsp;&emsp;死马当活马医。首先要在博客根目录的_config.yml写入

```_config.yml
markdown_it_plus:
    highlight: true
    html: true
    xhtmlOut: true
    breaks: true
    langPrefix:
    linkify: true
    typographer:
    quotes: “”‘’
    pre_class: highlight
```

然后需要写入Ketax的css style，随便找个layout.js用得到的js文件就行（我放入了\matery\layout\_partial\head.js），写入`<link rel="stylesheet" type="text/css" href="https://cdnjs.cloudflare.com/ajax/libs/KaTeX/0.11.1/katex.min.css">`，试了试，成功了。。反正Katex那里不好使看不出来，只要正常显示就行。
&emsp;&emsp;之后又发现了[upupming/hexo-renderer-markdown-it-plus](https://github.com/upupming/hexo-renderer-markdown-it-plus#readme)。这个是原renderer的fork修改版本，添加了一些插件。说实话人本来就支持markdown-it系列的插件，但是既然是修改版，就用了吧。先更换renderer如下：

```bash
npm un hexo-renderer-marked --save
npm i @upupming/hexo-renderer-markdown-it-plus --save
```

&emsp;&emsp;主题配置中改为：

```_config.yml
markdown_it_plus:
  render:
    html: true						
    xhtmlOut: false
    breaks: true
    linkify: true
    typographer: true
    quotes: '“”‘’'
  plugins:
  anchors:	
    level: 2
    collisionSuffix: 'v'
    permalink: true
    permalinkClass: header-anchor
    permalinkSide: 'left'
    permalinkSymbol: ¶
```

&emsp;&emsp;前文中加入的支持Katex的css style就不用动了。大功告成
