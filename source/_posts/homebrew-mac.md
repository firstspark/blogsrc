---
title: homebrew工具：MAC上的apt-get、yum神器
date: 2016-08-29 23:21:03
tags:
  - 工具
---
Homebrew官网：http://brew.sh/index_zh-cn.html
Linux下的软件包管理主流的两大工具是：RedHat的yum，Ubuntu的apt-get .同样的今天介绍的homebrew就是类似的工具，是Mac OSX上的软件包管理工具，能在Mac中方便的安装软件或者卸载软件。

### 安装
Homebrew的安装非常简单，终端下输入下面的命令，中间需要输入密码之类的验证。
```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

### 使用
常用命令：
```
搜索软件：brew search 软件名，如brew search wget
安装软件：brew install 软件名，如brew install wget
卸载软件：brew remove 软件名，如brew remove wget
```
