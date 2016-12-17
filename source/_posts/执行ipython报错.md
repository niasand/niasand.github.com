---
title: 执行ipython报错
date: 2016-12-17 12:23:21
tags: [杂谈,python]
---

最近在我的mac上执行iPython突然报错，报错如下：

```python
ImportError: No module named shutil_get_terminal_size
```

然后去网上查了下解决办法：

1. 下载 backports.shutil_get_terminal_size 源码安装

```python
cd /data/tmp
wget https://pypi.python.org/packages/ec/9c/368086faa9c016efce5da3e0e13ba392c9db79e3ab740b763fe28620b18b/backports.shutil_get_terminal_size-1.0.0.tar.gz#md5=03267762480bd86b50580dc19dff3c66 --no-check-certificate
tar -zxf backports.shutil_get_terminal_size-1.0.0.tar.gz
cd  backports.shutil_get_terminal_size-1.0.0
python setup.py install
```

2. 修改代码

   ```python
   vi /usr/local/lib/python2.7/site-packages/IPython/utils/terminal.py

   将 

   from backports.shutil_get_terminal_size import get_terminal_size as _get_terminal_size

   改成如下，然后保存：

   from shutil_backports import get_terminal_size as _get_terminal_size

   ```

   ​

   改成图中红框部分：

    ![terminal.py](http://7jpsil.com1.z0.glb.clouddn.com/keliloQQ20161217-0.png) 



然后问题就迎刃而解啦。

备注：Python2的启动应该使用get_terminal_size在模块shutil_backports中；