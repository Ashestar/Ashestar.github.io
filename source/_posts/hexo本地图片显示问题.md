---
title: Hexo本地图片显示问题
date: 2021-08-21 20:30:00
tags: 
    - 博客
categories:
    - 博客
    - Hexo
---

# 前言

在之前的博客描述中我提到了使用本地图片的方法（第二种），但实际应用还是无法显示，所以在网上搜索得到新的解决方案(详见参考文章)

<!--more-->

# 解决方案

1. 确认 `hexo_project/_config.yml` 中配置好 `post_asset_folder: true`
2. 在 `hexo_project` 目录执行如下命令下载插件
    
    ```bash
    npm install https://github.com/CodeFalling/hexo-asset-image --save
    ```

3. 假设文章目录中有如下结构

    ```bash
    # folder
    your_post
    |-picture.jpg
    # article
    your_post.md
    ```

    在 `your_post.md` 中使用 `![pic](your_post/picture.jpg)` 插入图片，编译生成的文章目录结构会是

    ```bash
    public/year/month/date/your_post
    |-picture.jpg
    |-index.html
    ```

    同时index.html中的图片路径会修改为 `<img src="/year/month/date/your_post/picture.jpg" alt="pic">` 而不是写markdown时的 `<img src="your_post/picture.jpg" alt="pic">` 路径。

    问题解决。

# 参考文章

[在 hexo 中无痛使用本地图片](https://www.cnblogs.com/lmf-techniques/articles/6911051.html)