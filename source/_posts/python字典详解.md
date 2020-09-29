---
layout: source/_posts
title: python字典详解
date: 2020-09-30 01:19:20
tags: [python]
---

初学python的小伙伴对字典不是很了解，今天写这篇博客对其进行详细描述：

字典也是python的数据类型中的一种，它由许多键值对组成，它是一种**可变**容器模型，一般情况下键是**唯一**的,字典支持嵌套。字典不允许同一个键出现两次，创建时如果同一键被赋值两次，只会记录后一个值。键必须不可变，可以用数字，字符串或元组充当，但是不能用列表，列表是可变的

```python
1 dic = {
2     'a':1,
3     'b':2,
4 　　 # 这是一个简单的字典例子
5 }
```

字典键值对关系如下表所示：

| 名称      | 唯一性 | 可存储数据类型     | 可变性 |
| --------- | ------ | ------------------ | ------ |
| key(键)   | 唯一   | 数字、字符串、元组 | 不可变 |
| value(值) | 不唯一 | 任意               | 可变   |

通过指定key（键）值访问对应的value(值)：

```python
1 dic = {
2     'a':1,
3     'b':2,
4 
5 }
6 print(dic['b'])　　# 2
```

遍历字典（key & value）：

```python
1 dic = {
2     'a':1,
3     'b':2,
4     'c':3,
5 
6 }
7 for i in dic.items():
8     print(i)
#输出：
('a', 1)
('b', 2)
('c', 3)
```

遍历value:

```python
 1 dic = {
 2     'a':1,
 3     'b':2,
 4     'c':3,
 5 
 6 }
 7 for i in dic:
 8     print(i)
 9 # value输出如下：
11 a
12 b
13 c
```

遍历key和value:

```python
dic = {
　　'a':1,
　　'b':2,
　　'c':3,
}
print(dic.keys())　　  # 输出key
print(dic.values())　　# 输出value```
```

转化为list进行操作（输出key）：

```python
1 dic = {
2     'a':1,
3     'b':2,
4     'c':3,
5 
6 }
7 list_ = list(dic.keys())
8 for i in list_:
9     print(i)
```

添加元素到字典里：

```python
dic = {     
    'a':1,
    'b':2,
    'c':3,
}
dic['d'] = 4
print(dic)
```

更新：

```python
dic = {     
    'a':1,
    'b':2,
    'c':3,
}
dic['a'] = 5
print(dic)
```

删除：

```python
del dict['Name']   #删除字典里键为Name的键值对
dict.clear()         #清空字典
del dict               # 删除字典
```

查看长度：

```python
dic = {     
    'a':1,
    'b':2,
    'c':3,
}
print(len(dic))
```