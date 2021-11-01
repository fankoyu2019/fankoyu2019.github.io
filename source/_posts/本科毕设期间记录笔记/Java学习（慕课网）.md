---
title: 后端|Java学习（慕课网）
tags:
 - 后端
 - Java
date: 2021-01-28 8:55:00
---



### Java学习（慕课网）

#### Java入门第二季

<!--more-->

##### 输入流

```java
Scanner scanner = new Scanner(System.in);
int i = scanner.nextInt();
String str = scanner.next();
```

##### equals 方法 与 ==**（⭐）**

1）对于==，

如果作用于基本数据类型的变量，则直接比较其存储的 “值”是否相等；

如果作用于引用类型的变量，则比较的是所指向的对象的地址  　　

2）对于equals方法，注意：equals方法不能作用于基本数据类型的变量  

如果没有对equals方法进行重写，则比较的是引用类型的变量所指向的对象的地址；

诸如String、Date等类对equals方法进行了重写的话，比较的是所指向的对象的内容。 

```java
String s3=new String("imooc");
String s4=new String("imooc"); 
//true java会判断如果是相同字符串 则自动指向同一地址
System.out.println(s3.equals(s4));
```

```java
//false java不会对自定义的类对象进行优化使其指向同一地址
Course cr1 = new Course("1","C语言");
Course cr2 = new Course("1","C语言");

System.out.println(cr1.equals(cr2));
```



##### 包的命名规范：

域名后缀.组织机构名.项目名[.模块名].包的职能

##### 修饰符：

private 只能在当前类中使用

(default) 只能被包中其他类访问

protected继承类可以访问

![1610094788485](C:\Users\ADMINI~1\AppData\Local\Temp\1610094788485.png)

##### 封装：

封装即隐藏功能的实现细节

利用对象与方法是实现封装的直接途径

良好的封装让代码更容易阅读和维护

##### 继承：

default，protected，public 可以继承

final 关键字 表示该类型不可被继承

​	public final class  类名

super 子类访问父类属性和方法

方法重载 设计时多态

方法重写 运行时多态

抽象类

​	abstract

##### 多态：关键--接口(Interface)

设计时多态 （重载）

运行时多态（继承）

同一个行为具有多个不同的表现形式或形态的能力

#### Java入门第三季

##### try-catch-finally:

如果try catch finally 没有return 会调用最后的return

##### String 

String 具有不可变性



