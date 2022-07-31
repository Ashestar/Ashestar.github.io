---
layout: pages
title: 绕过ParallelsDesktop试用期到期限制启动虚拟机
date: 2022-07-31 23:46:01
tags:
  - 配置
categories:
  - 配置
---

从官网下载的正版 Paralles Desktop 登录后可以试用 14 天，14 天后就不能试用了，上网搜索办法绕过过期限制直接启动已安装的虚拟机。

<!--more-->

找了个配置视频，将这个过程设置为快捷指令的方式来使用，配置好后还挺方便的，当然你要是自己手输命令也不是不可以。

[如何绕过试用期到期限制，正常使用 Parallels Desktop 启动虚拟机](https://www.bilibili.com/video/BV1ev4y1K7qm)

主要的配置脚本如下，感谢各位大佬的教程。

```bash
# 第一步：将电脑时间设置为你试用期到期之前
sudo systemsetup -setusingnetworktime Off && sudo systemsetup -setdate 05:14:2022

# 第二步：通过命令直接启动虚拟机
/usr/local/bin/prlctl start "YourVmName"

# 第三步：将电脑时间同步改回去
sudo systemsetup -setusingnetworktime On
```
