---
layout: pages
title: AppleSilicon芯片macOS配置
date: 2022-07-26 20:06:13
tags:
  - 配置
  - macOS
categories:
  - 配置
  - macOS
---

买了 Apple Silicon 芯片的电脑，就需要考虑安装软件的适配性和配置上的差异了，所幸我不是第一个吃螃蟹的，现在软件生态也逐步地建立起来了。

<!--more-->

# Homebrew

最重要的 Homebrew 一定要先配置好，使用 gitee 上大佬配置好的脚本，无痛快捷。

```bash
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"
```

执行后根据提示选择镜像源就好，会提示安装 git 和 Xcode Command Line Tool，安装完重新执行安装 Homebrew 的命令。

m 系列芯片 的 macOS 上 Homebrew 的路径是 `/opt/homebrew`。

# 参考文章

[MacBook 使用笔记：安装 Homebrew（M1）](https://zhuanlan.zhihu.com/p/372576355)
