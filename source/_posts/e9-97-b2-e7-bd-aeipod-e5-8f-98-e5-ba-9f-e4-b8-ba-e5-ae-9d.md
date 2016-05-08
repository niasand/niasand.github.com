title: 闲置iPod变废为宝
tags:
  - git
  - 越狱
id: 1316
categories:
  - 杂谈
date: 2015-06-29 22:01:04
---

打开cydia，搜索并安装如下软件：

1\. git：现在流行的版本控制软件，linux内核就是用他管理的
2\. MobileTerminal：iOS上的控制台
3\. coreutils：核心命令，方便操作
4\. rsync：用于对不支持git的设备进行同步
5\. openssh-server：方便远程管理和访问

jailbreak后的iOS系统，使用ssh登录的默认用户名是mobile，密码alpine。登录后就可以拥有一个跟linux差不多的环境了。此时建立个目录，比如叫 gitrepo ，用来存储所有的版本库：

$ mkdir gitrepo
$ cd gitrepo

然后建立一个版本库，比如存储自己笔记用的：

$ git init --bare mynotes

然后就可以在自己的电脑上也安装上git，并首次取出这个版本库：

$ git clone mobile@&lt;ip&gt;:gitrepo/mynotes

有图有真相：

![](http://7jpsil.com1.z0.glb.clouddn.com/QQ截图20150629210717.png)

&nbsp;

![](http://7jpsil.com1.z0.glb.clouddn.com/IMG_0001.PNG)

&nbsp;

[参考链接](http://www.zhihu.com/question/31487793/answer/52845004)