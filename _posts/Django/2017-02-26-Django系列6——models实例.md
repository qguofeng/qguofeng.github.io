---
layout: post
title: "Django系列6——models实例"
date: 2017-02-26  17:00:00
categories: ['Python','Web','Django']
---
```bash
友情提示:
该文章由作者QGUOFENG(http://qiuguofeng.com/)所写
欢迎指出错误一起学习
欢迎大家转载，借鉴，如果标明出处不胜感激
```

我们来模拟一个场景：作者出版书籍！<br />
从字面上我们可以得出信息：该数据模型有，作者，出版社，书籍这三个基础的数据模型：<br />
然后我们再来细分<br />
```bash
作者模型可以拆分为两个模型：作者基础信息，作者详细信息
作者基础信息：作者姓名
作者详细信息：性别，email,地址，出生日期
```
出版社模型
```bash
出版社模型：出版商名称，地址，城市，国家
```
书籍模型
```bash
书籍模型：书籍名称,出版社，作者，出版日期
```
所以共有4个数据模型：作者基础信息，作者详细信息，出版社，书籍<br />
这四个模型的关系为：
```bash
作者基础信息与作者详细信息为一对一关系(一个作者只有一个详细信息)
作者详细信息与书籍为多对多关系(一个作者可以写多本书，一本书可以由多个作者写)
书籍和出版商是一对多的关系（一本书只有一个出版商印刷，一个出版商可以印刷多本书）
```
将数据模型以代码的形式写入models.py
```bash
(test1) QGUOFENG 18:11 My_first_blog\$ vi blog/models.py 
--------------------------------------------------------------------
from django.db import models

# Create your models here.
#作者基础信息
class Author(models.Model):
    name = models.CharField("姓名",max_length=50)

#作者详细信息
class AuthorDetail(models.Model):
    sex = models.BooleanField('性别',max_length=1,choices=((0,'男'),(1,'女'),))
    email = models.EmailField()
    address = models.CharField("住址",max_length=50)
    birthday = models.DateField("生日")
    author = models.OneToOneField(Author)

#出版商信息
class Publisher(models.Model):
    name = models.CharField("出版商名称",max_length=30)
    address = models.CharField("地址",max_length=50)
    city = models.CharField("城市",max_length=60)
    country = models.CharField("国家",max_length=50)
    class Meta:
        verbose_name = '出版商'
        verbose_name_plural = verbose_name

#书籍信息
class Book(models.Model):
    title = models.CharField(max_length=100)
    authors = models.ManyToManyField(Author)
    publisher = models.ForeignKey(Publisher)
    publication_date = models.DateField()

--------------------------------------------------------------------
```
生成同步数据库脚本
```bash
(test1) QGUOFENG 18:44 My_first_blog\$ python manage.py makemigrations
Migrations for 'blog':
  blog/migrations/0001_initial.py:
    - Create model Author
    - Create model AuthorDetail
    - Create model Book
    - Create model Publisher
    - Add field publisher to book
```
同步数据
```bash
(test1) QGUOFENG 18:45 My_first_blog\$ python manage.py migrate
Operations to perform:
  Apply all migrations: admin, auth, blog, contenttypes, sessions
Running migrations:
  Applying blog.0001_initial... OK
```
