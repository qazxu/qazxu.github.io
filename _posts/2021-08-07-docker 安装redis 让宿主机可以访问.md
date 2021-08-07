---
layout: mypost
title: docker 安装redis 让宿主机可以访问
categories: [docker]
---

### redis 镜像获取

```shell
docker pull redis  #后面可以带上tag号, 默认拉取最新版本
```

### redis引用本地配置文件

#### linux

安装之前去定义我们的redis.conf文件, 这一步很重要, redis.conf目录 $PWD/conf/redis.conf

````shell
wget http://download.redis.io/redis-stable/redis.conf
````

最好将该目录权限改为当前的user, 执行命令:

````shell
sudo chown -R $USER ~/conf
````

#### windows

1、下载文件至本地目录

例

```shell
D:\workSoftware\docker\redis\redis.conf

d/workSoftware/docker/redis/redis.conf
```

#### 修改redis.conf

注释掉bind 127.0.0.1, 修改protected-mode off (新版本是 no)

### 创建redis容器

#### linux

````shell
docker run -p 6379:6379 --name myredis -v $PWD/conf/redis.conf:/etc/redis/redis.conf -v $PWD/data:/data -d redis redis-server /etc/redis/redis.conf --appendonly yes
````

#### win10

```shell
docker run -p 6379:6379 --name myredis -v d/workSoftware/docker/redis/redis.conf:/etc/redis/redis.conf -v d/workSoftware/docker/redis/data:/data -d redis redis-server /etc/redis/redis.conf --appendonly yes
```



### 参考
- [docker 安装redis , 让宿主机可以访问](https://www.cnblogs.com/shihuibei/p/10663112.html)
- [Win10中docker的安装与使用](https://blog.csdn.net/zzq060143/article/details/91050272)
