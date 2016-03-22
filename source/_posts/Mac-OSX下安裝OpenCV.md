---
title: Mac-OSX下安裝OpenCV
date: 2016-03-22 23:33:18
tags:
---

最近在Mac下安装Opencv遇到了一些问题，在安装完Opencv后，在Python中import的时候，系统总是提示没有cv这么个模块。然后尝试了编译，最终还是采用brew install的安装方法，最后解决了这个问题。简单记录下安装过程。

 安装前提：

- 系统已经安装Xcode

- 系统已经安装HomeBrew

1.Mac 下可以直接使用 brew 来安装OpenCV，具体步骤如下：

```sh

#确保Mac下的Python是最新版本，可以先卸载，再安装
brew uninstall python; brew install python

#添加opencv的源
brew tap homebrew/science

#安装nunpy, opencv
pip install numpy
brew install opencv
brew info opencv
```

2.安装完了之后，可以检查下opencv的安装路径，我安装的是2.4.12_2 版本，路径为(不同的版本路径不同)

```sh
/usr/local/Cellar/opencv/2.4.12_2
```
3.接着就是设置环境变量,如果是zsh，.bash_profile可以换成.zshrc

```sh
cd /Library/Python/2.7/site-packages/
ln -s /usr/local/Cellar/opencv/2.4.12_2/lib/python2.7/site-packages/cv.py cv.py
ln -s /usr/local/Cellar/opencv/2.4.12_2/lib/python2.7/site-packages/cv2.so cv2.so
```

4.将下面这句命令加入到.bash_profile
```sh
export PYTHONPATH=/usr/local/lib/python2.7/site-packages:$PYTHONPATH
```
5.加完了之后，执行source命令，让环境变量生效
```sh
source ~/.bash_profile
```
6.打开python，可以import下啦
```python
import cv
import cv2.cv as cv
```

到这里，Mac下的opencv安装就完成啦。
