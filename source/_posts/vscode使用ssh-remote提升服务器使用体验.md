---
title: winscp配合vscode提升服务器使用体验
author: lihy
tags:
  - 2020夏
  - vscode
  - ssh-remote
  - winscp

top: false
cover: false
date: 2020-07-30 22:20:08
categories: 教程
summary: winscp配合vscode提升服务器使用体验
img:
coverImg:
---

## 适用

- 使用远程服务器
- 用户体验不佳，想提升自己的用户体验

## 安装 winscp

&emsp;&emsp;如果你还是用 putty 或者直接命令行操作……那想必体验不会很好。上句话是对 windows 用户说的，本教程也只适用 windows。  
&emsp;&emsp;提升体验的第一步，安装 winscp 得到 ftp 类型软件的体验，推荐你直接去[官方教程](https://winscp.net/eng/docs/guide_install)。安装完后如果你有 putty 可以直接导入配置，没有当我没说。  
给个示例图，直接拖动就能完成文件操作。
<img src="https://s1.ax1x.com/2020/07/30/aMVXff.jpg">

## 服务器免密登录

&emsp;&emsp;需要用到 git 来使用 ssh 命令，点击[这里](https://git-scm.com/download)下载一个git版本安装。安装后打开 cmd 输入`git --version`来检查是否安装成功。想了解 git 的话查看[文档（使用指南）](https://git-scm.com/book/en/v2)。如果 cmd 中没有反应，可能是环境变量没有设置正确。设置一下环境变量，把你安装git后的目录添加到环境变量，如`c:/program files/git/usr/bin`<img src="https://s1.ax1x.com/2020/07/19/UWIVpV.jpg">。这样就可以愉快的在cmd或git bash中使用`ssh, bash, ssh-keygen`等linux命令了~

### 创建密钥

&emsp;&emsp;在`git bash`(或 cmd）中输入`ssh-keygen -t rsa -C "youremail"`一路回车直到出现`cat ~/.ssh/id_rsa.pub`，这说明密钥已经生成。密钥存放在个人的主文件夹下，一般来说是`C:\Users\xxx\.ssh`,xxx是你的用户名
<img src="https://s1.ax1x.com/2020/07/19/UWbLkQ.png">

### 将密钥放入服务器

&emsp;&emsp;&emsp;&emsp;这里需要将刚刚生成的id_rsa.pub的内容加入到authorized_keys中如，直接命令行操作即可。你需要找到你生成的id_rsa.pub路径，然后使用scp命令传输文件

```cmd
scp your_path/id_rsa.pub user_name@ipv4_address:/home/your_user_name/.ssh/authorized_keys
```

&emsp;&emsp;注意更改路径、服务器地址、你在服务器的用户名。然后输入密码完成密钥传输，退出cmd。重新打开cmd尝试私钥登录。输入`ssh user_name@ipv4_address`查看是否可以免密登录，如果没成功尝试`ssh user_name@ipv4_address -i your_path/id_rsa.pub`手动指定私钥文件来连接（这里你的电脑自动读取了本地用户的.ssh文件夹中的私钥和服务器达成了连接）。

如果你已经装好了winscp则可以不使用命令行来操作。打开你的用户根目录，会有一个.ssh文件夹（如果没有自己创建一个）
<img src="https://s1.ax1x.com/2020/07/30/aMmXfx.jpg">
&emsp;&emsp;进入.ssh文件夹，将id_rsa.pub的内容写入authorized_keys文件中，没有authorized_keys自己创建一个即可。
<img src="https://s1.ax1x.com/2020/07/30/aMmvp6.jpg">
&emsp;&emsp;上面几步都成功的话，你已经无需在登录服务器的时候输入密码了。尝试在终端输入ssh user_name@ipv4_address查看是否可以免密登录，如果没成功尝试`ssh user_name@ipv4_address -i your_path/id_rsa.pub`手动指定私钥文件来连接。（这里你的电脑自动读取了本地用户的.ssh文件夹中的私钥和服务器达成了连接）

&emsp;&emsp;一般是不用-i指定路径的，如果一直按照默认操作的话，密钥会生成在本地用户根目录的.ssh文件夹中，如果你的密钥不在用户根目录的.ssh文件夹中，把密钥复制到你的用户根目录的.ssh文件夹中，没有.ssh文件夹自己创建一个。

## 安装 remote-ssh

&emsp;&emsp;从VS code中的扩展商店中添加Remote Development插件，如下图所示。
<img src="https://s1.ax1x.com/2020/07/30/aMl4pR.md.jpg">
&emsp;&emsp;装完后侧边栏会出现箭头指向的远程连接标志
点击远程连接的标志，然后点击小齿轮配置标志
<img src="https://s1.ax1x.com/2020/07/30/aM3VaD.jpg">
&emsp;&emsp;在 Select SSH configuration file to edit 中，选择第一个配置文件即可
<img src="https://s1.ax1x.com/2020/07/30/aM3EVO.jpg">
&emsp;&emsp;编辑.ssh/config内容，Host后面填写你想要显示在的名称，其他两个填入服务器ip和管理员分配给你的用户名
<img src="https://s1.ax1x.com/2020/07/30/aM3ZIe.jpg">
&emsp;&emsp;远程连接的基础是你可以本地cmd使用ssh命令，如果不可以则该插件无法使用。

## 开始体验

&emsp;&emsp;上面一切就绪之后，点击远程连接标志就会显示你所设置的服务器名，鼠标点击连接，需要注意的是需要你选择一下服务器的系统，正确选择即可。  
关于Remote-SSH的更多信息，查看[官方文档](https://code.visualstudio.com/docs/remote/ssh#_getting-started)
