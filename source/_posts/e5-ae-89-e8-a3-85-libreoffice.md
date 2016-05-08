title: 安装 LibreOffice
id: 556
categories:
  - 杂谈
date: 2012-03-17 20:31:51
tags:
---

整个过程非常的顺利，中文版还是看着舒服，好吧，我承认，我的英文水平有限。

1.删除原来的版本RC2版本
sudo apt-get autoremove LibreOffice.*

2.解压下载下来的LibO_3.3.0_Linux_x86_install-deb_en-US.tar.gz软件包

3.安装
在终端进入解压的LibO_3.3.0rc4_Linux_x86_install-deb_en-US目录下面的DEBS目录,安装所有在DEBS子目录里的deb包
sudo dpkg -i *.deb

4.设置快捷方式
在终端进入解压的目录下面的DEBS/desktop-integration目录,安装在DEBS/desktop-integration目录里的deb包
sudo dpkg -i *.deb

5.安装中文包
在终端进入解压LibO_3.3.0rc4_Linux_x86_langpack-deb_zh-CN目录下面的DEBS/目录,安装在DEBS/目录里的deb包
sudo dpkg -i *.deb

6.安装LibO_3.3.0rc4_Linux_x86_helppack-deb_zh-CN
在终端进入解压LibO_3.3.0rc4_Linux_x86_helppack-deb_zh-CN目录下面的DEBS/目录,安装在DEBS/目录里的deb包
sudo dpkg -i *.deb

&nbsp;