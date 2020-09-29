---
layout: source/_posts
title: pip下载速度慢解决办法
date: 2020-09-29 23:19:45
tags: pip
---

## pypi 镜像使用帮助

pypi 镜像每 5 分钟同步一次。

### 临时使用

```bash
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple some-package
```

注意，`simple` 不能少, 是 `https` 而不是 `http`

### 设为默认

升级 pip 到最新的版本 (>=10.0.0) 后进行配置：

```bash
pip install pip -U
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```

如果您到 pip 默认源的网络连接较差，临时使用本镜像站来升级 pip：

```bash
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple pip -U
参考：https://mirrors.tuna.tsinghua.edu.cn/help/pypi/
```