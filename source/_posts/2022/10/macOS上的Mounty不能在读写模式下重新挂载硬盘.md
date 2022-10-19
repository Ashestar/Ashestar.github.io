---
layout: pages
title: macOS上的Mounty不能在读写模式下重新挂载硬盘
date: 2022-10-19 22:36:21
tags:
  - 配置
  - macOS
categories:
  - 配置
  - macOS
---

macOS 上使用 Mounty 重新挂载硬盘时报错，不能重新挂载。

<!--more-->

原因：硬盘先前没有正常卸载（比如我当时是直接拔下硬盘的）。

解决方案：在 Windows 上执行 chkdsk 命令修复硬盘。

```bash
# i为硬盘的盘符
chkdsk i: /f
```
