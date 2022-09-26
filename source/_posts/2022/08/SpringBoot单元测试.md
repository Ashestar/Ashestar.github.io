---
layout: pages
title: SpringBoot单元测试
date: 2022-08-05 09:14:04
tags:
  - Java
  - SpringBoot
categories:
  - Java
  - SpringBoot
---

> 学会写单元测试应该也是程序员的基本功，但可惜我之前压根没动过念头，
> 经过这么久才开始接触单元测试，我觉得自己实在是废了。

<!--more-->

# 需要的依赖

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-test</artifactId>
  <scope>test</scope>
</dependency>
```

# 实践

可以写一个测试类，在里面定义每个单元测试需要做的操作。

```java
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeEach;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
public class BaseTests {
    /**
     * @description 执行测试之前的操作
     * @return
     * @date 2022/8/21 10:39
     * @author neonat
     */
    @BeforeEach
    public void before() {
        System.out.println("start testing");
        setMockMvc();
    }

    /**
     * @description 执行测试之后的操作
     * @return
     * @date 2022/8/21 10:39
     * @author neonat
     */
    @AfterEach
    public void after() {
        System.out.println("end testing");
    }
}
```

# 单元测试内模拟发送 Http 请求

1. 通过 MockMVC 对象

```java
/**
 * @description 测试Test类
 * @date 2022/8/21 10:38
 * @author neonat
 * @version 1.0.0
 */
@SpringBootTest
class DemoApplicationTests {

    @Autowired
    private WebApplicationContext webApplicationContext;

    // 伪造MVC环境，不会启动tomcat，速度更快
    protected MockMvc mockMvc;

    /**
     * @description 在测试之前注册mockMVC
     * @return
     * @date 2022/8/20 17:10
     * @author neonat
     */
    public void setMockMvc() {
        mockMvc = MockMvcBuilders.webAppContextSetup(webApplicationContext).build();
    }

    /**
     * @description 测试带参数的get请求
     * @return
     * @date 2022/8/21 10:37
     * @author neonat
     */
    @Test
    void testMore() throws Exception {
        MvcResult result = mockMvc.perform(MockMvcRequestBuilders
                        .get("/test/more")// get请求
                        .param("integer", "12")// 参数
                        .contentType(MediaType.APPLICATION_JSON)// 返回类型
                )
                .andExpect(MockMvcResultMatchers.status().isOk())// 期望返回结果正确
                .andReturn();
        // 打印返回内容
        System.out.println(result.getResponse().getContentAsString());
    }
}
```

2. 通过 RestTemplate 远程调用

```java
// example cann't run, leave it for next time
```

# 其他事项

## 测试类无法启动原因

1. 需要写启动类；
2. 启动类所在的包要和单元测试所在的包在同一级根目录下。

## @Value 和@Autowired 注解不生效

使用过程中发现`@Value` 和 `@Autowired` 注解不生效，测试运行的对象注入值为 null。

以下为文章分析原因：

Spring 中，实例由容器管理，测试类中，容器因为没有对应的上下文，没有办法进行注入类的实例化操作，因此需要提供一个上下文环境给测试类，即添加如下两个注解到测试类。

```java
@RunWith(SpringRunner.class) //意指让程序运行于Spring测试环境
@SpringBootTest(classes = HiveDeal.class) //给测试类提供上下文环境
```

注解说明：
`@RunWith` 类级别注解，提供了一种更改测试运行程序的默认机制。可以根据指定的类运行程序，指定的类必须是 `Runner` 类的子类。
`@SpringBootTest` 通过这个注解，可以使 `JUnit` 单元测试执行在 `SpringBoot` 环境中，提供上下文环境。

但是我实际使用过程中发现 `@Autowired` 里面再调用 `@Autowired` ，还是不能实现连续自动装配，尝试将多个类都放在 `@SpringBootTest` 中提供上下文环境，还是有问题，就算没出错也很繁琐，最后只能将上下文环境放在启动类中，但是这样使用测试实例时就是完完全全启动一个 SpringBoot 应用了，很重，暂时没有什么好的办法。

# 参考文章

- [springboot 测试类](https://blog.csdn.net/lihuihui01/article/details/115975416)
- [用 SpringBoot 单元测试如何模拟发送 HTTP 请求](https://blog.csdn.net/qq_35746632/article/details/100108651)
- [springboot 单元测试（get 请求）](https://blog.csdn.net/qq_44014971/article/details/108056557)
- [SpringBoot 单元测试，@Value 注解执行不生效问题解决，测试类依赖注入实例失败问题解决](https://blog.csdn.net/MDJ_D2T/article/details/123779419)
