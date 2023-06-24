---
layout: pages
title: macOS平铺式桌面尝鲜
date: 2023-06-24 19:54:24
tags:
  - 配置
  - macOS
categories:
  - 配置
  - macOS
---

在使用了 sway 桌面后，我觉得平铺式桌面好像更好，不会说需要想办法调整打开的窗口布局，然后在 b 站刷到了关注的 up 主介绍的 macOS 上的平铺式桌面 yabai，虽然官方文档写的是最新支持到 Monterey，但我还是想试一下最新的 Ventura 能不能安装使用一下。

<!--more-->

# 准备工作

## 关闭 SIP（系统完整性保护）

> m 系列芯片的我不知道为什么重启的时候快捷键不能进恢复模式 😣

1. 关机，长按关机键直到出现 HD 盘和选项界面
2. 选择选项，选择用户，输入密码
3. 左上角实用工具-终端
4. 输入`csrutil disable`，回车，输入 y 并回车，输入密码
5. 输入`csrutil status`，查看是否关闭
6. 重启电脑

## 最好使用 homebrew 进行安装，更便捷

# 安装

1. 安装 yabai

```bash
brew install koekeishiya/formulae/yabai --HEAD
```

2. 安装 skhd

```bash
brew install koekeishiya/formulae/skhd
```

3. 安装 jq - 主要在寻找桌面和显示器的时候使用

```bash
brew install jq
```

3. 生成证书

根据官方文档，想运行最新版的 yabai，需要生成一个自签名证书

> First, open Keychain Access.app. In its menu, navigate to Keychain Access, then
> Certificate Assistance, then click Create a Certificate.... This will open the
> Certificate Assistant. Choose these options:
>
> Name: yabai-cert,
> Identity Type: Self-Signed Root
> Certificate Type: Code Signing
>
> Click Create, then Continue to create the certificate.

```bash
codesign -fs 'yabai-cert' $(which yabai)
```

4. 安装完需要执行以下命令

```bash
# 执行install出错，提示not a valid option，但我看其他人的教程都需要执行这个，暂时不知道什么影响
#sudo yabai --install-sa

# load就可以
sudo yabai --load-sa

# 如果load提示没有nvram boot-args，执行如下命令
sudo nvram boot-args=-arm64e_preview_abi
```

# 配置

1. 创建 yabai 和 skhd 的配置文件

```bash
mkdir ~/.config/yabai

# yabai
# 注意，在.config 下的话，用的是 yabairc 没有.
cp /opt/homebrew/share/yabai/examples/yabairc ~/config/yabai/yabairc

chmod u+x ~/config/yabai/yabairc

# skhd
cp /opt/homebrew/share/yabai/examples/skhdrc ~/.skhdrc
```

2. 启动 yabai 和 skhd

```bash
# start yabai
yabai --start-service

# start skhd
skhd --start-service
```

# 自启动加载策略

```bash
sudo visudo -f /private/etc/sudoers.d/yabai

# 然后输入一下内容，其中 <user>为当前 mac 的用户名
<user> ALL = (root) NOPASSWD: /usr/local/bin/yabai --load-sa


# 之后在 yabairc 将以下命令取消注释
yabai -m signal --add event=dock_did_restart action="sudo yabai --load-sa"
sudo yabai --load-sa
```

# 总结

平铺式桌面现在感觉用得很不错，但是快捷键设置不习惯，而且抄的博主的快捷键似乎有部份按了没反应，后续再看看怎么回事。

# 参考文章

- [macbook air m1 芯片关闭 sip 教程（超详细，防踩坑）](https://blog.csdn.net/weixin_44791976/article/details/110826057)
- [Mac 下的平铺式桌面 - Yabai](https://www.cnblogs.com/tdg-yyx/p/15972309.html)
- [codesign 命令 error: The specified item could not be found in the keychain](https://blog.csdn.net/Carrgan/article/details/104441967)
- [Installing yabai (from HEAD)](<https://github.com/koekeishiya/yabai/wiki/Installing-yabai-(from-HEAD)#configure-scripting-addition>)
- [macOS 13 Ventura #1297](https://github.com/koekeishiya/yabai/issues/1297)
