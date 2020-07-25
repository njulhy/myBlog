---
title: hexo主题多行公式显示问题
author: lihy
tags:
  - 2020夏
  - hexo-renderer
  - matery
top: false
cover: false
mathjax: false
date: 2020-07-24 14:11:48
categories: 网站搭建
summary: hexo-matery多行公式显示问题
img:
coverImg:
---

### 不使用 mathjax，使用 Katex

&emsp;&emsp;这是因为渲染引擎和 mathjax 部分字符冲突，可以选择使用Katex。Katex是一个轻量级的快速的公式排版库。您不必太担心显示的问题，毕竟这是全世界人都在用的东西。  
&emsp;&emsp;您可以直接参考[这里](https://katex.org/docs/autorender.html)，或者借用我的经验。  
&emsp;&emsp;直接在您使用的主题渲染会用到的页面（任意一个后缀为.ejs 且渲染时会加载的文件——实际上 hexo 是将您的各个界面借助 ejs 文件来生成 html）里面加入以下代码。举例来说，如果您使用了 matery 主题，您可以加在`matery\layout\layout.ejs`, `matery\layout_partial\head.ejs`等等都可以。这些 ejs 文件在每一个 markdown 转化的时候都用得到。

```javascript
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.12.0/dist/katex.min.css" integrity="sha384-AfEj0r4/OFrOo5t7NnNe46zW/tFgW6x/bCJG8FqQCEo3+Aro6EYUG4+cU+KJWu/X" crossorigin="anonymous">
<script defer src="https://cdn.jsdelivr.net/npm/katex@0.12.0/dist/katex.min.js" integrity="sha384-g7c+Jr9ZivxKLnZTDUhnkOnsh30B4H0rpLUpJ4jAIKs4fnJI+sEnkvrMWph2EDg4" crossorigin="anonymous"></script>
<script defer src="https://cdn.jsdelivr.net/npm/katex@0.12.0/dist/contrib/auto-render.min.js" integrity="sha384-mll67QQFJfxn0IYznZYonOWZ644AWYC+Pt2cHqMaRhXVrursRwvLnLaebdGIlYNa" crossorigin="anonymous"
    onload="renderMathInElement(document.body);"></script>
```

&emsp;&emsp;现在您可以去查看效果了。

&emsp;&emsp;介绍下 Katex ：
Simple API, no dependencies – yet super fast on all major browsers.

- Fast: KaTeX renders its math synchronously and doesn’t need to reflow the page.
- Print quality: KaTeX’s layout is based on Donald Knuth’s TeX, the gold standard for math typesetting.
- Self contained: KaTeX has no dependencies and can easily be bundled with your website resources.
- Server side rendering: KaTeX produces the same output regardless of browser or environment, so you can pre-render expressions using Node.js and send them as plain HTML.

### 更换渲染引擎

**其实还是使用 Katex 渲染……绕一大圈还是回去而已**，只不过更换的渲染引擎照它的官方文档会更快（我们体验不到啥的）。

&emsp;&emsp;这是因为渲染引擎和 mathjax 部分字符冲突。可以选择更换渲染引擎。hexo 自带的 renderer engine 是 hexo-renderer-marked, 后面有人修改了一下自带的 renderer engine 命名为[hexo-renderer-kramed](https://www.npmjs.com/package/@neilsustc/markdown-it-katex)，但是只是做出了[小小的改进：](https://github.com/hsfzxjy/hexo-renderer-kramed/blob/master/lib/renderer.js)，主要改进如下：

```javascript
// Change inline math rule
function formatText(text) {
  // Fit kramed's rule: $$ + \1 + $$
  return text;
  // return text.replace(/`\$(.*?)\$`/g, '$$$$$1$$$$');
  // return text;
}
```

&emsp;&emsp;对于一些 hexo 主题似乎解决了问题。但是至少对本文使用的 matery 主题还是存在冲突问题。然后本人在主题的一个[issue 这里](https://github.com/blinkfox/hexo-theme-matery/issues/119)找到了正常显示公式的办法，只可惜我使用了 netlify 来自动 deploy，修改本地的 inline 规则没有帮助。

&emsp;&emsp;这之后，又在 github 上面寻找其他的渲染引擎。直到[hexo-renderer-markdown-it-plus](https://github.com/CHENXCHEN/hexo-renderer-markdown-it-plus)出现，看见它支持 Katex，便想试一试。首先更换 renderer engine，一般操作是：

```bash
npm un hexo-renderer-marked --save
npm i hexo-renderer-markdown-it-plus --save
```

&emsp;&emsp;当然本人使用的是 yarn 包管理器，操作大同小异。在安装了新的 renderer 后还需要开启 Katex，[Katex](https://katex.org/docs/autorender.html)号称其在 web 端渲染很快。但是在本 renderer 的开发者口中 Katex 不能正确显示数学公式：
Katex plugin is enabled by default. However, this renderer alone does not work out of the box for mathematical formulas to display correctly on your website. Therefore, you do not need to do anything if you do not want to use Katex. Otherwise, if you want to use katex, you must add this css style to your website:
`https://cdnjs.cloudflare.com/ajax/libs/KaTeX/0.9.0/katex.min.css`
&emsp;&emsp;死马当活马医。首先要在博客根目录的\_config.yml 写入

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

然后需要写入 Ketax 的 css style，随便找个 layout.js 用得到的 js 文件就行（我放入了\matery\layout_partial\head.js），写入`<link rel="stylesheet" type="text/css" href="https://cdnjs.cloudflare.com/ajax/libs/KaTeX/0.11.1/katex.min.css">`，试了试，成功了。。反正 Katex 那里不好使看不出来，只要正常显示就行。
&emsp;&emsp;之后又发现了[upupming/hexo-renderer-markdown-it-plus](https://github.com/upupming/hexo-renderer-markdown-it-plus#readme)。这个是原 renderer 的 fork 修改版本，添加了一些插件。说实话人本来就支持 markdown-it 系列的插件，但是既然是修改版，就用了吧。先更换 renderer 如下：

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

&emsp;&emsp;前文中加入的支持 Katex 的 css style 就不用动了。大功告成
