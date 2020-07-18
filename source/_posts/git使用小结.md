---
title: git命令小结
date: 2020-07-15 13:36:28
tags: vim, git, learning
categories: 学习
summary: git命令小结
---

<!-- TOC -->

- [Git](#git)
    - [第一件事：Git global setup](#第一件事git-global-setup)
    - [基本流程](#基本流程)
    - [本地项目远程操作](#本地项目远程操作)
        - [Create a new repository](#create-a-new-repository)
        - [将本地项目放入远程仓库](#将本地项目放入远程仓库)
        - [Existing Git repository](#existing-git-repository)
        - [一些细节](#一些细节)
    - [简单操作](#简单操作)
- [Vim](#vim)
    - [插入模式](#插入模式)
    - [保存退出](#保存退出)

<!-- /TOC -->

## Git

- [下载地址](https://git-scm.com/)
- [文档（使用指南）](https://git-scm.com/book/en/v2)

### 第一件事：Git global setup

设置你的用户名和邮件地址。 这一点很重要，因为每一个 Git 提交都会使用这些信息，它们会写入到你的每一次提交中，不可更改

    git config --global user.name "Your Name"
    git config --global user.email “email@example.com”

如果使用了 --global 选项，那么该命令只需要运行一次，因为之后无论你在该系统上做任何事情， Git 都会使用那些信息。 当你想针对特定项目使用不同的用户名称与邮件地址时，可以在那个项目目录下运行没有 --global 选项的命令来配置。

### 基本流程

- 先发出命令，重要的是 add, rm
- 提交命令 commit
- pull/push # github desktop 易于操作

### 本地项目远程操作

[参考](https://blog.csdn.net/u011479200/article/details/81230083)

#### Create a new repository

    git clone .git地址
    cd yveshe-git
    touch README.md
    git add README.md
    git commit -m "add README"
    git push -u origin master

#### 将本地项目放入远程仓库

    cd existing_folder
    git init
    git remote add origin https://gitlab.com/YvesHe/yveshe-git.git
    git add .
    git commit -m "提交说明"
    git push -u origin master

#### Existing Git repository

    cd existing_repo
    git remote rename origin old-origin
    git remote add origin https://gitlab.com/YvesHe/yveshe-git.git
    git push -u origin --all
    git push -u origin --tags

#### 一些细节

- 拉下来远程仓库
  > git remote add origin https://gitlab.com/YvesHe/yveshe-git.git

远程仓库默认名称为一般为 origin

- 将本地所有分支推向远程仓库
  > git push -u yveshe-git-remote --all
  > --all 选项代表不管是否存在对应的远程分支，将本地的所有分支都推送到远程主机中.
- 登录远程服务器将本地标签推向远程仓库
  由于使用 git push 命令不会向远程仓库推送标签(tag),这里使用--tags 指明向远程仓库中推送标签

  > git push -u yveshe-git-remote --tags

- 删除服务器上的 master 分支

  > git push yveshe-git-remote --delete master

- 注意事项
  如果远程主机的版本比本地版本更新，推送时 Git 会报错，要求先在本地做 git pull 合并差异，然后再推送到远程主机。这时，可以使用--force 来强制推送 git push --force origin  
  另外将本地的分支与指定的远程分支建立追踪关系格式 git branch --set-upstream [local-branch][remote-branch]

### 简单操作

    git init # 将当前目录变为仓库（生成一个./git文件夹）
    git add [obj] # add后文件，
    git status
    git diff # 查看工作区和版本库区别
    git commit -m "command" # command如add, modify, add a line,
    git log # 查看修改日志
    git reset command # 如--hard HEAD^回滚一个版本， --hard HEAD~10回滚十个版本
    git reflog # 查看每一次执行的命令

## Vim

### 插入模式

i 在光标左侧插入正文  
a 在光标右侧插入正文  
o 在光标所在行的下一行增添新行  
O 在光标所在行的上一行增添新行  
I 在光标所在行的开头插入  
A 在光标所在行的末尾插入

ESC 或 Ctrl+[退出插入模式

###　保存退出
保存

> :w

退出

> : q 在未作修改的情况下退出；  
> : q! 放弃所有修改，退出编辑程序。  
> :wq 先保存，后退出
