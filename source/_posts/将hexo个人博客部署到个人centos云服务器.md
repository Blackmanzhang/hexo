---
layout: source/_posts
title: 将hexo个人博客部署到个人centos云服务器
date: 2020-09-29 21:18:49
tags: hexo
---

**声明：此文章仅为转载部署Hexo至个人云服务器。**

原文章地址：https://zhuanlan.zhihu.com/p/120743882

服务器的购买与连接之类的部分暂不赘述，直接进入正题部分：

## 一.在服务器建立 Git 仓库

以下操作建立在 root 权限之下，如权限不足请申请 root 权限或采用 `sudo` 关键字。

下文用到的 `ganahBlog.git` 为我自己的仓库名，可自行设定换成你喜欢的文字； root 为我的远端主机用户名，在后续需要用得到，如果不为 root ，在进行到文章相应位置自行修改。

首先安装 Git 和 Nginx 服务，如有请跳过：

```bash
yum update
yum install git nginx -y
```

建立文件路径：

```
mkdir /var/repo/
```

![img](https://pic4.zhimg.com/80/v2-14f7dfb5cc00e1b1e5055c317b2da483_720w.jpg)

修改权限：

```bash
chown -R $USER:$USER /var/repo/
chmod -R 755 /var/repo/
```

创建远程 Git 仓库：

```
cd /var/repo
git init --bare {自定义仓库名name}.git
```

![img](https://picb.zhimg.com/80/v2-a6a72c4f6d4292de044f0c5764b4f6e0_720w.jpg)

## 二.配置 Nginx 托管文件目录

创建目录并修改目录所有权和权限：

```bash
mkdir -p /var/www/hexo

chown -R $USER:$USER /var/www/hexo
chmod -R 755 /var/www/hexo
```

修改 Nginx 的 `default` 文件使得 root 指向刚刚创建的 `/var/www/hexo`目录：

```bash
vim /etc/nginx/sites-available/default
```

注意 vim 编辑方式：按照以上命令进入后为普通模式，具体介绍可以去找网上的内容，此处只说明如何进行下来的操作：

执行命令后编辑方式如下（下文有类似操作不再提及）：

- 点击 i 进入编辑模式；
- 使用键盘上下键查到下图的 `server` 字段 中的 `root`；
- 修改 `root` 的指向；
- 按住 `Esc`退出编辑模式；
- 输入 `:wq` 保存并退出 vim 编辑器;
- 如果有域名可以将server_name 字段后写入自己的域名。

 

![img](https://pic3.zhimg.com/80/v2-6cc2a8852f38a09ad418dbc5dbaf6083_720w.jpg)

 

- 最后重启 nginx 服务:

```bash
service nginx restart
```

此时已经搭建好自己的nginx服务器，输入自己的服务器公网IP地址即可访问如下界面：

 

![img](https://pic4.zhimg.com/80/v2-e0d93d7688813a6513a6dd1e81deb3ff_720w.jpg)

 

由于服务器暂未搭建任何东西，故访问出现403 ForBidden，如果没有成功则会出现 404 Not Found 或是加载失败等提示。

 

![img](https://pic4.zhimg.com/80/v2-ed3d33fda7ce4f9acda04041753643dc_720w.jpg)

 

当然，为了确认是否真的真的搭建成功，也可以在/var/www/hexo 目录下建立index.html 文件：

执行命令：

```bash
vim index.html
```

写入如下代码：

```html
<html>

<body>
<p>This is my Blog.</p>
</body>

</html>
```

 

最后输入公网IP发现不再是 403 ForBidden，而是访问了 index.html ：

 

![img](https://pic3.zhimg.com/80/v2-8e7fc49ea14629fd912f93956f4cd4b2_720w.jpg)

 

## 三.Git 钩子（hooks)

执行下面的命令，在自动生成的`ganahBlog.git/hooks` 目录下创建一个新的钩子文件：

```bash
vim /var/repo/ganahBlog.git/hooks/post-receive
```

打开文件后，加入下面的代码：

```bash
#!/bin/bash

git --work-tree=/var/www/hexo --git-dir=/var/repo/ganahBlog.git checkout -f
```

 

![img](https://pic1.zhimg.com/80/v2-730afb22caa39dd8cc840f6c0218a2dc_720w.jpg)

 

将文件保存（方法参加上文）后，赋予该文件可执行权限：

```bash
chmod +x /var/repo/ganahBlog.git/hooks/post-receive
```

## 四. 使用 Git 部署本地 Hexo 到远端服务器

将服务器地址添加到受信任的站点，在本地任意目录从服务器上把`hexo_static`仓库克隆下来：

```bash
git clone root@{云服务器IP}:/var/repo/ganahBlog.git
```

- 注意：如果你在远端服务器创建了 Git 用户并设定为拥有者，请将 root 改成 git （git用户）。

编辑站点配置文件`_config.yml` , 将 url 改成`https://{云服务器IP}/`

 

![img](https://picb.zhimg.com/80/v2-91f5600ca630707517ed007ee8e205ac_720w.jpg)

 

将 deploy 目标改为 `{服务器用户名}@{服务IP}:/var/repo/ganahBlog.git`：

 

![img](https://pic2.zhimg.com/80/v2-82fe5b073a41cc15a722fa9d2ca54b33_720w.jpg)

 

在个人博客站点目录下，打开 Git bash ,使用 `hexo clean && hexo g -d` 部署：

 

![img](https://pic3.zhimg.com/80/v2-457b6029aae9767a139597d02621993f_720w.jpg)

 

使用IP访问：

 

![img](https://picb.zhimg.com/80/v2-dee26978af8cfb4a9c4aa19497a52912_720w.jpg)

 

使用自己的域名访问：

 

![img](https://picb.zhimg.com/80/v2-c69f4a8e95431c29963906b1e2f5f2cf_720w.jpg)

 

效果参考本人个人博客地址：

[https://www.ganahe.top](https://link.zhihu.com/?target=https%3A//www.ganahe.top)

## 五. 错误集锦与解决措施参考

## （一）部署后访问无效

使用 `https://{服务器IP}:80` 来访问自己刚刚部署的服务器站点，没有出现博客：

 

![img](https://pic2.zhimg.com/80/v2-f474d50d05b22e14a768fbe925928e2f_720w.jpg)

 

- 请检查以上操作是否有误或命令部分写错。
- 查看服务器的错误日志文件：/var/log/nginx/error.log

 

![img](https://pic4.zhimg.com/80/v2-bb2c13ca18e9bfcd3a92957d2595f2e7_720w.jpg)

 

此时可以发现是 /var/www/hexo 没有 index.html 文件，该文件存放在本地博客站点 public文件夹下

- 发现错误信息（不同的情况错误可能不同，本文仅代表个人遇到的情况，具体情况请具体分析，可以留言，或许我可以帮得上忙，或是自行搜索相关的文章。

## （二）部署到远端服务器被拒绝

 

![img](https://pic4.zhimg.com/80/v2-28827d962ff862771befa460f749dd08_720w.jpg)

 刚配置好的 git 仓库服务器，首次提交的时候会报上图的错误，git 默认拒绝了 push 操作，需要进行设置。

1. 检查远端主机的 ~/.ssh 下文件的文件，公钥是否正确配置；
2. 定位到仓库 ganahBlog.git 下，在 config 文件添加如下代码（可以使用：vim /var/repo/ganahBlog.git/config 进入文件）：

```
js [receive] denyCurrentBranch = ignore
```

 

![img](https://pic3.zhimg.com/80/v2-49d3d695159b604df072b1e362dbcb3a_720w.jpg)

 

最后在该路径下执行如下代码：

```bash
git config receive.denyCurrentBranch ignore
```

成功提交：

 

![img](https://pic4.zhimg.com/80/v2-0cbab17e540d324c1c0433759d6a0f67_720w.jpg)

 

## （三）拉取失败处理办法

 

![img](https://pic2.zhimg.com/80/v2-e7611fa4faf316902d3d3250922f4052_720w.jpg)

 

缺少公钥，定位到服务器的 ~/.ssh/下，将自己的公钥粘贴到authorized_keys上去，如果没有公钥，自行查找 Git 公钥生成办法，本文不针对该内容；文件不存在则新建即可：

 

![img](https://pic3.zhimg.com/80/v2-0bee9aa969dc674f6167f89a74b241a5_720w.jpg)

 

加入方法与上面编辑 default 文件一样，XShell提供了外部粘贴的办法：

 

![img](https://pic1.zhimg.com/80/v2-e88df2c4e1b6061e81543c0f54492275_720w.jpg)

 

也可以使用下面的指令实现：

```bash
ssh [远程主机用户名]@[远程主机IP] 'mkdir -p .ssh && cat >>.ssh/authorized_keys' < ~/.ssh/authorized_keys.pub
```

结果测试：

 

![img](https://picb.zhimg.com/80/v2-4b92939534c9fb441614e1e05dbfa8bf_720w.jpg)

云仓库结构：

 

![img](https://pic3.zhimg.com/80/v2-3319b92f88e597863ea01f04421cd499_720w.jpg)

 

## （四）无法查看 push 后的 git 中文件的原因与解决方法

在初始化远程仓库时使用 git --bare init 而不要使用：git init （不建议）；

可以使用命令 git reset --hard 看到 push 后的内容。

两个初始化命令是有区别的，在此不做赘述。

## （五）更新远端公钥后连接不上出现下面的问题

 

![img](https://pic1.zhimg.com/80/v2-5432a568eaa720078ddc12c63902307d_720w.jpg)

 

```bash
ECDSA host key for 125.36.25.875 has changed and you have requested strict checking.
Host key verification failed.
```

错误信息说明远端主机公钥信息已修改，需要更新：

```bash
ssh-keygen -R "远程服务器ip"
```

 

![img](https://picb.zhimg.com/80/v2-0f861efe7643ba38418de653c16a9839_720w.jpg)

 

此时测试连接远端主机成功：

 

![img](https://pic4.zhimg.com/80/v2-05f49a9d2a7ebfaa81d9b531bdc30f0f_720w.jpg)

 

## （六）建立 git 用户

以上第五条参考即在 git 用户下进行登录验证等操作。

为了安全，可以通过 git 用户管理仓库，而不是 root。

```bash
adduser git # 按照指引输入密码等

su git # 切换到git用户 git用户家目录 /home
```

将密钥添加到 git 用户下：

```bash
mkdir /home/git/.ssh
vim /home/git/.ssh/authorized_keys
```

写入公钥即可。

记得更新（第四条），测试连接：

```bash
ssh -v git@{服务器IP}
```

## 六. TIP

想要域名访问服务器的网站，请记得要备案，不然......

 

![img](https://pic3.zhimg.com/80/v2-1bc1570a48033b7acc53232b0a26ff4c_720w.jpg)

 