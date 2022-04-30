---
title: Linux解决VMware问题
date: 2021-08-26 21:00:00
tags:
  - 配置
categories:
  - 配置
---

我在 Manjaro 升级 linux 内核后启动虚拟机突然报找不到 vmmon 模块错误，大致如下：

<!--more-->

```bash
could not open /dev/ vmmon，Please make sure that the kernel module `vmmon’ is loaded.
```

大部分回答说的都是执行以下命令：

```bash
sudo vmware-modconfig --console --install-all
```

但我执行后却报找不到文件

```bash
[AppLoader] GLib does not have GSettings support.
sh: line 1: /usr/lib/systemd/scripts/vmware: No such file or directory
Unable to stop services
```

还有人说执行：

```bash
/etc/init.d/vmware start
```

同样报错：

```bash
no such file or directory: /etc/init.d/vmware
```

也试了重装 VMware，还是一样。

最后在回答中看到有这三条命令，试了一下，问题终于解决。

```bash
sudo modprobe -v vmmon

sudo modprobe -v vmnet

sudo vmware-networks --start
```

# 参考文章

[Install VMware : Could not open /dev/vmmon: No such file or directory. Please make sure that the kernel module `vmmon' is loaded [closed]](https://stackoverflow.com/questions/49205162/install-vmware-could-not-open-dev-vmmon-no-such-file-or-directory-please-ma)
