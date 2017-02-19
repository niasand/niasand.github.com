---
title: 学习使用locust作性能测试
date: 2017-02-19 12:06:13
tags: [性能测试]
---

   之前用tsung做过性能测试，但是要用xml去配置接口参数，而且对字符的转义也做得不是很友好。 后来听说有个locust性能测试工具很强大，于是在家试了一下。 目前Locust只支持python2

   首先我用Flask写了一个简单的接口,代码如下：

```python
# coding: utf-8
from flask import Flask, request,jsonify
app = Flask(__name__)

@app.route('/')
@app.route('/learn_locust',methods=['GET','POST'])
def locust_test():
    _id = request.args.get('username')
    rv = {"code": {"data": {"username": _id, "created": "2017-02-19 18:00:00"}}}
    error = {"code":{"data":{"error":"not good"}}}
    if _id == 'bb8':
        return jsonify(rv)
    else:
        return jsonify(error)

if __name__ == '__main__':
    app.run(host='0.0.0.0', debug=True, port=5000)
```



启动flask服务，然后调用一下：

```shell
curl http://localhost:5000/learn_locust\?username=bb8
```

发现返回如下图：

![屏幕快照 2017-02-19 下午12.25.11](http://7jpsil.com1.z0.glb.clouddn.com/locust%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202017-02-19%20%E4%B8%8B%E5%8D%8812.25.11.png)



嗯。 没有问题，接下来就是写locust脚本。脚本如下：

```python
from locust import HttpLocust, TaskSet, task
'''
首先从locust类中导入HttpLocust类，这个类用来做http协议的请求
然后再导入TaskSet类，定义一组用户要执行的任务集。这个类会执行用户的task属性的任务。TaskSet可以被嵌套，一个TaskSet的task装饰器可以嵌套另一个TaskSet。
TaskSet在执行task过程中的时候会有一个最小和最大等待时间，min_wait和max_wait参数。

task类，是可以很方便的让我们用装饰器去为TaskSet声明一个事务，他可以带上一个权重参数，如果权值越高，那么被选中的概率就越大
'''


class UserBehavior(TaskSet):
    def on_start(self):
        ''' on_start is called when a Locust start before any task is scheduled '''
        self.gslb()

    @task()
    def gslb(self):
        url = self.locust.host + '/learn_locust'
        ''' 设定好url， headers和url的参数'''
        headers = {
            'User-Agent': 'Dalvik/1.6.0 (Linux; U; Android 4.4.4; HM NOTE 1S MIUI/7.2.9)',
            'Connection': 'Keep-Alive',
            'Accept-Encoding': 'gzip',
        }

        params = {
            'username': 'bb8'
        }

        self.response = self.client.request(
            method='GET',
            url=url,
            headers=headers,
            params=params,
        )

    ### Additional tasks can go here ###


class WebsiteUser(HttpLocust):
    task_set = UserBehavior
    min_wait = 1000
    max_wait = 3000

```

然后将上述文件命令为locustfile.py， 接着执行

```shell
locust --host=http://localhost:5000
```

如果你的文件名不是*locustfile.py*,  那么你要加个参数-f， 如下

```python
locust -f locust_files/my_locust_file.py --host=http://example.com
```

执行完后，你会看到![屏幕快照 2017-02-19 下午12.55.05](http://7jpsil.com1.z0.glb.clouddn.com/locust%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202017-02-19%20%E4%B8%8B%E5%8D%8812.55.05.png)

然后打开webui地址：

```shell
http://127.0.0.1:8089/
```

你会首先要求设置下并发用户数和用户加载策略，我设置的是10个用户并发，然后Hatch Rate是每秒启动多少用户的意思， 如果设置为10，就是同时启动10个了。点击start swarming 会看到如下图

![屏幕快照 2017-02-19 下午12.57.09](http://7jpsil.com1.z0.glb.clouddn.com/locust%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202017-02-19%20%E4%B8%8B%E5%8D%8812.57.09.png)

其中， Average为平均响应时间的测试指标，最后一列的reqs/sec相当TPS，Locust叫做rps。 

在停掉python命令后，也可以看到一些测试数据：



![屏幕快照 2017-02-19 下午1.02.55](http://7jpsil.com1.z0.glb.clouddn.com/locust%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202017-02-19%20%E4%B8%8B%E5%8D%881.02.55.png)



如果不喜欢web界面，也可以这样去执行

```python
locust --host=http://localhost:5000 --no-web -c 1 -n 4 
```

-c是指并发数，-n是执行的次数。再配合代理，调试也不复杂。

 Locust也可以做分布式执行，需要装一个pyzmq。

性能测试首先而在于分析性能测试的需求，设计性能测试场景，尽可能的模拟真实环境中的压力（正常和异常情况）。然后结果是考察并发用户数、响应时间、tps这类指标吧。

