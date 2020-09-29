---
layout: source/_posts
title: deepin安装go，配置环境变量
date: 2020-09-29 22:37:46
tags: [go,deepin]
---

如果你想在深度Deepin操作系统上搭建开发环境，本文教你安装Go的方法，同时附上Go环境变量配置的方法，总的来说，在深度系统中搭建Go开发环境只需要三个步骤，以下为你介绍。

**第一步、Go安装包下载**

我们在下载地址 https://golang.google.cn/dl/ 中下载go1.12.9.linux-amd64.tar.gz软件包：

![在深度Deepin系统上安装Go的方法，附Go环境变量配置](https://www.linux110.com/picture/19/1-1ZRQ5144Y48.JPG)

**第二步、解压并安装Go**

在Deepin系统中可以直接->右键点击该文件->提取（解压）到当前文件夹。

或者终端下命令：tar zxvf  go1.12.9.linux-amd64.tar.gz，如下图所示：

![在深度Deepin系统上安装Go的方法，附Go环境变量配置](https://www.linux110.com/picture/19/1-1ZRQ51456103.JPG)

接下来将解压的文件拷贝到系统目录下，有3种常用软件安装目录，想尽快安装golang的可以忽略：

/usr：系统软件安装目录。

/usr/local：用户软件安装目录。

/opt：大型软件安装目录。

在这里我们选择安装到/usr/local，移动解压后生成的go文件夹到/usr/local/目录下：

终端下命令（移动）：sudo mv go /usr/local

或者命令（拷贝）：sudo cp -r go /usr/local

![在深度Deepin系统上安装Go的方法，附Go环境变量配置](https://www.linux110.com/picture/19/1-1ZRQ51503133.JPG)

**第三步、Go环境变量配置**

接下来来到最后一步环境变量配置，也往往是容易忽略的：

终端下命令：

```bash
vim  ~/.bashrc
```

或者：

```bash
vim ~/profile
```

注：bashrc对系统所有用户有效，profile对当前用户有效。

有三个变量GOPATH、PATH、GOROOT：·GOROOT就是go的安装路径；·GOPATH就是go的工作目录；·PATH是go安装路径下的bin目录。

输入完命令后按insert进行输入，将以下内容粘贴到里面（随便空白处即可，建议最开始或结尾）：

```bash
export GOROOT="/usr/local/go"
export GOPATH="/go"
export PATH=$PATH:/usr/local/go/bin
```

输入完后，按ESC退出，输入:wq，进行保存。

保存完成后，还有一步操作，就是让更改的环境变量进行生效，在终端中输入以下命令：

```bash
source  ~/.bashrc
```

或者：

```bash
source ~/profile
```

此刻，golang安装配置完成，可通过以下命令检测：

```bash
go version
```

![在深度Deepin系统上安装Go的方法，附Go环境变量配置](https://www.linux110.com/picture/19/1-1ZRQ51513150.JPG)

输入go version后能看到版本号就表示安装成功了，通过返回的数据显示：以上安装的是go1.12.9 linux/amd64版本。

转载于：https://www.linux110.com/jishu/90.html