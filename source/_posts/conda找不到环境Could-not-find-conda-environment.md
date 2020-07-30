---
title: conda找不到环境Could not find conda environment
author: lihy
tags:
  - 2020夏
  - conda
  - python_environment
top: false
cover: false
date: 2020-07-30 16:25:59
categories: pytorch
summary: 解决Could not find conda environment
img:
coverImg:
---

## 问题说明

&emsp;&emsp;在使用`conda activate [environment name]`或者`source activate [environment name]`时，可能出现你明明安装了一个环境，但是 conda 却发现不了，然后提示`Could not find conda environment`。但是气人的是但是使用命令`conda info --envs`后，发现环境是存在的。

&emsp;&emsp;我是使用服务器的时候出现的这个问题。由于我是普通用户，在自己的根目录新装了一个 anaconda，这样似乎有普通用户和 root 用户的 conda 嵌套的问题。但是无伤大雅，问题是 conda 在激活后识别不了我安装的环境。

## 问题解决

&emsp;&emsp;解决步骤 1. 查看环境目录 2.手动添加环境目录

1. 查看环境目录 :
   先查看你的路径中有没有安装环境的目录，这里的目录是指你安装的环境的一级目录。如果使用了 anaconda3 安装，相应的环境会在`user_path/anaconda3/env`中，这里的 user_path 在 windows 中一般在`C:\Users\PC`中，这里我自己电脑的用户名是 pc；如果是 linux 则在你自己的用户根目录下，比如服务器的普通账号根目录。bash 输入

   ```bash
   conda config --show envs_dirs
   ```
  
2. 手动添加环境目录:
   如果没看到你的 anaconda3/envs 目录的话，手动添加即可。当然你的环境可能装到了别的地方…总之你自己得知道你把环境装到了哪里。上面说了 anaconda3 所装环境的位置，如果你是服务器普通用户使用了 root 用户的 conda 命令，那会装在你的用户根目录的".conda/envs"下面。bash 输入下面命令，**手动添加路径 **

   ```bash
   conda config --append envs_dirs your_path
   ```

   &emsp;&emsp;**注意将你的路径替换掉”your_path“**，比如`conda config --append envs_dirs /home/user/anaconda3/envs`或者`conda config --append envs_dirs /home/user/.conda/envs`， 总之找到你的 envs 文件夹即可。实在不行就直接找到你的环境硬核添加: 比如我有一个环境在”d:/a/b/"目录下（如果你不知道什么是环境直接理解为下一级目录有 bin, lib 等等的大目录），我直接`conda config --append envs_dirs d:/a/b`这样直接完成硬核添加。但是一般不管 pip 还是 conda 都会把你的环境装到一个大的 envs 目录下面，你只要添加大的 envs 目录就行了。

&emsp;&emsp;如果你**手残添加错了，那删掉**就行了（不删也没什么，可别把别人的环境给删掉了），bash 输入

```bash
conda config --remove envs_dirs your_path
```

如果对 conda 命令一知半解请移步[here](https://docs.conda.io/projects/conda/en/latest/commands/create.html)，如果需要互相交流请移步我的[主页](njulhy.com)留言

参考
[stackoverflow 的解答](https://stackoverflow.com/questions/58369030/could-not-find-conda-environment)
