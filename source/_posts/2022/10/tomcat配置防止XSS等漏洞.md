---
layout: pages
title: tomcat配置防止XSS等漏洞
date: 2022-10-04 18:16:02
tags:
  - 配置
  - tomcat
categories:
  - 配置
  - tomcat
---

现在公司对需要投产的外联应用除了做代码扫描等工作，还需要进行渗透性测试，真的是改得欲仙欲死，最多的就是反射型 XSS 漏洞和存储型 XSS 漏洞了，这里先不表，还有就是需要对部署应用的 tomcat 进行配置，消掉扫出来的头缺失漏洞：X-Frame-Options、X-XSS-Protection、X-Content-Type-Options 头缺失漏洞。

<!--more-->

# 步骤

1. 在 web.xml（`$tomcat/conf/web.xml` or `$project/src/main/webapp/WEB-INF/web.xml`）中配置 filter

```xml
<filter>
  <filter-name>httpHeaderSecurity</filter-name>
  <filter-class>org.apache.catalina.filters.HttpHeaderSecurityFilter</filter-class>
  <!-- Frame-Options -->
  <async-supported>true</async-supported>
  <init-param>
    <param-name>antiClickJackingEnabled</param-name>
    <param-value>true</param-value>
  </init-param>
  <init-param>
    <param-name>antiClickJackingOption</param-name>
    <param-value>SAMEORIGIN</param-value>
  </init-param>
  <!-- XSS-Protection -->
  <xssProtectionEnabled>true</xssProtectionEnabled>
  <!-- Content-Type-Options -->
  <BlockContentTypeSniffingEnabled>true</BlockContentTypeSniffingEnabled>
</filter>
<filter-mapping>
  <filter-name>httpHeaderSecurity</filter-name>
  <url-pattern>/*</url-pattern>
</filter-mapping>
```

2. 验证

重启 tomcat 后，在 http 请求中可以看到 返回的 Headers 中包含了

```
X-Content-Type-Options: nosniff
X-Frame-Options: SAMEORIGIN
X-XSS-Protection: 1; mode=block
```

这样就配置成功了。

# 来源

- [关于网信办的扫描漏洞“X-Frame-Options”，“X-XSS-Protection“等三联漏洞](https://blog.csdn.net/qq_20324305/article/details/117414274)
