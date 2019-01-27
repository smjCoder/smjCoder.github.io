---
layout: post
title: SourceTree操作指南
subtitle: 
date: 2018-04-13
author: JY
header-img: img/post-bg-map.jpg
catalog: true
tags:
    - Git
    - SourceTree
---

# SourceTree 是什么
拥有可视化界面的项目版本控制软件，适用于git项目管理Windows、Mac可用

下载地址：[Windows](https://downloads.atlassian.com/software/sourcetree/windows/ga/SourceTreeSetup-2.4.8.0.exe) \ [Mac](https://downloads.atlassian.com/software/sourcetree/Sourcetree_2.7.1d.zip)

#	获取项目代码

## 点击克隆/新建

![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-26-ilovepdf_com.jpg?x-oss-process=style/iPic)
## 在弹出框中输入项目地址，http 或者 ssh 地址都可以

![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-26-ilovepdf_com-1.jpg?x-oss-process=style/iPic)

如果箭头指向的仓库类型表明“这不是一个标准的Git仓库”，可能是有以下原因：

项目地址获取错误没有项目访问权限

## 点击“克隆”，等待项目克隆完成，完成后，左侧只有一个分支 master

![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-26-ilovepdf_com-2.jpg?x-oss-process=style/iPic)

克隆完成后，得到的是发布后的 master 源码，如果想要获取最新的正在开发中的源码，需要对项目流进行初始化，点击“Git工作流”

![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-26-ilovepdf_com-3.jpg?x-oss-process=style/iPic)

直接点“确定”，获取 develop 分支源码
 
![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-26-ilovepdf_com-4.jpg?x-oss-process=style/iPic)

__开发任务都是在develop分支上完成的__

# Git‐ﬂow 工作流

## 分支共有5种类型

* master，最终发布版本，整个项目中有且只有一个
* develop，项目的开发分支，原则上项目中有且只有一个
* feature，功能分支，用于开发一个新的功能
* release，预发布版本，介于 develop 和 master 之间的一个版本，主要用于测试
* hotfix，修复补丁，用于修复 master 上的 bug，直接作用于 master

master 和 develop 上文中已介绍过，当开发中需要增加一个新的功能时，可新建 feature 分支，用于增加新功能，并且不影响开发中的 develop 源码，当新功能增加完成后，完成 feature 分支，将新功能合并到 develop 中，更新 develop 上的代码

### feature 分支（新功能开发）

首先当前开发分支指向 develop，点击“Git工作流”，

![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-26-ilovepdf_com-5.jpg?x-oss-process=style/iPic)

选择“建立新的功能”

![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-26-ilovepdf_com-7.jpg?x-oss-process=style/iPic)
 
在预览中可看到，feature 分支是从 develop 分出的，输入功能名称，点击确定，项目结构中增加 feature 分支，并且当前开发分支指向新建的 
feature 分支

![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-26-ilovepdf_com-8.jpg?x-oss-process=style/iPic)

在`F_add_feature`分支下进行开发任务，并提交

![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-26-ilovepdf_com-9.jpg?x-oss-process=style/iPic)

以上操作共提交3次，现项目文件夹下共三个文件

![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-26-ilovepdf_com-10.jpg?x-oss-process=style/iPic)
 
当切换为 develop 分支后，会发现，在 develop 下并没有新增的三个文件，说明在 feature 下进行操作，并不影响 develop 分支源码

完成 feature 开发后，将 feature 中的源码合并到 develop 分支。将当前分支指向`F_add_feature`分支，点击“Git工作流”，选择“完成功能”

![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-26-ilovepdf_com-11.jpg?x-oss-process=style/iPic)
![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-26-ilovepdf_com-12.jpg?x-oss-process=style/iPic)
 
预览中，表明 feature 分支将合并到 develop，点击确定，进行提交合并，合并成功后
 
![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-26-ilovepdf_com-13.jpg?x-oss-process=style/iPic)

需要再增加新的功能时，重复以上操作即可

当多人协作开发时，可能会出现，不同人员对同一文件进行操作，从而引起合并冲突，对这种情况进行模拟，在当前新建两个 feature，分别对 feature_1.txt 文件进行修改，然后分别合并

![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-26-ilovepdf_com-14.jpg?x-oss-process=style/iPic)

feature_1 在 feature_1.txt 下做如下操作

![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-26-ilovepdf_com-15.jpg?x-oss-process=style/iPic)

feature_2 在 feature_1.txt 下做如下操作

![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-26-ilovepdf_com-16.jpg?x-oss-process=style/iPic)

先后合并`F_feature_1`和`F_feature_2`，会出现冲突

![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-26-ilovepdf_com-17.jpg?x-oss-process=style/iPic)

点击”关闭“，查看未提交的更改，提示 feature_1.txt 出现冲突

![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-26-ilovepdf_com-18.jpg?x-oss-process=style/iPic)

打开 feature_1.txt

![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-26-ilovepdf_com-19.jpg?x-oss-process=style/iPic)

出现`<<<<<<< HEAD`、`=======` 、`>>>>>>>feature/F_feature_2`，`HEAD`和`=====`之间表示当前分支下的代码，`=====`和`>>>>>>> feature/F_feature_2`之间表示要合并的分支下的代码，`>>>>>>> feature/F_feature_2`表示了要合并的分支的分支名称，根据情况区分要保留的代码，要删除的代码，最后再删除`<<<<<<< HEAD`、`=======`、`和>>>>>>> feature/F_feature_2`

![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-26-ilovepdf_com-20.jpg?x-oss-process=style/iPic)

将修改的代码再进行一次提交

![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-26-ilovepdf_com-21.jpg?x-oss-process=style/iPic)

一旦出现 feature 合并冲突，要合并的 feature 分支不会被删除，如`F_feature_2`，确保合并没有问题后，可手动删除`F_feature_2`

### release分支（准备发布新版本）

当开发到一定阶段，可以发布测试版本时，可以从 develop 分支，建立 release 分支，进入预发布测试阶段。点击“Git工作流”，选择“建立新的发布版本”

![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-26-ilovepdf_com-22.jpg?x-oss-process=style/iPic)
![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-26-ilovepdf_com-23.jpg?x-oss-process=style/iPic)

预览中可以看到，release 是从 develop 分出的，输入发布版本名‘R_v1.0’，点击确定

![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-26-ilovepdf_com-24.jpg?x-oss-process=style/iPic)

R_v1.0 为阶段性发布版本，主要用于发布前进行测试，后续的开发工作仍旧在 develop 上进行，如果在测试过程中发现问题，直接在 release  上进行修改，修改完成后进行提交

对 release 分支 R_v1.0 进行两次修改后，测试完成，可以进行正式发布，在当前分支指向 R_v1.0 分支下，点击“Git工作流”，选择“完成发布版本”

![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-26-ilovepdf_com-25.jpg?x-oss-process=style/iPic)
![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-26-ilovepdf_com-26.jpg?x-oss-process=style/iPic)

在预览中可以看到，R_v1.0 向 develop 和 master 分别合并，点击确定，完成正式发布

![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-26-ilovepdf_com-27.jpg?x-oss-process=style/iPic)

完成合并后，默认指向 develop 为当前分支，master 增加多个版本更新，将 master 分支推送到 origin，完成线上发布

### hotfix分支（修复补丁）

正式版本发布后，develop 可继续进行后续开发，当正式版本出现问题时，需要进行问题的修改，可以在 master 分支建立修改补丁 hotfix

将当前分支切换到 master，点击“Git工作流”，选择“建立新的修复补丁”

![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-26-ilovepdf_com-28.jpg?x-oss-process=style/iPic)
![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-26-ilovepdf_com-29.jpg?x-oss-process=style/iPic)

预览中 hotfix 分支是从 master 拉去出来的，输入修复补丁名，点确定

![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-26-ilovepdf_com-30.jpg?x-oss-process=style/iPic)

在该分支下进行 master 的问题修改，修改完成后进行提交。当所有补丁问题修改完成后，点击“Git工作流”，选择“完成修复补丁”

![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-26-ilovepdf_com-31.jpg?x-oss-process=style/iPic)
![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-26-ilovepdf_com-32.jpg?x-oss-process=style/iPic)

预览中，`H_ﬁx_1`向 master 和 develop 分别合并，点击确定，完成分支合并

![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-26-ilovepdf_com-33.jpg?x-oss-process=style/iPic)

合并完成后，默认当前分支为 develop，master 分支有版本需要更新， 当前分支切换为 master，进行推送，完成补丁修复

在完成发布版本和完成修复补丁时，如果遇到冲突，可仿照冲突修改办法，再进行后续操作
