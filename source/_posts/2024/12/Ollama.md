---
title: Ollama
date: 2024-12-14 09:00:00
tags:
  - Ollama
  - AI
categories:
  - Ollama
  - AI
---

使用[Ollama](https://github.com/ollama/ollama)的记录。

<!-- more -->

# 安装

1. 通过homebrew安装（简单方便）

```bash
brew install ollama
```

2. 通过应用安装

官方下载地址，https://ollama.com/download

3. 检查ollama命令路径（配置服务用到）

```bash
which ollama
# homebrew 安装的路径在 `/opt/homebrew/bin/ollama` 
```

# 部分命令

```bash
# Start Ollama, used when you want to start ollama without running the desktop application.
ollama serve

# Pull and run a model
ollama run llama3.2

# Pull a model
ollama pull llama3.2

# Remove a model
ollama rm llama3.2

# Show model information
ollama show llama3.2

# List models on your computer
ollama list
```

# macOS配置服务启动

1. 创建服务配置文件

```bash
vim ~/Library/LaunchAgents/com.user.ollama.service.plist
```

以下配置定义了服务方式运行及跨域设置，使得内网其他的网络访问被允许。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>com.user.ollama.service</string>
    <key>ProgramArguments</key>
    <array>
        <string>/opt/homebrew/bin/ollama</string>
        <string>serve</string>
    </array>
    <key>RunAtLoad</key>
    <true/>
    <key>KeepAlive</key>
    <true/>
    <key>StandardErrorPath</key>
    <string>/tmp/ollama.err</string>
    <key>StandardOutPath</key>
    <string>/tmp/ollama.out</string>
    <key>EnvironmentVariables</key>
    <dict>
        <key>OLLAMA_HOST</key>
        <string>0.0.0.0</string>
        <key>Ollama_ORIGINS</key>
        <string>*</string>
    </dict>
</dict>
</plist>
```

2. 启动/停止服务

```bash
# 授予权限
chmod 644 ~/Library/LaunchAgents/com.user.ollama.service.plist
# 启动服务
launchctl load ~/Library/LaunchAgents/com.user.ollama.service.plist
# 停止服务
launchctl unload ~/Library/LaunchAgents/com.user.ollama.service.plist
```

3. 配合相关gui程序方便使用，如[Chatbox](https://github.com/Bin-Huang/Chatbox)，更多程序可在Ollama的仓库界面获取。

# 参考文章

- [M4 Mac mini深入探索之本地大语言模型，并以服务方式部署Ollama到MacOS](https://www.milaone.com/archives/154.html)