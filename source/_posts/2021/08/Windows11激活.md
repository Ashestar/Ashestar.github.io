---
title: Windows11 激活
date: 2021-08-30 21:50:00
tags:
  - 配置
  - Windows
categories:
  - 配置
  - Windows
---

装了 Windows11 的测试版，一直提示激活。找了半天的激活码，没一个能用的，最后找到一个教程。

<!--more-->

# 激活

在 cmd 命令行依次执行以下命令（管理员模式）：

```powershell
// 安装产品密钥
slmgr /ipk W269N-WFGWX-YVC9B-4J6C9-T83GX
// 更改计算机名称
slmgr /skms kms.03k.org
// 激活
slmgr /ato
```

也可创建一个 bat 文件，内容就是上面代码，以管理员身份运行。

最后激活成功。

# 参考文章

[教大家怎么永久激活 win11 专业版](https://koudaipe.com/win11/23454.html)