![img](http://img.mukewang.com/53d9f7d200010bb007780366.jpg) 

![img](http://img.mukewang.com/53a9260b0001808e06540410.jpg) 

StringBuilder 

StingBuffer 线程安全

![img](http://img.mukewang.com/53a7d34300011c6005970125.jpg) 

##### Java包装类

![img](http://img.mukewang.com/53abd8450001911703630223.jpg) 

##### Collection

​	List ->ArrayList,LinkedList

​	Queue ->LinkedList

​	Set ->HashSet

List

List<>   泛型 不能添加泛型规定的类型及其子类型以外的对象

1.泛型集合中的限定类型b不能使用基本数据类型

2.可以通过使用包装类限定允许存入的基本数据类型

##### Map

HashMap

遍历Map

​	通过keySet遍历  Set<> keySet = Map.keySet()

​	通过 entrySet方法遍历 返回Map所有键值对Set<Entry< , >> entrySet = Map.entrySet();

##### Collections.Sort

对象进行sort要实现Comparable接口

Comparable ---默认比较接口

​	其实现类需实现compareTo（）方法，compareTo（）方法返回正数表示大，负数表示小，0表示相等



Comparator ---临时比较接口

​	其实现类要实现compare（）方法

```
public class StudentCompartor implements Comparator<Student> {

    @Override
    public int compare(Student o1, Student o2) {
        return o1.getName().compareTo(o2.getName());
    }
}
```

```
Collections.sort(list,new StudentCompartor());
```

#### Maven项目依赖管理

##### Tomcat

端口8009 （不知道啥时候改成这个了！）**（⭐）**

#### Servlet基础

##### servlet 请求

![1610459806080](C:\Users\ADMINI~1\AppData\Local\Temp\1610459806080.png)

1.继承HttpServlet

2.重写doGet()方法或者doPost()方法

3.在web.xml中注册Servlet

```xml
<servlet>
  <servlet-name>HttpServlet</servlet-name>
  <servlet-class>ujn.edu.servlet.Servlet</servlet-class>
  <load-on-startup>1</load-on-startup> //加载优先级
</servlet>
<servlet-mapping>
  <servlet-name>HttpServlet</servlet-name>
  <url-pattern>/Servlet</url-pattern>
</servlet-mapping>
```

**或者直接在类上加注释@WebServlet("/Servlet")**



servlet类文件更新后 tomcat重新装载servlet



**jsp:useBean   class 属性必须有包名.类名**（⭐）



##### 路径跳转

```java
 @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //请求重定向方式跳转到test.jsp,当前路径是(我猜的)maven01/ujn/edu/servlet/
        //使用request.getContextPath获得上下文对象
//        resp.sendRedirect(req.getContextPath()+"/test.jsp");

        //服务器内部跳转
        req.getRequestDispatcher("/test.jsp").forward(req,resp);
//        req.getRequestDispatcher("test.jsp").forward(req,resp);
    }
```

##### 获取初始化参数

web.xml

```xml
<servlet>
<init-param>
  <param-name>username</param-name>
  <param-value>admin</param-value>
  <param-name>password</param-name>
  <param-value>123456</param-value>
</init-param>
</servlet>
```

##### MVC简介

![1610542237584](C:\Users\ADMINI~1\AppData\Local\Temp\1610542237584.png)

![1610546320068](C:\Users\ADMINI~1\AppData\Local\Temp\1610546320068.png)

![1610708657913](C:\Users\ADMINI~1\AppData\Local\Temp\1610708657913.png)

dao层：baidao层叫数据访问层，全称为data access object，属于一种比较底层，比较基础的操作，具体到对于某个表、某个实体的增删改查
service层：service层叫服务层，被称为服务，肯定是相比之下比较高层次的一层结构，相当于将几种操作封装起来。

#### Servlet报表

（⭐）Tomcat中 部署DBCP 相对路径 有问题 （TODO）





#### Java注解

##### Java中常见注解

java中自带注解

​	@Override 表示继承的父类方法

​	@Deprecated 表示方法过时 

​	@SuppressWarnings("deprecation") 忽略过时方法

第三方注解

Spring

​	@Autowired

​	@Service

​	@Repository

Mybatis

​	@InsertProvider

​	@UpdateProvider

​	@Options

##### 自定义注解

1.使用@interface 关键字定义注解

2.成员以无参无异常方式声明

3.可以用default为成员指定一个默认值

4.如果注解只有一个成员，则成员名必须取名为value(),在使用时可以忽略成员名和赋值号(=)

5.注解可以没有任何成员，这样就成为了标识注解

```java
@Target({ElementType.METHOD,ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Inherited
@Documented
public @interface Description {
    String desc();
    String author();
    int age() default 18;
}
```

//注意Inherited 继承父类 但不能继承接口 

解析注解

Test.java

```java
package zhujie;

@Description(desc = "I am class annotation",author = "fanko")
public class Test {
    @Description(desc = "I am method annotation",author = "fanko")
    public void test(){
    }
}
```

ParseAnn.java

```java
public class ParseAnn {
    public static void main(String[] args) {
        try {
            //1.使用类加载器加载类
            Class c = Class.forName("zhujie.Test");
            //2.找到类上面的注解
            boolean isExist = c.isAnnotationPresent(Description.class);
            if(isExist){
                //3.拿到注解实例
                Description d = (Description) c.getAnnotation(Description.class);
                System.out.println(d.desc());
            }

            //4.找到方法上的注解
            Method[] ms = c.getMethods();
            for(Method m: ms){
                boolean isMExist = m.isAnnotationPresent(Description.class);
                if(isMExist){
                    Description d = (Description) m.getAnnotation(Description.class);
                    System.out.println(d.desc());
                }
            }
            //另外一种解析方法
            for (Method m : ms) {
                Annotation[] as =m.getAnnotations();
                for (Annotation a : as) {
                    if(a instanceof Description){
                        Description d = (Description)a;
                        System.out.println(d.desc());
                    }
                }
            }
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }

    }
}
```

#### JDBC

##### 连接数据库

porm.xml

用5.1.6版本，注意用高版本会出现不知名错误**（⭐）**

```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.6</version>
</dependency>
```

DBUtil.java

```java
package util;

import java.sql.*;

public class DBUtil {
    private static final String URL="jdbc:mysql://127.0.0.1:3306/imooc";
    private static final String USER="root";
    private static final String PASSWORD="123456";
    private static Connection conn = null;
    static {
        try {
            //1.加载驱动程序
            Class.forName("com.mysql.jdbc.Driver");
            //2.获得数据库的连接
            conn = DriverManager.getConnection(URL, USER, PASSWORD);
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
    public static Connection getConnection(){
        return conn;
    }
}
```

//3.实现增删改查

```
PreparedStatement preparedStatement = connection.prepareStatement(sql);
preparedStatement.execute();
ResultSet resultSet = preparedStatement.executeQuery();
```

##### JDBC存储过程的调用

```java
//1.调用无参存储过程
Connection connection = DBUtil.getConnection();
CallableStatement cs = conn.prepareCall("call sp_select_nofilter()");
cs.execute();
ResultSet rs = cs.getResultSet();

//2.调用带输入参数的存储过程
//....
CallableStatement cs = conn.prepareCall("call sp_select_filter(?)");
cs.setString(1,sp_name);
//....

//3.调用带输出参数的存储过程
//....
CallableStatement cs = conn.prepareCall("call sp_select_count(?)");
cs.registerOutParameter(1,Types.INTEGER);
//cs.execute();
count = cs.getInt(1);
```

##### JDBC事务管理

事务（TRANSACTION）是作为单个逻辑工作单元执行的一系列操作，这些操作作为一个整体一起向系统提交，要么都执行，要么都不执行。

1.原子性(Atomicity):事务是一个完整的操作。

2.一致性(Consistency):当事务完成时，数据必须处于一致状态。

3.隔离性(Isolation):对数据进行修改的所有并发事务是彼此隔离的。

4.永久性(Durability):事务完成后，它对数据库的修改被永久保持。



JDBC对事务管理的支持

1.通过提交commit() 或 回退rollback() 

2.事务操作默认是自动提交

3.可以通过调用setAutoCommit(false)来禁止自动提交



##### JDBC连接池

为解决频繁的数据库连接压力

常用开源连接池

一、dbcp**（⭐）**

​	commons-dbcp2-2.1.1.jar

​	commons-pool2-2.4.2.jar

​	commons-logging-1.2.jar

2.在项目根目录增加配置文件     *IDEA中要放在resources文件下*

​	dbcp.properties

```
driverClassName=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/lab2?characterEncoding=UTF-8
username=root
password=123456
maxTotal=30
maxIdle=10
initialSize=5
//！！！前面不要加jdbc。。。
```

二、c3p0





![1610808375246](C:\Users\ADMINI~1\AppData\Local\Temp\1610808375246.png)

#### JAVA反射

1)静态成员，普通数据类型不是对象

2)类是对象，类是java.long,Class类的实例对象

```java
Foo foo1 = new Foo();
//任何一个类都是Class的实例对象，三种表示方式
//1.实际告诉我们任何一个类都有一个隐含的静态成员变量class
Class c1 = Foo.class;
//2.已知该类的对象通过getClass方法
Class c2 = foo1.getClass();
/* c1,c2 表示l了Foo类的类类型（class type）*/
//3.
Class c3 = null;
try{
c3 = Class.forName("com.imooc.reflect.Foo");
}catch(Expection e){
    e.printStackTrace();
}
//c1 == c2 == c3  true

//我们可以通过类的类类型创建该类的实例对象--->
Foo foo = (Foo) c1.newInstance()//需要有无参的构造方法
```

##### Class 类

3)编译时刻加载类是静态加载类、运行时刻加载类是动态加载类

//new 创建对象 是 静态加载类， 在编译时刻就需要加载所有可能使用到的类。

//通过动态加载类 解决

Class  c = Class.forName("类的全称");

4)基本数据类型，void关键字 都存在类类型

