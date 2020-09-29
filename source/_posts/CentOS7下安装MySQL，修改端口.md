---
layout: source/_posts
title: CentOS7下安装MySQL，修改端口
date: 2020-09-30 00:42:09
tags: [mysql,centos]
---

CentOS7服务器上部署MySQL：

一、添加yum源

MySQL官网：https://dev.mysql.com/downloads/repo/yum/

进入官网去选择合适的rpm包，包的作用是添加MySQL yum源，请下载对应的yum源，我用的centos7,下载的是 mysql80-community-release-el7-3.noarch.rpm。下载之后通过XFTP上传文件到服务器，然后执行 yum localinstall 命令：

```bash
yum localinstall mysql80-community-release-el7-3.noarch.rpm
```

如果权限不够请使用sudo执行，执行完毕过后我们可以使用 cd /etc/yum.repos.d进入到目录中查看文件，发现会有如下两个文件：

- mysql-community.repo
- mysql-community-source.repo

二、安装

当我们添加好yum源之后就可以执行安装命令：

```bash
yum install mysql-community-server
```

三、启动

安装完成之后使用 systemctl start 执行启动MySQL命令：

```bash
systemctl start mysqld.service
```

使用 systemctl status mysqld.service 可以查看MySQL的运行状态。

![img](https://img2018.cnblogs.com/blog/1622662/201906/1622662-20190621092240004-1710454417.png)

关闭命令:

```bash
systemctl stop mysqld.service
```

重启命令：

```bash
systemctl restart mysqld.service
```

四、修改密码

当MySQL服务启动之后我们就需要对密码进行更改：MySQL 默认创建了 root 用户的密码，这个密码打印在 MySQL 的日志文件`/var/log/mysqld.log`中，可以通过`temporary password`关键字来找出这个临时的密码。

```bash
grep 'temporary password' /var/log/mysqld.log
```

找到密码之后使用改密码连接数据库：

```bash
mysql -u root -p
```

然后修改密码：

```mysql
ALTER USER 'root'@'localhost' IDENTIFIED BY 'NewPassword';
```

执行上述命令密码将被修改为：NewPassword

PS：新版本的MySQL对密码强度有限制，执行到上一步的时候，会提示密码强度不够，则应更改为更高强度的密码。

密码更改完成之后重启MySQL服务：

```bash
systemctl restart mysqld.service
```

 

五、开放远程连接

MySQL默认只对本机开放连接，我们则需要对mysql表的host字段进行修改以支持其他主机连接,%表示所有。

```mysql
# 先连接数据库
use mysql;
update user set host = '%' where user = 'root';
```

更改完成之后刷新权限：

```mysql
flush privileges;
```

然后在navicat中连接：(mysql新版的和之前的密码加密方式不同，若遇到连接错误，请使用navicat12尝试进行连接，或者修改mysql的密码加密方式，暂不赘述)

![img](https://img2018.cnblogs.com/blog/1622662/201906/1622662-20190621094006190-511237256.png)

连接名取什么无所谓，主机是服务器ip，端口如果没有改动，默认3306，记得在服务器控制台中和防火墙开放端口，否则依然无法连接！

**备注：修改端口和防火墙放行**

进入mysql数据库，输入show global variables like 'port'; 查看端口号

mysql> show global variables like 'port';  
+---------------+-------+  
| Variable_name | Value |  
+---------------+-------+  
| port | 3306 |  
+---------------+-------+  
1 row in set (0.00 sec) 

修改端口，编辑文件，vim /etc/my.conf，添加端口参数：

port=2020

![img](https://img2018.cnblogs.com/i-beta/1622662/201911/1622662-20191118101835348-1275355187.png)

注：2020为我自己修改的端口号！之后连接端口就是2020

然后重启mysql:

```bash
systemctl restart mysqld.service
```

防火墙：

查看防火墙状态：

```bash
systemctl status firewalld
```

![img](https://img2018.cnblogs.com/i-beta/1622662/201911/1622662-20191118102150527-956749905.png)

 

 如图，防火墙正在运行

开放端口放行：

```bash
firewall-cmd --zone=public --add-port=3306/tcp --permanent
```

加上--permanet参数永久生效，如果前面修改了端口号，此处的3306就应改成对应的端口号，然后使用命令重新读取防火墙规则：

```bash
firewall-cmd --reload
```

![img](https://img2018.cnblogs.com/i-beta/1622662/201911/1622662-20191118102515528-1687476673.png)

至此，MySQL端口号和防火墙配置完毕！

参考：

- https://dev.mysql.com/doc/refman/8.0/en/linux-installation-yum-repo.html
- https://dev.mysql.com/doc/refman/8.0/en/connecting-disconnecting.html
- https://dev.mysql.com/doc/refman/8.0/en/mysql-server.html
- http://dev.mysql.com/downloads/repo/yum/
- https://blog.csdn.net/lihao21/article/details/80692068 