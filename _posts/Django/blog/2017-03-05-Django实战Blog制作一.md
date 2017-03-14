---
layout: post
title: "Django实战Blog制作一"
date: 2017-03-05  13:00:00
description: "python学习笔记"
tag: Django 
---
### 个人博客需求分析
* **文章模型**
* 文章标题 
* 文章描述
* 文章发布日期
* 文章内容
* 点击次数
* 分类
* 标签

* **标签模型**
* 标签名称

* **分类模型**
* 分类名称
* 分类排序

* **评论**
* 由于自己搭建的个人博客的局限性，常见一个评论模型让别人来创建不切实际，所以采用网络上的评论模型，例如多说等

* **网站HTML+CSS+JS模板**

* **后台管理**

### 模型实现
* **models.py**
* tag
![图片](/assets/active_images/Django/blog/1/tag.png)

* 分类
![图片](/assets/active_images/Django/blog/1/category.png)

* 文章模型
![图片](/assets/active_images/Django/blog/1/active.png)

### 后台实现+富文本编辑+文件上传
* **设置后台管理员**
![图片](/assets/active_images/Django/blog/1/admin1.png)

* **admin.py**
![图片](/assets/active_images/Django/blog/1/admin2.png)

* **富文本编辑**
* 步骤一:引入富文本编辑器kindeditor
```bash
下载地址:http://kindeditor.net/down.php
```
![图片](/assets/active_images/Django/blog/1/fu1.png)

* 步骤二:在admin.py中写入代码(如上图中的1)

* 步骤三:在kindeditor中创建文件config.js写入代码
![图片](/assets/active_images/Django/blog/1/fu2.png)

* **文件上传**

* 在根目录下创建文件夹uploads
![图片](/assets/active_images/Django/blog/1/upload1.png)

* 在项目blog中创建py文件upload.py
![图片](/assets/active_images/Django/blog/1/upload2.png)

* upload.py文件内容
![图片](/assets/active_images/Django/blog/1/upload3_1.png)
![图片](/assets/active_images/Django/blog/1/upload3_2.png)

* 配置文件setting.py增加内容
![图片](/assets/active_images/Django/blog/1/upload5.png)

* 在url文件中配置信息(红色标注为配置的信息）
![图片](/assets/active_images/Django/blog/1/upload6.png)

### 日志文件配置
* **在setting.py中增加如下内容**
![图片](/assets/active_images/Django/blog/1/rz1.png)
![图片](/assets/active_images/Django/blog/1/rz2.png)
![图片](/assets/active_images/Django/blog/1/rz3.png)
![图片](/assets/active_images/Django/blog/1/rz5.png)
![图片](/assets/active_images/Django/blog/1/rz6.png)

转载请注明：[邱国锋的博客](http://qiuguofeng.com) » [点击阅读原文](http://qiuguofeng.com/2017/03/Django实战Blog制作一/)
