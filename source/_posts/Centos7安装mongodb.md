---
layout: source/_posts
title: Centos7安装mongodb
date: 2020-09-29 23:24:23
tags: [centos,mongodb]
---

1.打开官网按照官网流程进行

mongodb官网：https://www.mongodb.com/

2.配置程序包管理系统（`yum`）[
](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-red-hat/#configure-the-package-management-system-yum)

创建一个`/etc/yum.repos.d/mongodb-org-4.2.repo`文件，以便您可以使用`yum`以下命令直接安装MongoDB ：

```bash
[mongodb-org-4.2]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/4.2/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-4.2.asc
```

3.安装MongoDB软件包

要安装最新的稳定版MongoDB，请发出以下命令：

```bash
sudo yum install -y mongodb-org
```

或者，要安装特定版本的MongoDB，请分别指定每个组件软件包，并将版本号附加到软件包名称中，如以下示例所示：

```bash
sudo yum install -y mongodb-org-4.2.5 mongodb-org-server-4.2.5 mongodb-org-shell-4.2.5 mongodb-org-mongos-4.2.5 mongodb-org-tools-4.2.5
```

您可以指定任何可用的MongoDB版本。但是`yum` ，当有新版本可用时，将升级软件包。为防止意外升级，请固定包装。要固定包，`exclude`请在`/etc/yum.conf`文件中添加以下指令：

```bash
exclude=mongodb-org,mongodb-org-server,mongodb-org-shell,mongodb-org-mongos,mongodb-org-tools
```

4.安装完成之后，即可使用以下命令查看状态：

```bash
systemctl status mongod.service　　# 查看mongod状态
systemctl start mongod.service　　# 启动
systemctl stop mongod.service 　　# 停止
systemctl enable mongod.service 　　# 自启
```

5.当mongod启动之后，即可使用mongo命令进行连接

6.默认配置文件在/etc下的mongod.conf，修改其配置，然后重启服务生效

 

参考链接：https://docs.mongodb.com/manual/tutorial/install-mongodb-on-red-hat/