---
title: 后端|Spring入门
tags:
 - 后端
 - Spring
date: 2021-01-28 11:38:00
---



### Spring入门

#### IoC

IoC概念：Inversion of Control ，控制反转、依赖注入

1.控制什么?

控制对象的创建及销毁（生命周期） 

2.反转什么?

将对象的控制权交给IoC容器 

<!--more-->

自己的IoC容器

约定

1. 所有的Bean的生命周期交由IoC容器管理
2. 所有被依赖的Bean通过构造方法执行注入
3. 被依赖的Bean需要优先创建



Spring实例化Bean

SpringIoC 工程

![1611044567216](C:\Users\ADMINI~1\AppData\Local\Temp\1611044567216.png)

```xml
//1.
<bean id="bean1" class="springbean.Bean" name="bean1_1,bean1_2"/>
<alias name="bean1" alias="bean1_3"/>
//2.
<bean class="springbean.Bean2Factory" factory-method="getBean2" id="bean2"/>
//3.
<bean id="bean3Factory" class="springbean.Bean3Factory"/>
<bean id="bean3" class="springbean.Bean3" factory-bean="bean3Factory" factory-method="getBean3"/>
```

#### 注入Bean 

SpringIoC_02 工程

![1611050134411](C:\Users\ADMINI~1\AppData\Local\Temp\1611050134411.png)

![1611063818486](C:\Users\ADMINI~1\AppData\Local\Temp\1611063818486.png)

简单方法

![1611063846539](C:\Users\ADMINI~1\AppData\Local\Temp\1611063846539.png)

![1611063860986](C:\Users\ADMINI~1\AppData\Local\Temp\1611063860986.png)

**（⭐）**name 名字不是 成员变量名称，而是set方法 除set 外 把第一个字母大写变小写



![1611063904269](C:\Users\ADMINI~1\AppData\Local\Temp\1611063904269.png)

![1611063922157](C:\Users\ADMINI~1\AppData\Local\Temp\1611063922157.png)

![1611063999533](C:\Users\ADMINI~1\AppData\Local\Temp\1611063999533.png)

![1611064005534](C:\Users\ADMINI~1\AppData\Local\Temp\1611064005534.png)



##### 分模块开发（引入其他配置文件

```
< import resource="xxxx.xml"/ >
```



![1611323221721](C:\Users\ADMINI~1\AppData\Local\Temp\1611323221721.png)

![1611323636056](C:\Users\ADMINI~1\AppData\Local\Temp\1611323636056.png)





#### Bean作用域

scope

![1611064393704](C:\Users\ADMINI~1\AppData\Local\Temp\1611064393704.png)

单例模式singleton :在一个Spring上下文环境中，只存在一个实例

多例模式prototype: 每次向Spring上下文请求实例，拿到的都是全新的实例

![1611126498717](C:\Users\ADMINI~1\AppData\Local\Temp\1611126498717.png)

希望每次	调用Bean1的某个方法时，该方法拿到的Bean2都是一个新的实例

![1611126604527](C:\Users\ADMINI~1\AppData\Local\Temp\1611126604527.png)

![1611126998531](C:\Users\ADMINI~1\AppData\Local\Temp\1611126998531.png)

Web环境作用域

![1611127294421](C:\Users\ADMINI~1\AppData\Local\Temp\1611127294421.png)

![1611127330213](C:\Users\ADMINI~1\AppData\Local\Temp\1611127330213.png)

![1611127365137](C:\Users\ADMINI~1\AppData\Local\Temp\1611127365137.png)

![1611128872835](C:\Users\ADMINI~1\AppData\Local\Temp\1611128872835.png)

自定义作用域

![1611134243311](C:\Users\ADMINI~1\AppData\Local\Temp\1611134243311.png)

![1611134268479](C:\Users\ADMINI~1\AppData\Local\Temp\1611134268479.png)

![1611134287854](C:\Users\ADMINI~1\AppData\Local\Temp\1611134287854.png)

#### Bean懒加载

![1611134759453](C:\Users\ADMINI~1\AppData\Local\Temp\1611134759453.png)

![1611134835669](C:\Users\ADMINI~1\AppData\Local\Temp\1611134835669.png)

如果某个Bean在程序整个运行周期都可能不会被使用，那么可以考虑懒加载

优点：尽可能的节省了资源

缺点：可能会导致某个操作响应时间增加

##### Bean初始化

1.使用init-method

2.让Bean实现InitializingBean接口

##### Bean销毁

1.使用destroy-method

2.让Bean实现DisposableBean接口

##### Bean 属性继承

![1611136536611](C:\Users\ADMINI~1\AppData\Local\Temp\1611136536611.png)

![1611136555071](C:\Users\ADMINI~1\AppData\Local\Temp\1611136555071.png)

#### Spring注解**（⭐）**

![1611385413198](C:\Users\ADMINI~1\AppData\Local\Temp\1611385413198.png)

![1611393844834](C:\Users\ADMINI~1\AppData\Local\Temp\1611393844834.png)

xml

![1611147451399](C:\Users\ADMINI~1\AppData\Local\Temp\1611147451399.png)

annotation

![1611147551325](C:\Users\ADMINI~1\AppData\Local\Temp\1611147551325.png)

简化

![1611149484897](C:\Users\ADMINI~1\AppData\Local\Temp\1611149484897.png)

![1611149598565](C:\Users\ADMINI~1\AppData\Local\Temp\1611149598565.png)



##### Bean取别名

![1611149710827](C:\Users\ADMINI~1\AppData\Local\Temp\1611149710827.png)

##### 

##### 通过注解注入Bean

![1611205600710](C:\Users\ADMINI~1\AppData\Local\Temp\1611205600710.png)



![1611205739708](C:\Users\ADMINI~1\AppData\Local\Temp\1611205813703.png)

![1611205881001](C:\Users\ADMINI~1\AppData\Local\Temp\1611205881001.png)

![1611205913594](C:\Users\ADMINI~1\AppData\Local\Temp\1611205913594.png)

![1611206122916](C:\Users\ADMINI~1\AppData\Local\Temp\1611206122916.png)

简单类型直接赋值：@Value("aaaa")直接将aaaa注入到string

private String string;

![1611206262577](C:\Users\ADMINI~1\AppData\Local\Temp\1611206262577.png)

![1611216849636](C:\Users\ADMINI~1\AppData\Local\Temp\1611216849636.png)

![1611216888776](C:\Users\ADMINI~1\AppData\Local\Temp\1611216888776.png)

![1611217405098](C:\Users\ADMINI~1\AppData\Local\Temp\1611217405098.png)



初始化和销毁

![1611217945625](C:\Users\ADMINI~1\AppData\Local\Temp\1611217945625.png)

![1611217894842](C:\Users\ADMINI~1\AppData\Local\Temp\1611217894842.png)

![1611217920925](C:\Users\ADMINI~1\AppData\Local\Temp\1611217920925.png)

##### Spring集成Junit


导入spring集成Junit的坐标,
使用@Runwith注解替换原来的运行期
使用@contextconfiguration指定配置文件或配置类
使用@Autowired注入需要测试的对象
创建测试方法进行测试

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:applicationContext.xml")
public class jdbcTemplateCRUDTest {
    @Autowired
    JdbcTemplate jdbcTemplate;
}
```
