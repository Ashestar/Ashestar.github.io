---
layout: pages
title: 解决go插件下载失败问题
date: 2022-07-24 23:02:34
tags:
  - 配置
  - golang
categories:
  - 配置
  - golang
---

国内下载 go 插件会报错，不用梯子只能配置 go mod 代理了。

<!--more-->

直接执行如下的命令就可启用 go mod 代理，这时 vscode 再下载 go 插件就不会报错了。

```bash
go env -w GO111MODULE=on

go env -w GOPROXY=https://proxy.golang.com.cn,direct
```

# 参考文章

- [一招完美解决 vscode 安装 go 插件失败问题](https://blog.csdn.net/qq_41065919/article/details/107710144)
