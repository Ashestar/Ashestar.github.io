---
title: WSL2-桌面安装
date: 2022-05-03 22:43:52
tags:
  - 配置
  - WSL
categories:
  - 配置
  - WSL
---

这一次尝试一下在 wsl2 安装桌面环境并使用远程桌面连接使用，此篇文章作为记录。

<!--more-->

# 桌面安装

## 先安装 xorg 桌面环境

linux 桌面很多都是 x 窗口的实现，基础服务，必装。

```bash
# xorg，将安装xorg-server，键盘驱动、鼠标驱动、显卡驱动等，若没有显卡驱动导致失败请自行安装适宜的显卡驱动
# xorg-xinit，xorg初始化程序，提供了 xinit、startx 和默认的 xinitrc 文件
sudo pacman -S xorg xorg-xinit
```

安装后请自行选择喜欢的桌面环境

## KDE 桌面

```bash
# kde plasma 桌面
sudo pacman -S plasma

# （可选）KDE应用和SDDM图形登录界面
sudo pacman -S kde-applications
sudo pacman -S sddm sddm-kcm
# 启动SSDM登录界面服务，注意到wsl会无法找到这个命令，下文会涉及到
sudo systemctl enable sddm
```

## DWM 桌面

这次也尝试一下平铺式桌面，没那么多花里胡哨的，想要花里胡哨也可以自定义，可以说是非常自由了。

我这次选择尝试的是 DWM。

```bash
# 克隆dwm源码
git clone https://git.suckless.org/dwm
cd dwm/
# 修改config.mk文件
vim config.mk
# X11INC = /usr/X11R6/include ->  X11INC = /usr/include/X11
# X11LIB = /usr/X11R6/lib  ->  X11LIB = /usr/lib/X11
# 编译安装
sudo make clean install
# X窗口服务启动时自动运行dwm窗口管理器
echo exec dwm >> ~/.xinitrc

# 默认使用的终端模拟器是st
git clone https://git.suckless.org/st
cd st/
# 修改config.mk文件
vim config.mk
# X11INC = /usr/X11R6/include ->  X11INC = /usr/include/X11
# X11LIB = /usr/X11R6/lib  ->  X11LIB = /usr/lib/X11
# 编译安装
sudo make clean install

# 默认的程序启动器是dmenu
sudo pacman -S dmenu
```

若安装完毕，可 `shift+alt+enter` 打开 st 终端， `alt+p` 打开 dmenu 并输入程序名称打开程序。

这些快捷键和终端模拟器、程序启动器可在 dwm 源码的 config.def.h 中修改，注意每次修改完都要重新编译安装一次。

ps：尝试过安装 alacritty 终端和 rofi 启动器，但好像在 wsl 中不生效，只有 suckless 公司的三件套合在一起才正常，可能是我 config.def.h 修改的不正确，不管了。

# Xrdp 安装

我们的目的是使用 Windows 远程桌面连接 wsl，所以需要安装配置 Xrdp。

## 必要软件

```bash
# 必要软件
yay -S xrdp

# （可选）xorgxrdp-glamor可以硬件加速，pulseaudio-module-xrdp实现声音重定向（wsl应该不需要）
yay -S xorgxrdp-glamor pulseaudio-module-xrdp

# 修改配置文件
echo "allowed_users=anybody" | sudo tee /etc/X11/Xwrapper.config

# X窗口服务启动时自动运行桌面环境
# 将.xinitrc文件拷贝到用户目录下并修改
cp /etc/X11/xinit/xinitrc ~/.xinitrc
vim ~/.xinitrc
# 注释掉.xinitrc最后几行
# twm &
# xclock -geometry 50x50-1+1 &
# xterm -geometry 80x50+494+51 &
# xterm -geometry 80x20+494-0 &
# exec xterm -geometry 80x66+0+0 -name login
# 1、若使用的是dwm，加上这句话
exec dwm
# 2、若使用的是kde，加上两个配置
export DESKTOP_SESSION=plasma
exec /usr/lib/plasma-dbus-run-session-if-needed startplasma-x11

# 启动xrdp服务，wsl2直接使用systemctl会失败
sudo systemctl enable xrdp
```

## 配置修改

wsl 使用直接使用远程桌面连接时可能会端口冲突，可修改 xrdp 默认的 3389 端口

