---
layout: post
title: Cordova 开发 iOS 说明书
subtitle: 
date: 2016-06-30
author: JY
header-img: 
catalog: true
tags:
    - iOS
    - Cordova
---

# 安装及项目创建

> 教程目录如下,请根据自己进度自行选择阅读
> 
> 1. 拥有 X-Code
> 2. 安装 node.js 环境
> 3. 使用终端安装 Cordova
> 4. 创建及运行项目

## 一. 安装 X-Code

## 二. 安装 Node.js 环境

首先进入官网下载 [Node.js](https://nodejs.org/en/)

![14815973227023](https://jy-blog.oss-cn-beijing-internal.aliyuncs.com/blog/2019-01-27-063416.png?x-oss-process=style/iPic)

## 三. 使用终端命令安装 Cordova

打开终端输入安装命令：

```
sudo npm install -g cordova
```
输入电脑密码后等待一会就能安装成功了。如下图

![14815976759197](https://jy-blog.oss-cn-beijing-internal.aliyuncs.com/blog/2019-01-27-063417.png?x-oss-process=style/iPic)

如未成功，请参考如下解决方案

* 如果没有安装 [GIT Client](https://git-scm.com)，请自行下载安装。
* 去 [Cordova 命令行帮助](http://cordova.apache.org/docs/en/4.0.0/guide/cli/index.html#The%20Command-Line%20Interface)查找关于你出问题的命令

## 四. 创建项目

1. 首先请建立一个文件夹，并通过终端 cd 进去 不然创建的项目在根目录下 你很难找到它
![14815982414230](https://jy-blog.oss-cn-beijing-internal.aliyuncs.com/blog/2019-01-27-063418.png?x-oss-process=style/iPic)
2. 通过终端命令创建一个项目
```
cordova create firstCordovaDoc com.aimi.firstCordova firstCordova
```
参数解释：
	* `firstCordovaDoc`:对应你整个项目的文件夹名称
	* `com.aimi.firstCordova`:对应 ios 工程的 bundleID
	* `firstCordova`:对应 ios 工程名
3. 为项目创建 ios 工程
```
cordova platform add ios
```
![14815990664527](https://jy-blog.oss-cn-beijing-internal.aliyuncs.com/blog/2019-01-27-063419.png?x-oss-process=style/iPic)
此时在项目文件夹下的`platforms`文件夹中会多出`ios`文件夹，进入后就可以看到ios工程了。
![14815992218387](https://jy-blog.oss-cn-beijing-internal.aliyuncs.com/blog/2019-01-27-063420.png?x-oss-process=style/iPic)
4. 最后我们来试用一下`Cordova`

`Cordova`的开发是使用h5开发，所以我们要找到其入口`Index.html`，如下图
![14815994855314](https://jy-blog.oss-cn-beijing-internal.aliyuncs.com/blog/2019-01-27-063421.png?x-oss-process=style/iPic)
__注意：是在`Staging`文件夹下的`Index.html`文件，而非根目录下的那个__

简单写一点html代码，替换index.html中的代码，看看混合开发的h5 app应用

```
<!DOCTYPE html>
<html lang="zh-CN">
    <head>
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
            <title>页面标题</title>
    </head>

    <body>
         <h1 align="center">
            YO!Man!
        </h1>
    </body>
</html>
```

# 支持入参及调回的插件开发

> 教程目录如下,请根据自己进度自行选择阅读
> 
> 1. 创建 ios 插件源文件
> 2. 实现插件的 OC 代码编写(包括 OC 回调 JS)
> 3. 配置 config.xml 文件
> 4. 简述 Cordova 插件运作原理
> 5. 写 JS 代码来调用我们的插件进行测试

## 一. 创建 ios 插件源文件

1. 首先找到`Plugins`文件夹，该文件夹下存放的是所有插件的源码文件
2. 创建我们的插件文件夹 com.anCordova.anAlert 文件夹，插件文件夹的命名规范还是要遵守一下的，类似 bundleID 很好理解
![14816076286904](https://jy-blog.oss-cn-beijing-internal.aliyuncs.com/blog/2019-01-27-063422.png?x-oss-process=style/iPic)

3. 在刚创建的文件夹下新建代码文件，继承cordova框架的`CDVPlugin`
![14816076963678](https://jy-blog.oss-cn-beijing-internal.aliyuncs.com/blog/2019-01-27-63423.png?x-oss-process=style/iPic)
4. 改动一下`#import`，因为`.h`文件报错，这是 Cordova 框架的bug，毕竟是改写的原生代码。

```
#import <Cordova/CDVPlugin.h>

@interface AMAlertHelper : CDVPlugin

@end
```

## 二. 实现插件的 OC 代码编写（包括 OC 回调 JS）

> 在AMAlertHelper.m实现alert调用及回调js方法
> 
> `.h`里需要声明方法

```
-(void)showAlertWithTitle:(CDVInvokedUrlCommand *)command{

    if (command.arguments.count>0) {
        //获取到入参数组中的第一个元素
        //自由约定入参数组的顺序
        NSString* title = command.arguments[0];

        //创建alertVC
        UIAlertController* alertVC = [UIAlertController alertControllerWithTitle:title message:nil preferredStyle:UIAlertControllerStyleAlert];
        UIAlertAction* action = [UIAlertAction actionWithTitle:@"确定" style:UIAlertActionStyleDefault handler:^(UIAlertAction * _Nonnull action) {
            //创建一个回调对象并附上String类型参数
            CDVPluginResult* pluginResult = [CDVPluginResult resultWithStatus:CDVCommandStatus_OK messageAsString:@"Hey!I'm OC!"];
            //通过cordova框架中的callBackID回调至JS的回调函数上
            [self.commandDelegate sendPluginResult:pluginResult callbackId:command.callbackId];
        }];
        [alertVC addAction:action];
        [self.viewController presentViewController:alertVC animated:YES completion:nil];
    }else{
        //如果没有入参,则回调JS失败函数
        CDVPluginResult* pluginResult = [CDVPluginResult resultWithStatus:CDVCommandStatus_ERROR messageAsString:@"没有入参alert title"];
        [self.commandDelegate sendPluginResult:pluginResult callbackId:command.callbackId];
    }

}
```
方法的入参必须是`CDVInvokedUrlCommand`对象，我们经常用到的参数有：
`callbackID`对应我们需要回调js时，指定发送的函数 id
`arguments`对应 JS 调用插件时给我们的入参

## 三. 配置 config.xml 文件

为了让 JS 能够调用我们的 OC 类，我们必须配置`config.xml`文件
注意：是`Staging`下的 config 文件
添加如下代码

```
<feature name="ocAlertModel">
        <param name="ios-package" value="AMAlertHelper" />
</feature>
```
`ocAlertModel`为我们给 OC 类命名的实例对象名称
`AMAlertHelper`为插件 OC 类名

## 四. 简述 Cordova 插件运作原理

这里简单解释下为什么要配置`config.xml`文件及 Cordova 插件原理

当我们自己做原生的`UIWebView`于 JS 做交互时,可以通过注入模型的方式来做

我们会在`UIWebView` 加载完成的代理中写上如下代码

```
self.jsContext = [webView valueForKeyPath:@"documentView.webView.mainFrame.javaScriptContext"];
AMAlertHelper * ocAlertmodel  = [[AMAlertHelper alloc] init];
self.jsContext[@"AMAlertModel"] = ocAlertmodel;
ocAlertmodel.jsContext = self.jsContext;
ocAlertmodel.webView = self.webView;
```
这样 WebView 中就有了我们的`ocAlertmodel`对象,那么当 JS 代码有调用`ocAlertmodel`对象的方法,则会进入到模型的实例方法中,完成 JS 对 OC 的调用

Cordova 插件同理,配置了`config.xml`后，Cordova 则会在编译时,将我们的插件类以模型方式注入到它的 WebView 中,并且对象名称就是我们配置的`name`属性
JS 在调用时就直接使用该对象名进行调用

## 五. 写 JS 代码来调用我们的插件进行测试

回到`Staging`下的`index.html`文件，直接替换如下代码

```
<!DOCTYPE html>
<html>
    <head>
        <title>AMAlert</title>
        <meta http-equiv="Content-type" content="text/html; charset=utf-8">
            <script type="text/javascript" charset="utf-8" src="cordova.js"></script>
            <script type="text/javascript" charset="utf-8">

            //调用OC插件方法
            function alertShow() {
                //以字符串形式调用OC注入模型的实例方法
                //通过cordova 将我们的模型名称,方法名,参数,成功回调的func及失败回调的func 传入
                cordova.exec(alertSuccess,alertFail,"ocAlertModel","showAlertWithTitle",["Hey,I'm JS!"]);
            }

            //调用成功的回调函数
            function alertSuccess(msg) {
                alert(msg);
            }

            //调用失败的回调函数
            function alertFail(msg) {
                alert('调用OC失败: ' + msg);
            }
            </script>
    </head>

    <body style="padding-top:50px">
        <button style="font-size:17px;" onclick="alertShow();">调用OC插件</button> <br>
    </body>
</html>
```
到此为止,简易插件的开发已经完成了,最终效果走一下

![14816089994096](https://jy-blog.oss-cn-beijing-internal.aliyuncs.com/blog/2019-01-27-063423.gif?x-oss-process=style/iPic)

## 六. 将 oc 方法映射成 js 代码上,并将插件打包供 h5 开发人员使用

完成以上这些只是插件开发好了,然而问题是

* js 调用 oc 插件是通过字符串形式的,我们不能这么低端,要让 h5 开发者通过 js 语言直接调用插件
* h5 的开发人员并没有安装 x-code 那么高端大气上档次的软件,我们需要将插件单独打包给他们

# 插件打包及映射 js 代码

> 教程目录如下,请根据自己进度自行选择阅读
> 
> 1. 创建插件打包文件夹及必要文件
> 2. 编写 JS 代码(oc 语法映射至 js 语法)
> 3. 配置 plugin.xml 文件
> 4. 添加本地插件,通过 js 的语法去调用插件.

## 一. 创建插件打包文件夹及必要文件

> 在桌面处创建插件打包文件夹 cordova-amAler-plugin-ios (遵守命名规范)，并创建子文件夹及子文件如下图，将之前开发插件的代码(AMAlertHelper.h 及.m)复制到 ios 文件夹下
> 
> 请创建好这样的结构后再进行后续动作

![14816096596614](https://jy-blog.oss-cn-beijing-internal.aliyuncs.com/blog/2019-01-27-063425.png?x-oss-process=style/iPic)

## 二.编写 JS 代码

之前在`index.html`调用插件时,用的是字符串形式的方法名.这里写一个将方法映射至 JS 调用的代码

其原理就是创建一个 JS 对象指向我们的 OC 对象，并且给 JS 对象创建一个实例方法指向我们的 OC 方法

> 打开之前创建的`AlertHelper.js`文件,并进行编写代码如下

```
var exec = require("cordova/exec");

//定义一个类名为AlertHelper的对象构建函数
function AlertModel() {};

//给AlertModel添加一个js方法jsAlertShow
//映射至之前写的方法上 ocAlertModel是我们给OC类命名的实例对象名称
//showAlertWithTitle是我们OC的方法
//option是入参
AlertModel.prototype.jsAlertShow = function (success,fail,option) {
     exec(success, fail, 'ocAlertModel', 'showAlertWithTitle', option);
};

//new一个AlertModel的类对象，并赋值给module.exports
var alertModel = new AlertModel();
module.exports = alertModel;
```

## 三. 配置 plugin.xml 文件

配置`plugin.xml` 就是为了告诉 Cordova 我们的文件路径在哪,我们的 oc 类名是什么，oc 对象名是什么，js 类名及 js 对象名是什么，等等。这样 Cordova 才能在安装插件时，正确的进行指向。

具体哪些配置对应了什么意思，在代码中已写了注释

> 打开前面创建的`plugin.xml`文件，并添加如下代码

```
<?xml version="1.0" encoding="UTF-8" ?>
<!--    id需要和文件夹名称保持一致 (插件的id)-->
<plugin xmlns="http://phonegap.com/ns/plugins/1.0"
    id="com.amCordova.amAlertHelper"
    version="1.0.0">
    <engines>
        <engine name="cordova" version=">=3.3.0" />
    </engines>

    <name>alertHelper</name>
    <description>插件的描述</description>

<!--    对应js映射文件的地址及名称-->
    <js-module src="www/AlertHelper.js" name="alertModel">

<!--    js调用时的对象名称-->
        <clobbers target="alertModel" />
    </js-module>

<!--    ios所有文件的存放地址-->
<!--如果有图片的话也需要在这里配置,前缀是source-file-->
    <platform name="ios">
        <source-file src="src/ios/AMAlertHelper.m" />
        <header-file src="src/ios/AMAlertHelper.h" />

        <config-file target="config.xml" parent="/widget">

<!--            插件映射至ios的类名-->
        <feature name="ocAlertModel">
            <param name="ios-package" value="AMAlertHelper" />
        </feature>
        </config-file>
    </platform>
</plugin>
```

## 四. 将本地插件添加至 Cordova 项目中测试我们的插件

至此为止,插件的开发已经全部完成了,所谓的打包其实就是我们那个带配置文件的插件文件夹

新建一个 Cordova 项目并且将我们的本地插件添加进去进行测试一下

1. 新建一个 Cordova 项目并且添加 ios 工程 (这里不详说了)
2. 进入到项目的目录下
```
cd /Users/aimi/Desktop/cordova/testCordovaDoc
```
3. 添加刚刚创建的本地插件包
```
cordova plugin add
/Users/aimi/Desktop/cordova/MyPlugin/com.amCordova.amAlert
```
4. 进行测试,通过js的语法去调用插件
```
alertModel.jsAlertShow(alertSuccess,alertFail,["Hey,I'm JS!"]);
```

> 替换 index.html 代码如下

```
<!DOCTYPE html>
<html>
    <head>
        <title>AMAlert</title>
        <meta http-equiv="Content-type" content="text/html; charset=utf-8">
            <script type="text/javascript" charset="utf-8" src="cordova.js"></script>
            <script type="text/javascript" charset="utf-8">

            //调用OC插件方法
            function alertShow() {
                //通过js代码调用
                alertModel.jsAlertShow(alertSuccess,alertFail,["Hey,I'm JS!"]);
            }

            //调用成功的回调函数
            function alertSuccess(msg) {
                alert(msg);
            }

            //调用失败的回调函数
            function alertFail(msg) {
                alert('调用OC失败: ' + msg);
            }
            </script>
    </head>

    <body style="padding-top:50px">
        <button style="font-size:17px;" onclick="alertShow();">调用OC插件</button> <br>
    </body>
</html>
```

