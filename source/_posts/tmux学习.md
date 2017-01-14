---
title: tmux学习
date: 2017-01-14 15:51:22
tags: [tmux,linux]
---

早闻tmux大名，今天最近在家花个把小时学习了下tmux。记录下tmux的常用快捷键。

新建一个dev的会话：
```shell
tmux new -s dev
```

新建带窗口的会话：
```shell
tmux new -s dev -n windows
```

分离会话（detach）
```shell
[Ctrl+b] [d]
```
连接会话：
```shell
tmux a -t dev
```


查看会话：
```shell
tmux ls
```


新建窗口：
```shell
[Ctrl+b] [c]
```

上一个窗口（previous）： [前缀] p
下一个窗口（next）： [前缀] n
跳转到指定窗口： [前缀] 0-9的数字
窗口列表： [前缀] w
更改窗口名称： [前缀] ,
关闭窗口： [前缀] &
垂直切分（把窗口垂直切分成左右两等分）： [前缀] %
水平切分（把窗口水平切分成上下两等分）： [前缀] "
按制定方向切换窗格： [前缀] 方向键
窗格切换： [前缀] o
更换窗格布局： [前缀] 空格