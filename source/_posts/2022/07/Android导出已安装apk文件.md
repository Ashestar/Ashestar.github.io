---
layout: pages
title: Android导出已安装apk文件
date: 2022-07-25 23:16:27
tags:
  - Android
categories:
  - Android
  - ADB
---

在`WSA`愉快的玩耍过程中，想要测试一下手机上已安装的应用，只能用流量的我心疼再下载一次的花费，于是搜索能不能通过`adb`导出自己手机上的软件，说实话还挺容易的。

<!--more-->

1. adb 连接手机

首先就是要让电脑通过`adb`连接上自己的手机了。

电脑要下载[adb](https://developer.android.google.cn/studio/command-line/adb?hl=zh-cn)

再打开手机的开发者选项，开启 USB 调试，后面可以通过两种方式连接。

- usb 连接

  ```bash
  # 查看并连接设备
  adb devices
  ```

- wifi 连接（在同一局域网）

  ```bash
  # adb connect
  adb connect phone_ip:phone_port
  ```

2. adb 导出已安装程序

```bash
# 查看设备已安装的应用
adb shell pm list packages

# 查看apk的路径
adb shell pm path package_name

# 导出apk到某个路径
adb pull package_path target_path
```

3. 通过 adb 安装 apk

```bash
adb install package_path
```

4. 我发现这样只是拿到安装包，我手机上的应用数据并没有拿下来，就比如安装完游戏想玩还是要下一遍更新数据。。。

# 参考文章

- [Android 导出已安装应用程序 apk 文件的两种方案](https://blog.csdn.net/zhangphil/article/details/84838096)
