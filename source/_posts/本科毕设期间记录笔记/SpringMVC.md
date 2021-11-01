---
title: 后端|SpringMVC
tags:
 - 后端
 - Spring
date: 2021-03-06 19:29:00
categories:
 - Spring
---



### Spring集成Web环境

1.配置ContextLoaderListener监听器

<!--more-->

```xml
<!--全局初始化参数-->
<context-param>
  <param-name>contextConfigLocation</param-name>
  <param-value>classpath:applicationContext.xml</param-value>
</context-param>
<!--配置监听器-->
<listener>
  <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
```

2.使用WebApplicationContextUtils获得应用上下文

### SpringMVC

***<u>F:\java_code\heima_SpringMVC</u>***

SpringMVC开发步骤

![1611656050360](C:\Users\ADMINI~1\AppData\Local\Temp\1611656050360.png)

需求:客户端发起请求，服务器端接受请求，执行逻辑并进行试图跳转。

1.导入SpringMVC相关坐标

2.配置SpringMVC核心控制器DispatcherServlet**（⭐）**

web.xml

```xml
<servlet>
  <servlet-name>DispatcherServlet</servlet-name>
  <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  <init-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:spring-mvc.xml</param-value>
  </init-param>
  <load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
  <servlet-name>DispatcherServlet</servlet-name>
  <url-pattern>/</url-pattern>
</servlet-mapping>
```

3.创建Controller类和视图界面

4.使用注解配置Controller类中业务方法的映射地址

5.配置SpringMVC核心文件spring-mvc.xml

6.客户端发起请求测试 



![1611737421344](C:\Users\ADMINI~1\AppData\Local\Temp\1611737421344.png)

1.用户发送请求至前端控制器DispatcherServlet。

2.DispatcherServlet收到请求调用HandlerMapping处理器映射器。

3.处理器映射器找到具体的处理器(可以根据xml配置、注解进行查找)，生成处理器对象及处理器拦截器(如果

有则生成)一并返回给DispatcherServlet.

4.DispatcherServlet调用HandlerAdapter处理器适配器。

5.HandlerAdapter经过适配调用具体的处理器(Controller，也叫后端控制器)。

6.Controller执行完成返回ModelAndView。

7.HandlerAdapter将controller执行结果ModelAndView返回给DispatcherServlet.

8.DispatcherServlet将ModelAndView传给ViewReslover视图解析器。

9.ViewReslover解析后返回具体View。

10.DispatcherServlet根据View进行渲染视图(即将模型数据填充至视图中)。DispatcherServlet响应用户。



#### xml试图解析器

```xml
<!--配置内部资源视图解析器-->
<bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <!--/jsp/success.jsp-->
    <property name="prefix" value="/jsp/"/>
    <property name="suffix" value=".jsp"/>
</bean>
```

重定向前缀：redirect:

转发前缀(默认值)：forward:



#### SpringMVC的数据响应方式

##### 1）页面跳转

直接返回字符串

同上

Controller 层 部分代码

```java
@RequestMapping(value = "/quick",method = RequestMethod.GET)
public String save(){
    System.out.println("UserController.save");
    return "success";
}
```

通过ModelAndView对象返回

```java
//1.
@RequestMapping(value = "/quick2")
public ModelAndView save2(){
    /*
        Model:模型 封装数据
        View:视图 展示数据
     */
    ModelAndView modelAndView = new ModelAndView();
    //设置模型数据
    modelAndView.addObject("username","itcast");
    //设置视图名称
    modelAndView.setViewName("success");
    return modelAndView;
}
//2.
@RequestMapping(value = "/quick3")
public ModelAndView save3(ModelAndView modelAndView){
    modelAndView.addObject("username","itheima");
    modelAndView.setViewName("success");
    return modelAndView;
}
//3.
@RequestMapping(value = "/quick4")
public String save4(Model model){
    model.addAttribute("username","save4");
    return "success";
}
//4.    不建议。不常用
@RequestMapping(value = "/quick5")
public String save5(HttpServletRequest request){
    request.setAttribute("username","save5");
    return "success";
}
```

##### 2）回写数据

直接返回字符串

```java
@RequestMapping(value = "/quick6")
public void save6(HttpServletResponse response) throws IOException {
    response.getWriter().println("hello itcast");
}
@RequestMapping(value = "/quick7")
@ResponseBody//告知SpringMVC框架 该方法不进行视图跳转，直接进行数据响应
public String save7() throws IOException {
    return "hello save7";
}
@RequestMapping(value = "/quick8")
@ResponseBody//告知SpringMVC框架 该方法不进行视图跳转，直接进行数据响应
public String save8() throws IOException {
    return "{\"username\":\"zhangsan\",\"age\":18}";
}
@RequestMapping(value = "/quick9")
@ResponseBody//告知SpringMVC框架 该方法不进行视图跳转，直接进行数据响应
public String save9() throws IOException {
    User user = new User();
    user.setName("lili");
    user.setAge(30);
    //使用JSON的转换工具，将对象转换成JSON格式字符串 再返回
    ObjectMapper objectMapper = new ObjectMapper();
    String json = objectMapper.writeValueAsString(user);
    return json;
}
```

