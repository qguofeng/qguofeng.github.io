---
layout: post
title:  "Django系列3——urls详解"
crawlertitle: "Django系列3——urls详解"
summary: "Django系列3——urls详解"
date:   2017-02-25 12:33:47 +0700
categories: posts
tags: ['Python','Web','Django','全部文章']
author: QGUOFENG
---
```bash
友情提示:
该文章由作者QGUOFENG(http://qiuguofeng.com/)所写,欢迎指出错误一起学习
欢迎大家转载，借鉴，如果标明出处不胜感激
```

一、urls.py:URL分发器(路由配置文件)
```bash
URL配置就像是Django所支撑网站的目录，它的本质是URL模式以及要为URL模式调用的视图函数之间的映射表。
你就是，以这种方式告诉Django，对于这个URL调用这段代码，对于那个URL要调用那段代码，
URL的加载是从配置文件开始
```
urlpatterns由于Django版本的不同，有两种配置方式<br />

方式一：最新版本配置(1.10)(没有前缀的情况---推荐使用该方式进行配置)
```bash
-----------------------------------------------------------
from django.conf.urls import url
from django.contrib import admin
from blog import views

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^test/', views.test,name="test"),
]
-------------------------------------------------------------
```
2、方式二：旧版本配置(有前缀的情况)(不推荐使用该方式进行配置）
```bash
------------------------------------------------------------
from django.conf.urls import url,patterns
from hello import views

urlpatterns = patterns('',
    (r'^test/$', views.test)
)
------------------------------------------------------------
或者
------------------------------------------------------------
from django.conf.urls import patterns
urlpatterns = patterns('hello',
    (r'^test/$', 'views.hello'),
)
------------------------------------------------------------
```

二、URL的基本配置
```bash
urlpatterns = [
    url(正则表达式，view函数，参数，别名，前缀）
]
参数说明：
*一个正则表达式字符串
*一个可调用对象，通常为一个视图函数或一个指定视图函数的字符串
*可选的要传递给视图函数的默认参数（字典形式）
*一个可选的name参数
*路径前缀
```

三、URL分解器(include函数)
```bash
通常一个URL分解器对应一个URL配置模块，它可以包含多个URL模块，也可以包括多个其他URL分解器
通过这种包含结构设计，实现Django对URL的层级解析
```
例如一个app相当于一个站点，多个app就是多个站点，如果所有的URL都由django项目中的url.py文件来配置将导致url中的代码臃肿
并且在如后的管理也很麻烦。URL分解器就是在各个app站点中建立一个url.py文件，在各自app中管理各自的url.py文件，然后只需在
项目的url.py中配置一条关联信息，就可以将各个站点的url配置分离开来。<br />
拿上一篇文章中搭建的基础网页做测试
```bash
#项目urls.py配置

(test1) QGUOFENG 18:38 My_first_blog\$ vi My_first_blog/urls.py 
----------------------------------------------------------------------------
from django.conf.urls import url,include	#记得导入include 
from django.contrib import admin
from blog import views

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^blog/', include('blog.urls')),	#增加一条路由指向blog中的url
]
----------------------------------------------------------------------------

#app blog中urls.py的配置（通过直接复制项目的urls.py来修改)
(test1) QGUOFENG 18:39 My_first_blog\$ sudo cp -a My_first_blog/urls.py blog/urls.py
(test1) QGUOFENG 18:44 My_first_blog\$ vi blog/urls.py 
----------------------------------------------------------------------------
from django.conf.urls import url
from django.contrib import admin
from blog import views

urlpatterns = [
    url(r'^test/', views.test,name="test"),
]
----------------------------------------------------------------------------

```
测试是否成功
```bash
(test1) QGUOFENG 18:40 My_first_blog\$ python manage.py runserver 8080
Performing system checks...

System check identified no issues (0 silenced).
February 25, 2017 - 10:40:56
Django version 1.10.5, using settings 'My_first_blog.settings'
Starting development server at http://127.0.0.1:8080/
Quit the server with CONTROL-C.

```
![test](/assets/active_images/Django/Django3/1.png)


