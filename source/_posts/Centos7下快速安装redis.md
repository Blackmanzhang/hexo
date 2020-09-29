---
layout: source/_posts
title: Centos7下快速安装redis
date: 2020-09-29 23:20:52
tags: [centos,redis]
---

源文件安装

在CentOS和Red Hat系统中，首先添加EPEL仓库，然后更新yum源：

```bash
sudo yum install epel-release
sudo yum update
```

然后安装Redis数据库：

```bash
sudo yum -y install redis
```

安装好后启动Redis服务即可：

```bash
sudo systemctl start redis
```

这里同样可以使用redis-cli进入Redis命令行模式操作。

另外，为了可以使Redis能被远程连接，需要修改配置文件，路径为/etc/redis.conf

```bash
vi /etc/redis.conf
```

需要修改的地方：

首先，注释这一行：

```
#bind 127.0.0.1
```

另外，推荐给Redis设置密码，取消注释这一行：

```bash
#requirepass foobared
```

foobared即当前密码，可以自行修改为

```bash
requirepass 密码
```

然后重启Redis服务，使用的命令如下：

```bash
sudo systemctl restart redis
```

```bash
systemctl start redis.service #启动redis服务器

systemctl stop redis.service #停止redis服务器

systemctl restart redis.service #重新启动redis服务器

systemctl status redis.service #获取redis服务器的运行状态

systemctl enable redis.service #开机启动redis服务器

systemctl disable redis.service #开机禁用redis服务器
```

转自：[https://www.centos.bz/2017/09/linux-centos-7%e5%ae%89%e8%a3%85redis%e6%9c%8d%e5%8a%a1%e5%99%a8%e6%95%99%e7%a8%8b/](https://www.centos.bz/2017/09/linux-centos-7安装redis服务器教程/)

 