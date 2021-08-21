---
title: Activiti学习记录
date: 2020-10-26 09:00:00
tags: 
    - Java
categories:
    - Java
    - Spring Boot
---

# Activiti 官方网站

先上官方文档，[Activiti 6 User Guide](https://www.activiti.org/userguide)

<!--more-->

# 找到的教程系列文章

- [SpringBoot Activiti6系列教程(一)-activiti-app部署](https://zhengjianfeng.cn/?p=162)
- [SpringBoot Activiti6系列教程(二)-基于mysql数据库初始化](https://zhengjianfeng.cn/?p=167)
- [SpringBoot Activiti6系列教程(三)-开发一个简单的SpringBoot activiti6应用程序](https://zhengjianfeng.cn/?p=170)
- [SpringBoot Activiti6系列教程(四)-流程部署](https://zhengjianfeng.cn/?p=176)
- [SpringBoot Activiti6系列教程(五)-activiti api](https://zhengjianfeng.cn/?p=179)
- [SpringBoot Activiti6系列教程(六)-Execution说明](https://zhengjianfeng.cn/?p=184)
- [SpringBoot Activiti6系列教程(七)-变量](https://zhengjianfeng.cn/?p=186)
- [SpringBoot Activiti6系列教程(八)-流程拒绝实现](https://zhengjianfeng.cn/?p=190)
- [SpringBoot Activiti6系列教程(九)-流程回退实现](https://zhengjianfeng.cn/?p=193)
- [SpringBoot Activiti6系列教程(十)-流程加签征询实现(完结篇)](https://zhengjianfeng.cn/?p=197)

# 架构说明

> ProcessEngineConfiguration类，主要作用是加载activiti.cfg.xml配置文件；

> ProcessEngine类，可以生成activiti的工作环境（建表），通过此类得到各个Service的接口；

> Service接口，快速实现数据库表的操作；
>
> ​		|---- RepositoryService
>
> ​		|---- RuntimeService
>
> ​		|---- TaskService				查看、处理任务
>
> ​		|---- HistoryService
>
> [Activiti核心API介绍](https://my.oschina.net/fuyung/blog/475181)

# 流程

> 流程部署
> 	1、单个文件部署（bpmn文件，png文件），addClasspathResource("*.bpmn")
>
> ​	2、将bpmn文件与png文件压缩成zip文件，再通过addZipInputStream()部署

>流程实例
>
>一个流程定义可以对应多个流程实例
>启动流程实例 RuntimeService startProcessInstanceByKey("key")

## 实例

> [组任务流程CandidateUsers](https://blog.csdn.net/qq_15204179/article/details/86298442)

> [分配组任务的三种方式](https://blog.csdn.net/zjx86320/article/details/50412263)

> [查询待办和已办任务](https://blog.csdn.net/ylforever/article/details/99708257)

> [Activiti业务键（businessKey）](https://www.cnblogs.com/cxyj/p/3893631.html)