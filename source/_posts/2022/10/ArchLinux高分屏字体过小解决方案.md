---
layout: pages
title: ArchLinux高分屏字体过小解决方案
date: 2022-10-20 23:27:15
tags:
  - 配置
  - Linux
categories:
  - 配置
  - Linux
---

之前通过一系列配置在 wsl2 上安装了桌面并远程连接过去后，发现桌面缩放不对劲，在我的 4k 屏上显示字体非常小，查了好久资料才看到可以通过配置 `X resources` 解决。

<!-- more -->

[Arch Linux - 4k/HiDPI 屏 X Window 字体太小](https://zhuanlan.zhihu.com/p/384806450)

在 `~/.Xresources` 文件上配置如下内容

```
Xft.dpi: 192

! These might also be useful depending on your monitor and personal preference:
Xft.autohint: 0
Xft.lcdfilter:  lcddefault
Xft.hintstyle:  hintfull
Xft.hinting: 1
Xft.antialias: 1
Xft.rgba: rgb
```

其中 Xft.dpi 尽量为 96 的倍数，以保证最佳显示效果。

之后重启 `X server` 或者直接 reboot。

# 未解决问题

使用的 st 终端字体还是小得跟蚂蚁一样，还需要继续查询解决方案。
