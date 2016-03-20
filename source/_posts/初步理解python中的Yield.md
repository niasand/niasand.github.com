---
title: 初步理解python中的Yield
date: 2016-03-20 20:02:12
tags: yield
---

```python
# coding: utf-8
def count(n):
    print "Counting"
    a = 1
    while n > 0:
       print "Before yield"
       yield n
       n -= 1
       print "End yield"
       print a
       print "---------\n"

def diedai_Count():
    for i in count(5):
        print i

def fibonacci():
    a=b=1
    yield a
    yield b
    while True:
        a,b = b,a+b
        yield b

def Generate_Fibonacii():
    for num in fibonacci():
        if num > 100:
            break
        print num

def nullYield():
    a = 1
    yield a

def gen_nullyield():
    for i in nullYield():
        if i == 2:
            print i
        else:
            print "Nothing"
gen_nullyield()
#diedai_Count()
#Generate_Fibonacii()

```