返回对象和集合

pom.xml

```xml
<dependency>
  <groupId>com.fasterxml.jackson.core</groupId>
  <artifactId>jackson-core</artifactId>
  <version>2.9.8</version>
</dependency>
<dependency>
  <groupId>com.fasterxml.jackson.core</groupId>
  <artifactId>jackson-databind</artifactId>
  <version>2.9.8</version>
</dependency>
<dependency>
  <groupId>com.fasterxml.jackson.core</groupId>
  <artifactId>jackson-annotations</artifactId>
  <version>2.9.8</version>
</dependency>
```

Controller

```java
@RequestMapping(value = "/quick10")
@ResponseBody//告知SpringMVC框架 该方法不进行视图跳转，直接进行数据响应
//期望SpringMVC自动将User转换成一个JSON格式的字符串 (配置)
public User save10() throws IOException {
    User user = new User();
    user.setName("lili");
    user.setAge(30);
    return user;
}
```

spring-mvc.xml

```xml
<!--配置处理器映射器-->
<!-- <bean id="" class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">
     <property name="messageConverters">
         <list>

             <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter"/>
         </list>
     </property>
 </bean>-->

 <!--mvc注解驱动-->
 <mvc:annotation-driven/>
```


#### SpringMVC获得请求参数



##### 基本类型参数

Controller中的业务方法的参数名称与请求参数的name一致，参数值会自动映射匹配

![1611820502814](C:\Users\ADMINI~1\AppData\Local\Temp\1611820502814.png)

```java
@RequestMapping(value = "/quick11")
@ResponseBody//告知SpringMVC框架 该方法不进行视图跳转，直接进行数据响应
public void save11(String username,Integer age) throws IOException {
    System.out.println("username = " + username);
    System.out.println("age = " + age);
}
```

##### POJO类型参数(简单JavaBean)

Controller中的业务方法的POJO参数与请求参数的name一致，参数值会自动映射匹配

![1611820747248](C:\Users\ADMINI~1\AppData\Local\Temp\1611820747248.png)

```java
@RequestMapping(value = "/quick12")
@ResponseBody//告知SpringMVC框架 该方法不进行视图跳转，直接进行数据响应
public void save12(User user) throws IOException {
    System.out.println("user = " + user);
}
```

##### 数组类型参数

Controller中的业务方法的数组名称与请求参数的name一致，参数值会自动映射匹配

![1611820916874](C:\Users\ADMINI~1\AppData\Local\Temp\1611820916874.png)

```java
@RequestMapping(value = "/quick13")
@ResponseBody//告知SpringMVC框架 该方法不进行视图跳转，直接进行数据响应
public void save13(String[] strings) throws IOException {
    System.out.println("Arrays.asList(strings) = " + Arrays.asList(strings));
}
```

##### 集合类型参数（1）

![1611821783523](C:\Users\ADMINI~1\AppData\Local\Temp\1611821783523.png)

Controller

```java
@RequestMapping(value = "/quick14")
@ResponseBody//告知SpringMVC框架 该方法不进行视图跳转，直接进行数据响应
public void save14(VO vo) throws IOException {
    System.out.println("vo = " + vo);
}
```

VO.java

```java
public class VO {
    private List<User> userList;

    public List<User> getUserList() {
        return userList;
    }

    public void setUserList(List<User> userList) {
        this.userList = userList;
    }

    @Override
    public String toString() {
        final StringBuffer sb = new StringBuffer("VO{");
        sb.append("userList=").append(userList);
        sb.append('}');
        return sb.toString();
    }
}
```



##### 集合类型参数（2）

当使用ajax提交时，可以指定contentType为json形式，那么在方法参数位置使用@RequestBody可以直接接收集合数据而无需使用POJO进行包装。

ajax.jsp

```jsp
<script src="${pageContext.request.contextPath}/js/jquery-3.3.1.js"></script>
<script>
   var userList = new Array();
   userList.push({username:"zhangsan",age:18});
   userList.push({username:"lisi",age:28});

   $.ajax({
      type:"POST",
      url:"${pageContext.request.contextPath}/user/quick15",
      data:JSON.stringify(userList),
      contentType:"application/json;charset=utf-8" 
   });

</script>
```

