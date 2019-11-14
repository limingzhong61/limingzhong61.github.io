---
title: docker
p: /docker/docker
date: 2019-11-12 15:39:56
categories: learn-note
tags: [learn-note,docker]
---
[TOC]

# 1.简介

**Docker**是一个开源的应用容器引擎；是一个轻量级容器技术；

Docker支持将软件编译成一个镜像；然后在镜像中各种软件做好配置，将镜像发布出去，其他使用者可以直接使用这个镜像；

运行中的这个镜像称为容器，容器启动是非常快速的。

![img](docker/%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20180303145450.png)

![img](docker/%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20180303145531.png)

## 2、核心概念

docker主机(Host)：安装了Docker程序的机器（Docker直接安装在操作系统之上）；

docker客户端(Client)：连接docker主机进行操作；

docker仓库(Registry)：用来保存各种打包好的软件镜像；

docker镜像(docker)：软件打包好的镜像；放在docker仓库中；

docker容器(Container)：镜像启动后的实例称为一个容器；容器是独立运行的一个或一组应用

![](docker/%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20180303165113.png)

使用Docker的步骤：

1）、安装Docker

2）、去Docker仓库找到这个软件对应的镜像；

3）、使用Docker运行这个镜像，这个镜像就会生成一个Docker容器；

4）、对容器的启动停止就是对软件的启动停止；

## 3、安装Docker

#### 1）、安装linux虚拟机

​	1）、VMWare、VirtualBox（安装）；

​	2）、导入虚拟机文件centos7-atguigu.ova；

​	3）、双击启动linux虚拟机;使用  root/ 123456登陆

​	4）、使用客户端连接linux服务器进行命令操作；

​	5）、设置虚拟机网络；

​		桥接网络===选好网卡====接入网线；

​	6）、设置好网络以后使用命令重启虚拟机的网络

```shell
service network restart
```

​	7）、查看linux的ip地址

```shell
ip addr
```

​	8）、使用客户端连接linux；

#### 2）、在linux虚拟机上安装docker

步骤：(安装不成功可以到菜鸟教程安装)

1、检查内核版本，必须是3.10及以上
uname -r
2、安装docker
yum install docker

```shell
sudo apt install docker.io
```



3、输入y确认安装
4、启动docker

```shell
[root@localhost ~]# systemctl start docker
[root@localhost ~]# docker -v
Docker version 1.12.6, build 3e8e77d/1.12.6
```

5、开机启动docker
[root@localhost ~]# systemctl enable docker
Created symlink from /etc/systemd/system/multi-user.target.wants/docker.service to /usr/lib/systemd/system/docker.service.
6、停止docker
systemctl stop docker

## 4、Docker常用命令&操作

### 1）、镜像操作

| 操作 | 命令                                            | 说明                                                     |
| ---- | ----------------------------------------------- | -------------------------------------------------------- |
| 检索 | docker  search 关键字  eg：docker  search redis | 我们经常去docker  hub上检索镜像的详细信息，如镜像的TAG。 |
| 拉取 | docker pull 镜像名:tag                          | :tag是可选的，tag表示标签，多为软件的版本，默认是latest  |
| 列表 | docker images                                   | 查看所有本地镜像                                         |
| 删除 | docker rmi image-id                             | 删除指定的本地镜像                                       |

https://hub.docker.com/

### 2）、容器操作

软件镜像（QQ安装程序）----运行镜像----产生一个容器（正在运行的软件，运行的QQ）；

| 操作     | 命令                                                         | 说明                                                         |
| -------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 运行     | docker run --name container-name -d image-name<br/>eg:docker run –name myredis –d redis<br/>-- | --name：自定义容器名<br/>-d：后台运行<br/>image-name:指定镜像模板 |
| 列表     | docker ps（查看运行中的容器）；                              | 加上-a；可以查看所有容器                                     |
| 启动     | docker stop container-name/container-id                      | 启动容器                                                     |
| 停止     | docker stop container-name/container-id                      | 停止当前你运行的容器                                         |
| 删除     | docker rm container-id                                       | 删除指定容器                                                 |
| 端口映射 | -p 6379:6379<br/>eg:docker run -d -p 6379:6379 --name myredisdocker.io/redis | -p:主机端口(映射到)容器内部的端口                            |
| 容器日志 | docker logs container-name/container-id                      |                                                              |
| 更多命令 | https://docs.docker.com/engine/reference/commandline/docker/ |                                                              |

