---
layout: source/_posts
title: CentOS7下安装Python3.7.4（与python2.x共存）
date: 2020-09-30 00:37:24
tags: [centos,python]
---

首先声明一下，这篇博客是因为遇到了太多复制粘贴，不加以验证的博客，加上我刚好重置了一下服务器，新安装的python3.7也遇到一些问题，需要重新配置。

致敬博主！参考博客如下：

源博客地址：https://blog.csdn.net/qq_39091354/article/details/86584046 标题：centos7+Python3.7的正确安装方法（与Python2.X共存）

源博客地址：https://blog.csdn.net/qq_36416904/article/details/79316972　标题：关于在centos下安装python3.7.0以上版本时报错ModuleNotFoundError: No module named '_ctypes'的解决办法

 

我们需要达到的目标是在centos7.4上，安装Python3.7.4（目前最新版），并同时与服务器上自带的python2共存。

1.打开python的官网，我们下载python的tgz文件：（此处没有使用wget是因为服务器使用命令下载比较慢，我选择自己下载了上传上去）

python官网下载地址：https://www.python.org/downloads/release/python-374/

![img](https://img2018.cnblogs.com/blog/1622662/201907/1622662-20190725225547503-474435666.png)

2.上传文件：

使用xftp上传文件到/usr/local下：

![img](https://img2018.cnblogs.com/blog/1622662/201907/1622662-20190725225832602-692798369.png)

上传上去过后，文件已经存在与local目录下。

3.解压文件：

```bash
tar zxvf 下载的文件名
```

例：

```bash
tar zxvf Python-3.7.4.tgz 　　(我的是3.7.4版本)
```

解压完成过后，local目录下就有一个Python-3.7.4文件夹

4.添加一些安装依赖：

```bash
yum -y install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel gcc libffi-devel gcc make automake autoconf libtool libffi-devel
```

libffi-devel这个是3.7版本需要的一个新的包，这包很重要，若没有安装此包，则会在安装的时候报错：**ModuleNotFoundError: No module named '_ctypes'**

**![img](https://img2018.cnblogs.com/blog/1622662/201907/1622662-20190725230728188-1969482576.png)**

我这里已安装过，则已经安装

5.进入Python-3.7.4解压目录：

```bash
cd Python-3.7.4
```

6.进行初始配置：

```bash
./configure --prefix=/usr/local/python3　　　　(我这里安装在/usr/local/python3 目录下，有需要安装在其他地方的则修改目录为想要安装的位置)
```

7.执行安装：

```bash
make && make install 
```

8.安装完成就配置软连接：

```bash
ln -s /usr/local/python3/bin/python3 /usr/bin/python3
ln -s /usr/local/python3/bin/pip3 /usr/bin/pip3
```

9.验证：

```bash
python3 -V
```

![img](https://img2018.cnblogs.com/blog/1622662/201907/1622662-20190725231420967-1424880216.png)

```bash
pip3 -V
```

 ![img](https://img2018.cnblogs.com/blog/1622662/201907/1622662-20190725231452562-141449130.png)

返回了安装的版本信息则说明安装成功，接下来可以升级pip3

```bash
pip3 install --upgrade pip
```

yum命令可以照常使用，至此安装结束。