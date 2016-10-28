---
title: appium+wda折腾小记
date: 2016-10-23 17:22:41
tags: [appium,wda]

---

​    看到Appium1.6出来也有一段时间了，期间工作一直特别忙，没有去升级。这个周末在家就把自己的电脑和公司的电脑都升级成了Appium1.6，然后顺带把ios的wda也一并琢磨了下。

   说到安装Appium，因为网络的原因，安装各种卡住，或者权限不足。最后一怒之下，把node删除了个干净，重新安装。

#### 一、如何干净的在mac电脑上删除node：

1. 删除/usr/local/lib中的所有node和node_modules
2. 删除/usr/local/lib中的所有node和node_modules的文件夹
3. 如果是从brew安装的, 运行brew uninstall node
4. 检查~/中所有的local, lib或者include文件夹, 删除里面所有node和node_modules
5. 在/usr/local/bin中, 删除所有node的可执行文件
6. 最后运行以下代码:

```shell    
sudo rm /usr/local/bin/npm
sudo rm /usr/local/share/man/man1/node.1
sudo rm /usr/local/lib/dtrace/node.d
sudo rm -rf ~/.npm
sudo rm -rf ~/.node-gyp

```



#### 二、如何不用sudo重装node

```shell
首先安装nvm。
To install or update nvm, you can use the install script using cURL:

curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.32.1/install.sh | bash

or Wget:

wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.32.1/install.sh | bash

The script clones the nvm repository to ~/.nvm and adds the source line to your profile (~/.bash_profile, ~/.zshrc, ~/.profile, or ~/.bashrc).

export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh" # This loads nvm

然后再
nvm  ls-remote
nvm install v6.9.1
                                        ----来源: https://github.com/creationix/nvm
```



#### 三、如何绕过蛋疼的墙

请使用  https://npm.taobao.org/ 来代替npm源

安装完cnpm后，然后就可以愉快的安装appium了,命令如下：

```shell
cnpm install appium -g --verbose
```



#### 四、WebDriverAgent下载使用

```shell
git clone https://github.com/facebook/WebDriverAgent.git

brew install Carthage

./Scripts/bootstrap.sh

```

然后用xcode打开WebDriverAgent的工程文件，编译前提是需要去注册一个免费的个人开发者账号，然后将WebDriverAgentLib和WebDriverAgentRunner的签名换成自己的。接着愉快的去编译，有问题再Google。编译教程：

https://testerhome.com/topics/4904  

https://testerhome.com/topics/6094

https://testerhome.com/topics/5804

然后运行WebDriverAgentRunner的test，就可以启动inspector了，可不管怎么样我的inspector死活起不来，后来看到有人直接走usb通信，瞬间觉得走网络low爆了，还没usb稳定。果断走usb通信。
具体如何走usb通信，由于大神讲的太清晰，我就不班门弄斧了。

请移步：https://diaojunxian.github.io/2016/08/30/iOS%E8%87%AA%E5%8A%A8%E5%8C%96%E5%AE%9E%E8%B7%B5%E2%80%94%E2%80%94WebDriverAgent-%E4%B8%83/



下面贴一下我运行inspector成功的皂片：



![inspector](http://7jpsil.com1.z0.glb.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202016-10-23%20%E4%B8%8B%E5%8D%885.47.03.png)