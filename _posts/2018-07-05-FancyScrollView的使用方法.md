---
layout: article
title: FancyScrollView的使用方法
key: 20180705
tags:
- 中文
- Unity3D
- 插件
toc: true
date: 2018-07-05 19:14:00
---
> FancyScrollView
>
> 一个通用的Unity ScrollView组件

<!--more-->

# FancyScrollView [![License](https://img.shields.io/badge/license-MIT-lightgrey.svg?style=flat)](http://mit-license.org)
一个通用的ScrollView组件，可实现高度灵活的Cell动画。还支持无限滚动。


![](/images/FancyScrollView/logo.png)
![](/images/FancyScrollView/screencast1.gif)
![](/images/FancyScrollView/screencast2.gif)

## 工作原理
FancyScrollView在更新单元格的位置时，将显示在屏幕上的范围内的值赋予每一个单元格。在单元格侧以0.0 ~ 1.0的值为基础，可以自由控制滚动中的外观。

## 使用方法
最简方法

- 向单元格传递数据的对象
- 滚动视图
- 单元格

以上是必须实现的方法。

### 相关脚本
定义用于将数据传递给单元格的对象。
```csharp
public class MyCellDto
{
    public string Message;
}
```
继承FancyScrollView，实现自己的滚动视图。

```csharp
using UnityEngine;
using System.Linq;
using FancyScrollView;

public class MyScrollView : FancyScrollView<MyCellDto>
{
    [SerializeField]
    ScrollPositionController scrollPositionController;

    void Awake()
    {
        base.cellData = Enumerable.Range(0, 50)
            .Select(i => new MyCellDto { Message = "Cell " + i })
            .ToList();

        scrollPositionController.SetDataCount(base.cellData.Count);
        scrollPositionController.OnUpdatePosition(base.UpdatePosition);
    }
}
```
继承FancyScrollViewCell并实现自己的单元格
```csharp
using UnityEngine;
using UnityEngine.UI;
using FancyScrollView;

public class MyScrollViewCell : FancyScrollViewCell<MyCellDto>
{
    [SerializeField]
    Text message;

    public override void UpdateContent(MyCellDto itemData)
    {
        message.text = itemData.Message;
    }

    public override void UpdatePosition(float position)
    {
        // position 是 0.0 ~ 1.0 的值
        // position 可以自由控制单元格的外观
    }
}
```
### Inspector设置
![](/images/FancyScrollView/inspector.png)
#### My Scroll View
|---------------------+-----------------------------------|
| 属性  | 说明   |
| :------------- | :----------------------------------------------------------- |
| Cell Interval  | 将单元格之间的间隔指定为 float.Epsilon ~ 1.0之间。           |
| Cell Offset    | 指定单元格的偏移。例如，指定了0.5，在滚动位置为0的情况下，最初的单元格的位置是0.5。 |
| Loop           | 开启后实现无限滚动。                                         |
| Cell Base      | 指定单元格的Prefab。                                         |
| Cell Container | 指定单元格的父元素。                                         |
|---------------------+-----------------------------------|

#### Scroll Position Controller
|---------------------+-----------------------------------|
| 属性                      | 说明                                                         |
| :------------------------ | :----------------------------------------------------------- |
| Viewport                  | 指定作为视图窗口的RectTransform。在指定的RectTransform的范围内进行手势的检测。 |
| Direction Of Recognize    | 将识别手势的方向指定为Vertical或Horisontal。                 |
| Movement Type             | 指定内容超过滚动范围的移动时使用的行为。                     |
| Scroll Sensitivity        | 指定滚动的灵敏度。                                           |
| Inertia                   | 指定惯性的打开/关闭。                                        |
| Deceleration Rate         | Inertia 只有在打开的情况下有效。指定减速率。                 |
| Snap - Enable             | Snap 如果有效的话，请打开。                                  |
| Snap - Velocity Threshold | 指定Snap开始的阈值。                                         |
| Snap - Duration           | 用秒数指定Snap时的移动时间。                                 |
| Data Count                | 需要展示的数据总数量，一般从脚本内设置。                     |
|---------------------+-----------------------------------|

## Q&A

#### 即使数据的数量很多，展示也没有问题么？
因为单元格只生成显示所需的数量，所以数据件数对性能的影响很小。
比起数据的数量。每个单元格之间的间隔（同时存在的单元格的数量）和单元格的展示效果，对展示有一定影响。

#### 我能够自己控制滚动位置么？
滚动的位置可以自由控制。可以将例子中使用的ScrollPositionController 更换成自己的实现。

#### 可以接收到在单元格中发生的事件么？
可以使用在单元格中发生的所有事件。
请参考例子（[Examples/02_CellEventHandling](https://github.com/setchi/FancyScrollView/tree/master/Assets/FancyScrollView/Examples/02_CellEventHandling)）。

#### 能够使用单元格无限滚动么？
对于使用无限滚动。步骤如下。
1. 将ScrollView 的「Loop」打开后，将单元格置于循环状态。
2. 在使用例子中 ScrollPositionController 的情况下，将「Movement Type」设定为「Unrestricted」，就变成无限滚动了。

![](/images/FancyScrollView/infiniteScrollSettings.png)

请参考例子（[Examples/03_InfiniteScroll](https://github.com/setchi/FancyScrollView/tree/master/Assets/FancyScrollView/Examples/03_InfiniteScroll)）。

## 开发环境
Unity 2017.2.0f3

## LICENSE
MIT