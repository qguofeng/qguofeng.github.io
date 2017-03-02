---
layout: post
title: "Django系列7——models数据库同步及更换Mysql数据库"
date: 2017-02-26  17:00:00
categories: ['Python','Web','Django']
---
```bash
友情提示:
该文章由作者QGUOFENG(http://qiuguofeng.com/)所写
欢迎指出错误一起学习
欢迎大家转载，借鉴，如果标明出处不胜感激
```

Django默认支持的数据库为sqlit3<br />
更换支持的数据库为Mysql数据库
<br />
首先创建一个数据库test_db来存放数据,以及创建一个用户来管理数据库
```bash
(test1) QGUOFENG 18:57 My_first_blog\$ mysql -uroot -p
Enter password: 

mysql> CREATE DATABASE test_db CHARACTER SET utf8;
Query OK, 1 row affected (0.10 sec)

mysql> CREATE USER 'test'@'localhost' IDENTIFIED BY '123456';
Query OK, 0 rows affected (0.39 sec)

mysql> GRANT ALL ON test_db.* TO 'test'@'localhost';
Query OK, 0 rows affected (0.04 sec)
```
修改django中的配置信息settings.py
```bash
(test1) QGUOFENG 19:00 My_first_blog\$ vi My_first_blog/settings.py
------------------------------------------------------------------
DATABASES = {
    'default': {
        #'ENGINE': 'django.db.backends.sqlite3',
        #'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
        'ENGINE':'django.db.backends.mysql',
        'NAME':'test_db',
        'USER':'test',
        'PASSWORD':'123456',
        'HOST':'127.0.0.1'
        #'PORT':
    }
}
------------------------------------------------------------------
```
修改django中__init__.py导入mysql支持
```bash
(test1) QGUOFENG 19:05 My_first_blog\$ vi My_first_blog/__init__.py
------------------------------------------------------------------
import pymysql
pymysql.install_as_MySQLdb()
------------------------------------------------------------------
```
重新同步数据库(无报错即完成)
```bash
(test1) QGUOFENG 19:08 My_first_blog\$ python manage.py migrate
Operations to perform:
  Apply all migrations: admin, auth, blog, contenttypes, sessions
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying blog.0001_initial... OK
  Applying sessions.0001_initial... OK
```
设置完成后我们来谈谈前面有提到过的一个目录migrations。<br />
该目录中存放的是当你运行python manage.py makemigrations时所生成的数据库同步脚本<br />
在app目录下，migrations目录和__init__.py是完成数据同步所必需的文件<br />

数据的操作至关重要，在不熟悉的情况下不要随意更改migrations中生成数据库同步脚本，以免造成不可挽回的错误，下面提供了几个常用的数据库操作命令：
```bash
flush           清空数据库、回复数据库到最初状态
makemigrations  生成数据库同步的脚本
migrate         同步数据库(*)
showmigrations  查看生成的数据库同步脚本(*)
sqlflush        查看生成清空数据库的脚本(*)
sqlmigrate      查看数据库同步的sql语句(*)
```

在开发的过程中，难免会出现不能同步数据库的情况，通常有两种解决办法
```bash
1、分析生成的数据库脚本，找出错误并解决
2、直接清空migrations目录中出__init__.py的所有数据重新生成数据库同步脚本
```
