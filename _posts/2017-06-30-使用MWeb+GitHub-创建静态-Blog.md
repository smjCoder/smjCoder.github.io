---
layout: post
title: 使用 MWeb+GitHub 创建静态 Blog
subtitle: 
date: 2017-06-30
author: JY
header-img: 
catalog: true
tags:
    - MWeb
    - GitHub
    - Blog
---

# 示例博客

[JY Blog](http://junyuyuan.top)

# 注册GitHub

[GitHub 网站](https://github.com)

![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-26-033951.jpg?x-oss-process=style/iPic)

# 创建网站仓库

## 点击创建仓库按钮

![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-26-033953.jpg?x-oss-process=style/iPic)

## 创建仓库，输入仓库名称

![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-26-33954.jpg?x-oss-process=style/iPic)

## 创建成功

![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-26-033956.jpg?x-oss-process=style/iPic)

# 使用MWeb生成静态网站

## 将文档库转换成静态网站

![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-26-033957.jpg?x-oss-process=style/iPic)

## 设置静态网站信息

![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-26-033958.jpg?x-oss-process=style/iPic)

# Clone 你的 pages 到 MWeb 静态网站生成目录中

__注意：__ 如果你之前都已经可以正常用 git 来发布静态网站了，可以跳过这一步。如果不是，请看下面的步骤：

1. 用 MWeb 生成静态网站。
2. 去 “MWeb 偏好设置” - “通用设置” - “生成的静态网站保存位置” 那里，点 “在 Finder 中显示” 按钮，进入 MWeb 的静态网站输出文件夹，在文件夹内应该可以看到你已生成的网站。比如说我生成的网站是 “MWeb中文官网”，就会看到一个 “MWeb中文官网” 的文件夹，文件夹内就是生成的静态网站了。下面会以 “MWeb中文官网” 做例子。
3. 在命令行中进入 “MWeb 的静态网站输出文件夹”。如果你不知道怎么做，可以在 Finder 中选择 “MWeb中文官网” 文件夹的上一级文件夹，按 `CMD + C` 复制，再打开命令行窗口，键入 cd 命令，加一个空格，再按 `CMD + V` 粘贴路径，再按 Enter。。
4. 删除 “MWeb中文官网” 文件夹，然后在命令行中执行：`git clone '你的 git pages 的 repo' 'MWeb中文官网'`。 这里的 `'你的 git pages 的 repo' 'MWeb中文官网'` 请换成你自己的。在 MWeb 中使用 “清理并重新生成” 命令，重新生成静态网站。

__注意：__ 在你 git 发布出问题的时候，也可以用上面的 3、4、5 这三个步骤进行初始化。

# 配置发布脚本

## 在下图，在 “MWeb 偏好设置” - “扩展” - “发布脚本” 中配置。

![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-26-033959.jpg?x-oss-process=style/iPic)

## 点击加载例子

![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-26-034000.jpg?x-oss-process=style/iPic)

## 使用发布脚本

使用方法非常简单，右键网站分类，选择 “复制发布脚本命令并打开终端（Terminal）...”，当终端打开后，在终端中按快捷键 `Command + V` 即可。如图：

![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-26-034002.jpg?x-oss-process=style/iPic)