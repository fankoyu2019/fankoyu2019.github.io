---
title: 后端|Spring整合Mybatis
tags:
 - 后端
 - Spring
 - Mybatis
date: 2021-02-08 11:38:00
categories:
 - Spring  
---





AccountServiceImpl.java

<!--more-->

```java
@Service("accountService")
public class AccountServiceImpl implements AccountService {
    @Override
    public void save(Account account) {
        InputStream resourceAsStream = null;
        try {
            resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
            SqlSessionFactory build = new SqlSessionFactoryBuilder().build(resourceAsStream);
            SqlSession sqlSession = build.openSession(true);
            AccountMapper mapper = sqlSession.getMapper(AccountMapper.class);
            mapper.save(account);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    @Override
    public List<Account> findAll()  {
        InputStream resourceAsStream = null;
        try {
            resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
            SqlSessionFactory build = new SqlSessionFactoryBuilder().build(resourceAsStream);
            SqlSession sqlSession = build.openSession(true);
            AccountMapper mapper = sqlSession.getMapper(AccountMapper.class);
            List<Account> all = mapper.findAll();
            return all;
        } catch (IOException e) {
            e.printStackTrace();
        }
        return null;
    }
}
```



![1612427203943](C:\Users\ADMINI~1\AppData\Local\Temp\1612427203943.png)

applicationContext.xml

```xml
<!--加载properties文件-->
<context:property-placeholder location="classpath:jdbc.properties"/>

<!--配置数据源-->
<bean id="dataSource" class="org.apache.commons.dbcp2.BasicDataSource">
    <property name="driverClassName" value="${jdbc.driver}"/>
    <property name="url" value="${jdbc.url}"/>
    <property name="username" value="${jdbc.username}"/>
    <property name="password" value="${jdbc.password}"/>
    <property name="maxTotal" value="${jdbc.maxTotal}"/>
    <property name="maxIdle" value="${jdbc.maxIdle}"/>
    <property name="initialSize" value="${jdbc.initialSize}"/>
</bean>

<!--配置sessionFactory-->
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="dataSource" ref="dataSource"/>
    <!--加载mybatis核心文件-->
    <property name="configLocation" value="sqlMapConfig-spring.xml"/>
</bean>

<!--扫描mapper所在的包 为mapper创建实现类-->
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
    <property name="basePackage" value="mapper"/>
</bean>

```



AccountServiceImpl.java

```java
@Service("accountService")
public class AccountServiceImpl implements AccountService {

    @Autowired
    private AccountMapper accountMapper;

    @Override
    public void save(Account account) {
        accountMapper.save(account);
    }

    @Override
    public List<Account> findAll()  {
        List<Account> all = accountMapper.findAll();
        return all;
    }
}
```