5）Class类的基本API操作

c.getName();

Method[] ms = c.getMethods();//获取所有的public的方法，包括父类继承而来的

c.getDeclaredMethods(); //获取所有该类自己声明的方法，不问访问权限

Class returnType = ms[i].getReturnType();//得到返回值类型的类类型

Class[] paramTypes = ms[i].getParameterTypes();//获取参数类型--->得到的是参数列表的类型的类类型

Field[] fs = c.getFields();//获取所有public 成员变量

c.getDeclaredFields();//获取自己声明的成员变量

Constructor[] cs = c.getConstructors();//获取所有public构造方法

Constructor[] cs = c.getDeclaredConstructors();

##### 方法的反射

方法的名称和方法的参数列表唯一决定某个方法

method.invoke(对象,参数列表)

Method m = c.getMethod("print",new Class[]{int.class,int.class});

Method m = c.getMethod("print",int.class,int.class);

方法的反射操作：用m对象来进行方法调用和a1.print调用效果完全相同

//方法如果没有返回值返回null,有返回值返回具体返回值

Object o =m.invoke(a1,new Object[]{10,20});

Object o = m.invoke(a1,10,20);

##### 通过Class，Method 来认识泛型的本质

![1610872983917](C:\Users\ADMINI~1\AppData\Local\Temp\1610872983917.png)

验证：我们可以通过方法的反射来操作，绕过编译

![1610873205553](C:\Users\ADMINI~1\AppData\Local\Temp\1610873205553.png)

#### Java Web过滤器（Servlet 2.5）已过时

过滤用户请求

过滤器是一个服务器端的组件，它可以截取用户端的请求与响应信息，并对这些信息过滤。

![1610965357869](C:\Users\ADMINI~1\AppData\Local\Temp\1610965357869.png)

![1610965420060](C:\Users\ADMINI~1\AppData\Local\Temp\1610965420060.png)

![1610971348411](C:\Users\ADMINI~1\AppData\Local\Temp\1610971348411.png)

```java
//重定向  地址会改变
response2.sendRedirect(req.getContextPath()+"/main.jsp");
//转发    地址不变
req.getRequestDispatcher("main.jsp").forward(request, response);
```

过滤器的分类

![1610974176443](C:\Users\ADMINI~1\AppData\Local\Temp\1610974176443.png)

##### Servlet (3.0) 直接注释

@WebFilter(filterName = "FirstFilter",value = {"/index.jsp"},dispatcherTypes = {DispatcherType.ASYNC,DispatcherType.REQUEST})

