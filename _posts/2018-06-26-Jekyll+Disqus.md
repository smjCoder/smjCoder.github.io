---
layout: article
title: Jekyll+Disqus给个人博客添加评论（施工中）
key: 20180625
tags:
- 中文
- 兴趣
- Jekyll
- Disqus
toc: true
date: 2018-06-26 19:25:00
---
> 我的博客是使用Jekyll搭建的，如果只具有展示博文的功能，阅读者就只能单纯的接受或者拒绝我的思想。
>
> 如果有评论呢？我一定会与阅读我博文的人产生思想的碰撞！

<!--more-->

#### 第一步：注册Disqus账号

简单的注册过程

![](/images/Jekyll+Disqus/dis_img.jpg)

进入注册界面，进行注册

![](/images/Jekyll+Disqus/dis_img5.jpg)

#### 第二步：添加你的网站

注册成功后，你会进入如下网站

![](/images/Jekyll+Disqus/dis_img1.jpg)

你需要设置两项内容

- Website Name：此项会显示在最终评论框的左上方位

![](/images/Jekyll+Disqus/dis_img2.png)

- Shortname：用于_config.yml文件

没看到这一项？你点击一下`Customize Your URL`就会出现。这就是你的`disqus_shortname`，设置后不可更改。

![](/images/Jekyll+Disqus/dis_img3.png)

设置完成后进入下个页面，根据情况选择收费模式

#### 第三步：生成并添加JS代码

进入Install Disqus页面，由于我用的是github+jekyll的静态博客，所以选择`universal code`即基本JS代码

![](/images/Jekyll+Disqus/dis_img3.jpg)

将Disqus生成的代码复制到`_includes`的`disqus_comments.html`中，如果没有这个HTML文件，就自己建一个空的HTML并把代码粘贴进去

#### 第四步：修改_config.yml文件

```yaml
disqus:
  shortname: # the Disqus shortname for the site
```

#### 第五步：简单配置

完成你的网站配置，在这里你可以进行一些设置来自定义你的评论样式

![](/images/Jekyll+Disqus/dis_img9.jpg)

#### 第六步：详细配置

你已经完成了你的网站评论配置，如果你还想进一步配置，可以进入设置界面进行修改

![](/images/Jekyll+Disqus/dis_img7.png)

#### 最后

推送你的网站库，就可以刷新你的博客查看效果了！