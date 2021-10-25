---
title: 后端|Spring|SpringAOP
tags:
 - 后端
 - Spring
date: 2021-01-24 22:53:00
categories:
 - Spring
---



### SpringAOP

Aspect Oriented Programming **面向切面编程**，是通过预编译方式和运行期间动态代理实现程序功能的统一维护的一种技术。

<!--more-->

作用：在程序运行期间，在不修改源码的情况下对方法进行功能增强

![1611462949509](C:\Users\ADMINI~1\AppData\Local\Temp\1611462949509.png)



JDKProxy

![1611477433382](C:\Users\ADMINI~1\AppData\Local\Temp\1611477433382.png)

```java
@Test
public void test(){
    //目标对象
    final TargetInterfaceImpl target = new TargetInterfaceImpl();
    //增强对象
    final Advice advice = new Advice();

    //返回值 就是动态生成的代理对象
    TargetInterface proxy = (TargetInterface) Proxy.newProxyInstance(
            target.getClass().getClassLoader(),//目标对象类加载器
            target.getClass().getInterfaces(),//目标对象相同的接口字节码对象数组
            new InvocationHandler() {

                //调用代理对象的任何方法 实质执行的都是invoke方法
                public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                    advice.before();//前置增强
                    Object invoke = method.invoke(target, args);//执行目标方法
                    advice.afterReturning();//后置增强
                    return invoke;
                }
            }
    );

    // 调用代理对象的方法
    proxy.save();
}
```



CglibProxy

![1611477443403](C:\Users\ADMINI~1\AppData\Local\Temp\1611477443403.png)

```java
@Test
public void test(){
    //目标对象
    final Target target = new Target();
    //增强对象
    final Advice advice = new Advice();

    //返回值 就是动态生成的代理对象 基于cglib
    //1.创建增强器
    Enhancer enhancer = new Enhancer();
    //2.设置父类（目标）
    enhancer.setSuperclass(Target.class);
    //3.设置回调
    enhancer.setCallback(new MethodInterceptor() {
        public Object intercept(Object o, Method method, Object[] args, MethodProxy methodProxy) throws Throwable {
            advice.before();//前置增强
            Object invoke = method.invoke(target, args);//执行目标方法
            advice.afterReturning();//后置增强
            return invoke;
        }
    });
    //4.创建代理对象
    Target proxy = (Target) enhancer.create();
    proxy.save();
}
```



##### AOP相关概念

Target(目标对象)︰代理的目标对象

Proxy(代理):一个类被AOP织入增强后，就产生一个结果代理类

Joinpoint(连接点)∶所谓连接点是指那些被拦截到的点。在spring中,这些点指的是方法，因为spring只支持方法类型的连接点。**可以被增强的方法叫连接点**

Pointcut(切入点)︰所谓切入点是指我们要对哪些Joinpoint进行拦截的定义。**真正被增强了**

Advice (通知/增强)︰所谓通知是指拦截到Joinpoint之后所要做的事情就是通知

Aspect(切面)︰是切入点和通知(引介)的结合

Weaving (织入)︰是指把增强应用到目标对象来创建新的代理对象的过程。spring采用动态代理织入，而AspectU采用编译期织入和类装载期织入 。 （将切点和通知连接的过程就是织入）



##### AOP开发明确的事项

1.需要编写的内容

编写核心业务代码(目标类的目标方法)

编写切面类，切面类中有通知(增强功能方法)

在配置文件中，配置织入关系，即将哪些通知与哪些连接点进行结合

2.AOP技术实现的内容

Spring框架监控切入点方法的执行。一旦监控到切入点方法被运行，使用代理机制，动态创建目标对象的代理对

象，根据通知类别，在代理对象的对应位置，将通知对应的功能织入，完成完整的代码逻辑运行。

3.AOP底层使用哪种代理方式

在spring中，框架会根据目标类是否实现了接口来决定采用哪种动态代理的方式。



##### 总结

aop:面向切面编程

aop底层实现:基于JDK的动态代理和基于Cglib的动态代理

aop的重点概念：
Pointcut(切入点)：被增强的方法

Advice(通知/增强)：封装增强业务逻辑的方法

Aspect(切面)：切点+通知

Weaving(织入)：将切点与通知结合的过程

开发明确事项:
谁是切点(切点表达式配置)
谁是通知(切面类中的增强方法)
将切点和通知进行织入配置

![1611492063573](C:\Users\ADMINI~1\AppData\Local\Temp\1611492063573.png)

![1611492162810](C:\Users\ADMINI~1\AppData\Local\Temp\1611492162810.png)

