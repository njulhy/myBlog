---
title: 从spys.one提取免费socks5代理
author: lihy
tags:
  - 2021春
  - socks5
  - proxy

top: true
cover: false
date: 2021-3-9 19:07:22
categories: python
summary: 从spys.one提取免费socks5代理
img:
coverImg:
---

## 重点

&emsp;&emsp;<b>请访问[github 的代码地址](https://github.com/njulhy/funny_code/blob/main/spider/get_socks_proxy.ipynb)来得到具体的代码和代码（ipynb 文件），代码完全开源，有清晰的代码结构和运行结果。
&emsp;&emsp;可以直接将本文代码粘贴到[在线 python 运行网站](https://repl.it/repls/TautRareDictionary)来运行。
&emsp;&emsp;本文内容是复制 github 文件得来，可能不够清晰，下面是 github 内容的展示（更加美观一些，初学者希望看见的小伙伴去标星一下）：
<img src="https://img-blog.csdnimg.cn/20201011014435221.png" width=60%>

## 闲话

&emsp;&emsp;学习及使用爬虫时，我们需要一些代理 ip 以免被反爬。但初学者或没有稳定长期需求的小伙伴则会想自己找免费的代理 ip，但是这种 ip 往往不太稳定，可能今天能用明天就不能用了，所以就需要写脚本来自动从一些提供免费代理 ip 的网站爬取。

## 本文所使用的 python 库

&emsp;&emsp;主要是两个：

1. reqests 库：提供常见的 get 和 post 等请求方式，构造请求头很直观
2. re 库：python 正则表达式的库

## 本文所爬取的网址

&emsp;&emsp;https://spys.one/en 是常见的提供免费代理 ip 的一个网站，本文将从该网站提取免费的 socks5 代理。即从https://spys.one/en/free-proxy-list/ 来提取代理。

&emsp;&emsp;该网址的浏览器界面长这样：

<center>
<img src="https://img-blog.csdnimg.cn/20201011005338145.png" width=70%>
</center>

## 首先导入我们所使用的 python 库

```python
import requests
import re
import sys
```

## 构造正则表达式、定义 request.post 参数

网页是需要 post 来请求的，data 参数有五个，如 xpp 参数为展示数量：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201011015238494.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0NzY5MTYy,size_16,color_FFFFFF,t_70#pic_center)

```python
# 提取数据的正则表达式
ip_pattern = r"onmouseout.*?class=spy14>(.*?)<script.*?font>" # ip
port_pattern = r"\d<script.*?document.*?font>.\+(.*?)</script>" # port
decode_text_pattern = r'''</table><script type="text/javascript">(.*?)</script>''' # decode

## 使用request.post时的参数
user_agent = "header = {'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_2) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/12.0.2 Safari/605.1.15'}"
headers = {'user-agent': user_agent}
url = "https://spys.one/en/socks-proxy-list/"
SHOW, AHM, SSL, Port, Type = '5', '4', '0', '0', '2'
data = {
    'xpp': SHOW,
    'xf1': AHM,
    'xf2': SSL,
    'xf4': Port,
    'xf5': Type
}
```

## 提取 ip、端口、解密表

解密表：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201011015238495.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0NzY5MTYy,size_16,color_FFFFFF,t_70#pic_center)
源码中端口的形式：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201011015238522.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0NzY5MTYy,size_16,color_FFFFFF,t_70#pic_center)

```python
def get_all_information(url, headers, data):
    try:
        proxies = {'socks5': '{}'.format("47.110.49.177:1080")}
        response = requests.post(url=url, data=data, headers=headers, proxies=proxies)
        if response.status_code == 200:
            print('You have got the source code of "{}".'.format(url))
            html_text = response.text
    except ConnectionError:
        sys.exit("something wrong")
    else:
        all_ip = re.findall(ip_pattern, html_text)
        all_port_text = re.findall(port_pattern, html_text) # (t0f6u1^h8q7)+(a1e5a1^m3h8)+(t0f6u1^h8q7)+(g7z6n4^d4o5))
        decode_text = re.search(decode_text_pattern, html_text).group(1)
    return all_ip, all_port_text, decode_text
all_ip, all_port_text, decode_text = get_all_information(url, headers, data)
print("There is {} proxy ips and the first ip is {}".format(len(all_ip), all_ip[0]))
print("There is {} ports of proxy ips and the first port is {}".format(len(all_port_text), all_port_text[0]))
print(decode_text)
```

## 将解密表格式化为字典形式

```python
def get_decode_key(decode_text):
    '''
    将形如155;k1s9=8233;d4w3c3=0^j0h8的内容提取为字典形式
    '''
    decode_key_chart = re.findall(r";(\w+=\d)\^", decode_text)
    decode_dict = {}
    for key in decode_key_chart:
        key, value = key.split("=")
        decode_dict[key] = value
    return decode_dict
decode_dict = get_decode_key(decode_text)
print("The decode dict is {}".format(decode_dict))
```

## 解密端口号

```python
def decode_port(all_port_text, decode_dict):
    '''
    将形如(l2z6a1^v2l2)+(j0c3q7^z6f6)+(l2z6a1^v2l2)+(p6u1v2^x4u1))的端口号根据上文的decode_dict提取为数字形式
    '''
    all_port = []
    for current_port_text in all_port_text:
        current_port = ""
        current_port_pre = re.findall(r"(\w+)\^", current_port_text)

        for key in current_port_pre:
            current_port += decode_dict[key]
        all_port.append(current_port)
    return all_port
all_port = decode_port(all_port_text, decode_dict)
print("Take the first port as example:\nencrpyt code:{}\nafter decode:{}".format(all_port_text[0], all_port[0]))
```

## 将 ip 和端口拼接为 ip:port 形式

```python
def get_proxy(all_ip, all_port):
    '''
    将上文的ip和端口拼接为ip:port的形式
    '''
    socks_pro = []
    for ip, port in zip(all_ip, all_port):
        socks_pro.append(ip+":"+port)
    return socks_pro
socks_pro = get_proxy(all_ip, all_port)
print("You have got {} proxies and the top ten are \n{}".format(len(socks_pro), socks_pro[:10]))
```
