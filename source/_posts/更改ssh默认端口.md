---
layout: centos7
title: 更改ssh默认端口
date: 2020-09-30 00:33:48
tags: centos
---

服务器默认为22端口，这样会造成有被暴力破解密码的风险，下面是更换ssh端口过程

**1.添加ssh端口**

```bash
vim /etc/ssh/sshd_config
```

打开配置文件，添加我们需要更改的端口号，此时不要删除默认22端口，让两个端口同时存在，如果我们直接修改了端口，然后启动防火墙之后，就会出现我们没有使用防火墙开放端口，导致我们连接不上服务器，我们暂且保留默认22，如果更改过后，使用新端口号没问题，再删除默认22端口不迟

![img](https://img2018.cnblogs.com/i-beta/1622662/201912/1622662-20191206005324226-1151657073.png)

 我们想把端口改为2020，就如图添加上去，保存退出，然后重启ssh服务

```bash
systemctl restart sshd.service
```

2。配置防火墙规则

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



首先启动防火墙，然后添加防火墙规则



```bash
firewall-cmd --zone=public --add-port=2020/tcp --permanent　　
#说明:#开放2020端口
#–zone 作用域
#–add-port=2020/tcp #添加端口，格式为：端口/通讯协议
#–permanent 永久生效，没有此参数重启后失效
#添加完毕过后重新读取防火墙规则或者重启防火墙，规则才生效
#重新读取防火墙规则
firewall-cmd --reload
#或者重启防火墙：
systemctl restart firewalld.service
```



3.断开当前ssh连接，然后把ssh连接端口改为我们示例所修改的2020尝试连接，如果连接正常，我们继续如下步骤：

![img](https://img2018.cnblogs.com/i-beta/1622662/201912/1622662-20191206010817708-2032870398.png)

 

 正常连接过后，我们此时就可以删除默认的22端口了

```bash
vim /etc/ssh/sshd_config
```

删除Port 22 这一行，只留下我们的Port 2020，然后重启ssh服务

```bash
systemctl restart sshd.service
```

至此，centos7默认ssh端口修改完成

 