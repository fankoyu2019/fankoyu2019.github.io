---
title: 后端|Mybatis
tags:
 - 后端
 - Mybatis
date: 2021-02-03 8:55:00
categories:
 - Mybatis
---



1.添加Mybatis坐标

```xml
<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis</artifactId>
  <version>3.5.4</version>
</dependency>
```

<!--more-->

2.创建user数据表

3.编写User实体类

4.编写映射文件UserMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="userMapper">

    <select id="findAll" resultType="domain.User">
        select * from user
    </select>

</mapper>
```

5.编写核心文件SqlMapConfig.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--配数据源环境-->
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"></transactionManager>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/user?characterEncoding=utf-8"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>
    </environments>

    <!--加载映射文件-->
    <mappers>
        <mapper resource="mapper/UserMapper.xml"/>
    </mappers>
</configuration>
```

6.编写测试类

```java
@Test
public void test1() throws IOException {
    //获得核心配置文件
    InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
    //获得session工厂对象
    SqlSessionFactory sessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
    //获得session回话对象
    SqlSession sqlSession = sessionFactory.openSession();
    //执行操作 参数：namespace + id
    List<User> userList = sqlSession.selectList("userMapper.findAll");
    //打印数据
    System.out.println(userList);
    //释放资源
    sqlSession.close();
}
```



##### 插入操作注意问题·插入语句使用insert标签

在映射文件中使用parameterType属性指定要插入的数据类型.

 Sql语句中使用#{实体属性名}方式引用实体中的属性值

插入操作使用的API是sqlSession.insert(“命名空间.id”,实体对象);

插入操作涉及数据库数据变化，所以要使用sqISession对象显示的提交事务，

即sqlSession.commit()



#### Mybatis核心配置文件

1.environments标签
其中，事务管理器(transactionManager)类型有两种:
JDBC:这个配置就是直接使用了JDBC的提交和回滚设置，它依赖于从数据源得到的连接来管理事务作用域。
MANAGED:这个配置几乎没做什么。它从来不提交或回滚一个连接，而是让容器来管理事务的整个生命周期（(比如JEE应用服务器的上下文)。默认情况下它会关闭连接，然而一些容器并不希望这样，因此需要将closeConnection属性设置为false来阻止它默认的关闭行为。

其中，数据源(dataSource）类型有三种:
UNPOOLED:这个数据源的实现只是每次被请求时打开和关闭连接。
POOLED:这种数据源的实现利用“池”的概念将JDBC连接对象组织起来。
JNDI:这个数据源的实现是为了能在如EJB或应用服务器这类容器中使用，容器可以集中或在外部配置数据源，然后放置一个JNDI上下文的引用。

![1612086812105](C:\Users\ADMINI~1\AppData\Local\Temp\1612086812105.png)

3.properties

```xml
<!--通过properties标签加载外部properties文件-->
<properties resource="jdbc.properties"/>
```

4.typeAliases标签

![1612087121874](C:\Users\ADMINI~1\AppData\Local\Temp\1612087121874.png)

```xml
<!--自定义别名-->
<typeAliases>
    <typeAlias type="domain.User" alias="user"/>
</typeAliases>
```



#### Mabatis 的dao层实现

![1612090972324](C:\Users\ADMINI~1\AppData\Local\Temp\1612090972324.png)

![1612098382935](C:\Users\ADMINI~1\AppData\Local\Temp\1612098382935.png)

#### 动态sql 

##### if

```xml
<select id="findByCondition" resultType="user" parameterType="user">
    select * from user
    <where>
        <if test="id!=0">
            and id = #{id}
        </if>
        <if test="username!=null">
            and username = #{username}
        </if>
        <if test="password!=null">
            and password = #{password}
        </if>
    </where>
</select>
```

##### foreach

```xml
<select id="findByIds" resultType="user" parameterType="list">
    select * from user
    <where>
        <foreach collection="list" open="id in (" close=")" item="id" separator=",">
            #{id}
        </foreach>
    </where>
</select>
```

##### sql语句抽取

```xml
<!--sql语句抽取-->
<sql id="selectUser">select * from user</sql>



<select id="findByCondition" resultType="user" parameterType="user">
        <include refid="selectUser"></include>
```

#### Mybatis核心配置文件

![1612184130364](C:\Users\ADMINI~1\AppData\Local\Temp\1612184130364.png)

![1612185599369](C:\Users\ADMINI~1\AppData\Local\Temp\1612185599369.png)

```
<dependency>
  <groupId>com.github.pagehelper</groupId>
  <artifactId>pagehelper</artifactId>
  <version>3.7.5</version>
</dependency>
<dependency>
  <groupId>com.github.jsqlparser</groupId>
  <artifactId>jsqlparser</artifactId>
  <version>0.9.1</version>
</dependency>
```

#### 一对一

```xml
<resultMap id="orderMap" type="order">
    <!--手动指定字段与实体属性的映射关系
        column : 数据表的字段名称
        property: 实体的属性名称
        -->
    <id column="oid" property="id"></id>
    <result column="ordertime" property="ordertime"></result>
    <result column="total" property="total"></result>
    <!--<result column="uid" property="user.id"></result>
    <result column="username" property="user.username"></result>
    <result column="Password" property="user.password"></result>
    <result column="birthday" property="user.birthday"></result>-->
    <!--
        property：当前实体(order)中的属性名称(private User user)
        javaType: 当前实体(order)中的属性的类型(User)
    -->
    <association property="user" javaType="domain.User">
        <id column="uid" property="id"/>
        <result column="username" property="username"/>
        <result column="Password" property="password"/>
        <result column="birthday" property="birthday"/>
    </association>
</resultMap>

<select id="findAll" resultMap="orderMap" >
    select * ,o.id oid from orders o ,user u where o.uid = u.id
</select>
```

#### 一对多

```xml
<resultMap id="userMap" type="user">
    <id column="uid" property="id"/>
    <result column="username" property="username"/>
    <result column="Password" property="password"/>
    <result column="birthday" property="birthday"/>

    <!--配置集合信息
        property:集合名称
        ofType:当前集合中的数据类型
    -->
    <collection property="orderList" ofType="order">
        <!--封装order的数据-->
        <id column="oid" property="id"/>
        <result column="ordertime" property="ordertime"/>
        <result column="total" property="total"/>
    </collection>
</resultMap>

<select id="findAll" resultMap="userMap" >
    select *,o.id oid from user u,orders o where u.id = o.uid
</select>
```

#### 多对多

```xml
<resultMap id="userRoleMap" type="user">
    <!--user的信息-->
    <id column="userId" property="id"/>
    <result column="username" property="username"/>
    <result column="password" property="password"/>
    
    <!--user内部的roleList的信息-->
    <collection property="roleList" ofType="domain.Role">
        <id column="roleId" property="id"/>
        <result column="roleName" property="roleName"/>
        <result column="roleDesc" property="roleDesc"/>
    </collection>
</resultMap>
<select id="findUserAndRoleAll" resultMap="userRoleMap">
    select * from sys_user u,sys_user_role ur ,sys_role r where u.id = ur.userId AND ur.roleId=r.id
</select>
```
### 注解开发

![1612341706528](C:\Users\ADMINI~1\AppData\Local\Temp\1612341706528.png)

![1612344455486](C:\Users\ADMINI~1\AppData\Local\Temp\1612344455486.png)

![1612344473894](C:\Users\ADMINI~1\AppData\Local\Temp\1612344473894.png)

