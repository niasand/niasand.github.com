title: python装饰器
tags:
  - python
  - 装饰器
id: 1338
categories:
  - 杂谈
date: 2015-07-12 23:08:44
---

python的装饰器一直觉得不理解，其实认真领会起来，还是可以理解的。

<pre class="lang:python decode:true " title="python装饰器" >def fun(egg):
    print "out name space"
    def inner():
        print "inner name space"
        ret = egg()
        return ret + 1
    return inner
@fun
def foo():
    return 5

foo()
#dd = fun(foo)
#dd()
</pre> 

深圳 7月12日:多云 28～33℃ 明:多云 28～34℃