---
title: nodejs 进阶
date: 2016-11-16 22:42:18
tags: nodejs
---
hexo--搭建

前言
Hexo是一个很好的博客，个人觉得他的界面干净利落，阅读体验也是很好。初次接触Hexo是在朋友的介绍下才知道的，hexo是需要手动搭建（觉得B格很高），在朋友的帮助下也终于是完成了。

开篇记录一下搭建的过程希望能帮助到浏览到这里的朋友
<!--more-->
我的系统为OS X，windows也可以参考。


简介

Hexo是一个开源的静态博客生成器，用node.js开发，作者是台湾大学生tommy351。

思路总结

安装过程中如果出现问题请到最后面查看问题总结，或许能帮助你解决问题。



环境配置

安装Git

安装过Xcode可以直接跳过这步，因为Xcode自带Git

Git有很多，Mac和Windows都可以直接到git-scm官网下载安装

安装node.js

奉上Macnode-v4.2.2.pkg安装包。

这两个软件的安装步骤就不多做介绍，一直点下一步就好

安装

安装hexo

sudo npminstall-ghexo

初始化

我们可以创建一个新文件夹作为hexo的安装目录，把所有的hexo文件都放里面，主要是为了方便管理。

初始化有两种方式：

直接指定目录

hexo init folderfolder:是指定目录的文件路径。例：/Documents/hexo

进入指定目录(cd /文件夹路径)

hexoinit

终端执行命令后显示的结果：

[info] Copying data[info] You are almost done! Don't forgettorun`npm install`beforestart bloggingwithHexo!

安装依赖包

sudo npminstall

生成静态页面

cd 到你的hexo安装目录（工作目录），执行如下命令

hexogenerate简写 hexog

必须到你新建的hexo安装目录下执行，否则不成功

本地启动测试

hexoserver简写 hexo s

到此本地服务已经完成，可以再浏览器中输入http://localhost:4000进行查看。接下来需要把我们创建的静态页面托管到github上，别人才能访问到

部署到Github

注册Github账号

这里就不再赘述，已有Github账号可以跳过此步骤。

新建repository(仓库)

登陆Github账号后，点击右上角的“+”号按钮，选择“New repository”



在Create a new repository界面填写格式如下图所示: 用户名.github.io

填写完成点Create repository创建完成



生成SSH Keys：

我们如何让本地git项目与远程的github建立联系？这时候就要用到SSH Keys

1、生成SSH Keys

使用ssh-keygen命令生成密钥对

ssh-keygen -t rsa -C"这里是你申请Github账号时的邮箱"

然后系统会要你输入密码：（我们输入的密码会在你提交项目的时候使用）

Enter passphrase (emptyforno passphrase):<输入加密串>Enter same passphrase again:<再次输入加密串>

（终端提示生成的文件路径）找到你生成的密钥找到id_rsa.pub用终端进入编辑，复制密钥。



2、添加你的SSH Key到ssh-agent

添加你的SSH Key到ssh-agent

//在后台打开 ssh-agenteval"$(ssh-agent -s)"添加你的SSH Key到ssh-agentssh-add ~/.ssh/id_rsa

添加SSH Key到Github：

1、添加SSH Key

通过命令复制SSH Key内容到系统剪贴板

pbcopy < ~/.ssh/id_rsa.pub

登陆Github,点击右侧用户按钮，选择Settings



点击 Add SSH key 按钮，将复制的密钥粘贴到 Key 栏



2、测试能不能链接成功

测试

ssh-T git@github.com

执行结果

Permanently addedtheRSA host keyforIP address '192.30.252.130'tothelistofknown hosts.Are you sure you wanttocontinueconnecting (yes/no)?<输入yes>Hi username! You've successfully authenticated,butGitHubdoesnot

现在你已经可以通过SSH链接到Github了

如果有问题，请再配置。参考网站

生成SSH Keys

Generating SSH Keys

Error: Permission denied (publickey)错误

Error: Permission denied (publickey)

设置你的用户名和密码：

Git会根据用户的名字和邮箱来记录提交，GitHub也是用这些信息来做权限的处理。

git config --global user.name"这里是你申请Github账号时的name"git config --global user.email"这里是你申请Github账号时的邮箱"

部署

编辑 _config.yml(在你的工作目录下)，把下面的your_username换成你的github用户名

deploy:type: gitrepo:https://github.com/your_username/your_username.github.io.gitbranch: master

执行部署命令

hexod-ghexogenerate和 hexo deploy 合写

问题：

1、 部署时出现：Error: EACCES, open ‘/Users/Desktop/hexo/public/js/script.js’

原因：权限问题在部署命令前加sudo

2、 deployer找不到git: ERROR Deployer not found: git

解决方法:npm install hexo-deployer-git --save

3、

{ [Error:Cannot find module'./build/Release/DTraceProviderBindings']code:'MODULE_NOT_FOUND'}{ [Error:Cannot find module'./build/default/DTraceProviderBindings']code:'MODULE_NOT_FOUND'}{ [Error:Cannot find module'./build/Debug/DTraceProviderBindings']code:'MODULE_NOT_FOUND'}

解决方法：

npminstall hexo --no-optional

4、npm install卡住不动

使用cnpm加速npm(原文地址：https://cnodejs.org/topic/5338c5db7cbade005b023c98)

npm--registry=https://registry.npm.taobao.org install koa
