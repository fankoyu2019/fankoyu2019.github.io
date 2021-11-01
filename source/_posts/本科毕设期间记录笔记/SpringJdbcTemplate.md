---
title: 后端|Spring|Spring JdbcTemplate
tags:
 - 后端
 - Spring
date: 2021-01-25 16:25:00
categories:
 - Spring
---



##### JdbcTemplate开发步骤

导入spring-jdbc和spring-tx坐标

创建数据库表和实体

创建JdbcTemplate对象

执行数据库操作

<!--more-->

xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd">
    <!--加载外部的properties文件-->
    <context:property-placeholder location="classpath:jdbc.properties"/>

    <!--数据源对象-->
    <bean id="dataSource" class="org.apache.commons.dbcp2.BasicDataSource">
        <property name="driverClassName" value="${jdbc.driver}"/>
        <property name="url" value="${jdbc.url}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
        <property name="maxTotal" value="${jdbc.maxTotal}"/>
        <property name="maxIdle" value="${jdbc.maxIdle}"/>
        <property name="initialSize" value="${jdbc.initialSize}"/>
    </bean>

    <!--jdbc模板对象-->
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <property name="dataSource" ref="dataSource"/>
    </bean>


</beans>
```



jdbc.properties

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/imooc?characterEncoding=utf-8
jdbc.username=root
jdbc.password=123456
#最大连接数
jdbc.maxTotal=30
#最大空闲连接
jdbc.maxIdle=10
#初始化连接数
jdbc.initialSize=5
```

