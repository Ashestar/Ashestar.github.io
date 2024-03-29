---
title: macOS配置
date: 2021-05-29 21:00:00
tags:
  - 配置
  - macOS
categories:
  - 配置
  - macOS
---

这里记录一下我使用 macOS 时做的一些配置。

<!--more-->

# Homebrew

​ Homebrew 是 mac 的包管理器，仅需执行相应的命令,就能下载安装需要的软件包，可以省掉自己去下载、解压、拖拽(安装)等繁琐的步骤。

> Homebrew 官方文档 https://brew.sh/

## 安装

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

可能会报 `bash curl: (7) Failed to connect to raw.githubusercontent.com port 443: Connection refused` 错误，建议换成 brew 镜像安装脚本。

```bash
/bin/bash -c "$(curl -fsSL https://cdn.jsdelivr.net/gh/ineo6/homebrew-install/install.sh)"
```

## 配置

​ Homebrew 安装软件时非常慢。为了提升安装速度，需要更改 Homebrew 的安装源，将其替换成国内镜像。
​ 这里用的是由中科大负责托管维护的 Homebrew 镜像。其中，前两个为必须配置的项目，后两个可按需配置。

- 替换 brew.git ：

  ```bash
  git -C "$(brew --repo)" remote set-url origin https://mirrors.ustc.edu.cn/brew.git
  ```

- 替换 homebrew-core.git ：

  ```bash
  git -C "$(brew --repo homebrew/core)" remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git
  ```

- 替换 homebrew-cask.git ：

  ```bash
  git -C "$(brew --repo homebrew/cask)" remote set-url origin https://mirrors.ustc.edu.cn/homebrew-cask.git
  ```

- 替换 homebrew-bottles ：

  根据使用的终端修改。

  若是 bash，则执行：

  ```bash
  echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.bash_profile

  source ~/.bash_profile
  ```

  若是 zsh，则执行：

  ```bash
  echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.zshrc

  source ~/.zshrc
  ```

  这样 Homebrew 配置就完成了

## 使用

```bash
// 查询：
brew search 软件名
// 安装：
brew install 软件名
// 卸载：
brew uninstall 软件名
// 更新 Homebrew：
brew update
// 查看 Homebrew 配置信息：
brew config
```

使用官方脚本同样会遇到 uninstall 地址无法访问问题，可以替换为下面脚本：

```bash
/bin/bash -c "$(curl -fsSL https://cdn.jsdelivr.net/gh/ineo6/homebrew-install/uninstall)"
```

# 配置使用 zsh 和 oh-my-zsh

Mac 终端默认 shell 为 bash，可配置为其他，我更喜欢 zsh 一点。

查看当前使用的 shell

```bash
echo $SHELL
```

查看安装的 shell

```bash
cat /etc/shells
```

## 安装 zsh

执行：

```bash
brew install zsh
```

切换终端为 zsh：

```
chsh -s /bin/zsh
```

重启终端即可使用 zsh

## 安装 oh-my-zsh

执行：

```bash
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

打开配置文件，可配置主题等

```bash
vim ~/.zshrc
```

更新配置

```bash
source ~/.zshrc
```

查看 zsh 的主题，[oh-my-zsh Themes](https://github.com/ohmyzsh/ohmyzsh/wiki/Themes)

```bash
cd ~/.oh-my-zsh/themes && ls
```

查看当前使用主题名称（设置主题为 random 时可查看当前随机到的主题名称）

```bash
echo $ZSH_THEME
```

查看自带插件

```bash
cd ~/.oh-my-zsh/plugins && ls
```

安装高亮语法插件

```bash
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

# 在配置文件中添加 `plugins=(zsh-syntax-highlighting)`
# zsh-syntax-highlighting 官方推荐放在最后面 各插件之间用空格隔开
```

安装根据历史输入指令的记录即时的提示的插件

```bash
git clone git://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

# `plugins=(git zsh-autosuggestions)`
```

注：报 “Insecure completion-dependent directories detected“ 错误解决办法

这是由于 `/usr/local/share/zsh` `/usr/local/share/zsh/site-functions` 这两个目录没有权限，给这个两个目录赋权就可以了。[issue](https://github.com/robbyrussell/oh-my-zsh/issues/6835)

```bash
chmod 755 /usr/local/share/zsh

chmod 755 /usr/local/share/zsh/site-functions
```

# 配置使用 Git

查看用户名和邮箱

```bash
git config user.name

git config user.email
```

配置用户名和邮箱

```bash
git config --global user.name "username"

git config --global user.email "email"
```

查看配置文件的位置，Git 会优先使用库级别的配置，再然后是 global 级别的配置，最后是 system 级别的配置

```bash
# /etc/gitconfig
# 系统级别的配置，适用于所有的用户和所有的库，存储在Git安装目录下，可以使用如下开头指令指定和修改
git config --system
```

```bash
# ~/.gitconfig
# 用户级别的配置，适用于当前登录的用户，存储在用户目录下，可以使用如下开头指令指定和修改
git config --gloabal
```

```bash
# .git/config
# 库级别的配置，适用于某个具体的库，存储在具体的库隐藏的.git文件夹下，可以使用如下开头指令指定和修改
git config
```

# Tips

- 取消更新小红点

  1. 在配置中取消勾选自动检查更新

  2. 终端执行如下两条指令

     ```bash
     defaults write com.apple.systempreferences AttentionPrefBundleIDs 0

     killall Dock
     ```

  3. 屏蔽网络访问，编辑 host 文件

     ```bash
     127.0.0.1 	swscan.apple.com
     127.0.0.1 	swcdn.apple.com
     127.0.0.1 	swdist.apple.com
     ```

- 查看隐藏文件

  1. 快捷键 command+shift+.

  2. 命令行：

  ```bash
   # 显示文件夹
   defaults write com.apple.finder AppleShowAllFiles TRUE
   # 重启finder
   killall Finder

   # 隐藏文件夹
   defaults write com.apple.finder AppleShowAllFiles FALSE
   # 重启finder
   killall Finder
  ```
