---
title: 使用爬虫增加csdn访问量
author: lihy
tags:
  - 2020秋
  - 爬虫
  - csdn
top: false
cover: false
date: 2020-10-07 19:07:22
categories: python
summary: 使用爬虫增加csdn访问量
img:
coverImg:
---


&emsp;&emsp;<b>本文代码地址为：[https://github.com/njulhy/funny_code/blob/main/spider/access_csdn.py](https://github.com/njulhy/funny_code/blob/main/spider/access_csdn.py)（如果用到还望标个星）。代码实现了

1. 使用多种请求头
2. 使用多个匿名代理
3. 自动获取某博主所有博文
4. 延时爬取

来避免被反爬。</b><br>
&emsp;&emsp;使用爬虫可以短时间迅速增加博文访问量。由于这几个月才开始写博客，也没什么推广，就想到自己写爬虫增加点访问量。

## 图谱

<img src="https://img-blog.csdnimg.cn/20201007192945448.png">

## 导入本文所用库

&emsp;&emsp;本文使用了下面几个库，运行下文代码前先导入它们：

```python
import re # 正则表达式
import requests # 模拟浏览器访问url地址
import time
import random
import os
```

## 获取所有博文的 url 地址

&emsp;&emsp;<b>csdn 的个人主页格式为</b>：https://blog.csdn.net/your_id/article/list/pages，其中your_id是你当初注册就产生的，page是博文列表的页数（如果博文较少一般只有一页）。
&emsp;&emsp;我们可以主页的源码获取博文的 url 地址，下面代码是获取我本人的所有博文地址，如果<b>想自用需要修改的是 pattern 和 pages 两个地方。运行下面的代码来得到所有博文 url 地址的列表：</b>

```python

def get_blog_url(url, pages=1):
    pattern = r"(https://blog.csdn.net/qq_34769162/article/details/[\d]+)"
    blog_url = []
    headers = {'user-agent': 'Mozilla/5.0 (Linux; Android 8.0.0; SM-G960F Build/R16NW) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/62.0.3202.84 Mobile Safari/537.36'}
    for i in range(1, pages+1):
        try:
            response = requests.get(url+"{}".format(i), headers=headers)
        except:
            print("OOps, we can't get your blog list")
            os._exit(0)
        blog_url += re.findall(pattern, response.text)
    return blog_url
all_blog_url = get_blog_url(url="https://blog.csdn.net/qq_34769162/article/list/", pages=2)
```

## 构造请求头

&emsp;&emsp;请求头最重要的是 user-agent，其他项如果没有发现很快被反爬可以不用管。我从网址[https://deviceatlas.com/blog/list-of-user-agent-strings#android](https://deviceatlas.com/blog/list-of-user-agent-strings#android)提取了大约 50 个手机浏览器的 user-agent。运行下面的代码来得到这些 user-agent 的列表：

```python
def get_user_agent(url):
    pattern = r'<tbody><tr><td>(.*?)</td>'
    headers = {'user-agent': 'Mozilla/5.0 (Linux; Android 8.0.0; SM-G960F Build/R16NW) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/62.0.3202.84 Mobile Safari/537.36'}
    try:
        response = requests.get(url=url, headers=headers)
    except:
        print("OOps, we can't get user agent list, you should fix it manully")
        os._exit(0)
    user_agent = re.findall(pattern, response.text)
    return user_agent
all_user_agent = get_user_agent("https://deviceatlas.com/blog/list-of-user-agent-strings#android") # 获取包含若干user_agent的列表

```

## 使用 proxey

&emsp;&emsp;request 库支持 http，https，socks，我从网上找了一些免费的匿名 http 服务器代理服务器（源码处有说明）并将它们格式化为 python 列表，下面只列出三个服务器的地址及端口：

```python
http_pro = [
    '134.122.120.49:8118',
    '54.152.162.43:80',
    '52.251.79.6:3128'
    ]
```

## 开始使用

&emsp;&emsp;首先我们用上文的函数得到博文 url 和 user_agent，然后设置循环 5000 轮（每篇博文增加 5k 访问），加上 proxey，以及设置了短暂的延时：

```python
if __name__ = "__main__"
	all_blog_url = get_blog_url(url="https://blog.csdn.net/qq_34769162/article/list/", pages=2)
    all_user_agent = get_user_agent("https://deviceatlas.com/blog/list-of-user-agent-strings#android")

    i = 0
    while i%len(all_blog_url)<5000:
        proxies = {'http': '{}'.format(random.choice(http_pro))}
        # url = random.choice(all_blog_url) # 随机挑选一个访问
        url = all_blog_url[i%len(all_blog_url)] # 按照顺序依次访问
        headers = {'User-Agent': random.choice(all_user_agent)}

        i += 1
        try:
            requests.get(url, headers=headers, proxies=proxies)
        except:
            print("something wrong")
            os._exit(0)
        time.sleep(random.random()/1000)
        if i%(len(all_blog_url)*5)==0:
            print("this is the {}th time".format(int(i/len(all_blog_url)))) # 每循环五次输出一下
```

## 效果

&emsp;&emsp;2020/10/7 早上开始写代码时访问总量 2w 出头，晚饭没吃的时候到了 6w.
## 结语

&emsp;&emsp;使用爬虫增加访问量是不当的行为，不应该滥用，努力提升博文质量才是正道。
&emsp;&emsp;欢迎评论交流以及指点~
