---
title: Git 分支管理
date: 2021-08-22 15:43:45
tags: 
    - Git
categories:
    - Git
---

Git进行本地仓库和远程仓库的分支管理。

<!--more-->

# 管理分支

## 本地分支

1. 查看本地分支

    ```bash
    # 查看分支
    git branch
    # 查看所有分支
    git branch -a
    ```

    结果

    ```bash
    # *号表示的是当前分支
    * master
    ```

2. 新建本地分支

    ```bash
    git branch [branch name]
    ```

3. 切换分支

    ```bash
    git checkout [branch name]
    ```

4. 创建&切换分支

    ```bash
    # 创建分支的同时切换到该分支
    git checkout -b [branch name]
    ```

5. 删除分支

    ```bash
    git branch -d [branch name]
    ```

6. 将分支推送到远程仓库

    ```bash
    git push origin [branch name]
    ```

## 远程分支

1. 查看远程分支

    ```bash
    git branch -r
    ```

2. 删除远程分支

    ```bash
    # 分支名称前的冒号代表删除
    git push origin :[branch name]
    ```

# 参考文章

[git创建新分支，并将本地代码提交到新分支上](https://blog.csdn.net/chen134225/article/details/95475960)