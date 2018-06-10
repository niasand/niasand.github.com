---
title: matplot图显示中文
date: 2018-06-04 23:56:46
tags: [matplot]
---

Python版本 3.6.1

添加SimHei字体（simhei.ttf文件）到

```shell
/usr/local/var/pyenv/versions/3.6.1/lib/python3.6/site-packages/matplotlib/mpl-data/fonts/ttf
```
中；

1. 下载地址：[黑体字体simhei.ttf](http://www.font5.com.cn/font_download.php?id=151&part=1237887120)

2. 删除~/.matplotlib/下的所有缓存文件

```shell
rm -rf ~/.matplotlib/*.cache
```

3-  在你要画图的的python文件中，添加

```python
plt.rcParams['font.sans-serif'] = ['SimHei']  # for Chinese characters
```

备注： /usr/local/var/pyenv/versions/3.6.1/lib/python3.6/site-packages/matplotlib/mpl-data/matplotlibrc不需要修改

