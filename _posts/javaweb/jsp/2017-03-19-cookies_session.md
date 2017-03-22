---
layout: post
title: "cookie_session"
date: 2017-03-19  13:00:00
description: "javaee学习笔记"
tag: JSP 
---
### 什么是会话
会话可以简单理解为:用户打开一个浏览器，点击多个超链接，访问服务器的多个web资源，然后关闭浏览器，整个过程称为一个会话<br >
### 会话过程中需要解决的问题
* 如何分别保存每个用户各自的数据

### 解决方式
保存会话数据的两种技术<br />
&nbsp;&nbsp;Cookie<br />
&nbsp;&nbsp;Session<br />
* ****
Cookie是客户端技术，程序把每个用户的数据以cookie的形式写给用户各自的浏览器，当用户使用浏览器再去访问服务器中的web资源时，就会带着各自的数据去。这样，web资源处理的就是用户各自的数据<br />
* ****
Session是服务器端技术，利用这个技术，服务器在运行时可以为每个用户的浏览器创建一个其独享的session对象，由于session为用户浏览器独享，所以用户在访问服务器的web资源时，可以把各自的数据放到各自的session中，当用户再去访问服务器中的其他web资源时，其他web资源在从用户各自的session中取出数据为用户服务<br />
### Cookie
* 使用cookie显示上次用户的访问时间
![no](/assets/active_images/javaweb/jsp/4.png)
![no](/assets/active_images/javaweb/jsp/5.png)
![no](/assets/active_images/javaweb/jsp/6.png)
![no](/assets/active_images/javaweb/jsp/7.png)
* 使用cookie实现浏览记录在前端页面的显示和清除浏览记录操作
* jsp文件
![no](/assets/active_images/javaweb/jsp/8.png)<br />

* 在前端页面显示浏览记录

![no](/assets/active_images/javaweb/jsp/9.png)
![no](/assets/active_images/javaweb/jsp/10.png)<br />

* 清除浏览记录
![no](/assets/active_images/javaweb/jsp/11.png)

* 结果
![no](/assets/active_images/javaweb/jsp/12.png)

* cookie常用的API<br />
&nbsp;&nbsp;cookie的构造方法	cookie(String name,String value)<br />
&nbsp;&nbsp;String getName()	获取cookie的名称<br />
&nbsp;&nbsp;String getValue()	获取cookie的值<br />
&nbsp;&nbsp;void setMaxAge(int expiry)	设置有效时间<br />
&nbsp;&nbsp;&nbsp;&nbsp;使cookie失效	setMaxAge(0):前提条件是设置有效路径<br />
&nbsp;&nbsp;void setPath(String url)	设置有效路径<br />
&nbsp;&nbsp;void setDomain(String pattern)	设置有效域名<br />

* cookie其他相关知识<br />
一个Cookie只能标识一种信息，它至少含有一个标识该信息的名称(NAME)和设置值(VALUE)<br />
一web站点可以给一个web浏览器发送多个cookie，一个web浏览器也可以存储多个web站点提供的cookie<br />
浏览器存放的cookie个数和大小都有限制<br />
如果创建了一个cookie，并将它发送到浏览器，默认情况下它是一个会话级别的cookie(存储在浏览器内存中),用户退出浏览器之后即被删除。若希望浏览器将该cookie存储在磁盘上，需要使用maxAge并给出一个以秒为单位的时间。<br />
删除持久cookie，可以将cookie最大时效设为0(删除cookie时，path必须一致，否则不会删除)<br />

### Session
* 使用session实现简单的购物车
购物页面jsp<br />
![no](/assets/active_images/javaweb/jsp/13.png)
购物后台实现<br />
![no](/assets/active_images/javaweb/jsp/14.png)
![no](/assets/active_images/javaweb/jsp/15.png)
结算页面<br />
![no](/assets/active_images/javaweb/jsp/16.png)

* 使用session实现登录验证码的校验
loginJSP文件<br />
![no](/assets/active_images/javaweb/jsp/17.png)
![no](/assets/active_images/javaweb/jsp/18.png)
验证码增加两行<br />
&nbsp;&nbsp;StringBuffer sb = new StringBuffer();//用来存放随机的字符<br />
&nbsp;&nbsp;sb.append(ch);// 把ch装起来，存入到session中<br />
&nbsp;&nbsp;request.getSession().setAttribute("code", sb.toString());// 存入session中<br />
login后台实现验证码参与验证
![no](/assets/active_images/javaweb/jsp/19.png)
实验结果
![no](/assets/active_images/javaweb/jsp/20.png)
* session<br />
在web开发中，默认情况，服务器为每个用户浏览器创建一会话对象(session对象），一个浏览器独占一个session对象

### session与cookie的主要区别
* Cookie是客户端技术，cookie把用户的数据写给用户的浏览器<br />
* Session是服务器端技术，把用户的数据写到用户独占的session中
<br />
转载请注明：[邱国锋的博客](http://qiuguofeng.com) » [点击阅读原文](http://qiuguofeng.com/2017/03/cookie_session/)

