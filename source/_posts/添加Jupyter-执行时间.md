---
title: 添加Jupyter 执行时间
date: 2018-06-03 23:27:56
tags: [Jupyter]
---


1、执行下面两台命令，可以在命令行用macdown或者open打开md文件。

```shell
sudo echo "open -a MacDown \$*" > /usr/local/bin/macdown

sudo chmod +x /usr/local/bin/macdown
```
2、配置Jupyter的执行时间。

用Jupyter扩展,
扩展/附加组件是一种非常有生产力的方式，能帮你提升在 Jupyter Notebooks 上的生产力。我认为安装和使用扩展的最好工具之一是 Nbextensions。在你的机器上安装它只需简单两行命令（也有其它安装方法，但我认为这个最方便）：


```Python
pip install jupyter_contrib_nbextensions

```

```shell
jupyter contrib nbextension install --user
```
完成这个工作之后，你会在你的 Jupyter Notebook 主页顶部看见一个 Nbextensions 选项卡。点击一下，你就能看到很多可在你的项目中使用的扩展。

要启用某个扩展，只需勾选它即可。下面我给出了 4 个我觉得最有用的扩展：

Code prettify：它能重新调整代码块内容的格式并进行美化。
Printview：这个扩展会添加一个工具栏按钮，可为当前笔记本调用 jupyter nbconvert，并可以选择是否在新的浏览器标签页显示转换后的文件。
Scratchpad：这会添加一个暂存单元，让你可以无需修改笔记本就能运行你的代码。当你想实验你的代码但不想改动你的实时笔记本时，这会是一个非常方便的扩展。
Table of Contents (2)：这个很棒的扩展可以收集你的笔记本中的所有标题，并将它们显示在一个浮动窗口中。
这只是少量几个扩展。我强烈建议你查看完整扩展列表并实验它们的功能。

本文引用： https://zhuanlan.zhihu.com/p/37553863