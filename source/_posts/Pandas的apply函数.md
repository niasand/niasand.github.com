---
title: Pandas的apply函数
date: 2017-12-04 00:03:30
tags:
---

Pandas的apply函数。
pandas本身内置了很多强大的函数，但是有时候没有我们需要的函数，怎么办，这时候，pandas提供了起强大的apply函数。

这是apply的文档

```python
Parameters:	
func : function

func should take a Series or DataFrame (depending on axis), and return an object with the same shape. Must return a DataFrame with identical index and column labels when axis=None

axis : int, str or None

apply to each column (axis=0 or 'index') or to each row (axis=1 or 'columns') or to the entire DataFrame at once with axis=None

subset : IndexSlice

a valid indexer to limit data to before applying the function. Consider using a pandas.IndexSlice

kwargs : dict

pass along to func
Returns:	
self : Styler
```

比方说我们要对最大的一个值高亮。那么怎么处理呢？
首先定一个函数

```python
def highlight_max(x):
    return ['background-color: yellow' if v == x.max() else '' for v in x]

```
然后我们构造一个dataframe

```python
df = pd.DataFrame(np.random.randn(5, 2))
```

接着用apply来应用这个函数

```python
df.style.apply(highlight_max)
```
