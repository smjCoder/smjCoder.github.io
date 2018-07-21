---
layout: article
title: 控制重叠ScrollView中Cell的层级显示
key: 20180721
tags:
- 中文
- Unity3D
- 插件
toc: true
date: 2018-07-21 13:17:00
---
> 最近接到一个需求，需要实现一个居中放大并且有重叠部分的ScrollView，折腾了比较久的时间，最后使用了FancyScrollView的基本实现+个人的一些自定义实现了相关功能，在这里记录一下实现中间放大并进行层级管理的过程。

需求效果：

![](/images/FancyScrollView/ScrollView效果实现.png)

<!--more-->

从图中可以看出，居中显示的Cell位于最上层，也就是同一个父类（content）的最下方，但是FancyScrollView只实现了不重叠Cell的中间放大效果，因为只要不是重叠的Cell，这种放大效果是不会穿帮的。

但是一旦有重叠部分，这种效果就会穿帮的很厉害，具体表现就是后加载的Cell永远在先加载的Cell的前方。

### 实现方法

要保证中间的Cell永远在最前方，且其他Cell依次显示在后方，我们就需要对Cell的位置偏差值进行计算（距离中心线的距离，CellOffset）。

好在FancyScrollView是通过动画控制Cell的Anchor来实现Cell的位置变换的，所以我们能够很轻易的得到Cell的Anchor值，因为我的需求是纵向滑动，所以我只需要取得AnchorMin.y（AnchorMax.y也可以）,并将它与默认的中间值0.5F进行比较，距离越远，显示就越靠后。

### 核心方法

使用 __Transform__ 自带的 __SetSiblingIndex__ 方法

```c#
public override void UpdatePosition(float position)
{
	_currentPosition = position;
	animator.Play(scrollTriggerHash, -1, position);
	animator.speed = 0;
	this.transform.SetSiblingIndex(3 - (int)Mathf.Abs((button.gameObject.GetComponent<RectTransform>().anchorMin.y - 0.5F) * 10));
}
```

方法中间包含了一些常数（如：3），这个常数需要根据你动画设置的偏移值进行计算，以达到最好的效果。