Controller

```java
@RequestMapping(value = "/quick15")
@ResponseBody//告知SpringMVC框架 该方法不进行视图跳转，直接进行数据响应
public void save15(@RequestBody List<User> userList) throws IOException {
    System.out.println("userList = " + userList);
}
```

spring-mvc.xml

```xml
<mvc:resources mapping="/js/**" location="/js/"/>
```

#### 静态资源访问的开启**（⭐）**

```xml
<!--开放资源的访问  1.-->
<!--<mvc:resources mapping="/js/**" location="/js/"/>
<mvc:resources mapping="/img/**" location="/img/"/>-->
<!--              2. -->
<mvc:default-servlet-handler/>
```

#### 请求数据乱码问题

web.xml

```xml
<!--配置全局过滤filter-->
<filter>
  <filter-name>CharacterEncodingFilter</filter-name>
  <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
  <init-param>
    <param-name>encoding</param-name>
    <param-value>utf-8</param-value>
  </init-param>
</filter>
<filter-mapping>
  <filter-name>CharacterEncodingFilter</filter-name>
  <url-pattern>/*</url-pattern>
</filter-mapping>
```

#### 参数绑定注解@RequestParam

当请求的参数名称与Controller的业务方法参数名称不一致时，就需要通过@RequestParam注解显式的绑定

```java
@RequestMapping(value = "/quick16")
@ResponseBody//告知SpringMVC框架 该方法不进行视图跳转，直接进行数据响应
public void save16(@RequestParam(value = "name") String username) throws IOException {
    System.out.println("username = " + username);
}

//localhost:8009/heima_Spring_MVC/user/quick17/zhangsan
@RequestMapping(value = "/quick17/{username}")
@ResponseBody//告知SpringMVC框架 该方法不进行视图跳转，直接进行数据响应
public void save17(@PathVariable(value = "username") String username) throws IOException {
    System.out.println("username = " + username);
}
```

#### 获取Restful风格的参数

Restful是一种软件架构风格、设计风格，而不是标准，只是提供了一组设计原则和约束条件。主要用于客户端和服务器交互类的软件，基于这个风格设计的软件可以更简洁，更有层次，更易于实现缓存机制等。

Restful风格的请求是使用“url+请求方式”表示一次请求目的的，HTTP协议里面四个表示操作方式的动词如下:

GET:用于获取资源

POST:用于新建资源

PUT:用于更新资源

DELETE:用于删除资源

![1611830382522](C:\Users\ADMINI~1\AppData\Local\Temp\1611830382522.png)



#### 自定义类型转换器

SpringMVC默认已经提供了一些常用的类型转换器，例客户端提交的字符串转换成int型进行参数设置。

但不是所有的数据类型都提供了转换器，没有提供的就需要自定义转换器，例如:日期类型的数据就需要自定义转换器。

自定义类型转换器的开发步骤:

定义转换器类实现Converter接口

在配置文件中声明转换器

在\< annotation-driven >中引用转换器



```java
import org.springframework.core.convert.converter.Converter;
public class DateConverter implements Converter<String, Date> {
    @Override
    public Date convert(String datestr) {
        //将日期字符串转换成真正的日期对象， 返回
        SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd");
        Date parse = null;
        try {
            parse = format.parse(datestr);
        } catch (ParseException e) {
            e.printStackTrace();
        }
        return parse;
    }
}
```

spring-mvc.xml

```xml
<!--声明自定义转换器-->
<bean id="ConversionService" class="org.springframework.context.support.ConversionServiceFactoryBean">
    <property name="converters">
        <list>
            <bean class="converter.DateConverter"/>
        </list>
    </property>
</bean>

<!--mvc注解驱动-->
    <mvc:annotation-driven conversion-service="ConversionService"/>
```

#### 获得请求头

@RequestHeader

使用@RequestHeader可以获得请求头信息，相当于web阶段学习的request.getHeader(name)

@RequestHeader注解的属性如下:
value:请求头的名称
required:是否必须携带此请求头



@CookieValue
使用@CookieValue可以获得指定Cookie的值@CookieValue注解的属性如下:
value:指定cookie的名称·
required:是否必须携带此cookie



#### 文件上传

1.三要素

表单项type = "file"

表单的提交方式post

表单的enctype属性是多部分表单形式，及enctype= "multipart/form-data"

```jsp
<form action="${pageContext.request.contextPath}/user/quick22" 
   method="post" enctype="multipart/form-data" >
名称<input type="text" name="username"><br/>
文件<input type="file" name="upload"><br/>
    <input type="submit" value="提交">
  </form>
```

