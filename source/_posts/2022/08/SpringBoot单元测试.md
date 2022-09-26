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
// todo
```

# 其他事项

测试类无法启动原因：

1. 需要写启动类；
2. 启动类所在的包要和单元测试所在的包在同一级根目录下。

# 参考文章

- [springboot 测试类](https://blog.csdn.net/lihuihui01/article/details/115975416)
- [用 SpringBoot 单元测试如何模拟发送 HTTP 请求](https://blog.csdn.net/qq_35746632/article/details/100108651)
- [](https://blog.csdn.net/qq_44014971/article/details/108056557)
