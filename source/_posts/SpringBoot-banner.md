---
title: SpringBoot banner
date: 2021-05-29 21:00:00
tags: 
    - Java
categories:
    - Java
    - Spring Boot
---

Spring Boot 启动时，控制台输出的图案叫 *banner*，如下所示的Spring图案。

```bash
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::                (v2.5.0)
```

<!--more-->

# 最简单方式改写banner输出的图案

1. 在src/main/resources目录下新建banner.txt
2. 在banner.txt上加上图案，可以在这个网站-[一个ASCII图案网站](http://patorjk.com/software/taag/)-上获得自己想要的ASCII图案

这样在启动项目时控制台就可以打印出自己的图案了。

也可以使用jpg、png甚至gif，都命名为banner。

或者通过设置`banner.image.location`属性来作为banner信息，这些图片会被转换为有艺术感的ASCII，并且打印在文本的顶部。

# 对banner做配置

1. 在启动类上设置

    ```java
    public static void main(String[] args) {
        // SpringApplication.run(TestApplication.class, args);
        SpringApplication springApplication = new SpringApplication(TestApplication.class);
        springApplication.setBannerMode(Banner.Mode.CONSOLE);
        springApplication.run(args);
    }
    ```
   
2. 在properties文件中做配置

    ```properties
    # 打印到控制台
    spring.main.banner-mode=console
    # 打印到日志文件
    spring.main.banner-mode=log
    # 不打印
    spring.main.banner-mode=off
    ```

3. banner配置解析

    * 模式

        ```java
        OFF					// 关闭banner
        CONSOLE			// 在控制台输出banner图案
        LOG					// 应为在日志中打印图案，控制台也有
        ```

    * 打印样式

        ```java
        AnsiColor         // 用来设定字符的前景色
        AnsiBackground    // 用来设定字符的背景色
        AnsiStyle         // 用来控制加粗、斜体、下划线等等。
        ```
    
    * 可以加上文件版本号等

        ```java
        ${application.title}                    // MANIFEST.MF文件中的应用名称

        ${application.version}                  //  这个是MANIFEST.MF文件中的版本号  

        ${application.formatted-version}        // 这个是上面的的版本号前面加v后上括号  

        ${spring-boot.version}                  // 这个是springboot的版本号  

        ${spring-boot.formatted-version}				// 同上
        ```

4. 示例

    ```bash
    ${AnsiColor.BRIGHT_MAGENTA}
        ______       ___
        /\__  _\     /\_ \
        \/_/\ \/     \//\ \     ___   __  __     __       __  __    ___   __  __
        \ \ \       \ \ \   / __`\/\ \/\ \  /'__`\    /\ \/\ \  / __`\/\ \/\ \
            \_\ \__     \_\ \_/\ \\ \ \ \_/ |/\  __/    \ \ \_\ \/\ \\ \ \ \_\ \ \
            /\_____\    /\____\ \____/\ \___/ \ \____\    \/`____ \ \____/\ \____/
            \/_____/    \/____/\/___/  \/__/   \/____/     `/___/> \/___/  \/___/
                                                            /\___/
                                                            \/__/
                    :: Spring Boot ::							${spring-boot.version}   
    ```