---
layout: source/_posts
title: Ubuntu19.10版本pycharm2019.3中无法输入中文解决办法
date: 2020-09-30 00:32:21
tags: [Ubuntu,Pycharm]
---

此处转载一下解决办法，原文章地址：https://blog.csdn.net/huzing2524/article/details/104037325

之前我找了很多都是在pycharm.sh中设置输入法都行不通，不知是不是版本的问题，添加这个配置就能在pycharm中输入中文了

- 在pychamr菜单栏-Help-Edit Custom VM Options中添加如下配置

- ```bash
  -Dauto.disable.input.methods=false
  ```

![img](https://img2018.cnblogs.com/i-beta/1622662/202002/1622662-20200205144439578-284899614.png)



 我是用的是搜狗输入法

![img](https://img2018.cnblogs.com/i-beta/1622662/202002/1622662-20200205144601794-1295309978.png)

 

 据说Webstorm和IDEA也行的通，具体的我没有进行尝试，大家可以试试