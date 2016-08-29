---
title: hexo+github快速搭建个人博客
tags:
  - github
  - hexo
  - next
comments: true
---



## 环境搭建
### 1.注册github帐号
拥有github网站帐号后，新建一个版本库，拥有了一个版本库链接，后面会用到，并启用GitHub Page，添加自己的域名（二级域名也可以）
### 2.在本地安装hexo环境
a.安装git，起初用的cygwin自带的git命令，为自己挖了坑（搭建环境过程一切顺利，当hexo deploy 提交到github时，到输入username就卡住了，死活输入不进去，后重新下载STL版的git后，使用GIT bash解决了问题）
b.安装node.js ，官网下载安装即可，安装后可获得node 和 npm 命令
c.安装hexo ，该过程在git bash命令行中安装，使用命令：$ cnpm install -g hexo-cli  与 $ cnpm install hexo --save .
### 3.本地运行hexo
初始化：$ hexo init [目录]，注意：如果不加目录就会初始化到当前目录，所以初始化前要先cd your-hexo-site;安装生成器: cnpm install；本地启动服务：$ hexo s -g ，之后打开浏览器，输入localhost:4000,就可以在本地看到你的个人博客了；
### 4.本地配置
修改blog/_config.yml文件，进行配置，主要配置项：
```
url: http://xxxxyyyy.github.io
deploy:
  type: git
  repository: https://github.com/xxxxyyyy/xxxxyyyy.com.io.git
  branch: master
```
### 5.发布博客
$ hexo d -g ,会提示输入github的用户名和密码，出现本次搭建博客的最大坑，username不能输入，使用git bash即可解决。
至此第一版本就更新到你的github版本库里了。

## 绑定独立的二级域名

1.在github版本库setting里配置github page,（注意：版本库branches选择master），配置如下
```
Source ： master branch
Custom domain：blog.vvsip.com
```
2.域名管理配置，设置DNS的解析，配置二级域名blog，类型CNAME，记录值xxxxyyyy.github.com.

3.这样配置后每次deploy后github上的CNAME会被清空，需要重新配置实在不便。解决方法：从github仓库Download ZIP，解压，找到CNAME文件，修改成需设置的域名，如果已配置域名则文件不需修改，然后放进Hexo\source目录下，提交上去，就OK了。


## 更换主题

网上有很多hexo的主题，最终选择了 NexT.Pisces，感觉还不错，较清新，而且适合国内使用习惯。
1.下载：
```bash
$ cd your-hexo-site
$ git clone https://github.com/iissnan/hexo-theme-next themes/next
```
2.配置：
  修改主配置文件：theme: next  ；修改主题配置文件：scheme: Pisces
  其他配置：修改菜单，左侧栏，头像；增加多说评论；增加阅读次数（LeanCloud)

3.优化
  a.更换google字体库
  修改主题配置文件：host: //fonts.useso.com
  

## 写文章
文章是用markdown格式的，可用markdownpad 软件，选择use free，分左右对比，书写较为方便；每次写完文章后运行$ hexo d -g进行发布。
基本语法:

1.在一段文本前加上 \# 符号，即可使其成为标题。Markdown 共支持六级标题，标题级数与 \# 的个数一一对应。

2.在一段文本前加上 \> 符号，即可将其引用。

3.文字两端分别插入一个星号使其 \* 倾斜 \*，插入两个星号使其\*\* 加粗 \*\*，插入 \` 符号使其生成行内代码块。

4.用 \[ 文字](链接) 的格式为中括号内的文字添加一段链接。插入图片时，在前者的开头加上 \! 即可，即 \!\[ 可选文字](图片链接)。

5.Markdown 支持无序列表和有序列表，你可以使用 \* 作为列表标记生成无序列表，或是用数字加英语句号的组合 1. 来生成有序列表。

6.空白行中输入 \* \* \* 来生成分割线。

参考：
http://blog.csdn.net/jzooo/article/details/46781805
http://theme-next.iissnan.com/getting-started.html
