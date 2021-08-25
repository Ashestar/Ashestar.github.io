---
title: linux驱动博通无线网卡
date: 2021-08-25 21:00:00
tags: 
    - 配置
categories:
    - 配置
---

解决基于Arch Linux的Linux发行版Manjaro无法识别博通无线网卡驱动的问题。

<!--more-->

# 问题描述

* 找不到无线网络
* 找不到无线开关


# 解决

1. 更新系统软件

  ```bash
  sudo pacman -Syu
  ```

2. 安装对应的linux-headers，可在设置查看对应的内核版本

  ```bash
  sudo pacman -S linux-headers
  ```

3. 安装博通网卡驱动

  ```bash
  sudo pacman -S broadcom-wl-dkms
  ```

4. 重启电脑，这时候应该就解决问题了，输入以下命令验证

  ```bash
  dkms status
  # 出现类似信息说明成功
  # broadcom-wl, 6.30.223.271, 5.10.59-1-MANJARO, x86_64: installed
  ```

# 参考文章

[论如何在linux上正确驱动博通网卡](https://rowe98.github.io/2019/07/19/bodcom_failure/)