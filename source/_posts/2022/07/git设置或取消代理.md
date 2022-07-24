---
layout: pages
title: github clone 失败解决
date: 2022-07-24 23:12:33
tags:
  - git
categories:
  - git
  - 配置
---

github clone 一直失败，挂了梯子，现在记录一下怎么配置 git 代理。

<!--more-->

# 设置代理

```bash
# http代理
git config --global http.proxy 'http://127.0.0.1:your_port'
git config --global http.proxy 'socks5://127.0.0.1:your_port'

# https代理
git config --global https.proxy 'https://127.0.0.1:your_port'
git config --global https.proxy 'socks5://127.0.0.1:your_port'
```

# 取消代理

```bash
git config --global --unset http.proxy

git config --global --unset https.proxy
```

# 查看代理

```bash
git config --global --get http.proxy

git config --global --get https.proxy
```

# 参考文章

- [github clone 超时、速度慢，git 设置代理和取消代理设置](https://blog.csdn.net/csdn_zhangshi/article/details/104882554)
