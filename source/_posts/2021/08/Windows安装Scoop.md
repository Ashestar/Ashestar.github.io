---
title: Windows10 安装 Scoop
date: 2021-08-23 21:50:00
tags:
  - 配置
  - Windows
categories:
  - 配置
  - Windows
---

Scoop 是可用于 windows 的一款包管理工具，可以下载程序并自动配置环境变量等，记录一下安装方法。

<!--more-->

# 安装条件

- 用户名不含中文字符
- Windows 7 SP1+ / Windows Server 2008+
- PowerShell 3+
- .NET Framework 4.5+

可在 PowerShell 运行以下代码获取版本信息：

```powershell
#查看Powershell版本
$PSVersionTable.PSVersion.Major
#查看.NET Framework版本
$PSVersionTable.CLRVersion.Major
```

先输入以下代码，保证后面的脚本有运行权限：

```powershell
set-executionpolicy remotesigned -scope currentuser

执行策略更改
执行策略可帮助你防止执行不信任的脚本。更改执行策略可能会产生安全风险，如 https:/go.microsoft.com/fwlink/?LinkID=135170
中的 about_Execution_Policies 帮助主题所述。是否要更改执行策略?
[Y] 是(Y)  [A] 全是(A)  [N] 否(N)  [L] 全否(L)  [S] 暂停(S)  [?] 帮助 (默认值为“N”): y
```

# 开始安装

在 PowerShell 输入以下代码下载 Scoop：

```powershell
iwr -useb get.scoop.sh | iex
# or
iex (new-object net.webclient).downloadstring('https://get.scoop.sh')
```

## 下载出现错误

1. 若下载中断，重新下载之前请先删除 `C:\Users\username\scoop` 文件夹。

2. 需要安全策略错误，如下所示

```powershell
PowerShell requires an execution policy in [Unrestricted, RemoteSigned, ByPass] to run Scoop.
For example, to set the execution policy to 'RemoteSigned' please run :
'Set-ExecutionPolicy RemoteSigned -scope CurrentUser'
```

修改 PowerShell 的安全策略：

```powershell
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
```

3. 无法连接到远程服务器

```powershell
使用“1”个参数调用“DownloadString”时发生异常:“无法连接到远程服务器”
所在位置 行:1 字符: 1
+ iex (new-object net.webclient).downloadstring('https://get.scoop.sh')
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (:) [], MethodInvocationException
    + FullyQualifiedErrorId : WebException
```

这个错误还有可能出现的 **“基础连接已经关闭：发送时发生错误“** 或 **“操作超时”** ；原因可能是 https://get.scoop.sh 无法访问，或者网络不稳定或者和谐，用以下命令再下载，还是不行的话可多试几次、更换网络或者用梯子

```powershell
iex (new-object net.webclient).downloadstring('https://raw.githubusercontent.com/lukesampson/scoop/master/bin/install.ps1')
```

4. 未能创建 SSL/TLS 安全通道

```powershell
iex : 使用“2”个参数调用“DownloadFile”是发生异常:“请求被中止: 未能创建 SSL/TLS 安全通道。”
所在位置 行:1 字符: 1
+ iex (new-object net.webclient).downloadstring('https://get.scoop.sh')
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (:) [Invoke-Expression], MethodInvocationException
    + FullyQualifiedErrorId : WebException, Microsoft.PowreShell.Commands.InvokeExpressionCommand
```

执行以下命令：

```powershell
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
```

# 安装成功

输入 `scoop help` 验证是否安装成功。

```powershell
Usage: scoop <command> [<args>]

Some useful commands are:

alias       Manage scoop aliases
bucket      Manage Scoop buckets
cache       Show or clear the download cache
checkup     Check for potential problems
cleanup     Cleanup apps by removing old versions
config      Get or set configuration values
create      Create a custom app manifest
depends     List dependencies for an app
export      Exports (an importable) list of installed apps
help        Show help for a command
home        Opens the app homepage
info        Display information about an app
install     Install apps
list        List installed apps
prefix      Returns the path to the specified app
reset       Reset an app to resolve conflicts
search      Search available apps
status      Show status and check for new app versions
uninstall   Uninstall an app
update      Update apps, or Scoop itself
virustotal  Look for app's hash on virustotal.com
which       Locate a shim/executable (similar to 'which' on Linux)

Type 'scoop help <command>' to get help for a specific command.
```

常用命令：

```powershell
# 查找软件
scoop search apps
# 下载软件
scoop install apps
# 卸载软件
scoop uninstall apps
```

# 提升下载速度

scoop 下载软件大多是从外部链接下载的，网速较慢且容易失败，可以安装 aria2 来提升下载速度：

```powershell
scoop install aria2
```

aria2 安装成功后再下载其他软件时会自动调用进行多线程下载，所以推荐先安装这个软件提升下载体验。

可以设置下载下载线程数（默认为 16 线程）

```powershell
scoop config aria2-max-connection-per-server 16
scoop config aria2-split 16
scoop config aria2-min-split-size 1M
```

# 添加仓库

scoop 自带的 main bucket 软件过少，我们需要添加官方维护的 extras bucket：

```powershell
scoop bucket add extras
```

也可以添加其他的第三方 bucket:

```powershell
scoop bucket add bucketname bucketaddress
```

例如添加 scoopbucket 并安装 cajviewer：

```powershell
# add scoopbucket
scoop bucket add scoopbucket https://github.com/yuanying1199/scoopbucket
# install cajviewer
scoop install scoopbucket/cajviewerlite
```

# 参考文章

[Scoop 安装详解](https://boyinthesun.cn/post/scoop/)

[scoop——强大的 Windows 命令行包管理工具](https://www.jianshu.com/p/50993df76b1c)
