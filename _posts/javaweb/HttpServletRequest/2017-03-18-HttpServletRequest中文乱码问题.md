---
layout: post
title: "HttpServletRequest中文乱码问题"
date: 2017-03-18  13:00:00
description: "javaee学习笔记"
tag: HTTPServletRequest 
---
### 中文乱码问题
* post请求乱码
* get请求乱码
### post请求乱码解决
* 设置request缓冲区的编码  request.setCharacterEncoding("utf-8");

### get请求乱码
* 方法一:修改tomcat中的配置文件server.xml<br />
&nbsp;&nbsp;&lt;Connector port="80" protocol="HTTP/1.1"  connectionTimeout="20000"  redirectPort="8443" URIEncoding="utf-8"/&gt;
* 方法二:逆向编解码<br />
&nbsp;&nbsp;URLEncoder.encode(编码对象,"编码格式");<br />
&nbsp;&nbsp;URLDecoder.decode(解码对象，"解码格式");<br />
* 方法三:<br />
&nbsp;&nbsp;new String("对象".getBytes("编码格式"),"编码格式");<br />
转载请注明：[邱国锋的博客](http://qiuguofeng.com) » [点击阅读原文](http://qiuguofeng.com/2017/03/HttpServletRequest中文乱码问题/)
