---
layout: pages
title: SpringBoot培训2022
date: 2022-09-16 10:15:12
tags:
  - 培训
  - Java
  - SpringBoot
categories:
  - 培训
  - SpringBoot
---

2022 年 SpringBoot 培训记录。

<!--more-->

# Spring 核心原理

## 简化开发的几个策略

1. 基于 pojo 的轻量级和最小侵入性编程
2. 通过依赖注入和面向接口松耦合
3. 基于切面和注解进行声明式编程
4. 通过切面和模版减少样板式代码

## 代理模式

将被代理类包装起来然后重新实现相同的方法，并且调用原来方法的同时可以在方法前后添加一个新的处理。

- 这种代理模式可以使用继承或组合来使用，当调用时需要使用的是代理类的对象。
- 通过继承被代理对象，重写被代理类方法，可以对其进行代理，被代理类无需实现接口。
- 被代理类和代理类都需要实现相同接口，被代理类作为代理类的组合属性成员传入。
- 静态代理：静态编码定义代理类。
- 动态代理：动态生成代理类。使用反射和字节码，在运行期间创建指定接口或类的子类以及它的实例对象的一项技术，通过此技术可以对代码进行无侵入式的增强。
  - **JDK 动态代理**：java.lang.reflect 包中的 Proxy 类和 InvocationHandler 接口提供了生成动态代理类的能力。
  - **CGLIB 动态代理**：第三方代码生成类库，运行时在内存中动态生成一个子类对象（通过继承方式）从而实现对目标对象功能的拓展。

## Bean 的拓展接口

- BeanPostProcessor：Bean 级别、针对具体的 Bean 进行处理，默认对 Spring 容器的所有 Bean 进行处理。
- BeanFactoryPostProcessor：对 BeanDefinition 对象进行修改，针对 Bean 容器，在当前 BeanFactory 初始化后、Bean 实例化之前修改 Bean 的定义属性。

# 面向切面 AOP

面向切面编程（AOP）通过提供另一种思考程序结构的方式来补充面向对象编程（OOP）。

OOP 中模块化的关键单元是类，AOP 中模块化的单元是切面。

切面支持跨多种类型和对象的关注点（例如事务管理）的模块化。

Spring IOC 容器不依赖 AOP，AOP 补充了 Spring IOC 以提供非常强大的中间件解决方案。

## 顺序问题

- 从 Spring Framework 5.2.7 开始，同一个切面类中定义的需要在同一个连接点运行的通知方法根据通知类型的优先级由高到低排序：@Around，@Before，@After，@AfterRunning，@AfterThrowing。
- 当同一个类中定义两条相同类型的 Advice 时，执行顺序是未定义的（不应该有多条相同的 Advice）。
- 不同切面类可以通过 Order 决定顺序，值越小优先级越高。

# 拦截器

Spring MVC 可以使用拦截器对请求进行拦截处理，用户可以自定义拦截器实现特定功能，自定义拦截器必须实现 HandlerInterceptor 接口。

拦截器方法执行顺序：

preHandle -> handle -> postHandle -> render -> afterCompletion
