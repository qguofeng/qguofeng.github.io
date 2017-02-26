---
layout: post
title: "Django系列5——models详解"
date: 2017-02-26  17:00:00
categories: ['Python','Web','Django']
---
```bash
友情提示:
该文章由作者QGUOFENG(http://qiuguofeng.com/)所写
欢迎指出错误一起学习
欢迎大家转载，借鉴，如果标明出处不胜感激
```
models是Django中用来设计数据库的一个简便的功能。
models模型类用来设计数据库，降低了开发者在数据库方面的知识储备，开发者只要理清各个数据之间关系
以及各数据需要用什么类型来定义，极大的放低了开发者的门槛<br />

每个数据类型都是django.db.models.Model的子类，它的父类Model包含了所有必要的和数据库交互的方法，并提供了一个简洁漂亮的定义数据库的语法<br />
每个模型就相当于单个数据库表,每个属性也是表中的一个字段。属性名就是字段名，它的类型相当于数据库的字段类型。但这条规则一个例外的情况就是多对多的关系，多对多关系时会多生出一张表
<br />
我们来了解一下模型常用字段类型
```bash
	BooleanField:布尔类型字段
	CharField:字符类型字段
	DateField:日期字段
	DateTimeField:日期时间字段
   	DecimalField:(精确)小数字段
	EmailField:Email字段
	FileField:文件字段(保存和处理上传的文件)
	FloatField:（浮点数)小数字段
	ImageField:图片字段（保存和处理上传的图片
	IntegerField:整数字段
	IPAddressField:IP字段
	SmallIntegerField:小整数字段
	TextField:文本字段
	URLField:网页地址字段
	...................
```
模型常用的字段选项
```bash
	null(null=True|False)
		数据库的字段的设置是否可以为空(数据库进行验证）
	blank(blank=True|False)
		字段是否为空django会进行校验（表单进行验证）
	choices轻量级的配置字段可选属性的定义
	default字段的默认值
	help_text字段文字帮助
	primary_key(=True|False)一般情况不需要进行定义
	是否主键如果没有显示指明主键的话，django会自动增加一个默认主键
	id = models.AutoField(primary_key=True)
	unique
	是否唯一，对于数据表而言
	verbose_name字段的详细名称，如果不指定该属性，默认使用字段的属性名称
```
字段类型和字段选项设置的详细信息
<a href="https://docs.djangoproject.com/en/1.9/ref/models/fields/">https://docs.djangoproject.com/en/1.9/ref/models/fields/</a>


模型扩展属性的定义
```bash
通过内部类Meta给数据模型增加扩展性
class Meta:
    verbose_name = "名称"		#为该类定义中文名称
    verbose_name_plural = "名称复数形式"#在英文中每个类都由复数形式，默认在名称后+s
    ordering=['排序字段']		#定义依据那个字段来排序
```
更多扩展属性：<a href="https://docs.djangoproject.com/en/1.9/ref/models/options/">https://docs.djangoproject.com/en/1.9/ref/models/options/</a>

模型方法的定义
```bash
定义模型方法和普通python类方法没有太大差别，定义模型方法可以将当前对应的数据组装成具体的业务逻辑
例如：
def __str__(self):
    return self.name
```
