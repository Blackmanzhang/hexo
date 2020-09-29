---
layout: source/_posts
title: CentOS7防火墙开放，删除，查看端口
date: 2020-09-30 00:35:33
tags: [centos]
---

1.防火墙的启动/停止/状态：

```bash
#启动防火墙
systemctl start firewalld.service
#关闭防火墙
systemctl stop firewalld.service
#重启防火墙
systemctl restart firewalld.service
#查看防火墙状态
systemctl status firewalld.service
#设置开机启动防火墙
systemctl enable firewalld.service
#设置开机不启动防火墙
systemctl disable firewalld.service
```

2.新增开放端口号

```bash
firewall-cmd --zone=public --add-port=80/tcp --permanent
#说明:
#–zone 作用域
#–add-port=80/tcp #添加端口，格式为：端口/通讯协议
#–permanent 永久生效，没有此参数重启后失效
 
#多个端口:
firewall-cmd --zone=public --add-port=80-90/tcp --permanent#添加完毕过后重新读取防火墙规则或者重启防火墙，规则才生效firewall-cmd --reload或者重启防火墙：systemctl restart firewalld.service
```

3.查看端口号

```bash
#centos7查看防火墙所有信息
firewall-cmd --list-all
#centos7查看防火墙开放的端口信息
firewall-cmd --list-ports
```

4.删除端口

```bash
#删除80端口
firewall-cmd --zone=public --remove-port=80/tcp --permanent
```

参考：https://blog.csdn.net/qq_36663951/article/details/82115086