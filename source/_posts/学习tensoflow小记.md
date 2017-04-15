---
title: 学习tensoflow小记
date: 2017-04-15 22:07:25
tags: [tensorflow]
---

​    今天在家学习了下TensorFlow,虽然背后的数学知识浅薄,  但能感觉到, 好强大.再导入tensorflow包的时候,就已经有了一个默认的隐藏图.

​	可以直接调用函数去查看. 

```python
import tensorflow as tf
graph = tf.get_default_graph()
print graph
[] # 会得到一个空的图
```

这个时候我们可以创建一个常量,如

```python
_input = tf.constant(1.0)
```

这个常量会以一个节点的形式存在,也就是图里面的一个op,python的变量名_input间接引用这个op

调用下面的函数得到op

```python
op = graph.get_operations()
print op
```

如果我们直接打印_input, 会得到一个shape(维度)为空的的32位的浮点型张量,这意味着什么呢?意味着只有一个数字.
那怎么才能打印出这个_input的值呢?

我们就要创建一个会话使得图的ops能够被执行,而且明确要求"运行"这个_input.如:

```python
sess =  tf.Session()
print sess.run(_input)
```



下面我在初级入门教程上学到了一个如何运用tensorflow寻找目标函数参数的方法.

首先.我们的这个模型是线性回归的模型. 所以模型的参数定义如下,浮点型,初始值为0.3和-0.3

```python
W = tf.Variable([.3],tf.float32) 
b = tf.Variable([-.3],tf.float32)
```

接着给x一个placeholder

```python
x = tf.placeholder(tf.float32)

# 这样我们的简单的线性回归模型如下
linear_model = W * x + b 
```

然后为了确认我们的模型与真实的数据集之间的差异,即系统误差. 所以我们需要一个loss function.如下

```pytho
y = tf.placeholder(tf.float32)  # y占位符
loss = tf.reduce_sum(tf.square(linear_model - y)) #loss函数
```

我们的目标是就是尽可能的减少系统误差,让这个误差越小越好.,当然如果误差为负,就不是越小越好了.实际输出（linear_model）和期望输出（y）的平方差来定义误差的值。

然后我们用优化器（optimizer）。使用梯度下降优化器使我们能够按照误差的导数（derivative）来更新权重,这里背后的原理—梯度下降, 我也是一知半解..

```python
optimizer = tf.train.GradientDescentOptimizer(0.01)  #梯度下降
train = optimizer.minimize(loss) #loss最小化
```

接着我们建立我们的训练数据

```python
# training data
x_train = [1,2,3,4]  #用来训练目标函数的数据
y_train = [0,-1,-2,-3] #期望的结果数据
train_data = {x:x_train,y:y_train} #训练数据
```

初始化变量

```python
init = tf.global_variables_initializer()  #对图中所有变量初始化,后面再添加变量后可以再次运行,但原有的init不包括新的变量
with tf.Session() as sess: #打开一个会话
    sess.run(init)    #运行变量
    for i in range(1000):#1000个loop
        sess.run(train,train_data) #开始用优化器即低度下降来训练数据
    curr_W, curr_b, curr_loss = sess.run([W, b, loss], train_data) 
    print "W: %s b: %s loss: %s" % (curr_W, curr_b, curr_loss) #打印weight,bias term和loss

```

下面是完整代码:

```python
#coding: utf-8
import tensorflow as tf
import numpy as np

def learn_tf_train_api():
    """
    The completed trainable linear regression model is shown here:
    """
    # Model Parameters
    W = tf.Variable([.3],tf.float32)
    b = tf.Variable([-.3],tf.float32)

    # Model input and output
    x = tf.placeholder(tf.float32)
    linear_model = W * x + b
    y = tf.placeholder(tf.float32)

    # loss
    loss = tf.reduce_sum(tf.square(linear_model - y))

    # optimizer
    optimizer = tf.train.GradientDescentOptimizer(0.01)
    train = optimizer.minimize(loss)

    # training data
    x_train = [1,2,3,4]
    y_train = [0,-1,-2,-3]
    train_data = {x:x_train,y:y_train}
    #trainging loop
    init = tf.global_variables_initializer()
    with tf.Session() as sess:
        sess.run(init)    ## reset values to incorrect defaults.
        for i in range(1000):
            sess.run(train,train_data)
        #evaluate training accuracy
        curr_W, curr_b, curr_loss = sess.run([W, b, loss], train_data)
        print "W: %s b: %s loss: %s" % (curr_W, curr_b, curr_loss)

 if __name__ == '__main__':
    learn_tf_train_api()
```



继续探索中,希望以后对我的工作能有所帮助.



