---
title: ArchLinux on WSLg
date: 2021-10-16 20:45:56
tags:
  - 配置
categories:
  - 配置
---

经过多次尝试，我终于成功在 WSLg(Windows Subsystem for Linux GUI) 上成功安装了 ArchLinux 并配置了中文输入法，真是令人唏嘘，特别是卡在配置中文输入法上一两个星期真是令人感到非常气馁。

所幸我终于成功了，本文就是在 ArchLinux on WSLg 上编写的，现在既可以愉快地玩游戏又可以码代码同时不用在 Windows 上配置恶心的环境了（没错，我再也不想体验在 windows 上配置环境变量等等繁琐的东西了）。

<!--more-->

# 准备

首先要做的准备就是启用 WSLg 了。由于我装的系统是 Windows11，所以对于版本什么的倒是不用担心，但是要注意 **WSLg 的 Windows 操作系统版本要求是 21362+**，可以使用 win+r 输入 winver 查看版本。

## 启用 wsl

用管理员打开 powershell 输入

```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

## 启用虚拟平台

用管理员打开 powershell 输入

```powershell
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

## 将 WSL2 设置为默认版本

（不知道为什么我设置了安装的系统还是 WSL1）用管理员打开 powershell 输入

```powershell
wsl --set-default-version 2
```

## 更新到 WSLg

```powershell
wsl --update
```

## 安装 LxRunOffline

这是个很好用的 wsl 管理工具，可以用来安装没有微软官方支持的 wsl 子系统，比如我现在安装的 ArchLinux。

如果你配置了 Windows 的包管理器比如 Scoop ，那就简单了，一条命令搞定

```powershell
scoop install lxrunoffline
```

不然就手工下安装包吧，下载地址：https://github.com/DDoSolitary/LxRunOffline/releases

选择最新版下载，解压后将 LxRunOffline.exe 放入任意一个 path 文件夹下（比如 C:/Windows/System32）

## 下载 ArchLinux

下载地址：https://mirrors.tuna.tsinghua.edu.cn/archlinux/iso/latest/

不管选择什么版本，记得需要的格式是 `tar.gz` 文件。

准备工作完成，下面开始正式安装.

# 安装 ArchLinux 到 WSLg

## 安装镜像

```powershell
LxRunOffline i -n <自定义名称> -f <Arch镜像位置> -d <安装系统的位置> -r root.x86_64
```

## 设置为 WSL2

```powershell
wsl --set-version <名称> 2
```

## 进入系统

```powershell
wsl -d <名字>
```

如果你只安装了一个子系统，那么只输入 `wsl` 会自动进入默认的子系统中。

PS：WSL 一般的管理命令

查看安装的子系统和对应版本

```powershell
wsl -l -v
```

关闭某个子系统

```powershell
wsl --shutdown <名字>
```

## 以下操作在子系统中进行

编辑镜像源

```bash
cd /etc/
```

用 windows 的文件管理器打开/etc 目录：

```bash
explorer.exe .
```

编辑 /etc/pacman.conf 文件，在最后加入：

```bash
[archlinuxcn]
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
```

编辑 /etc/pacman.d/mirrolist 文件，选择一些 China 的源，将源注释去掉（选择部分即可），譬如我选择的是清华源：

```bash
#Server = http://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch
```

更新系统，依次执行以下命令：

```bash
# 更新
pacman -Syy

pacman-key --init

pacman-key --populate

# 添加 archlinuxcn 的keyring
pacman -S archlinuxcn-keyring

# 安装 sudo vim yay 等软件
pacman -S base base-devel vim git yay
```

给 root 用户添加密码：

```bash
passwd
```

添加一个普通用户并设置密码：

```bash
useradd -m -g wheel <用户名>

passwd
```

编辑文件 /etc/sudoers ，去掉 %wheel ALL=(ALL) ALL 的注释

```bash
vim /etc/sudoers
```

查看普通用户 id：

```bash
id -u <用户名>
```

## 设置进入子系统时使用普通用户

在 powershell 中执行：

```powershell
lxrunoffline su -n <子系统名字> -v <账户id>
```

这样初步的设置就完成了。

# 安装中文支持和中文输入法

## 中文支持

1. 安装中文 Locale

修改/etc/locale.gen 文件，取消对应项之前的注释符

```bash
sudo vim /etc/locale.gen
```

我目前只启用了两个

```bash
en_US.UTF-8 UTF-8
zh_CN.GBK GBK
```

2. 启用中文 locale

```bash
sudo locale-gen
```

3. 安装中文字体

```bash
sudo pacman -S wqy-zenhei
```

## 中文输入法

就是这一步卡了我好久，试过 ibus，fcitx， fcitx5，开始时怎么都调用不了输入法，最后还是选择了 fcitx，不过要额外下载一个 dbus-x11。

1. 必要软件和输入法，我本人使用 Rime，可换成其他的

```bash
sudo pacman -S fcitx fcitx-configtool fcitx-rime
```

2. 切换到 root 用户生成 dbus 机器码

```bash
dbus-uuidgen > /var/lib/dbus/machine-id
```

3. 安装 dbus-x11

```bash
yay dbus-x11
```

4. 配置输入环境

将以下内容添加到 ~/.bashrc 配置文件中

```bash
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS=@im=fcitx
export DefaultIMModule=fcitx
# fcitx 自启
fcitx-autostart &>/dev/null
```

```bash
vim ~/.bashrc

source ~/.bashrc
```

bash 输入 fcitx-configtool ，配置输入法。

然后就可以在 gui 程序中使用中文输入法啦。

# 参考文章

- [在 WSL2 中安装 ArchLinux](https://www.cnblogs.com/kainhuck/p/13835833.html)
- [Arch 支持中文字体以及安装中文输入法](http://ocdman.github.io/2017/11/22/Arch%E6%94%AF%E6%8C%81%E4%B8%AD%E6%96%87%E5%AD%97%E4%BD%93%E4%BB%A5%E5%8F%8A%E5%AE%89%E8%A3%85%E4%B8%AD%E6%96%87%E8%BE%93%E5%85%A5%E6%B3%95/)
- [WSL2-不输 Mac 的开发体验（一）：WSL2 的安装及基本配置](https://zhuanlan.zhihu.com/p/407555801)
- [archlinux 中 fcitx 安装配置与常见问题](https://blog.csdn.net/sinat_33528967/article/details/97611020archlinux中fcitx安装配置与常见问题)
