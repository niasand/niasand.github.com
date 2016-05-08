title: 用win7系统是发现按右键无法创建记事本
id: 384
date: 2010-12-01 12:27:00
tags:
---

转：方法一、
运行regedit打开注册表编辑器
展开HKEY_CLASSES_ROOT
找到.txt
选中.txt，查看右侧窗格的“默认值”数据是不是txtfile，如果不是，就双击修改成txtfile 。
在.txt上右击，选“新建——项”
把新建项命名为ShellNew，如果有ShellNew就不用新建了。
选中shellNew，然后“新建”——“字符串值”，将其名称设置为“nullfile”(不包含引号)，如果有“nullfile”就不用新建了，值 留空即可。一般出现无法新建本文文件，就是由这两个地方引起的。然后刷新一下注册表和桌面。还不能出来，就重启或注销一下。

方法二、
没找到.txt
那么就运行 notepad
然后把下边的内容复制进去，注意REGEDIT4要在第一行，最后一行要有个回车。
然后另存在.reg文件，注意存的时候，选所有文件，再命名为比如txt.reg，存好后双击导入。
REGEDIT4
[HKEY_CLASSES_ROOT.txt]
@="txtfile"
"PerceivedType"="text"
"Content Type"="text/plain"
[HKEY_CLASSES_ROOT.txtShellNew]
"nullfile"=""

可以打开了。

记录和分享~~    :-)