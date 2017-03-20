---
layout: post
title: "HttpServletRequest"
date: 2017-03-18  13:00:00
description: "javaee学习笔记"
tag: HTTPServletRequest 
---
### 常用方法
* 获取客户机信息<br />
&nbsp;&nbsp;getRemoteAddr	获取IP地址<br />
&nbsp;&nbsp;getMethod		获取请求方式<br />
&nbsp;&nbsp;getContextPath	获取虚拟路径<br />
![no](/assets/active_images/javaweb/servlet/HttpServletRequest/1.png)
* 获取请求头信息<br />
&nbsp;&nbsp;String getHeader()<br />
&nbsp;&nbsp;long getDateHeader()<br />
&nbsp;&nbsp;int get<br />
&nbsp;&nbsp;请求头<br />
&nbsp;&nbsp;&nbsp;&nbsp;referer		记住当前网页的来源<br />
&nbsp;&nbsp;&nbsp;&nbsp;User_Agent	当前浏览器的版本<br />
&nbsp;&nbsp;&nbsp;&nbsp;if-modified-since控制缓存<br />
![no](/assets/active_images/javaweb/servlet/HttpServletRequest/2.png)
* 获取请求参数
&nbsp;&nbsp;String getParameter(String name)<br />
&nbsp;&nbsp;String[] getParameterValues(String name)<br />
&nbsp;&nbsp;Map getParameterMap()<br />
&nbsp;&nbsp;Enumeration getParameterNames()<br />
![no](/assets/active_images/javaweb/servlet/HttpServletRequest/3.png)

转载请注明：[邱国锋的博客](http://qiuguofeng.com) » [点击阅读原文](http://qiuguofeng.com/2017/03/HttpServletRequest/)