步骤：

```shell
1、搜索镜像
[root@localhost ~]# docker search tomcat
2、拉取镜像
[root@localhost ~]# docker pull tomcat
3、根据镜像启动容器
docker run --name mytomcat -d tomcat:latest
4、docker ps  
查看运行中的容器
5、 停止运行中的容器
docker stop  容器的id
6、查看所有的容器
docker ps -a
7、启动容器
docker start 容器id
8、删除一个容器
 docker rm 容器id
9、启动一个做了端口映射的tomcat
[root@localhost ~]# docker run -d -p 8888:8080 tomcat
-d：后台运行
-p: 将主机的端口映射到容器的一个端口    主机端口:容器内部的端口

10、为了演示简单关闭了linux的防火墙
service firewalld status ；查看防火墙状态
service firewalld stop：关闭防火墙
11、查看容器的日志
docker logs container-name/container-id

更多命令参看
https://docs.docker.com/engine/reference/commandline/docker/
可以参考每一个镜像的文档

```

![](docker\关闭firewall的参数.png)

### 3）、安装MySQL示例

```shell
docker pull mysql
#mysql 版本最好在5.7左右，不然navicat连不上
[root@MiWiFi-R3A-srv ~]# docker pull mysql:5.7.27
```



错误的启动

```shell
[root@localhost ~]# docker run --name mysql01 -d mysql
42f09819908bb72dd99ae19e792e0a5d03c48638421fa64cce5f8ba0f40f5846

mysql退出了
[root@localhost ~]# docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                           PORTS               NAMES
42f09819908b        mysql               "docker-entrypoint.sh"   34 seconds ago      Exited (1) 33 seconds ago                            mysql01
538bde63e500        tomcat              "catalina.sh run"        About an hour ago   Exited (143) About an hour ago                       compassionate_
goldstine
c4f1ac60b3fc        tomcat              "catalina.sh run"        About an hour ago   Exited (143) About an hour ago                       lonely_fermi
81ec743a5271        tomcat              "catalina.sh run"        About an hour ago   Exited (143) About an hour ago                       sick_ramanujan


//错误日志
[root@localhost ~]# docker logs 42f09819908b
error: database is uninitialized and password option is not specified 
  You need to specify one of MYSQL_ROOT_PASSWORD, MYSQL_ALLOW_EMPTY_PASSWORD and MYSQL_RANDOM_ROOT_PASSWORD；这个三个参数必须指定一个
```

正确的启动

```shell
[root@localhost ~]# docker run --name mysql01 -e MYSQL_ROOT_PASSWORD=123456 -d mysql
nicolas@iZuf6i77uwsb2oiszspgvkZ:/home/admin$ sudo docker run --name mysql01 -e MYSQL_ROOT_PASSWORD=123456 -d mysql:5.7.27
b874c56bec49fb43024b3805ab51e9097da779f2f572c22c695305dedd684c5f
[root@localhost ~]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
b874c56bec49        mysql               "docker-entrypoint.sh"   4 seconds ago       Up 3 seconds        3306/tcp            mysql01
```

做了端口映射

```shell
[root@localhost ~]# docker run -p 3306:3306 --name mysql02 -e MYSQL_ROOT_PASSWORD=123456 -d mysql
nicolas@iZuf6i77uwsb2oiszspgvkZ:/home/admin$ sudo docker run -p 3306:3306 --name mysql01 -e MYSQL_ROOT_PASSWORD=123456 -d mysql:5.7.27

ad10e4bc5c6a0f61cbad43898de71d366117d120e39db651844c0e73863b9434
[root@localhost ~]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
ad10e4bc5c6a        mysql               "docker-entrypoint.sh"   4 seconds ago       Up 2 seconds        0.0.0.0:3306->3306/tcp   mysql02
```



几个其他的高级操作

