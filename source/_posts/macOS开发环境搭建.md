---
title: macOS开发环境搭建
date: 2021-06-05 09:00:00
tags: 
    - 配置
categories:
    - 配置
---

这里记录一些开发软件的安装配置，推荐先安装好Homebrew，简单省事。

<!--more-->

# Java 环境搭建

1. 安装 AdoptOpenJDK 11
   
    ```bash
    brew install AdoptOpenJDK/openjdk/adoptopenjdk11
    ```

2. 查看安装的JDK信息

    ```bash
    # 查看Java版本
    java -version
    # 查看JDK安装路径
    /usr/libexec/java_home -V
    ```

    注：homebrew下载的openjdk安装目录是`/usr/local/opt/openjdk-xx`，AdoptOpenJDK安装目录是`/Library/Java/JavaVirtualMachines/adoptopenjdk-xx`

3. 参考文章

    [macOS 所有版本 JDK 安装指南 (with Homebrew)](https://www.cnblogs.com/imzhizi/p/macos-jdk-installation-homebrew.html)


# Redis

1. Homebrew安装Redis

   ```bash
   brew install redis
   ```

2. 查看安装及配置文件位置

   > Homebrew安装的软件会默认在`/usr/local/Cellar/`路径下
   >
   > redis的配置文件`redis.conf`存放在`/usr/local/etc`路径下

3. 启动redis

   ```bash
   # 方式一：使用brew帮助我们启动软件
   brew services start redis
   # 方式二：
   redis-server
   ## 使用某个配置文件启动
   redis-server /usr/local/etc/redis.conf
   ```

4. 查看redis服务进程

   ```bash
   # 通过以下命令查看redis服务是否正在运行
   ps axu | grep redis
   ```

5. redis-cli

   ```bash
   # 启动 redis 客户端，打开终端并输入输入以下命令即可连接本地的 redis 服务
   redis-cli
   # redis 默认端口号6379，默认 auth 为空，可用如下命令指定 host 和 port 连接 redis
   redis-cli -h 127.0.0.1 -p 6379
   ```

   注：连接后可执行 **PING** 命令，该命令用于检测 redis 服务是否启动，返回 **PONG**

6. 关闭 redis 服务

   ```bash
   # 发送 shutdown 命令关闭 redis --right way
   redis-cli shutdown
   # 强行终止 redis 服务
   sudo pkill redis-server
   ```


# Node.js

1. 安装

    ```bash
    # 安装 nodejs
    brew install nodejs
    ```

2. 查看安装的node.js信息

    ```bash
    # 查看node版本
    node -v
    # 查看npm版本号
    npm -v
    # 更新npm版本(安装nodejs时自动安装，当有更新时需手动更新)
    npm -g install npm
    ```

# Vue

1. 安装vue

    ```bash
    # 全局安装vue-cli
    npm install -g @vue/cli
    ```

2. uniapp 项目
   
    ```bash
    # 创建uni-app 正式版
    vue create -p dcloudio/uni-preset-vue my-project
    # 创建uni-app alpha版
    vue create -p dcloudio/uni-preset-vue#alpha my-alpha-project

    # 运行、发布uni-app，%PLATFORM%取值：mp-weixin 为微信小程序，h5为H5，mp-alipay为支付宝小程序，更多请见官网
    npm run dev:%PLATFORM%
    npm run build:%PLATFORM%
    ```

    [uni-app官网 (dcloud.io)](https://uniapp.dcloud.io)