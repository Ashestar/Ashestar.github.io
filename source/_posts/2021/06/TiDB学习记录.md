---
title: TiDB学习记录
date: 2021-06-22 16:00:00
tags:
  - SQL
  - TiDB
categories:
  - SQL
  - TiDB
---

先上 TiDB 官方文档链接，[TiDB 简介 - PingCAP Docs](https://docs.pingcap.com/zh/tidb/stable/overview)

<!--more-->

# 在 macOS 上部署本地测试环境

1. 下载安装 TiUP：

   ```bash
   curl --proto '=https' --tlsv1.2 -sSf https://tiup-mirrors.pingcap.com/install.sh | sh
   ```

2. 声明全局环境变量：

   ```bash
   source .bash_profile
   ```

3. 启动集群：

   - 直接执行 `tiup playground` 命令会运行最新版本的 TiDB 集群，其中 TiDB、TiKV、PD 和 TiFlash 实例各 1 个：

     ```bash
     tiup playground
     ```

   - 可以指定 TiDB 版本以及各组件实例个数，命令类似于：

     ```bash
     tiup playground v5.0.2 --db 2 --pd 3 --kv 3 --monitor
     ```

   > 以这种方式执行的 playground，在结束部署测试后 TiUP 会清理掉原集群数据，重新执行该命令后会得到一个全新的集群。
   >
   > 若希望持久化数据，可以执行 TiUP 的 `--tag` 参数：
   >
   > ```bash
   > tiup --tag <your-tag> playground
   > ```

4. 访问 TiDB 数据库：

   - 使用 TiUP `client` 连接 TiDB：

     ```bash
     tiup client
     ```

   - 使用 MySQL 客户端连接 TiDB：

     ```bash
     # 此连接地址会在启动集群时在终端提示
     mysql --host 127.0.0.1 --port 4000 -u root
     ```

5. 通过 `http://127.0.0.1:9090` 访问 TiDB 的 Prometheus 管理界面。

6. 通过 `http://127.0.0.1:2379/dashboard` 访问 [TiDB Dashboard](https://docs.pingcap.com/zh/tidb/stable/dashboard-intro) 页面，默认用户名为 `root`，密码为空。

7. 清理集群：

   - ctrl+c 终止进程。

   - 执行以下命令清理所有集群：

     ```bash
     tiup clean --all
     ```

# 文章收录

- [TiDB 源码阅读系列文章（一）序](https://zhuanlan.zhihu.com/p/36500508)