2.文件上传原理
当form表单修改为多部分表单时，request.getParameter0将失效。
enctype= "application/x-www-form-urlencoded”时，form表单的正文内容格式是:key=value&key=value&key=value
当form表单的enctype取值为Mutilpart/form-data时，请求正文内容就变成多部分形式:

![1611842915192](C:\Users\ADMINI~1\AppData\Local\Temp\1611842915192.png)



##### 单文件上传步骤

1.导入fileupload和io坐标

```xml
<dependency>
  <groupId>commons-fileupload</groupId>
  <artifactId>commons-fileupload</artifactId>
  <version>1.3.3</version>
</dependency>
<dependency>
  <groupId>commons-io</groupId>
  <artifactId>commons-io</artifactId>
  <version>2.6</version>
</dependency>
```

2.配置文件上传解析器

```xml
<!--配置文件上传解析器-->
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
    <property name="defaultEncoding" value="utf-8"/>
    <property name="maxUploadSize" value="5242800"/>
</bean>
```

3.编写文件上传代码

```java
@RequestMapping(value = "/quick22")
@ResponseBody//告知SpringMVC框架 该方法不进行视图跳转，直接进行数据响应
public void save22(String username, MultipartFile upload) throws IOException{
    System.out.println("username = " + username);
    //获得上传文件名称
    String filename = upload.getOriginalFilename();
    upload.transferTo(new File("F:\\java_code\\"+filename));
}
```

##### 多文件上传

```java
@RequestMapping(value = "/quick22")
@ResponseBody//告知SpringMVC框架 该方法不进行视图跳转，直接进行数据响应
public void save22(String username, MultipartFile upload,MultipartFile upload2) throws IOException{
    System.out.println("username = " + username);
    //获得上传文件名称
    String filename = upload.getOriginalFilename();
    upload.transferTo(new File("F:\\java_code\\"+filename));

    String filename2 = upload2.getOriginalFilename();
    upload.transferTo(new File("F:\\java_code\\"+filename2));
}
```

也可for循环   ----->MultipartFile[] upload



#### SpringMVC拦截器

***<u>F:\java_code\heima_SpringMVC_interceptor</u>***

拦截器（interceptor）的作用

SpringMVC类似于Servlet开发中的过滤器Filter，用于对处理器进行预处理和后处理。

将拦截器按一定的顺序联结成一条链，这条链称为拦截器链(Interceptor Chain)。在访问被拦截的方法或字段时，拦截器链中的拦截器就会按其之前定义的顺序被调用。拦截器也是AOP思想的具体实现。

![1611890837649](C:\Users\ADMINI~1\AppData\Local\Temp\1611890837649.png)

步骤

1.创建拦截器类实现HandlerInterceptor接口

```java
public class MyInterceptor1 implements HandlerInterceptor {
    @Override
    //在目标方法执行之前 执行
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("MyInterceptor1.preHandle");
        return true;
    }

    @Override
    //在目标方法执行之后 视图返回之前执行
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("MyInterceptor1.postHandle");
    }

    @Override
    //在整个流程都执行完毕后 执行
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("MyInterceptor1.afterCompletion");
    }
}
```

2.配置拦截器

```xml
<!--配置拦截器-->
<mvc:interceptors>
    <mvc:interceptor>
        <!--对哪些资源执行拦截操作-->
        <mvc:mapping path="/**"/>
        <bean class="interceptor.MyInterceptor1"/>
    </mvc:interceptor>
</mvc:interceptors>
```

3.测试拦截器的拦截结果



拦截器方法说明

![1611904726727](C:\Users\ADMINI~1\AppData\Local\Temp\1611904726727.png)





#### SpringMVC异常处理

方法1.使用SpringMVC提供的简单异常处理器SimpleMappingExceptionResolver

![1612017877468](C:\Users\ADMINI~1\AppData\Local\Temp\1612017877468.png)

方法2.实现Spring的异常处理接口HandlerExceptionResolver自定义自己的异常处理器

1.创建异常处理器实现HandlerExceptionResolver接口

```java
public class MyExceptionResolver implements HandlerExceptionResolver {
    @Override
    /*
        参数Exception :异常对象
        返回值ModelAndView     ： 跳转到错误视图信息
     */
    public ModelAndView resolveException(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, Exception e) {
        ModelAndView modelAndView = new ModelAndView();
        if(e instanceof MyException){
            modelAndView.addObject("info","自定义异常");

        }
        else if(e instanceof ClassCastException){
            modelAndView.addObject("info","类转换异常");
        }
        modelAndView.setViewName("error");
        return modelAndView;
    }
}
```

2.配置异常处理器

```xml
<!--配置自定义异常处理器-->
<bean class="resolver.MyExceptionResolver"/>
```

3.编写异常页面

