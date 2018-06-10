---
title: springmvc+spring+mybatis整合(ssm)
date: 2018-06-11 16:43:11

---

## 整合思路

### 项目分成

1. 表现层:spring(controller)
2. 业务层:service
3. 持久层:mybatis(mapper)

说明:controller,service,mapper都是javaBean.通过spring整合.

![image](http://wx3.sinaimg.cn/mw690/71081ab7gy1fs5dj413jdj20hi0c5aae.jpg)

## 整合步骤
### 准备环境
jdk : 1.7<br>
ide : eclipse<br>
数据库: mysql<br>
web容器: tomcat<br>
### 准备数据
![image](http://wx4.sinaimg.cn/mw690/71081ab7gy1fs5divir40j207v0760t0.jpg)
### 创建整合项目

![image](http://wx2.sinaimg.cn/mw690/71081ab7gy1fs5divxksqj20930b2mx8.jpg)

### 配置pom.xml文件,加入依赖包
mybatis框架包<br>
spring框架包(包括springmvc)<br>
mybatis-spring整合包<br>
数据库驱动包<br>
数据库连接池(dbcp)<br>
log4j日志包<br>
jstl标签库<br>

```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>cn.itheima</groupId>
	<artifactId>ssm</artifactId>
	<version>0.0.1-SNAPSHOT</version>

	<packaging>war</packaging>

	<name>ssm Maven Webapp</name>
	<url>http://maven.apache.org</url>

	<properties>
		<!-- spring版本号 -->
		<spring.version>4.3.8.RELEASE</spring.version>
		<!-- aspectj版本号 -->
		<aspectj.version>1.6.12</aspectj.version>
		<!-- jstl标签版本 -->
		<jstl.version>1.2</jstl.version>
		<!-- mybatis版本号 -->
		<mybatis.version>3.4.5</mybatis.version>
		<!-- mybatis-spring整合包版本 -->
		<mybatis.spring.version>1.3.1</mybatis.spring.version>
		<!-- mysql版本号 -->
		<mysql.version>5.1.30</mysql.version>
		<!-- dbcp数据源连接池jar包 -->
		<dbcp.version>1.2.2</dbcp.version>
		<!-- log4j日志包版本 -->
		<slf4j.version>1.7.7</slf4j.version>
		<log4j.version>1.2.17</log4j.version>
		<!-- commons-lang版本 -->
		<commons.lang.version>2.6</commons.lang.version>
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
		<dependency>
			<groupId>org.aspectj</groupId>
			<artifactId>aspectjweaver</artifactId>
			<version>${aspectj.version}</version>
		</dependency>
		<!-- JSTL标签类 -->
		<dependency>
			<groupId>jstl</groupId>
			<artifactId>jstl</artifactId>
			<version>${jstl.version}</version>
		</dependency>
		<!-- mybatis核心包 -->
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis</artifactId>
			<version>${mybatis.version}</version>
		</dependency>
		<!-- mybatis-spring整合包 -->
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis-spring</artifactId>
			<version>${mybatis.spring.version}</version>
		</dependency>
		<!-- 导入Mysql数据库链接jar包 -->
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>${mysql.version}</version>
		</dependency>
		<!-- 导入dbcp数据源连接池jar包 -->
		<dependency>
			<groupId>commons-dbcp</groupId>
			<artifactId>commons-dbcp</artifactId>
			<version>${dbcp.version}</version>
		</dependency>
		<!-- 日志文件管理包 -->
		<dependency>
			<groupId>log4j</groupId>
			<artifactId>log4j</artifactId>
			<version>${log4j.version}</version>
		</dependency>
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-api</artifactId>
			<version>${slf4j.version}</version>
		</dependency>
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-log4j12</artifactId>
			<version>${slf4j.version}</version>
		</dependency>
		<dependency>
			<groupId>commons-lang</groupId>
			<artifactId>commons-lang</artifactId>
			<version>${commons.lang.version}</version>
		</dependency>

		<!-- jsp依赖包，只在编译时需要 -->
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>jsp-api</artifactId>
			<version>2.0</version>
			<scope>provided</scope>
		</dependency>
	</dependencies>
	<build>
		<finalName>ssm</finalName>
		<plugins>
			<plugin>
				<groupId>org.apache.tomcat.maven</groupId>
				<artifactId>tomcat7-maven-plugin</artifactId>
				<version>2.1</version>
				<configuration>
					<!-- tomcat 的端口号 -->
					<port>8080</port>
					<!-- 访问应用的路径 -->
					<path>/ssm</path>
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
### 准备配置文件
#### sqlMapConfig.xml
准备别名

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN" 
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	
	<!-- 配置别名 -->
	<typeAliases>
		<!-- 包扫描的方式配置 -->
		<package name="cn.ssm.po"/>
	</typeAliases>

</configuration>
```
#### springmvc.xml
配置扫描controller<br>
配置处理器映射器<br>
配置处理器适配器<br>
配置视图解析器<br>

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
        <context:component-scan base-package="cn.itheima.ssm.controller"/>
        
        <!-- 注解驱动方式配置处理器映射器和处理器适配器 -->
        <mvc:annotation-driven/>
        
        <!-- 配置视图解析器 -->
        <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        	<!-- 配置视图的公共目录路径 -->
        	<property name="prefix"  value="/WEB-INF/jsp/"/>
        	<!-- 配置视图的扩展名称 -->
        	<property name="suffix"  value=".jsp"/>
        </bean>

</beans>
```

#### applicationContext-dao.xml
配置数据源对象(DataSource)<br>
配置mybatis核心对象(SqlSessionFactory)<br>
配置mapper扫描器(MapperScannerConfigurer)<br>


```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:p="http://www.springframework.org/schema/p"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:tx="http://www.springframework.org/schema/tx"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans 
	http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
	http://www.springframework.org/schema/context 
	http://www.springframework.org/schema/context/spring-context-4.0.xsd
	http://www.springframework.org/schema/aop 
	http://www.springframework.org/schema/aop/spring-aop-4.0.xsd 
	http://www.springframework.org/schema/tx 
	http://www.springframework.org/schema/tx/spring-tx-4.0.xsd
	http://www.springframework.org/schema/util 
	http://www.springframework.org/schema/util/spring-util-4.0.xsd">
	
	<!-- 加载db.properties文件 -->
	<context:property-placeholder location="classpath:db.properties"/>
	
	<!-- 配置数据源 -->
	<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource" 
		destroy-method="close">
		
		<property name="driverClassName" value="${db.driverClassName}"/>
		<property name="url" value="${db.url}" />
		<property name="username" value="${db.username}" />
		<property name="password" value="${db.password}" />
		
		<!-- 最大连接数量 -->
		<property name="maxActive" value="${db.maxActive}"/>
		<!-- 最小空闲连接 -->
		<property name="minIdle" value="${db.minIdle}"/>
		<!-- 最大空闲连接 -->
		<property name="maxIdle" value="${db.maxIdle}"/>
		<!-- 初始化连接数 -->
		<property name="initialSize" value="${db.initialSize}"/>
		<!-- 超时等待时间以毫秒为单位 -->
		<property name="maxWait" value="${db.maxWait}"/>
	</bean>
	
	<!-- 配置sqlSessionFactory -->
	<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
		<!-- 注入数据源对象 -->
		<property name="dataSource" ref="dataSource"/>
		<!-- 加载mybatis主配置文件 -->
		<property name="configLocation" value="classpath:mybatis/sqlMapConfig.xml"/>
	</bean>
	
	<!-- 配置mapper扫描器 -->
	<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
		<!-- 配置要扫描的包，说明：
			1.如果有多个包，在同一个父包下，配置父包即可
			2.如果不在同一个父包，以半角逗号进行分割：","
		 -->
		 <property name="basePackage" value="cn.itheima.ssm.mapper"/>
	</bean>
	
</beans>
```
#### applicationContext-service.xml
配置扫描service

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:p="http://www.springframework.org/schema/p"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:tx="http://www.springframework.org/schema/tx"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans 
	http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
	http://www.springframework.org/schema/context 
	http://www.springframework.org/schema/context/spring-context-4.0.xsd
	http://www.springframework.org/schema/aop 
	http://www.springframework.org/schema/aop/spring-aop-4.0.xsd 
	http://www.springframework.org/schema/tx 
	http://www.springframework.org/schema/tx/spring-tx-4.0.xsd
	http://www.springframework.org/schema/util 
	http://www.springframework.org/schema/util/spring-util-4.0.xsd">
	
	<!-- 配置service扫描 -->
	<context:component-scan base-package="cn.itheima.ssm.service"/>

</beans>
```

#### applicationContext-trans.xml
配置事务管理器(transactionManager)<br>
配置通知(txAdvice)<br>
配置切面(aop:config)

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:p="http://www.springframework.org/schema/p"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:tx="http://www.springframework.org/schema/tx"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans 
	http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
	http://www.springframework.org/schema/context 
	http://www.springframework.org/schema/context/spring-context-4.0.xsd
	http://www.springframework.org/schema/aop 
	http://www.springframework.org/schema/aop/spring-aop-4.0.xsd 
	http://www.springframework.org/schema/tx 
	http://www.springframework.org/schema/tx/spring-tx-4.0.xsd
	http://www.springframework.org/schema/util 
	http://www.springframework.org/schema/util/spring-util-4.0.xsd">
	
	<!-- 配置事务管理器 -->
	<bean id="transactionManager" 
		class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
		<!-- 指定数据源 -->
		<property name="dataSource" ref="dataSource"/>
	</bean>
	
	<!-- 配置事务通知 -->
	<tx:advice id="txAdvice" transaction-manager="transactionManager">
		<tx:attributes>
			<!-- 配置传播行为 -->
			<tx:method name="add*" propagation="REQUIRED"/>
			<tx:method name="insert*" propagation="REQUIRED"/>
			<tx:method name="save*" propagation="REQUIRED"/>
			<tx:method name="update*" propagation="REQUIRED"/>
			<tx:method name="delete*" propagation="REQUIRED"/>
			
			<tx:method name="find*" propagation="SUPPORTS" read-only="true"/>
			<tx:method name="select*" propagation="SUPPORTS" read-only="true"/>
			<tx:method name="get*" propagation="SUPPORTS" read-only="true"/>
			<tx:method name="query*" propagation="SUPPORTS" read-only="true"/>
		</tx:attributes>
	</tx:advice>
	
	<!-- 事务切面配置 -->
	<aop:config>
		<!-- 指定cn.itheima.ssm.service包下的任意类的任意方法，需要事务 -->
		<aop:advisor advice-ref="txAdvice" 
			pointcut="execution(* cn.itheima.ssm.service..*.*(..))"/>
	</aop:config>
	
</beans>
```
#### db.properties

```
db.driverClassName=com.mysql.jdbc.Driver
db.url=jdbc:mysql://127.0.0.1:3306/79_springmvc?characterEncoding=utf-8
db.username=root
db.password=admin

db.maxActive=10
db.minIdle=2
db.maxIdle=5
db.initialSize=5
db.maxWait=6000
```

#### log4j.properties


```
# Global logging configuration
log4j.rootLogger=INFO, stdout

# Console output...
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n
```
#### web.xml
配置加载spring配置文件<br>
配置spring监听器(ContextLoaderListener)<br>
配置前端控制器(DispatcherServlet)

```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xmlns:web="http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" id="WebApp_ID" version="2.5">
  <display-name>ssm</display-name>
  
  <!-- 加载spring配置文件 -->
  <context-param>
  	<param-name>contextConfigLocation</param-name>
  	<param-value>classpath:spring/applicationContext-*.xml</param-value>
  </context-param>
  
  <!-- 配置spring监听器 -->
  <listener>
  	<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>
  
  <!-- 配置前端控制器 -->
  <servlet>
  	<servlet-name>ssm</servlet-name>
  	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  	
  	<!-- 加载springmvc主配置文件 -->
  	<init-param>
  		<param-name>contextConfigLocation</param-name>
  		<param-value>classpath:spring/springmvc.xml</param-value>
  	</init-param>
  	<!-- 配置tomcat启动的时候加载前端控制器 -->
  	<load-on-startup>1</load-on-startup>
  </servlet>
  
  <servlet-mapping>
  	<servlet-name>ssm</servlet-name>
  	<!-- 配置请求的url规则，说明：
  		1.*.do，表示以.do结尾的请求进入前端控制器
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
### 整合好的项目
![image](http://wx3.sinaimg.cn/mw690/71081ab7gy1fs5diwb8g0j20960hrgly.jpg)

## 需求

查询商品列表数据,现实商品列表页面(通过浏览器访问)
![image](http://wx2.sinaimg.cn/mw690/71081ab7gy1fs5diwrofyj20qs096jsl.jpg)

## 需求实现

### 表现层开发

#### 准备商品列表jsp页面


```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" 
"http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>查询商品列表</title>
</head>
<body>

	<form action="${pageContext.request.contextPath }/queryItem.do"
		method="post">
		查询条件：
		<table width="100%" border=1>
			<tr>
				<td>
					<!--商品名称：<input type="text" name="item.name" value=""/>-->
					<input type="submit" value="查询" />
				</td>
			</tr>
		</table>
		商品列表：
		<table width="100%" border=1>
			<tr>
				<td>商品名称</td>
				<td>商品价格</td>
				<td>生产日期</td>
				<td>商品描述</td>
				<td>操作</td>
			</tr>
			<c:forEach items="${itemList}" var="item">
				<tr>
					<td>${item.name }</td>
					<td>${item.price }</td>
					<td><fmt:formatDate value="${item.createtime}"
							pattern="yyyy-MM-dd HH:mm:ss" /></td>
					<td>${item.detail }</td>

					<td><a
						href="${pageContext.request.contextPath }/queryItemById.do?id=${item.id}">修改</a></td>

				</tr>
			</c:forEach>

		</table>
	</form>

</body>

</html>
```
#### 编写商品controller


```
package cn.itheima.ssm.controller;

import java.util.ArrayList;
import java.util.Date;
import java.util.List;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

import cn.itheima.ssm.po.Item;

/** 
 * @ClassName: ItemController 
 * @Description: 商品处理器
 * @author 传智 小杨老师  
 * @date 2018-5-31 下午5:56:52 
 *  
 */
@Controller
public class ItemController {
	
	/**
	 * 查询商品列表数据
	 * action="${pageContext.request.contextPath }/queryItem.do"
	 */
	@RequestMapping("/queryItem.do")
	public ModelAndView queryItem(){
		
		// 1.创建ModelAndView对象
		ModelAndView mav = new ModelAndView();
		
		// 2.设置商品模型数据
		// 准备商品静态数据
		List<Item> itemList = new ArrayList<Item>();
		for(int i=0;i<2;i++){
			// 创建商品对象
			Item item = new Item();
			
//			商品Id
			item.setId(i);
//			商品名称
			item.setName("商品名称："+i);
//			商品价格
			item.setPrice(100f);
//			商品日期
			item.setCreatetime(new Date());
//			商品描述
			item.setDetail("商品描述："+i);
			
			itemList.add(item);
			
		}
		
		mav.addObject("itemList", itemList);
		
		// 3.响应商品列表页面
		mav.setViewName("item/itemList");
		
		return mav;
		
		
	}

}
```

#### 初步测试
![image](http://wx4.sinaimg.cn/mw690/71081ab7gy1fs5dix6vgkj21hc0a2wf9.jpg)


### 持久层开发

说明:商品mapper接口和映射文件使用逆向工程生成,不需要卡覅

![image](http://wx1.sinaimg.cn/mw690/71081ab7gy1fs5dixkueaj20990d5jrm.jpg)

### 业务层开发

#### 编写商品service接口
```java
package cn.itheima.ssm.service;

import java.util.List;

import cn.itheima.ssm.po.Item;

public interface ItemService {
	
	/**
	 * 查询全部商品列表
	 */
	List<Item> queryItemList();

}
```
#### 编写商品service实现类

```java
package cn.itheima.ssm.service.impl;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import cn.itheima.ssm.mapper.ItemMapper;
import cn.itheima.ssm.po.Item;
import cn.itheima.ssm.service.ItemService;


@Service
public class ItemServiceImpl implements ItemService {
	
	// 注入商品mapper
	@Autowired
	private ItemMapper itemMapper;

	/**
	 * 查询全部商品列表
	 */
	public List<Item> queryItemList() {
		
		List<Item> list = itemMapper.selectByExample(null);
		
		return list;
	}

}
```

### 修改商品controller,增加查询数据库的商品


```
/**
	 * 查询商品列表数据
	 * action="${pageContext.request.contextPath }/queryItem.do"
	 */
	@RequestMapping("/queryItem.do")
	public ModelAndView queryItem(){
		
		// 1.创建ModelAndView对象
		ModelAndView mav = new ModelAndView();
		
		// 2.设置商品模型数据
		// 准备商品静态数据
		/*List<Item> itemList = new ArrayList<Item>();
		for(int i=0;i<2;i++){
			// 创建商品对象
			Item item = new Item();
			
//			商品Id
			item.setId(i);
//			商品名称
			item.setName("商品名称："+i);
//			商品价格
			item.setPrice(100f);
//			商品日期
			item.setCreatetime(new Date());
//			商品描述
			item.setDetail("商品描述："+i);
			
			itemList.add(item);
			
		}*/
		
		// 查询数据库商品，替换静态商品数据
		List<Item>  itemList = itemService.queryItemList();
		
		mav.addObject("itemList", itemList);
		
		// 3.响应商品列表页面
		mav.setViewName("item/itemList");
		
		return mav;
		
		
	}
```

![image](http://wx1.sinaimg.cn/mw690/71081ab7gy1fs5dixy86sj21gu0blt9n.jpg)


# springmvc参数绑定
说明:参数绑定就是在通过处理器方法的形参,接收到请求的url或者表单的参数数据
## 需求

http://127.0.0.1:8080/ssm/queryItemById.do?id=1
根据商品Id查询商品数据，显示到商品编辑页面。

## 需求实现

### 准备商品编辑页面

```
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" 
"http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>修改商品信息</title>

</head>
<body>
	<!-- 上传图片是需要指定属性 enctype="multipart/form-data" -->
	<!-- <form id="itemForm" action="" method="post" enctype="multipart/form-data"> -->
	<form id="itemForm"
		action="${pageContext.request.contextPath }/updateItem.do"
		method="post">
		<input type="hidden" name="id" value="${item.id }" /> 修改商品信息：
		<table width="100%" border=1>
			<tr>
				<td>商品名称</td>
				<td><input type="text" name="name" value="${item.name }" /></td>
			</tr>
			<tr>
				<td>商品价格</td>
				<td><input type="text" name="price" value="${item.price }" /></td>
			</tr>
		
			<%-- <tr>
				<td>商品生产日期</td>
				<td><input type="text" name="createtime"
					value="<fmt:formatDate value="${item.createtime}" pattern="yyyy-MM-dd HH:mm:ss"/>" /></td>
			</tr> --%>
		<%-- 
			<tr>
				<td>商品图片</td>
				<td>
					<c:if test="${item.pic !=null}">
						<img src="http://127.0.0.1:8081/pic/${item.pic}" width=100 height=100/>
						<br/>
					</c:if>
					<input type="file"  name="pictureFile"/> 
				</td>
			</tr>
			 --%>
			<tr>
				<td>商品简介</td>
				<td><textarea rows="3" cols="30" name="detail">${item.detail}</textarea>
				</td>
			</tr>
			<tr>
				<td colspan="2" align="center"><input type="submit" value="提交" />
				</td>
			</tr>
		</table>

	</form>
</body>

</html>
```

### 声明商品service接口方法

```
package cn.itheima.ssm.service;

import java.util.List;

import cn.itheima.ssm.po.Item;

/** 
 * @ClassName: ItemService 
 * @Description: 商品service接口
 * @author 传智 小杨老师  
 * @date 2018-5-31 下午6:07:43 
 *  
 */
public interface ItemService {
	
	/**
	 * 查询全部商品列表
	 */
	List<Item> queryItemList();
	
	/**
	 * 根据商品Id查询商品数据
	 */
	Item queryItemById(Integer id);

}
```

### 实现商品service方法


```
/**
	 * 根据商品Id查询商品数据
	 */
	public Item queryItemById(Integer id) {
		Item item = itemMapper.selectByPrimaryKey(id);
		return item;
	}
```
### 在商品controller中增加,根据商品Id查询的方法


```
/**
	 * 根据商品Id查询商品
	 * http://127.0.0.1:8080/ssm/queryItemById.do?id=1
	 * 形参request对象，接收请求的商品id参数。springmvc框架在执行方法的时候，会传递request对象。
	 */
	@RequestMapping("/queryItemById.do")
	public ModelAndView queryItemById(HttpServletRequest request){
		
		// 1.获取商品Id参数
		String id = request.getParameter("id");
		
		// 2.查询商品数据
		Item item = this.itemService.queryItemById(Integer.parseInt(id));
		
		// 3.创建ModelAndView对象
		ModelAndView mav = new ModelAndView();
		mav.addObject("item", item);
		mav.setViewName("item/itemEdit");
		
		
		return mav;
	}
```
![image](http://wx1.sinaimg.cn/mw690/71081ab7gy1fs5diyi1ivj20yq0dlgmt.jpg)

## 默认支持的参数类型

### HttpServletRequest
作用:通过request对象,获取请求的参数类型
### HttpServletResponse
作用:通过response对象,操作会话域的数据
### Model/ModelMap
说明:
1. Model是模型,是一个接口,用于设置响应的模型数据
2. Model是一个实现类,使用ModleMap和Model都是一样的.使用Model最终也是实例化ModelMap
3. 使用Model响应数据模型,就可以不使用ModelAndView,视图可以使用字符串String返回

```
/**
	 * 使用Model响应模型数据，使用字符串String响应视图
	 */
	@RequestMapping("/queryItemById.do")
	public String queryItemById(Model model,HttpServletRequest request){
		
		// 1.获取商品Id参数
		String id = request.getParameter("id");
		
		// 2.查询商品数据
		Item item = this.itemService.queryItemById(Integer.parseInt(id));
		
		// 3.使用model响应模型数据
		/**
		 * addAttribute和addObject是相同的意思
		 */
		model.addAttribute("item", item);

		return "item/itemEdit";
	}
```

## 简单参数类型绑定
### 通过简单类型Integer,接受请求的商品Id参数


```
/**
	 * 使用简单参数类型，接收请求的商品Id参数
	 * http://127.0.0.1:8080/ssm/queryItemById.do?id=1
	 */
	@RequestMapping("/queryItemById.do")
	public String queryItemById(Model model,Integer id){
		
		// 1.查询商品数据
		Item item = this.itemService.queryItemById(id);
		
		// 2.使用model响应模型数据
		/**
		 * addAttribute和addObject是相同的意思
		 */
		model.addAttribute("item", item);

		return "item/itemEdit";
	}
```

![image](http://wx2.sinaimg.cn/mw690/71081ab7gy1fs5diz20o4j20x90eqq45.jpg)



==__注意事项：使用简单类型绑定参数，建议使用简单类型的包装类型（Integer），不建议使用简单类型的基础类型（int）。原因是基础类型不能为null值，如果不传递会报异常。__==

![image](http://wx1.sinaimg.cn/mw690/71081ab7gy1fs5dizu4gvj21h3077wfr.jpg)


### 常用简单类型

![image](http://wx1.sinaimg.cn/mw690/71081ab7gy1fs5dj4iuu0j20oh064q2w.jpg)


### @RequestParam注解

作用:设置请求的参数名称,与方法形参名称匹配
#### value属性


```
/**
	 * @RequestParam注解讲解专用
	 * http://127.0.0.1:8080/ssm/queryItemById.do?itemId=1
	 * 
	 * 注解说明：
	 * 	@RequestParam：设置请求的参数名称，与方法形参的名称匹配
	 * 属性：
	 * 		value：设置请求的参数名称
	 */
	@RequestMapping("/queryItemById.do")
	public String queryItemById(Model model,@RequestParam(value="itemId")Integer id){
		
		// 1.查询商品数据
		Item item = this.itemService.queryItemById(id);
		
		// 2.使用model响应模型数据
		/**
		 * addAttribute和addObject是相同的意思
		 */
		model.addAttribute("item", item);

		return "item/itemEdit";
	}
```

#### required属性


```
/**
	 * @RequestParam注解讲解专用
	 * http://127.0.0.1:8080/ssm/queryItemById.do?itemId=1
	 * 
	 * 注解说明：
	 * 	@RequestParam：设置请求的参数名称，与方法形参的名称匹配
	 * 属性：
	 * 		value：设置请求的参数名称
	 * 		required：设置请求的参数是否必须要传递。true：必须传递；fasle：可以传递可以不传递。默认true。
	 */
	@RequestMapping("/queryItemById.do")
	public String queryItemById(Model model,@RequestParam(value="itemId",
	required=true)Integer id){
		
		// 1.查询商品数据
		Item item = this.itemService.queryItemById(id);
		
		// 2.使用model响应模型数据
		/**
		 * addAttribute和addObject是相同的意思
		 */
		model.addAttribute("item", item);

		return "item/itemEdit";
	}
```
#### default属性

```
/**
	 * @RequestParam注解讲解专用
	 * http://127.0.0.1:8080/ssm/queryItemById.do?itemId=1
	 * 
	 * 注解说明：
	 * 	@RequestParam：设置请求的参数名称，与方法形参的名称匹配
	 * 属性：
	 * 		value：设置请求的参数名称
	 * 		required：设置请求的参数是否必须要传递。true：必须传递；fasle：可以传递可以不传递。默认true。
	 * 		defaultValue：设置参数默认值。如果传递使用实际参数值；不传递使用默认值
	 */
	@RequestMapping("/queryItemById.do")
	public String queryItemById(Model model,@RequestParam(value="itemId",
	required=true,defaultValue="2")Integer id){
		
		// 1.查询商品数据
		Item item = this.itemService.queryItemById(id);
		
		// 2.使用model响应模型数据
		/**
		 * addAttribute和addObject是相同的意思
		 */
		model.addAttribute("item", item);

		return "item/itemEdit";
	}
```
## pojo参数类型

### 需求
修改商品数据,保存数据库
### 需求实现
#### 声明service接口方法

```
package cn.itheima.ssm.service;

import java.util.List;

import cn.itheima.ssm.po.Item;

/** 
 * @ClassName: ItemService 
 * @Description: 商品service接口
 * @author 传智 小杨老师  
 * @date 2018-5-31 下午6:07:43 
 *  
 */
public interface ItemService {
	
	/**
	 * 查询全部商品列表
	 */
	List<Item> queryItemList();
	
	/**
	 * 根据商品Id查询商品数据
	 */
	Item queryItemById(Integer id);
	
	/**
	 * 修改商品数据，保存数据库
	 */
	void updateItem(Item item);

}
```
#### 实现service接口方法


```
/**
	 * 修改商品数据，包数据库
	 */
	public void updateItem(Item item) {
		itemMapper.updateByPrimaryKeySelective(item);
		
	}
```

#### 在商品controller中,增加修改商品的方法


```
/**
	 * 修改商品数据，保存数据库
	 * http://127.0.0.1:8080/ssm/updateItem.do
	 */
	@RequestMapping("/updateItem.do")
	public String updateItem(Item item){
		
		try{
			// 保存商品数据
			this.itemService.updateItem(item);
		}catch(Exception e){
			e.printStackTrace();
			
			// 发生异常，提示保存失败
			return "common/failure";
		}
		
		// 保存成功，提示成功
		return "common/success";
		
		
	}
```

![image](http://wx3.sinaimg.cn/mw690/71081ab7gy1fs5dj0tc8yj21210ffwg1.jpg)

乱码的原因: 是因为tomcat默认字符集是ISO-8859-1,不支持中文

#### 中文乱码解决

说明:spring框架提供字符集编码的过滤器(CharachterEncodingFilter),解决 __post__ 请求的中文乱码<br>
在web.xml中,配置字符集编码过滤器:

```
<!-- 配置字符集编码过滤器 -->
  <filter>
  	<filter-name>encodingFilter</filter-name>
  	<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
  	
  	<!-- 指定使用的编码 -->
  	<init-param>
  		<param-name>encoding</param-name>
  		<param-value>UTF-8</param-value>
  	</init-param>
  </filter>
  
  <filter-mapping>
  	<filter-name>encodingFilter</filter-name>
  	<!-- 配置所有请求都经过字符集编码过滤器处理 -->
  	<url-pattern>/*</url-pattern>
  </filter-mapping>
```
## pojo包装参数类型

### 需求

使用pojo包装类型,接受请求的商品名称综合查询条件

![image](http://wx2.sinaimg.cn/mw690/71081ab7gy1fs5dj1go4pj20kg0b3wey.jpg)

### 需求实现

#### 编写商品pojo包装类型


```
package cn.itheima.ssm.po;

/** 
 * @ClassName: QueryVo 
 * @Description: pojo包装类型
 * @author 传智 小杨老师  
 * @date 2018-5-31 下午7:30:13 
 *  
 */
public class QueryVo {
	
	// 包装商品
	/**
	 * 商品名称：<input type="text" name="item.name" value=""/>
	 */
	private Item item;

	/**
	 * @return the item
	 */
	public Item getItem() {
		return item;
	}

	/**
	 * @param item the item to set
	 */
	public void setItem(Item item) {
		this.item = item;
	}

}
```

#### 在queryItem方法中,增加形参QueryVo


```
/**
	 * pojo包装参数类型讲解专用
	 * 
	 * 形参queryVo，接收请求的商品综合查询条件
	 */
	@RequestMapping("/queryItem.do")
	public ModelAndView queryItem(QueryVo queryVo){
		
		// 1.创建ModelAndView对象
		ModelAndView mav = new ModelAndView();

		// 查询数据库商品，替换静态商品数据
		List<Item>  itemList = itemService.queryItemList();
		
		mav.addObject("itemList", itemList);
		
		// 3.响应商品列表页面
		mav.setViewName("item/itemList");
		
		return mav;
		
		
	}
```

![image](http://wx2.sinaimg.cn/mw690/71081ab7gy1fs5dj25syej21350fm75z.jpg)

#### 中文get请求乱码解决

##### 方式一

说明:修改java代码,使用UTF-8编码重新编码参数数据

```
/**
	 * pojo包装参数类型讲解专用
	 * 
	 * 形参queryVo，接收请求的商品综合查询条件
	 */
	@RequestMapping("/queryItem.do")
	public ModelAndView queryItem(QueryVo queryVo){
		
		// get请求乱码解决=======================start
		try{
			// 1.获取参数数据
			String name = queryVo.getItem().getName();
			
			// 2.根据ISO-8859-1获取到参数的字节码
			byte[] bytes = name.getBytes("ISO-8859-1");
			
			// 3.使用UTF-8重新编码参数数据
			name = new String(bytes, "UTF-8");
			
			// 4.重新设置参数数据
			queryVo.getItem().setName(name);
		}catch(Exception e){
			e.printStackTrace();
		}
	
		
		// get请求乱码解决=======================end
		
		// 1.创建ModelAndView对象
		ModelAndView mav = new ModelAndView();

		// 查询数据库商品，替换静态商品数据
		List<Item>  itemList = itemService.queryItemList();
		
		mav.addObject("itemList", itemList);
		
		// 3.响应商品列表页面
		mav.setViewName("item/itemList");
		
		return mav;
		
		
	}
```
##### 方式二
说明:直接修改tomcat默认的字符集编码
<br>1.项目开发阶段

```
<build>
		<finalName>ssm</finalName>
		<plugins>
			<plugin>
				<groupId>org.apache.tomcat.maven</groupId>
				<artifactId>tomcat7-maven-plugin</artifactId>
				<version>2.1</version>
				<configuration>
					<!-- tomcat 的端口号 -->
					<port>8080</port>
					<!-- 访问应用的路径 -->
					<path>/ssm</path>
					<!-- URL按UTF-8进行编码，解决中文参数乱码 -->
					<uriEncoding>UTF-8</uriEncoding>
					<!-- tomcat名称 -->
					<server>tomcat7</server>
				</configuration>
			</plugin>
		</plugins>
	</build>
```
2.部署阶段


![image](http://wx4.sinaimg.cn/mw690/71081ab7gy1fs65ztcda9j20r109sjs0.jpg)
图二
![image](http://wx4.sinaimg.cn/mw690/71081ab7gy1fs5dj2m3ihj215k09gjuf.jpg)


```
<Connector URIEncoding="UTF-8" port="8081" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
```


## 自定义参数类型

说明:自定义参数类型解决比如日期类型的数据,格式多不固定,需要根据业务需求才能确定
### 自定义参数转换器
说明:spring框架提供了转换的标准,一个接口(Converter)

```
package cn.itheima.ssm.converter;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

import org.springframework.core.convert.converter.Converter;

/** 
 * @ClassName: DateConverter 
 * @Description: 自定义日期转换器 
 * @author 传智 小杨老师  
 * @date 2018-5-31 下午7:54:25 
 *  
 *  Converter<S, T>
 *  S,Source，源，转换之前的数据类型，这里是字符串String类型的日期
 *  T,Target，目标，转换之后的数据类型，这里是Date类型的日期
 */
public class DateConverter implements Converter<String, Date> {

	/**
	 * 转换方法
	 */
	public Date convert(String source) {
		// 1.定义转换格式对象(2016-02-03 13:22:53)
		SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
		
		try {
			// 转换成功直接返回
			return format.parse(source);
		} catch (ParseException e) {
			e.printStackTrace();
		}
		
		// 转换异常，返回null
		return null;
	}

}
```
### 配置自定义转换器

在springmvc.xml配置

```
<!-- 注解驱动方式配置处理器映射器和处理器适配器 -->
        <mvc:annotation-driven conversion-service="conversionService"/>
        
        <!-- 配置自定义转换服务 -->
        <bean id="conversionService" class="org.springframework.format.support.FormattingConversionServiceFactoryBean">
        	<property name="converters">
        		<set>
        			<bean class="cn.itheima.ssm.converter.DateConverter"/>
        		</set>
        	</property>
        </bean>
```

# springmvc与struts2比较

## 相同点
都是基于mvc的表现层框架,都用于web项目的开发
## 不同点

1. 前端控制器不一样.springmvc前端控制器是一个Servlet(DispatchServlet).struts2的前端控制器是一个filter(StrutsPreparedAndExecutorFilter)
2. 接受请求参数不一样.springmvc的通过处理器方法的形参接受请求的参数数据,是基于方法的开发,是线程安全的,可以设计为单例或者多例模式的开发.推荐使用单利模式的开发,原因是执行效率更高(默认就是是单例模式开发).struts2是使用类的成员变量接受请求的参数数据,是基于类的开发,是线程不安全的,只能设计为多例模式开发.
3. 与spring整合不一样.springmvc是spring框架的一部分,不需要整合




































