---
title: ubuntu服务器普通用户安装anaconda
author: lihy
tags:
  - 2020夏
  - anaconda
  - ubuntu
top: false
cover: false
date: 2020-07-30 10:30:13
categories: 教程
summary: ubuntu服务器普通用户安装anaconda
img:
coverImg:
---

## 下载 anaconda

&emsp;&emsp;对普通用户，安装 anaconda 能屏蔽掉外面的 conda 环境，自己再怎么折腾也不用怕打扰到别人或者被别人打扰。  
&emsp;&emsp;首先你需要下载一个安装包，你可以点击[here](https://repo.anaconda.com/archive/Anaconda3-2020.07-Linux-ppc64le.sh)直接下载 2020-7 版本，默认支持 python3.8.当然这样你会直接下载到本地……如果在内网的话本地传服务器速度尚可，但是如果在外网几百 M 得传大半天。所以**将安装包直接下载到 ubuntu 服务器**，ssh 登录后在服务器 bash 输入

```bash
wget https://repo.anaconda.com/archive/Anaconda3-2020.07-Linux-ppc64le.sh
```

&emsp;&emsp;注意这里的 url 是 2020 年 7 月的 anaconda 版本，python 默认 3.8。如果需要的版本比较低推荐去**清华镜像**下载，这里是[清华镜像传送门](https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/)。懒得动手的话 bash 输入（清华镜像目前 anaconda 的版本只有 18 年的，下面贴出镜像中最新的版本，默认支持 python3.7

```bash
wget https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/Anaconda3-5.3.1-Linux-x86_64.sh
```

如果需要别的版本去[官网](https://www.anaconda.com/products/individual#)下载。

&emsp;&emsp;解释一下 wget：The wget command allows you to download files from the Internet using a Linux operating system such as Ubuntu. Use this command to download either a single Web page or a complete copy of your company's website. It also includes an option for downloading any external links included on the site.

## 安装 anaconda

&emsp;&emsp;现在在你打开 bash 的目录下已经有了 anaconda 的.sh 文件（如果是下载到 windows 上传上去的传到你自己的根目录最好，否则后文的命令你需要自行修改），而.sh 文件在 ubuntu 中是可以直接安装的。如果你有 sudo 权限可以直接在.sh 文件所在目录打开终端直接输入文件名。比如你的文件名叫 my.sh，直接输入`./my.sh`或`my.sh`。这里由于你只是普通用户，只能选择安装在你的用户目录下面，终端输入

```bash
bash Anaconda3-2020.07-Linux-ppc64le.sh
```

&emsp;&emsp;注意如果不是上面的 2020-7 版本自行改上面的文件名。接着按照提示走就行，总之一路回车，回车不动就输入 yes 即可，这样就可以把 anaconda 安装到你的用户目录下面了。安装完之后可能会问你是否需要安装 vscode，看你自己需要了，感觉这里没什么必要安装。个人而言使用 vscode 中的 ssh-remote，配置 ssh-remote 的时候会自动装的。如果不用 vscode 中的 ssh-remote，反正我不知道这里装 vscode 怎么用。

## 配置一下

### 了解原理看此节

&emsp;&emsp;对原理没兴趣，移步下一小节。  
&emsp;&emsp;激活是让一些命令（比如 conda 命令）的路径选择到你的安装路径。安装后理论上会产生一个.bashrc 文件，这个 bashrc 文件是你安装了 anaconda 后用户目录（和 anaconda 大目录并列的地方）出现的文件，执行 source 命令后 bash 会把后面文件中的内容当作 bash 命令来执行。一般来说你的.bashrc 中写了几个 if-else-fi 语句，但是实现的功能主要是这一句`export PATH="/home/your_user_name/anaconda3/bin:$PATH"`，意思是把一些命令的路径选择到 PATH 后面的字符串地址，如果你打开/home/your_user_name/anaconda3/bin 文件夹，会发现比如 conda 文件——你在 bash 使用 conda 的时候就是执行了这里的 conda 文件。  
&emsp;&emsp;严谨一点的话，你需要每次打开 bash 都执行.bashrc 中的内容；不严谨的话，.bashrc 就是做一个路径指向，因为我们自己使用无需考虑各种判断条件，直接找到你的 anaconda3/bin 路径然后 bash 输入（一定**注意你的路径**）

1. 不严谨的做法

   - bash 输入以下内容指定路径

      ```bash
      export PATH="/home/your_user_name/anaconda3/bin:$PATH"
      ```

      尝试bash命令`conda info --envs`，如果显示的路径是你自己安装的路径，那么

   - 在你打开终端的界面创建一个".bash_profile"文件（一般来说都是用户根目录，但你需要注意你打开命令行的位置，比如 vscode 的 ssh-remote 支持你打开文件夹登录，这时你需要在该目录下激活你的 conda 环境）
   - 在你的".bash_profile"文件中写入`export PATH="/home/your_user_name/anaconda3/bin:$PATH"`，注意你的**路径**

2. 严谨的做法

   - bash 执行.bashrc内容指定路径

        ```bash
        source your_path/.bashrc
        ```

        一般anaconda都直接装到了你的用户根目录，.bashrc也产生在这里，则*your_path=.*，即bash输入`source ./.bashrc`，但是建议你使用绝对路径以便当你不在用户根目录打开bash时快速迁移。

        尝试命令`conda info --envs`，如果显示的路径是你自己安装的路径，那么

   - 在你打开终端的界面创建一个".bash_profile"文件（一般来说都是用户根目录，但你需要注意你打开命令行的位置，比如 vscode 的 ssh-remote 支持你打开文件夹登录，这时你需要在该目录下激活你的 conda 环境）
   - 在你的".bash_profile"文件中写入`source your_path/.bashrc`，注意你的**路径**

&emsp;&emsp;这里两点需要注意，一是你需要明确的知道自己是在哪个文件夹下打开 bash 的，二是路径一定要正确。  
&emsp;&emsp;科普一点小知识：在你打开 linux 的 bash 时，会依次查找以下文件并将其内容当作命令行内容执行（因此你把上文.bash_profile 换成.bash_login 也是可行的）

```bash
/etc/profile
~/.bash_profile
~/.bash_login
~/.profile
```

，点击[here](https://apple.stackexchange.com/questions/12993/why-doesnt-bashrc-run-automatically#comment13715_13019)进一步了解。

### 激活 conda

&emsp;&emsp;本节参考[here](https://blog.csdn.net/moses1994/article/details/81507802)
&emsp;&emsp;安装后理论上会产生一个.bashrc 文件，终端输入

```bash
source .bashrc
```

&emsp;&emsp;你可以使用`conda info --nevs`来查看你是否成功激活。

止步这里的话每次打开终端都需要激活了，采用下面的方式简化此操作：

1. 在你打开 bash 的目录下创建一个".bash_profile"文件（一般来说都是用户根目录，但你需要注意你打开命令行的位置，比如 vscode 的 ssh-remote 支持你打开文件夹登录，这时你需要在该目录下激活你的 conda 环境）
2. 在你的".bash_profile"文件中写入`source your_path/.bashrc`，注意你的**路径**

参考
[Ubuntu16.04 服务器普通用户（非管理员账户）在自己目录下安装 TensorFlow， Keras 等（亲测）](https://blog.csdn.net/moses1994/article/details/81507802)