```bash
sudo vim /etc/xrdp/xrdp.ini
# 如将端口改成3390
# port=3390
```

# wsl2 与 systemd 进程

## 20221020

**wsl2 现已原生支持 systemd**，目前需要 wsl 版本 **0.67.6** 及以上。

方法：升级 wsl 版本后，通过以下命令建立 wsl.conf 文件并开启 systemd：

```bash
echo -e "[boot]\nsystemd=true" | sudo tee -a /etc/wsl.conf
```

来源：[WSL 2 上启用微软官方支持的 systemd](https://www.cnblogs.com/wswind/p/wsl2-official-systemd.html)

## 原来记录

wsl2 本身是由 Windows 负责的，因此根进程不是 systemd，导致直接使用 `systemctl` 命令会失败。

可以使用 `pstree` 查看到进程树。

所以我们要安装 `daemonize` 软件并修改一下，使得我们的 wsl 系统能正常开启 systemd 服务。

```bash
# 安装 daemonize
sudo pacman -S daemonize

sudo vim /etc/profile
# 文件/etc/profile末尾加入
SYSTEMD_PID=$(ps -ef | grep '/lib/systemd/systemd --system-unit=basic.target$' | grep -v unshare | awk '{print $2}')
if [ -z "$SYSTEMD_PID" ]; then
  sudo /usr/sbin/daemonize /usr/bin/unshare --fork --pid --mount-proc /lib/systemd/systemd --system-unit=basic.target
  SYSTEMD_PID=$(ps -ef | grep '/lib/systemd/systemd --system-unit=basic.target$' | grep -v unshare | awk '{print $2}')
fi
if [ -n "$SYSTEMD_PID" ] && [ "$SYSTEMD_PID" != "1" ]; then
  exec sudo /usr/bin/nsenter -t $SYSTEMD_PID -a su - $LOGNAME
fi
```

wsl 重启后再使用 `pstree` 可以看到正常 systemd 服务正常启动了，`systemctl` 命令此时可以使用。

# 后记

我发现将 wsl 的 systemd 服务启动后，`systemctl` 命令正常，远程桌面也正常，目的算是达到了。但是由于 wsl 不是被 Windows 接管，所以本来的 wslg 不能正常使用了，点击程序不会弹出程序窗口，真是鱼与熊掌不可兼得。

无奈下只能退回，毕竟对比起来远程桌面不是那么方便，还是 wslg 更舒服一点，这个桌面只能算是尝鲜吧。

wsl 目前（2022/05/04）只能 100% 或 200% 整数倍缩放，我使用的是 4k 高分屏，Windows 使用的是 150%，wsl 会降级到 100%，导致字体很小，开启到 200% 缩放字是大了，但看起来字体发虚模糊，还是只能调整 gui 应用的字体大小。远程桌面下终端字体大小不知道为什么还调不了，属实是难受，也是我最终没有使用远程桌面这个方案的原因之一。

# 参考文章

- [【桌面篇】Archlinux 安装 kde 桌面](https://www.cnblogs.com/huanhao/p/archlinuxdesktop.html)
- [从零开始配置自己的 Arch Linux 桌面（极简）](https://zhuanlan.zhihu.com/p/112536524)
- [Arch Linux 下安装 dwm (平铺式窗口管理器)](https://blog.csdn.net/weixin_44335269/article/details/117886927)
- [入坑 dwm——原来窗口管理器还可以这样用？！](https://zhuanlan.zhihu.com/p/183861786)
- [简单配置窗口管理器（dwm）](https://zhuanlan.zhihu.com/p/408552473)
- [dwm 美化](https://www.cnblogs.com/lanuage/p/15970577.html)
- [配置 Xrdp - 通过 RDP 连接 Arch Linux KDE 远程桌面](https://alvin.red/2021/11/06/archlinux-xrdp/)
- [WSL2 使用 xrdp 实现图形桌面](https://zhuanlan.zhihu.com/p/149501381)
- [WSL2 的 Linux 中运行 systemctl 命令](https://zhuanlan.zhihu.com/p/335162006)
- [WSL2 Ubuntu 永久开启 systemctl 命令的简单方法](https://www.cnblogs.com/MorStar/p/15078738.html)
