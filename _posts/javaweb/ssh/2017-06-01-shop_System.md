---
layout: post
title: "商城系统"
date: 2017-06-01  13:00:00
description: "javaee学习笔记"
tag: SSH 
---
[TOC]

### 商城系统路程图

![ssh](/assets/active_images/javaweb/ssh/ssh.png)

### 需求分析

```
该商城系统整体采用MVC模式的Struts2框架，数据源利用SpringIOC注入，dao层使用Spring的HibernateTemplate实现。模型层严格安装javaBean规范要求，用struts2控制流程，JSP纯标签页显示，运用SpringIOC的注入对各层的解耦，提高程序的可扩展性，易于维护。
```



#### 前台:用户模块

#### 前台:分类模块

#### 前台:商品模块

#### 前台:购物模块

#### 前台:订单模块

#### 后台:用户模块

#### 后台:一级分类

#### 后台:二级分类

#### 后台:商品模块

#### 后台:订单模块

### 数据库分析设计（主要表的设计）

![ssh](/assets/active_images/javaweb/ssh/table.png)

#### 一级分类表

```
create table category(
	cid int primary key auto_increment,		//一级分类id
	cname varchar(20)						//一级分类名称
);
```

#### 二级分类表

````
CREATE TABLE `categorysecond` (
  `csid` int(11) NOT NULL AUTO_INCREMENT,	//二级分类id
  `csname` varchar(255) DEFAULT NULL,		//二级分类名称
  `cid` int(11) DEFAULT NULL,				//一级分类id（外键）
  PRIMARY KEY (`csid`),
  KEY `FK936FCAF21DB1FD15` (`cid`),			
  CONSTRAINT `FK936FCAF21DB1FD15` FOREIGN KEY (`cid`) REFERENCES `category` (`cid`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
````

#### 商品表

```
CREATE TABLE `product` (
  `pid` int(11) NOT NULL,					//商品id		
  `pname` varchar(255) DEFAULT NULL,		//商品名称
  `market_price` double DEFAULT NULL,		//商品市场价格			
  `shop_price` double DEFAULT NULL,			//商品商城价格
  `image` varchar(255) DEFAULT NULL,		//商品图片
  `num` int(11) DEFAULT NULL,				//商品库存
  `pdesc` varchar(255) DEFAULT NULL,		//商品详细描述
  `is_hot` int(11) DEFAULT '0',				//热门商品标记
  `pdate` date DEFAULT NULL,				//商品修改日期
  `csid` int(11) DEFAULT NULL,				//所属二级分类id（外键）
  `is_recommend` int(11) DEFAULT '0',		//推荐商品标记
  `up_down` int(11) DEFAULT '1',			//商品是否上架
  PRIMARY KEY (`pid`),
  KEY `FKED8DCCEFB9B74E02` (`csid`),		
  CONSTRAINT `FKED8DCCEFB9B74E02` FOREIGN KEY (`csid`) REFERENCES `categorysecond` (`csid`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

#### 用户表

```
create table user(
	uid int primary key auto_increment,		//用户id
	username varchar(20) ,					//用户名称
	password varchar(20) ,					//用户密码
	name varchar(20),						//用户收件人名称
	email varchar(30),						//用户邮箱		
	phone varchar(20),						//用户收件人联系方式
	addr varchar(50),						//用户收货地址
	sex varchar(10),						//用户性别		
	state int,								//用户是否激活
	code varchar(64)						//用户激活码
);
```

#### 订单表

```
CREATE TABLE `orders` (
  `oid` int(11) NOT NULL AUTO_INCREMENT,	//订单id
  `total` double DEFAULT NULL,				//订单总计
  `ordertime` datetime DEFAULT NULL,		//订单项
  `state` int(11) DEFAULT NULL,				//订单状态：0位付款，1：未发货,2:未收货，3：完成
  `addr` varchar(50) DEFAULT NULL,			//收货地址
  `phone` varchar(20) DEFAULT NULL,			//收货联系方式
  `uid` int(11) DEFAULT NULL,				//收货人用户id（外键）
  PRIMARY KEY (`oid`),
  KEY `FKC3DF62E5AA3D9C7` (`uid`),
  CONSTRAINT `FKC3DF62E5AA3D9C7` FOREIGN KEY (`uid`) REFERENCES `user` (`uid`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

#### 订单项表

```
CREATE TABLE `orderitem` (
  `itemid` int(11) NOT NULL AUTO_INCREMENT,	//订单项id
  `count` int(11) DEFAULT NULL,				//订单项中商品数量小计
  `subtotal` double DEFAULT NULL,			//订单项价格小计
  `pid` int(11) DEFAULT NULL,				//商品id（外键）
  `oid` int(11) DEFAULT NULL,				//订单id（外键）
  PRIMARY KEY (`itemid`),
  KEY `FKE8B2AB6166C01961` (`oid`),
  KEY `FKE8B2AB6171DB7AE4` (`pid`),
  CONSTRAINT `FKE8B2AB6166C01961` FOREIGN KEY (`oid`) REFERENCES `orders` (`oid`),
  CONSTRAINT `FKE8B2AB6171DB7AE4` FOREIGN KEY (`pid`) REFERENCES `product` (`pid`)
) ENGINE=InnoDB CHARSET=utf8; 
```

### ssh环境搭建(如果用maven管理直接导入pom.xml文件)

```
导包：
	（如果使用maven管理，直接将所有包的信息写入pom.xml)
自己导包：
	struts2:
		 * struts2框架解压路径/apps/struts2-blank.war/WEB-INF/lib/*.jar		//基础包
		 * struts2框架解压路径/lib/struts2-spring-plugin-2.3.15.3.jar		//spring整合包
		 * struts2框架解压路径/lib/struts2-json-plugin-2.3.15.3.jar			//json插件包
	spring3:
		* spring开发基础包：
			spring框架解压路径/lib/
				spring-beans-3.2.0.RELEASE.jar
				spring-context-3.2.0.RELEASE.jar
				spring-core-3.2.0.RELEASE.jar
				spring-expression-3.2.0.RELEASE.jar
			spring框架依赖包解压路径/
				com.springsource.org.apache.commons.logging-1.1.1.jar	//
				com.springsource.org.apache.log4j-1.2.15.jar			//日志
		* Spring的AOP开发(Aspectj)
			spring框架解压路径/lib/
				spring-aop-3.2.0.RELEASE.jar
				spring-aspects-3.2.0.RELEASE.jar
			spring框架依赖包解压路径/
				com.springsource.org.aopalliance-1.0.0.jar
				com.springsource.org.aspectj.weaver-1.6.8.RELEASE.jar
		* Spring的JDBC支持、事务管理、整合Hibernate
			spring框架解压路径/lib/
            	spring-jdbc-3.2.0.RELEASE.jar
            	spring-tx-3.2.0.RELEASE.jar
            	spring-orm-3.2.0.RELEASE.jar
        * Spring整合web项目:
        	spring框架解压路径/lib/spring-web-3.2.0.RELEASE.jar
        * Spring整合Junit单元测试
        	spring框架解压路径/lib/spring-test-3.2.0.RELEASE.jar
    
    Hibernate框架jar包
        * hibernate框架解压路径/hibernate3.jar
        * hibernate框架解压路径/lib/required/*.jar
        * hibernate框架解压路径/lib/jpr/*.jar
        * hibernate框架整合log4j
            * slf4j-log4j12-1.7.2.jar
        * 数据库驱动包
        * c3p0连接池jar包.	
=============================================================================
=============================================================================
ssh所需要的配置文件，配置文件的作用，及相关配置
ssh：需要的主要的配置文件
	* web.xml:配置struts2的过滤器和spring的监听器
		//核心过滤器
        <filter>
            <filter-name>struts2</filter-name>
            <filter-class>
                org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter
            </filter-class>
        </filter>
	
        <filter-mapping>
            <filter-name>struts2</filter-name>
            <url-pattern>/*</url-pattern>
        </filter-mapping>
        
        <!-- Spring框架使用监听器,服务器启动的时候加载Spring的配置文件 -->
		<listener>
			<listener-class>
				org.springframework.web.context.ContextLoaderListener
			</listener-class>
		</listener>
		<!-- 监听器默认加载WEB-INF/application.xml -->
          <context-param>
              <param-name>contextConfigLocation</param-name>
              <param-value>classpath:applicationContext.xml</param-value>
          </context-param>
	* struts.xml:struts2配置
	
	* applicationContext.xml:管理hibernate的配置文件,对所有包的管理
		<!-- 引入外部jdbc.properties文件 -->
		<context:property-placeholder location="classpath:jdbc.properties"/>
		
		<!-- 配置连接池的信息 -->
        <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
            <!-- 数据库连接的四个基本参数 -->
            <property name="driverClass" value="${jdbc.driver}"/>
            <property name="jdbcUrl" value="${jdbc.url}"/>
            <property name="user" value="${jdbc.user}"/>
            <property name="password" value="${jdbc.password}"/>
        </bean>
	
		<!-- 配置Hibernate的相关属性 -->
		<bean id="sessionFactory" 					class="org.springframework.orm.hibernate3.LocalSessionFactoryBean">
            <!-- 注入连接池 -->
            <property name="dataSource" ref="dataSource"/>
            <!-- 配置Hibernate的其他的属性 -->
            <property name="hibernateProperties">
                <props>
                    <!-- Hibernate的方言 -->
                    <prop key="hibernate.dialect">org.hibernate.dialect.MySQLDialect</prop>
                    <prop key="hibernate.show_sql">true</prop>
                    <prop key="hibernate.format_sql">true</prop>
                    <prop key="hibernate.hbm2ddl.auto">update</prop>
                    <prop key="hibernate.connection.autocommit">false</prop>
                </props>
            </property>
        </bean>

        <!-- 声明式事务管理 -->
        <!-- 配置事务管理器 -->
        <bean id="transactionManager" class="org.springframework.orm.hibernate3.HibernateTransactionManager">
            <!-- 注入 sessionFactory-->
            <property name="sessionFactory" ref="sessionFactory"/>
        </bean>

        <tx:annotation-driven transaction-manager="transactionManager"/>
        
===================================================================================
===================================================================================
* jdbc.properties：数据库配置文件
    jdbc.driver = com.mysql.jdbc.Driver
    jdbc.url = jdbc:mysql:///shop
    jdbc.user = root
    jdbc.password =123		
* log4j.propertyies:日志配置文件
	### direct log messages to stdout ###
    log4j.appender.stdout=org.apache.log4j.ConsoleAppender
    log4j.appender.stdout.Target=System.out
    log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
    log4j.appender.stdout.layout.ConversionPattern=%d{ABSOLUTE} %5p %c{1}:%L - %m%n

    ### direct messages to file mylog.log ###
    log4j.appender.file=org.apache.log4j.FileAppender
    log4j.appender.file.File=c:/mylog.log
    log4j.appender.file.layout=org.apache.log4j.PatternLayout
    log4j.appender.file.layout.ConversionPattern=%d{ABSOLUTE} %5p %c{1}:%L - %m%n

    ### set log levels - for more verbose logging change 'info' to 'debug' ###

    log4j.rootLogger=info, stdout		
```

### 前台功能实现

#### 注册

```
主要完成的功能实现：
	struts2的DispatchAction实现页面跳转
	JSP页面显示（利用jsp标签和struts标签）
	SpringIOC对包进行管理
	dao使用Hibernate进行数据操作
	用户注册页面js代码实现数据简单校验
	用户注册页面使用AJAx技术实现后台用户名校验
	用户注册页面使用validation配置文件进行后台校验
	用户注册页面使用验证码进行简单校验，防止机器注入
```



![ssh](/assets/active_images/javaweb/ssh/regist.png)

* 与数据库中的user表建立联系

```
× javabean
package myproject.user;
/**
 * 用户模块:用户实体类
 * @author qguofeng
 *
 */
public class User {
	private Integer uid;
	private String username;
	private String password;
	private String name;
	private String email;
	private String phone;
	private String addr;
	private String sex;
	private Integer state;
	private String code;
		。。。。。
		。。。。。
}
==============
User.hbm.xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-mapping PUBLIC 
    "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
    "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">
<hibernate-mapping>
	<class name="myproject.user.User" table="user">
		<!-- 配置唯一标识 -->
		<id name="uid" column="uid">
			<generator class="native"></generator>
		</id>
		<!-- 配置普通属性 -->
		<property name="username" column="username"/>
		<property name="password" column="password"/>
		<property name="name" column="name"/>
		<property name="email" column="email"/>
		<property name="phone" column="phone"/>
		<property name="addr" column="addr"/>
		<property name="sex" column="sex"/>
		<property name="state" column="state"/>
		<property name="code" column="code"/>
	</class>
</hibernate-mapping>
==================
applicationContext.xml配置
<!-- 配置映射文件 -->
		<property name="mappingResources">
			<list>
				<value>myproject/user/User.hbm.xml</value>
			</list>
		</property>

```

* 校验

```
× 前台js的简单校验：
	<!-- 简单的js校验-->
	function checkForm(){
		var username = document.getElementById("username").value;
		if(username==""){
			alert("用户不能为空");
			return false;
		}
		
		var password = document.getElementById("password").value;
		if(password==""){
			alert("密码不能为空");
			return false;
		}
		
		var repassword = document.getElementById("repassword").value;
		if(password != repassword){
			alert("两次密码不一致");
			return false;
		}
	}
=================================================
× 配置文件校验
UserAction-user_regist-validation.xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE validators PUBLIC
  		"-//Apache Struts//XWork Validator 1.0.3//EN"
  		"http://struts.apache.org/dtds/xwork-validator-1.0.3.dtd">
  		
<validators>
	<field name="username">
		<field-validator type="requiredstring">
			<message>用户名不能为空</message>
		</field-validator>
	</field>
	
	<field name="password">
		<field-validator type="requiredstring">
			<message>密码不能为空</message>
		</field-validator>
		<field-validator type="stringlength">
			<param name="maxLength">12</param>
			<param name="minLength">6</param>
			<message>密码长度在6-12之间</message>
		</field-validator>
	</field>
	
	<field name="email">
		<!-- 记住这个坑啊 -->
		<field-validator type="requiredstring">
			<message>邮箱不能为空</message>
		</field-validator>
		<field-validator type="email">
			<message>邮箱格式不正确</message>
		</field-validator>
	</field>
</validators>
===============================================================
× 验证码校验：
	实现代码。。。。。。。。。。
	校验代码
        //从session中获取验证码进行验证
        String checkcode1 = (String) ServletActionContext.getRequest().getSession().getAttribute("checkcode");
            if(checkcode==null || ! checkcode1.equalsIgnoreCase(checkcode)){
                //验证码错误
                this.addActionError("验证码错误");
                return "registInput";
            }else{
                userService.regist(user);
                return "registSuccess";
            }	
 ========================================================
 × 发送邮件
 		//发送激活邮件
		/*try {
			myproject.utils.MailUtils.sendMail(user.getEmail(), uuid, user.getUsername());
		} catch (UnsupportedEncodingException e) {
			e.printStackTrace();
		} catch (MessagingException e) {
			e.printStackTrace();
		}*/
		
× 写邮件：
package myproject.utils;

import java.io.UnsupportedEncodingException;
import java.util.Date;
import java.util.Properties;

import javax.mail.MessagingException;
import javax.mail.Session;
import javax.mail.Transport;
import javax.mail.internet.InternetAddress;
import javax.mail.internet.MimeMessage;

/**
 * 发送激活邮件的工具
 * @author qguofeng
 *
 */
public class MailUtils {
	//写邮件
	//写邮件
	/**
	 * 
	 * @param session			又来链接服务器的
	 * @param sendMail			发件人邮箱
	 * @param receiveMail		收件人邮箱
	 * @param code				激活码
	 * @param username			收件人在商城的用户名
	 * @return
	 * @throws UnsupportedEncodingException
	 * @throws MessagingException
	 */
	public static MimeMessage createMimeMessage(Session session,String sendMail,String receiveMail,String code,String username) throws UnsupportedEncodingException, MessagingException{
		//创建一封邮件
		MimeMessage message = new MimeMessage(session);
		
		//From:发件人
		message.setFrom(new InternetAddress(sendMail, "QGUOFENG商城激活邮件", "UTF-8"));
		
		//收件人
		message.setRecipient(MimeMessage.RecipientType.TO, new InternetAddress(receiveMail,username,"UTF-8"));
		
		//邮件的主题
		message.setSubject("QGUOFENG商城激活邮件!", "UTF-8");
		
		//编辑邮件正文
		message.setContent("<h1>来自QGUOFENG商城的官网激活邮件</h1><h3><a href='http://localhost:8080/Myproject/user_active.action?code="+code+"'>http://localhost:8080/Myproject/user_active.action?code="+code+"</a></h3>", "text/html;charset=UTF-8");
		
		//设置发件时间
		message.setSentDate(new Date());
		
		//保存设置
		message.saveChanges();
		
		return message;
	}
	
	//发送邮件
	public static void sendMail(String to,String code,String username) throws UnsupportedEncodingException, MessagingException{
		//设置邮箱服务器的基本信息
		 Properties props = new Properties();
         props.setProperty("mail.transport.protocol", "smtp");
         props.setProperty("mail.smtp.host", "smtp.163.com");
         props.setProperty("mail.smtp.auth", "true");
         
         //1.session对象,与服务器链接
         Session session  = Session.getDefaultInstance(props);
         
         //2.构建邮件信息
         MimeMessage message = createMimeMessage(session, "qguofengsir@163.com", to,code,username);
         
         // 4. 根据 Session 获取邮件传输对象
         Transport transport = session.getTransport();
         transport.connect("qguofengsir@163.com", "qguofengsir2017");
         
         // 6. 发送邮件, 发到所有的收件地址, message.getAllRecipients() 获取到的是在创建邮件对象时添加的所有收件人, 抄送人, 密送人
         transport.sendMessage(message, message.getAllRecipients());
         
         // 7. 关闭连接
         transport.close();
	}
}

× 邮件激活代码
/**
	 * 前台:激活用户的代码实现
	 */
	public String active(){
		//根据激活码查询用户
		User exitUser = userService.findByCode(user.getCode());
		if(exitUser!=null){
			//修改用户状态
			//激活用户   0:未激活		1:激活
			exitUser.setState(1);
			//修改用户状态
			userService.update(exitUser);
			
			//显示添加成功的信息
			this.addActionMessage("激活成功!请去登录!");
			return "msg";
		}else{
			this.addActionMessage("激活失败!请重新注册或重新尝试激活!");
			return "msg";
		}
	}
	
==================================
Jquery用户名异步校验
<!--异步验证用户名-->
	function checkUserName(){
		var username = $("#username").val();
		$("#span1").load("${pageContext.request.contextPath}/user_checkUserName.action?"+new 		Date().getTime(),{'username':username});
	}

```

#### 登录

```
主要完成的功能实现：
	struts2的DispatchAction实现页面跳转
	JSP页面显示（利用jsp标签和struts标签）
	SpringIOC对包进行管理
	dao使用Hibernate进行数据操作
	后台进行用户校验，错误返回错误信息，成功跳转首页
```



![ssh](/assets/active_images/javaweb/ssh/login.png)

```
	UserAction
	/**
	 * 前台:用户模块登录页面制作
	 */
	@InputConfig(resultName="loginInput")
	public String login(){
		User exitUser = userService.login(user);
		if(exitUser!=null){
			ServletActionContext.getRequest().getSession().setAttribute("exitUser", exitUser);
			return "loginSuccess";
		}else{
			this.addActionError("用户名或者密码错误");
			return "loginInput";
		}
	}
	
```



#### 主页面

```
主要完成的功能实现：
	struts2的DispatchAction实现页面跳转
	JSP页面显示（利用jsp标签和struts标签）
	dao使用Hibernate进行数据操作
	主页面：一级分类，二级分类的显示
	主页面：新品，热销，推荐商品显示
```



![ssh](/assets/active_images/javaweb/ssh/index1.png)

![ssh](/assets/active_images/javaweb/ssh/index2.png)

#### 商品分类页

```
主要完成的功能实现：
	struts2的DispatchAction实现页面跳转
	JSP页面显示（利用jsp标签和struts标签）
	SpringIOC对包进行管理
	dao使用Hibernate进行数据操作
	商品分类页显示一级分类，二级分类
	商品分类页显示商品信息（根据分类显示）
```



![ssh](/assets/active_images/javaweb/ssh/product.png)

#### 商品详情页

主要完成的功能实现：

	struts2的DispatchAction实现页面跳转
	JSP页面显示（利用jsp标签和struts标签）
	SpringIOC对包进行管理
	dao使用Hibernate进行数据操作
	商品详细页显示商品的详细信息（根据商品id）
![ssh](/assets/active_images/javaweb/ssh/productDetail.png)

#### 购物车

```
主要完成的功能实现：（用户存入session中，若不存在，跳转公共页面携带错误信息）
	struts2的DispatchAction实现页面跳转
	JSP页面显示（利用jsp标签和struts标签）
	SpringIOC对包进行管理
	dao使用Hibernate进行数据操作
	购物车订单和订单项显示（根据用户显示）
```



![ssh](/assets/active_images/javaweb/ssh/orders.png)

#### 用户信息

```
主要完成的功能实现：（用户存入session中，若不存在，跳转公共页面携带错误信息）
	struts2的DispatchAction实现页面跳转
	JSP页面显示（利用jsp标签和struts标签）
	SpringIOC对包进行管理
	dao使用Hibernate进行数据操作
	用户基本信息显示（根据用户id）
	用户订单显示（各个转台订单数量显示）
```



![ssh](/assets/active_images/javaweb/ssh/usercenter1.png)

![ssh](/assets/active_images/javaweb/ssh/usercenter2.png)

![ssh](/assets/active_images/javaweb/ssh/usercenter3.png)

### 后台页面

```
使用struts2中的拦截器interceptor，继承Interceptor的子类MethodFilterInterceptor实现方法doIntercept
在配置文件中对后台所有页面进行登录拦截（除登录方法），实现简单的登录权限控制
```



#### 登录

```
主要完成的功能实现：
	struts2的DispatchAction实现页面跳转
	JSP页面显示（利用jsp标签和struts标签）
	SpringIOC对包进行管理
	dao使用Hibernate进行数据操作
```



![ssh](/assets/active_images/javaweb/ssh/admin_adminLogin.png)

#### 主页

```
主要完成的功能实现：（用户存入session中，若不存在，跳转公共页面携带错误信息）
	struts2的DispatchAction实现页面跳转
	JSP页面显示（利用jsp标签和struts标签）
	SpringIOC对包进行管理
	dao使用Hibernate进行数据操作
	显示该系统注册人数，成交订单数量，商品数量，用户真实用户率(激活用户/注册用户)
```



![ssh](/assets/active_images/javaweb/ssh/adminIndex.png)

#### 商品列表管理

```
主要完成的功能实现：（用户存入session中，若不存在，跳转公共页面携带错误信息）
	struts2的DispatchAction实现页面跳转
	JSP页面显示（利用jsp标签和struts标签）
	SpringIOC对包进行管理
	dao使用Hibernate进行数据操作
	商品的显示，修改，添加，上架下架，删除
```



![ssh](/assets/active_images/javaweb/ssh/adminProduct.png)

![ssh](/assets/active_images/javaweb/ssh/adminProductAdd.png)

#### 分类管理

```
主要完成的功能实现：（用户存入session中，若不存在，跳转公共页面携带错误信息）
	struts2的DispatchAction实现页面跳转
	JSP页面显示（利用jsp标签和struts标签）
	SpringIOC对包进行管理
	dao使用Hibernate进行数据操作
	一级分类对应二级分类的增加，修改，删除
	一级分类的曾删改查
```



![ssh](/assets/active_images/javaweb/ssh/adminCategory.png)

* 添加

  ​![ssh](/assets/active_images/javaweb/ssh/adminCategory1.png)				

#### 订单管理

```
主要完成的功能实现：（用户存入session中，若不存在，跳转公共页面携带错误信息）
	struts2的DispatchAction实现页面跳转
	JSP页面显示（利用jsp标签和struts标签）
	SpringIOC对包进行管理
	dao使用Hibernate进行数据操作
	订单的显示
	根据订单状态分为：
		未付款订单
		未发货订单
		未收货订单
		未出库订单
		成交订单
```



* 订单列表

  ![ssh](/assets/active_images/javaweb/ssh/adminOrder.png)

* 发货未出库的订单

![ssh](/assets/active_images/javaweb/ssh/adminOrder1.png)

* 未发货的订单

![ssh](/assets/active_images/javaweb/ssh/adminOrder2.png)

#### 会员管理

主要完成的功能实现：（用户存入session中，若不存在，跳转公共页面携带错误信息）

	struts2的DispatchAction实现页面跳转
	JSP页面显示（利用jsp标签和struts标签）
	SpringIOC对包进行管理
	dao使用Hibernate进行数据操作
	会员简单信息显示
* 会员列表

![ssh](/assets/active_images/javaweb/ssh/adminUser.png)

转载请注明：[邱国锋的博客](http://qiuguofeng.com) » [点击阅读原文](http://qiuguofeng.com/2017/06/shop_System/)

