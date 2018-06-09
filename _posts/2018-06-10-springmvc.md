---
title: springmvc
date: 2018-06-10 00:37:13

---

springmvc是spring框架的一个模块(一部分),是基于mvc的表现层框架,用于web项目的开发.
![image](http://wx2.sinaimg.cn/mw690/71081ab7gy1fs5dg5x5pmj20hg0crabk.jpg)

## mvc是什么
mvc是表现层的一个模型.Model(模型) -View(试图) -Conroller(控制器),三层框架的设计模型.主要用于实现前端页面的展现与后端业务数据处理逻辑的分离.
![image](http://wx3.sinaimg.cn/mw690/71081ab7gy1fs5dj31cmmj20kk0ac3yo.jpg)

mvc设计模型的优点: 分成架构的设计,实现了业务系统个耳光组件之间的解耦.有利于实现系统的可扩展性,可维护性.有利于实现系统的并行开发,提升开发率.

# springmvc的入门程序
## 准备环境

jdk:1.7<br>
ide: eclipse<br>
web容器:tomcat<br>
## 创建项目

![image](http://wx2.sinaimg.cn/mw690/71081ab7gy1fs5dg6wm4vj20at0almx9.jpg)

配置pom.xml文件,加入依赖包

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  
  <groupId>cn.itheima</groupId>
  <artifactId>springmvc-first</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  
  <packaging>war</packaging>
  <name>springmvc-first Maven Webapp</name>
	<url>http://maven.apache.org</url>

	<properties>
		<!-- spring版本号 -->
		<spring.version>4.3.8.RELEASE</spring.version>
		<!-- jstl标签版本 -->
		<jstl.version>1.2</jstl.version>
	</properties>

	<dependencies>
		<!-- springmvc依赖包 -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-core</artifactId>
			<version>${spring.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-web</artifactId>
			<version>${spring.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-tx</artifactId>
			<version>${spring.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-jdbc</artifactId>
			<version>${spring.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
			<version>${spring.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-aop</artifactId>
			<version>${spring.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context-support</artifactId>
			<version>${spring.version}</version>
		</dependency>
		<!-- JSTL标签类 -->
		<dependency>
			<groupId>jstl</groupId>
			<artifactId>jstl</artifactId>
			<version>${jstl.version}</version>
		</dependency>
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>4.12</version>
			<scope>test</scope>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.apache.tomcat.maven</groupId>
				<artifactId>tomcat7-maven-plugin</artifactId>
				<version>2.1</version>
				<configuration>
					<!-- tomcat 的端口号 -->
					<port>8080</port>
					<!-- 访问应用的路径 -->
					<path>/springmvc-first</path>
					<!-- URL按UTF-8进行编码，解决中文参数乱码 -->
					<uriEncoding>UTF-8</uriEncoding>
					<!-- tomcat名称 -->
					<server>tomcat7</server>
				</configuration>
			</plugin>
		</plugins>
	</build>
</project>
```
## 准备配置文件
### springmvc.xml
__说明:它是springmvc框架的主配置文件,文件名可以修改__


```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:p="http://www.springframework.org/schema/p"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:mvc="http://www.springframework.org/schema/mvc"
	xsi:schemaLocation="http://www.springframework.org/schema/beans 
	http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
        http://www.springframework.org/schema/mvc 
        http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd
        http://www.springframework.org/schema/context 
        http://www.springframework.org/schema/context/spring-context-4.0.xsd">

</beans>
```
### web.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xmlns:web="http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" id="WebApp_ID" version="2.5">
  <display-name>springmvc-first</display-name>
  
  <!-- 配置前端控制器：DispatcherServlet -->
  <servlet>
  	<servlet-name>springmvc-first</servlet-name>
  	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  	
  	<!-- 加载springmvc主配置文件 -->
  	<init-param>
  		<param-name>contextConfigLocation</param-name>
  		<param-value>classpath:springmvc.xml</param-value>
  	</init-param>
  	
  	<!-- 配置什么时候加载前端控制器，说明：
  		1.配置大于等于0的整数，表示web容器启动的时候加载
  		2.配置小于0的整数，表示在第一次请求到达的时候加载
  	 -->
  	<load-on-startup>1</load-on-startup>
  </servlet>
  
  <servlet-mapping>
  	<servlet-name>springmvc-first</servlet-name>
  	<!-- 配置拦截的请求url规则，说明：
  		1.*.do,表示以.do结尾的请求进入前端控制器
  		2./，表示所有请求都进入前端控制器
  	 -->
  	<url-pattern>*.do</url-pattern>
  </servlet-mapping>
  
  <!-- 默认的欢迎页面 -->
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>
</web-app>
```

## 编写jsp页面

```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>springmvc入门程序案例</title>
</head>
<body>
	你好，世界！${hello}
</body>
</html>
```

## 编写控制器(处理器)cotroller
__说明: 相当于struts2中的action__

```java
package cn.itheima.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;


@Controller
public class HelloController {
	
	/**
	 * 问好
	 * ModelAndView：模型和视图。用于设置响应的视图；用于设置响应的模型数据
	 * @RequestMapping：配置请求的url
	 */
	@RequestMapping("/hello.do")
	public ModelAndView hello(){
		
		// 1.创建ModelAndView对象
		ModelAndView mav = new ModelAndView();
		
		// 2.设置响应模型数据
		/**
		 * addObject：设置模型数据
		 * 参数：
		 * 		attributeName：模型名称
		 * 		attributeValue:模型数据
		 */
		mav.addObject("hello", "210大家");
		
		// 3.设置响应的页面
		/**
		 * setViewName:设置响应的视图
		 * 参数：
		 * 		viewName：视图名称（是视图的物理路径）
		 */
		mav.setViewName("/WEB-INF/jsp/hello.jsp");
		
		return mav;
		
	}

}
```

## 在springmvc.xml中,配置扫描controller

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:p="http://www.springframework.org/schema/p"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:mvc="http://www.springframework.org/schema/mvc"
	xsi:schemaLocation="http://www.springframework.org/schema/beans 
	http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
        http://www.springframework.org/schema/mvc 
        http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd
        http://www.springframework.org/schema/context 
        http://www.springframework.org/schema/context/spring-context-4.0.xsd">
        
        <!-- 配置扫描controller -->
        <context:component-scan base-package="cn.itheima.controller"/>

</beans>
```


## 启动测试
![image](http://wx4.sinaimg.cn/mw690/71081ab7gy1fs5diues9kj20vm0903yq.jpg)

## 入门程序总结
1. 加入springmvc的依赖包
2. 编写主配置文件springmvc.xml
3. 在web.xml中配置前端控制器:DispatcherServlet
4. 编写一个jsp页面:hello.jsp
5. 编写一个控制器:HelloController.java
6. 在主配置文件中springmvc.xml,配置扫描controller
7. 启动测试

# springmvc三大组件
>处理映射器(HandlerMapping)
>处理器适配器(HandlerAdapter)
>试图解析器(ViewResolver)

## 处理器映射器
作用: 根据请求的url找到处理器方法

### 默认的处理器映射器

在DispatcherServlet.properties

```
org.springframework.web.servlet.HandlerMapping=org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping,\
	org.springframework.web.servlet.mvc.annotation.DefaultAnnotationHandlerMapping
```
==__注意事项：默认的处理器映射器，已经过时。企业项目中不推荐使用。__==

### 配置处理器映射器


```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:p="http://www.springframework.org/schema/p"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:mvc="http://www.springframework.org/schema/mvc"
	xsi:schemaLocation="http://www.springframework.org/schema/beans 
	http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
        http://www.springframework.org/schema/mvc 
        http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd
        http://www.springframework.org/schema/context 
        http://www.springframework.org/schema/context/spring-context-4.0.xsd">
        
        <!-- 配置扫描controller -->
        <context:component-scan base-package="cn.itheima.controller"/>
        
        <!-- 配置处理器映射器 -->
        <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping"/>

</beans> 
```
## 处理器适配器

作用:执行处理器方法
### 默认的处理器适配器

```
org.springframework.web.servlet.HandlerAdapter=org.springframework.web.servlet.mvc.HttpRequestHandlerAdapter,\
	org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter,\
	org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerAdapter
```

==__注意事项：默认的处理器适配器，已经过时。企业项目中不推荐使用。__==

### 配置处理器适配器

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:p="http://www.springframework.org/schema/p"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:mvc="http://www.springframework.org/schema/mvc"
	xsi:schemaLocation="http://www.springframework.org/schema/beans 
	http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
        http://www.springframework.org/schema/mvc 
        http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd
        http://www.springframework.org/schema/context 
        http://www.springframework.org/schema/context/spring-context-4.0.xsd">
        
        <!-- 配置扫描controller -->
        <context:component-scan base-package="cn.itheima.controller"/>
        
        <!-- 配置处理器映射器 -->
        <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping"/>
        
        <!-- 配置处理器适配器 -->
        <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter"/>

</beans> 
```

==__注意事项：处理器映射器与处理器适配器必须要配对使用。__==

![image](http://wx4.sinaimg.cn/mw690/71081ab7gy1fs5diuyt57j21br0hwmz2.jpg)

## 处理器映射器和处理器配置方式二(重要)

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:p="http://www.springframework.org/schema/p"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:mvc="http://www.springframework.org/schema/mvc"
	xsi:schemaLocation="http://www.springframework.org/schema/beans 
	http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
        http://www.springframework.org/schema/mvc 
        http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd
        http://www.springframework.org/schema/context 
        http://www.springframework.org/schema/context/spring-context-4.0.xsd">
        
        <!-- 配置扫描controller -->
        <context:component-scan base-package="cn.itheima.controller"/>
        
        <!-- 配置处理器映射器 
        <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping"/>-->
        
        <!-- 配置处理器适配器 -->
        <!-- <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter"/> -->
        
        <!-- 注解驱动方式配置处理器映射器和处理器适配器,说明：
        	1.它等于同时配置了RequestMappingHandlerMapping/RequestMappingHandlerAdapter
        	2.在企业项目中，推荐使用的方式
         -->
        <mvc:annotation-driven/>

</beans> 
```
## 视图解析器
作用: 把逻辑视图(在controller中设置视图名称)解析成物理视图(在浏览器看到的页面).
### 默认的视图解析器

```
org.springframework.web.servlet.ViewResolver=org.springframework.web.servlet.view.InternalResourceViewResolver
```
### 配置视图解析器

```
<!-- 配置视图解析器 -->
        <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        	<!-- 配置视图的公共目录名称 -->
        	<property name="prefix" value="/WEB-INF/jsp/"/>
        	<!-- 配置视图的扩展名称 -->
        	<property name="suffix" value=".jsp"/>
        </bean>
```

# springmvc框架原理
![image](http://wx3.sinaimg.cn/mw690/71081ab7gy1fs5dj31cmmj20kk0ac3yo.jpg)








































































































