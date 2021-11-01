---
title: 后端|Spring事务控制
tags:
 - 后端
 - Spring
date: 2021-01-28 11:41:00
---



事务控制

<!--more-->

![1611580188412](C:\Users\ADMINI~1\AppData\Local\Temp\1611580188412.png)

```xml
<!--配置平台事务管理器-->
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"/>
</bean>

<!--通知 事务的增强-->
<tx:advice id="txAdvice" transaction-manager="transactionManager">
    <!--设置事务的属性信息-->
    <tx:attributes>
        <tx:method name="transfer"/>
        <tx:method name="*"/>
    </tx:attributes>
</tx:advice>

<!--配置事务的aop织入-->
<aop:config>
    <aop:advisor advice-ref="txAdvice" pointcut="execution(* service.impl.*.*(..))"/>
</aop:config>
```





#### Annotation配置

```xml
<!--配置平台事务管理器-->
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"/>
</bean>

<!--事务注解驱动-->
<tx:annotation-driven transaction-manager="transactionManager"/>
```

@Transactional 可以在类上也可以在方法上， 就近原则