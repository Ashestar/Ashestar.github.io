---
title: Hexo Blog
date: 2021-08-15 19:53:45
tags:
  - 博客
categories:
  - 博客
  - Hexo
---

# 前言

前面我用 `jkell` 弄了个 Github 博客，虽然能凑活着用，但还是对效果不是很满意。后面看到别人都在用 Hexo 的 Next 主题界面，看起来简洁又好看，于是在经过很长时间后（懒癌晚期），我终于下载了 `hexo cli`，尝试了一下，效果还成，这是写的第一篇博客，后面会将以前写的博客也更新上来。

<!--more-->

# hexo 下载配置

官网： http://hexo.io

github: https://github.com/hexojs/hexo

1. 安装
   ```bash
   npm install hexo-cli -g
   ```
2. 在本地新建一个文件夹存放代码
3. 初始化
   ```bash
   hexo init
   ```
4. 启动博客
   ```bash
   hexo s
   # or
   hexo server
   ```
5. 根据提示在浏览器输入 `http://localhost:4000` 就可以访问到本地启动的博客了。
6. 输入`hexo help`可以查看命令选项
7. 写文章
   ```bash
   hexo new 'your_article_name'
   ```
   运行此命令会在 `hexo_project/source/_posts` 文件夹下建立一个新 markdown 文件，当然你可以不用命令，自己手动建新 md 文件，不过要注意最前面的标题前的格式与目前主题文章保持一致
8. 编译发布新文章

   ```bash
   # 清除以前编译的文件
   hexo clean

   # 编译文章页面
   hexo generate

   # 方便写法
   hexo clean && hexo generate
   ```

9. 修改主题（以 Next 主题为例）
   1. 进入 Hexo 项目文件夹
   2. 下载主题
      ```bash
      git clone https://github.com/theme-next/hexo-theme-next themes/next
      ```
   3. 修改站点配置文件 `_config.yml` 中的 `theme: landscape` ，改为 `theme: next`
10. 将博客发布到代码仓库，例如 GitHub
11. 安装插件
    ```bash
    npm install hexo-deployer-git --save
    ```
12. 编辑项目的 `_config.yml` 文件，修改如下参数

    ```bash
    # Set your site url here. For example, if you use GitHub Page, set url as 'https://username.github.io/project'
    url: https://neonatal.gitee.io/

    # 发布仓库
    deploy:
        type: git
        repo: ssh地址 # 仓库的ssh地址
        branch: website # 要推送到的分支
    ```

13. 发布到代码库
    ```bash
    hexo d
    # or
    hexo delpoy
    ```
14. 不出意外应该可以通过 pages 访问到博客界面了
15. 注：`GitHub`建立`Github Pages`仓库命名应为`username.github.io`，而`Gitee`建立`Gitee Pages`仓库应为`username`，这样就可以通过 https://username.github.io 或 https://username.gitee.io 访问了。
16. 再注：也可以将整个项目放到`GitHub`上，更推荐这样做。

# 添加标签、分类等页面

添加页面的过程是类似的，下面以添加标签页为例：

1. 新建页面，用 Hexo 新建页面命令
   ```bash
   hexo new page "tages"
   ```
2. 打开 `/source/tages/index.md` ，设置其类型 `type` 值为“tages”
   ```bash
   title: 标签
   date: 2021-08-20 20:14:27
   type: "tags"
   ```
3. 打开 `themes/your_theme/_config.yml` ，将对应项取消注释即可，没有的话自己添加需要的页面名称
   ```yml
   menu:
     home: / || fa fa-home
     #about: /about/ || fa fa-user
     tags: /tags/ || fa fa-tags
     #categories: /categories/ || fa fa-th
     archives: /archives/ || fa fa-archive
   ```

# 文章url设置

hexo文章的url在博客根目录的 `_config.yml` 文件中进行配置，默认配置如下：

```yml
# 年/月/日/文章路径
permalink: :year/:month/:day/:title/
```

这里的 `:title` 为 `source/_post` 下的相对路径，但是这样的话很容易造成url中文乱码，和不同浏览器因为字符集的问题导致的url失效。

可以改为使用hash值生成文章url，会自动生成hash值，保证不重复且不会因为编码出错。

```yml
permalink: :year/:month/:day/:hash/
```

其他tag可见官方文档：[永久链接（Permalinks）](https://hexo.io/zh-cn/docs/permalinks)。

# 插入图片

1. 绝对引用
   - 少量使用图片时可用此方法，将图片文件放入 `your_blog/public/images` 文件夹下，使用 `![picture](/public/images/picture.jpg)` 方式直接引用。
   - 存在问题：图片管理麻烦，删除文章时图片仍在，引用关系混乱
2. 相对引用
   - 在 `your_blog/_config.yml` 文件中配置 `post_asset_folder: true` ，新建文章的时候建立同名文件夹，将图片放入
   - 使用 `![picture](article/picture.jpg)` 引用图片
   - 优点：管理比较方便
3. CDN 或图床引用
   - 所使用图片在网络图库上，直接使用图片链接即可，`![picture](https://website.com/picture.jpg)`
   - 缺点：可能会失效，可自己使用图床网站上传自己的图片得到链接

# 参考

- [Hexo 官方文档](https://hexo.io/zh-cn/docs/)
- [使用 Hexo+GitHub 搭建个人免费博客教程（小白向）](https://zhuanlan.zhihu.com/p/60578464)
- [Hexo 新建标签、分类、归档等页面](https://blog.csdn.net/weixin_41287260/article/details/97758641)
- [Hexo 博客插入图片的方法](https://www.cnblogs.com/hugochen1024/p/12570656.html)
- [hexo文章url设置](https://blog.csdn.net/qq_41942221/article/details/116007091)