```shell
docker run --name mysql03 -v /conf/mysql:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag
把主机的/conf/mysql文件夹挂载到 mysqldocker容器的/etc/mysql/conf.d文件夹里面
改mysql的配置文件就只需要把mysql配置文件放在自定义的文件夹下（/conf/mysql）

docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
指定mysql的一些配置参数

#创建mysql容器+character-set
docker run -p 3306:3306 --name mysql03 -e MYSQL_ROOT_PASSWORD=123456 -d mysql:5.7.27 --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci

docker run -d -p 3307:3306 -e MYSQL_ROOT_PASSWORD=db123456 --name mysql -e TZ=Asia/Shanghai mysql:5.7


```

```shell
-e TZ=Asia/Shanghai     时区修改为中国时区

-p 3306：3306：将容器的3306端口映射到主机的3306端口

-v $ PWD / conf / my.cnf：/etc/mysql/my.cnf：将主机当前目录下的conf / my.cnf挂载到容器的/etc/mysql/my.cnf

-v $ PWD / logs：/ logs：将主机当前目录下的日志目录挂载到容器的/ logs

-v $ PWD / data：/ mysql_data：将主机当前目录下的数据目录挂载到容器的/ mysql_data

-e MYSQL_ROOT_PASSWORD = 123456：初始化 root用户的密码
```

**修改时区和字符集**

```shell

docker run -p 3306:3306 --name ch-timezone-mysql -e MYSQL_ROOT_PASSWORD=123456 -e TZ=Asia/Shanghai -d mysql:5.7.27 --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
```



### 安装mysql之后进行操作

3.进入容器bash并进入mysql命令行：

```shell
ubuntu@VM-0-5-ubuntu:/etc/apt$ sudo docker exec -it mysql02 bash

```

Docker容器启动的时候，如果要挂载宿主机的一个目录，可以用-v参数指定。

譬如我要启动一个centos容器，宿主机的/test目录挂载到容器的/soft目录，可通过以下方式指定：

```shell
docker run -it -v /test:/soft centos /bin/bash
docker run -it -v /hmoe/ubuntu:/tmp mysql02 /bin/bash
```

# 安装redis

```shell
runoob@runoob:~$ mkdir -p ~/redis ~/redis/data
```

```java
sudo docker run -p 6379:6379 -v $PWD/data:/data  -d redis redis-server --appendonly yes

```

命令说明：

**-p 6379:6379 :** 将容器的6379端口映射到主机的6379端口

**-v $PWD/data:/data :** 将主机中当前目录下的data挂载到容器的/data

**redis-server --appendonly yes :** 在容器执行redis-server启动命令，并打开redis持久化配置

## 连接、查看容器

使用redis镜像执行redis-cli命令连接到刚启动的容器,主机IP为172.17.0.1

```
runoob@runoob:~/redis$ docker exec -it 43f7a65ec7f8 redis-cli
172.17.0.1:6379> info
# Server
redis_version:3.2.0
redis_git_sha1:00000000
redis_git_dirty:0
redis_build_id:f449541256e7d446
redis_mode:standalone
os:Linux 4.2.0-16-generic x86_64
arch_bits:64
multiplexing_api:epoll
...
```

```shell
docker exec -it 239fadb88042 redis-cli
```

## 官方文档

### 配置文件映射

Alternatively, you can specify something along the same lines with docker run options.

```
$ docker run -v /myredis/conf/redis.conf:/usr/local/etc/redis/redis.conf --name myredis redis redis-server /usr/local/etc/redis/redis.conf
```

Where `/myredis/conf/` is a local directory containing your `redis.conf `file. Using this method means that there is no need for you to have a Dockerfile for your redis container.

### 其他

```shell
docker run -d --privileged=true -p 6379:6379 --restart always -v /root/docker/redis/conf/redis.conf:/etc/redis/redis.conf -v /root/docker/redis/data:/data --name myredis redis redis-server /etc/redis/redis.conf --appendonly yes
```

```
-d                                -> 以守护进程的方式启动容器
-p 6379:6379                      -> 绑定宿主机端口
--name myredis                    -> 指定容器名称
--restart always                  -> 开机启动
--privileged=true                 -> 提升容器内权限
-v /root/docker/redis/conf:/etc/redis/redis.conf    -> 映射配置文件
-v /root/docker/redis/data:/data   -> 映射数据目录
--appendonly yes                   -> 开启数据持久化
```

