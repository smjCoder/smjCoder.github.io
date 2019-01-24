---
layout: post
title: Unity3D 无法 BuildAndRun
subtitle: 电脑 adb interface 驱动失效解决办法
date: 2019-01-23
author: JY
header-img: 
catalog: true
tags: 
    - Unity
---

> 来到新公司，电脑的系统为 Win7 x64，使用安卓测试机进行测试的时候，发现没有办法 BuildandRun，经过排查，发现是 adb interface 的驱动异常造成的，经过努力，将解决办法记录下来。



# 解决步骤

1. 在设备管理器中，找到驱动异常的 ADB Interface，右键， __“更新驱动程序软件”__
2. 选择 __“浏览计算机以查找驱动程序软件”__
![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-24-151312.png?x-oss-process=style/iPic)
3. 继续选择 __“从计算机的设备驱动程序列表中选择”__
![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-24-WechatIMG29.png?x-oss-process=style/iPic)
4. 不做任何选择，直接点击 __“下一步”__
5. 选择 __“从磁盘安装”__ ， __“浏览”__ Android SDK 目录中的 `android_winusb.inf` 文件，路径为 `%SDK目录%\extras\google\usb_driver`，点击 “确定”
![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-24-WechatIMG30.png?x-oss-process=style/iPic)
![](https://jy-blog.oss-cn-beijing.aliyuncs.com/blog/2019-01-24-WechatIMG31.png?x-oss-process=style/iPic)
6. 选择 __“Android Composite ADB Interface”__ ，点击 __“下一步”__
7. 忽略安装中的警告，选择 __“继续安装”__
8. 安装完成