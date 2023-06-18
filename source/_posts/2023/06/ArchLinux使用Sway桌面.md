---
layout: pages
title: ArchLinux使用Sway桌面
date: 2023-06-18 21:57:36
tags:
  - 配置
  - Linux
categories:
  - 配置
  - Linux
---

我的 ManjaroToGo 在一次更新后 grub 可能是坏了，找不到启动盘，使用镜像 chroot 尝试修复了好几次都不行，重装切换成 archlinux 算了，再一想，没尝试过平铺式桌面，而且现在的 wayland 也似乎能用了，所以直接装 archlinux+sway。

没错，我天天折腾没用的东西。

<!-- more -->

# 安装系统

没什么好说的，网上教程很多，照着教程一步步配置就好，但是我想装的是 linuxToGo，所以 grub 引导写入的命令要加上 `--removable` 选项，我也是装了好几次最后都不能在 U 盘启动才知道这个问题。

# 安装 SwayWM

安装 sway 桌面

```bash
sudo pacman -S wlroots sway
```

配置文件设置

```bash
cp /etc/sway/config ~/.config/sway/

vim ~/.config/sway/config
```

使用 vulkan 做渲染后端

```bash
vim ~/.pam_environment
#
WLR_RENDERER=vulkan
```

登录 tty1 自动进入 sway

```bash
vim ~/.bash_profile
#
[[ -f ~/.bashrc ]] && . ~/.bashrc
[ "$(tty)" = "/dev/tty1" ] && exec sway
```

wofi 启动器和 alacritty 终端

```bash
sudo pacman -S wofi alacritty
```

waybar 状态栏

```bash
sudo pacman -S waybar otf-font-awesome

vim ~/.config/sway/config
#
#bar {
#    position top
#    swaybar_command waybar
#}
```

# fcitx5 输入法

安装

```bash
sudo pacman -S fcitx5 fcitx5-chinese-addons fcitx5-pinyin-moegirl
```

配置

```bash
vim ~/.config/sway/config
### Auto start
exec --no-startup-id fcitx5 -d

vim ~/.pam_environment
#
GTK_IM_MODULE DEFAULT=fcitx
QT_IM_MODULE  DEFAULT=fcitx
XMODIFIERS    DEFAULT=@im=fcitx
INPUT_METHOD  DEFAULT=fcitx
SDL_IM_MODULE DEFAULT=fcitx
```

wps 的中文输入

```bash
sudo vim /usr/bin/wps
# 添加如下两行
#!/bin/bash
export XMODIFIERS="@im=fcitx"
export QT_IM_MODULE="fcitx"
```

# 参考文章

**特别感谢各位大佬的分享记录**，让我能尝试这些新技术新玩意。

- [Archlinux+Sway+Windows11 完全配置记录](https://github.com/CelestialCosmic/themeblog/issues/22)
- [4.1 Installing GRUB using grub-install](https://www.gnu.org/software/grub/manual/grub/html_node/Installing-GRUB-using-grub_002dinstall.html#:~:text=For%20removable%20installs%20you%20have%20to%20use%20--removable,and%20--efi-directory%20%3A%20%23%20grub-install%20--efi-directory%3D%2Fmnt%2Fusb%20--boot-directory%3D%2Fmnt%2Fusb%2Fboot%20--removable)
- [探索 linux 桌面全面 wayland 化（基于 swaywm）](https://zhuanlan.zhihu.com/p/462322143)
- [ArchLinux 安装 fcitx5 以及拼音输入法](https://blog.csdn.net/HideOnLie/article/details/107615277)
- [Linux：WPS 不能使用中文输入法](https://blog.csdn.net/wishchin/article/details/89674250)
