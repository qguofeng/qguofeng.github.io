---
layout: post
title: "Django系列4——views详解"
date: 2017-02-26  15:00:00
categories: ['Python','Web','Django']
---
```bash
友情提示:
该文章由作者QGUOFENG(http://qiuguofeng.com/)所写
欢迎指出错误一起学习
欢迎大家转载，借鉴，如果标明出处不胜感激
```
view.py主要的内容是视图函数(view function)他与URL一起配合使用。<br />
用户通过浏览器访问地址来访问页面，而地址的相关信息在URL.py中配置好了，url配置将用户的请求url对应的view function 匹配，调用view function进行数据处理，然后选择对应的templates目录中的模板进行渲染展示，或将数据直接返回
<br />
例如：实现在url地址为a,b中分别显示testa，显示b.html模板中的数据b
1、配置url地址a,b
```bash
(test1) QGUOFENG 16:29 My_first_blog\$ vi blog/urls.py
-----------------------------------------------------
from django.conf.urls import url
from django.contrib import admin
from blog import views

urlpatterns = [
    url(r'^test/', views.test,name="test"),
    url(r'^a/', views.a,name="a"),		#a
    url(r'^b/', views.b,name="b"),		#b
]

-----------------------------------------------------
```
2、配置views.py
```bash
(test1) QGUOFENG 16:32 My_first_blog\$ vi blog/views.py 
-----------------------------------------------------
from django.shortcuts import render
from django.http import HttpResponse	#引入django.http中的HttpResponse

# Create your views here.
#a
def a(request):
    return HttpResponse("<h1>testa</h1>")

#b
def b(request):
    return render(request,'b.html')

-----------------------------------------------------
```
3、测试模板b.html
```bash
(test1) QGUOFENG 16:43 My_first_blog\$ echo "b" > blog/templates/b.html 
```
4、启动本地浏览测试
```bash
python manage.py runserver
Performing system checks...

System check identified no issues (0 silenced).
February 26, 2017 - 08:39:03
Django version 1.10.5, using settings 'My_first_blog.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.

```
![a](/assets/active_images/Django/Django4/a.png)
![b](/assets/active_images/Django/Django4/b.png)

本质上views就是处理http（响应与请求)
同时view function 还可处理数据库中的数据将其渲染到templates目录中的html文件中
