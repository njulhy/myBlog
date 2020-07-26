---
title: 从npm迁移到yarn
author: lihy
tags:
  - 2020夏
  - npm
  - yarn
top: false
cover: false
mathjax: false
date: 2020-07-24 14:03:13
categories: 笔记
summary: 从npm迁移到yarn
img:
coverImg:
---

## 官方文档

&emsp;&emsp;详见[官方文档](https://classic.yarnpkg.com/zh-Hans/docs/migrating-from-npm/)

## npm install 命令

npm install 的-save 和-save-dev 这两个参数的使用

> npm install moduleName # 安装模块到项目目录下
> npm install -g moduleName # -g 的意思是将模块安装到全局，具体安装到磁盘哪个位置，要看 npm config prefix 的位置。
> npm install -save moduleName # -save 的意思是将模块安装到项目目录下，并在 package 文件的 dependencies 节点写入依赖。
> npm install -save-dev moduleName # -save-dev 的意思是将模块安装到项目目录下，并在 package 文件的 devDependencies 节点写入依赖。

## CLI 命令比较

| npm (v5)                              | yarn                          |
| ------------------------------------- | ----------------------------- |
| npm install                           | yarn install                  |
| (不适用)                              | yarn install --flat           |
| (不适用)                              | yarn install --har            |
| npm install --no-package-lock         | yarn install --no-lockfile    |
| (不适用)                              | yarn install --pure-lockfile  |
| npm install [package]                 | yarn add [package]            |
| npm install [package] --save-dev      | yarn add [package] --dev      |
| (不适用)                              | yarn add [package] --peer     |
| npm install [package] --save-optional | yarn add [package] --optional |
| npm install [package] --save-exact    | yarn add [package] --exact    |
| (不适用)                              | yarn add [package] --tilde    |
| npm install [package] --global        | yarn global add [package]     |
| npm update --global                   | yarn global upgrade           |
| npm rebuild                           | yarn install --force          |
| npm uninstall [package]               | yarn remove [package]         |
| npm cache clean                       | yarn cache clean [package]    |
| rm -rf node_modules && npm install    | yarn upgrade                  |
