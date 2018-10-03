---
title: '安装gitlab,rancher和harbor'
date: 2018-07-15 22:42:41
tags: [技术]
---

周末第一天去了香港跑代购，出门回家都是风大雨大，算是赚了个饭钱。如果中午一餐没在香港吃，还可以省下几十块。哈哈

然后回到家后，吃了晚饭就去网吧帮朋友装gitlab，rancher和harbor。

---

## 装rancher集群版

    首先要装一个mysql数据库，朋友说要集群版的，无奈当天晚上没搞定集群版的，回家查了下，可以用innodb方案。
    步骤如下：
    1. 打开 [rancher安装手册](https://docs.xtplayer.cn/rancher/installing/installing-server/)
    2. 注意下安装需求，不然可能会安装失败。
 
      - docker要用overlay的storage driver
      - 要关闭防火墙以及selinux，不然还得装selinux插件
      - rancher不支持mac，所以不要再mac上捣鼓了
      - mysql要用5.7，用默认COMPACT选项运行Antelope， 运行MySQL 5.7，使用Barracuda。默认选项ROW_FORMAT需设置成Dynamic
      - max_connections记得设置大一点，按照一个节点需要50个来算。
      - max_packet_size一定要设置大一点，我设置的1024M

    安装手册上还有推荐配置，大家可以按照推荐配置去配。

3. 安装docker
4. 在数据库里面建库建表。
5. 然后执行启动rancher的命令(如果是HA的话，每一个主机上都要使用这个命令执行)，打开ip加8080端口就可以看到rancher界面啦

```python
docker run -d --restart=unless-stopped -p 8080:8080 -p 9345:9345 rancher/server \
     --db-host myhost.example.com --db-port 3306 --db-user username --db-pass password --db-name cattle \
     --advertise-address <IP_of_the_Node>
```

---



##装gitlab

开始准备用 [docker-compose文件](https://github.com/sameersbn/docker-gitlab/blob/master/docker-compose.yml) 来装的，结果发现官方有很全的安装文档。

因为朋友的是centos7，所有就按照下面的文档直接复制粘贴一路下来：
[gitlab安装](https://about.gitlab.com/installation/#centos-7)


##装harbor

1. 下载offline安装包咯，简单点。 [offline](https://storage.googleapis.com/harbor-releases/release-1.5.0/harbor-offline-installer-v1.5.1.tgz)
2. 在要装harbor的机器上安装docker和docker-compose.
3. 然后修改harbor.cfg里面的ip为安装机器的ip，密码默认是Harbor123456, 当然你可以修改为你自己的。
4. 然后root执行./prepare，然后再root执行install.sh
5. 坐等一会，安装完了，打开浏览器，输入安装harbor的机器的ip地址就可以看到ui界面啦。