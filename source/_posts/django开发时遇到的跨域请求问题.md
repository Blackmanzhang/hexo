---
layout: source/_posts
title: django开发时遇到的跨域请求问题
date: 2020-09-30 02:10:35
tags: [Django]
---

使用django进行web开发的时候会遇到一个问题，后端一切正常，但前端访问后端的时候会报错，错误如下：

![跨域请求报错](https://img2018.cnblogs.com/blog/1622662/201906/1622662-20190603094921088-1008274846.png)

遇到这种情况就是django的跨域问题。我们接下来对此进行解决：

1.使用pip命令安装django-cors-middleware

```bash
1 pip install django-cors-middleware
```

2.有的小伙伴使用pycharm进行开发，然后他在pip里对上述模块进行了安装，并且安装成功了，但他进入到pycharm继续开发的时候依然会报错，因为pip在不使用虚拟环境的时候，默认安装在python的安装路径下。我们需要对编译器路径进行调整。有两种方法可以进行调整。

　　1）切换项目编译器到python默认安装路径(我的是在c盘安装路径下：)

![img](https://img2018.cnblogs.com/blog/1622662/201906/1622662-20190603095934551-1731975835.png)

　　2）在虚拟环境下安装django-cors-middleware（使用pycham为例：）

　　　　此界面没有django-cors-middleware模块时使用右边的加号对其进行安装即可（相信大家都会这个...）

![img](https://img2018.cnblogs.com/blog/1622662/201906/1622662-20190603100151580-174944210.png)

 

![img](https://img2018.cnblogs.com/blog/1622662/201906/1622662-20190603100129620-137547115.png)

 　　　ps:至此安装模块结束

3.对项目的settings.py文件进行修改：

```python
 1 INSTALLED_APPS = [
 2     'django.contrib.admin',
 3     'django.contrib.auth',
 4     'django.contrib.contenttypes',
 5     'django.contrib.sessions',
 6     'django.contrib.messages',
 7     'django.contrib.staticfiles',
 8     'corsheaders',    # 添加这一行
 9     'test1',
10 ]
```

```python
 1 MIDDLEWARE = [
 2     'django.middleware.security.SecurityMiddleware',
 3     'django.contrib.sessions.middleware.SessionMiddleware',
 4     'django.middleware.common.CommonMiddleware',
 5     'django.middleware.csrf.CsrfViewMiddleware',
 6     'django.contrib.auth.middleware.AuthenticationMiddleware',
 7     'django.contrib.messages.middleware.MessageMiddleware',
 8     'django.middleware.clickjacking.XFrameOptionsMiddleware',
 9     'corsheaders.middleware.CorsMiddleware',    # 添加这行和下面一行
10     'django.middleware.common.CommonMiddleware',    
11 ]
```

```python
1 CORS_ORIGIN_ALLOW_ALL = True　　当这一行添加过后，所有的访问都将被允许
```

至此，跨域问题已解决