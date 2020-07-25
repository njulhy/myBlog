---
title: hexo主题定制
author: lihy
tags:
  - 2020夏
  - hexo
  - matery相册
  - 域名绑定
top: false
cover: false
mathjax: false
date: 2020-07-24 12:51:05
categories: 教程
summary: 域名、相册、特效定制等
img:
coverImg:
---

本文是[“快速搭建个人网站”](https://njulhy.com/2020/07/19/kuai-su-da-jian-ge-ren-wang-zhan/)的续篇，**特此说明**

## 1 个人定制

&emsp;&emsp;大部分需要定制的已经在模板修改好了。写作前面的部分已经让本人难以为继，如果以后有心会完善后面的部分。有问题去留言板讨论或者联系本人，您可以在博客网站找到本人的联系方式。  
&emsp;&emsp;**请仔细查阅根目录的\_config.yml 和主题目录的\_config.yml 来定制个人内容**，一些需要修改的如网站名称等在根目录的\_config.yml 和主题目录的\_config.yml 手动查找修改即可，这能满足大部分的需求
&emsp;&emsp;**主题目录下的 README_CN.md 对主题的很多定制给出了详细的说明**，很多博客的部分定制内容都是摘抄主题的 readme 文档。

### 1.1 使用自己的域名

&emsp;&emsp;netlify 生成的网址冗长而无意义，您可以自行购买域名。只是搭建个人博客网站的话您可以在腾讯云购买到首年 8 元的域名（不是打广告，阿里大概也一样），如果坚持使用.com, .cn 等比较“高级”的域名，大概每年 50-60 元（可以少吃一顿火锅了）。  
&emsp;&emsp;在购买自己的域名后，需要将之与您的博客网站联系起来。这里您有**两个操作**需要完成。

1. 在域名购买处修改**DNS 解析**（这样地址栏输入的网址就会指向 netlify 了）。netlify 建议您使用两条**CNAME**记录：如下图所示（这是腾讯购买的域名）
   <img src="https://s1.ax1x.com/2020/07/23/UOIdmQ.png">
   &emsp;&emsp;主机记录@是指您注册的域名，这条记录将您申请到的域名如`xxx.com`指向 netlify 官网；另一条的主机记录 WWW 是确保地址栏输入`www.xxx.com`也能正确指向。
2. 登录 netlify，打开您发布的网站。点击`Domain settings`（在这里你还可以看到`git commit -m "提交说明"`中的*提交说明*部分，因为 netlify 是自动部署的~）
   <img src="https://s1.ax1x.com/2020/07/23/UOTKIA.jpg">
   然后点击`add domain alias`，添加您购买的域名即可。
   <img src="https://s1.ax1x.com/2020/07/23/UOTDzV.jpg">

&emsp;&emsp;如果想要加快网站响应速度，需要自行购买一些域名解析之类的服务。cloudfare 是一个可行的选择，您可以使用 cloudflare 来加速网站并且保护网站信息安全，cloudflare 也提供了免费套餐，但是意义不大。（至少我本人使用没感觉加快什么……此外我的域名是在国外的 godaddy 网站申请的，这网站的域名解析不能使用@记录指定网址，@只能指向 ipv4 地址。。而如果您本身在国内申请的我不觉得会发生@记录不能指向网址的情况，而指定了 netlify 后\*\*netlify 会自动采取一些加速措施）

### 1.2 实现个人相册并加密相册

&emsp;&emsp;本文实现的相册参考了[这篇博客](https://yafine66.gitee.io/posts/3b98.html#%E6%9F%A5%E7%9C%8B%E6%95%88%E6%9E%9C)，但是界面部分根据友情链接界面做了个人修改。您只需要修改相册配置文件和修改一下自己存放图片的网址即可，具体操作参考上面这篇博文。  
&emsp;&emsp;此外[这篇博客](https://hehung.top/2019/14ec1095.html)的相册制作可能比较符合一些小伙伴的兴趣，当然直接更改相册相关的 layout 和 css 文件可能更加容易。有兴趣的小伙伴可以美化一下相册布置。

### 1.3 hexo 实现 mermaid 图表功能

&emsp;&emsp;首先感谢写插件的人。  
&emsp;&emsp;主题本身并没有支持 mermaid，但是我们可以通过插件的方式来自行使用此功能。你可以通过复制我的操作或者参考官方教程，官方教程在[这里](https://github.com/webappdevelp/hexo-filter-mermaid-diagrams)。

1. 安装该插件，在博客根目录下打开 cmd 或者 git bash 输入`yarn add hexo-filter-mermaid-diagrams`(npm 的命令为`npm install hexo-filter-mermaid-diagrams`)
2. 在**主题配置**中写入

   ```_config.yml
   # mermaid chart
   mermaid: ## mermaid url https://github.com/knsv/mermaid
   enable: true  # default true
   version: "7.1.2" # default v7.1.2
   options:  # find more api options from https://github.com/knsv/mermaid/blob/master/src/mermaidAPI.js
      #startOnload: true  // default true
   ```

   <img src="https://s1.ax1x.com/2020/07/24/Uj8JfK.jpg">

3. 在主题的 layout.ejs 中写入

   ```ejs
   <% if (theme.mermaid.enable) { %>
   <script src='https://unpkg.com/mermaid@<%= theme.mermaid.version %>/dist/mermaid.min.js'></script>
   <script>
      if (window.mermaid) {
         mermaid.initialize({theme: 'forest'});
      }
   </script>
   <% } %>
   ```

   <img src="https://s1.ax1x.com/2020/07/24/Uj8GY6.jpg">

### 1.4 其他个人定制

&emsp;&emsp;对于其他个人定制点击[这篇博客](https://qvchuang.top/archives/d3c10307.html#toc-heading-5)，里面对于内容定制给出了较为详尽的说明。但是对于诸如“樱花飘落”、“天气信息”、“每日诗词”、“闲聊么”、“tidio”等功能并不推荐（如果您朋友多可以添加一个闲聊么，如果觉得诗词提升气质可以添加一个每日诗词，如果想搞个简易客服可以添加一个 tawk.to——这是因为我当时申请 tidio 似乎要收费。。）。与我而言，这些意义不大而且会拖慢本不快的响应速度。您也可以删除本模板的雪花飘落、鼠标拖尾等特效来使响应速度加快。

&emsp;&emsp;推荐您采纳[这篇博客](https://sunhwee.com/posts/6e8839eb.html#toc-heading-44)第三部分的优化操作。当然不想折腾还是就此作罢，毕竟前面的操作已经足够使人精疲力竭了。

&emsp;&emsp;生活愉快，不断进步~~~
