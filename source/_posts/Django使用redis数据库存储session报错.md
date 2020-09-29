---
layout: source/_posts
title: Django使用redis数据库存储session报错
date: 2020-09-30 00:09:19
tags: [Django,redis]
---

转载自（https://blog.csdn.net/weixin_44520259/article/details/93388802）

我想用redis来存储session，于是我在settings.py文件里做了如下设置：

```bash
#将session的存储位置设为redis数据库
SESSION_ENGINE='redis_sessions.session'
#设置服务器ip
SESSION_REDIS_HOST='localhost'
#填写redis端口号
SESSION_REDIS_PORT=6379
#选择redis里的1号数据库存储session(redis里有16个数据库，编号从0开始)
SESSION_REDIS_DB=1
#填写我的redis密码
SESSION_REDIS_PASSWORD='mypassword'
#给存入redis里的session加上前缀"session"，便于查找session
SESSION_REDIS_PREFIX='session'
```

但是当我在浏览器中输入网址，并执行到存储session那一步时，报错了。报错内容如下：（注：此处我的报错和博主不一样，仅供参考）

```
redis.exceptions.TimeoutError: Timeout connecting to server
```

![img](https://img2020.cnblogs.com/blog/1622662/202004/1622662-20200408135617134-158605637.png)

 

### 解决办法：参考博主的解决办法（源博客：https://blog.csdn.net/weixin_44520259/article/details/93388802）

```python
#设置session的存储位置为redis
SESSION_ENGINE = 'redis_sessions.session'
#下面是一个字典
SESSION_REDIS = {
    #设置redis服务器ip
    'host': 'localhost',
    #设置redis端口
    'port': 6379,
    #指定我想要的redis数据库编号
    'db': 1,
     #填写我的redis密码
    'password': 'mypassword',
     #指定我想要的前缀
    'prefix': 'session',
     #设置超时时间
    'socket_timeout': 1
}
```

我已成功解决超时错误，成功在1号数据库存入session,再次感谢博主经验分享~

 