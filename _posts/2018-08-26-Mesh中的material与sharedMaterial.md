---
layout: article
title: Mesh中的material与sharedMaterial
key: 20180826
tags:
- 中文
- Unity3D
toc: true
date: 2018-08-26 13:30:00
---
> 改变Mesh的material时的注意事项

<!--more-->

也是解决Bug时发现的问题，我们的美术发现他们使用的材质在Editor下和客户端下的offset移动速度不一样，在手机端明显要快两倍到三倍。

我们的offset平移代码是这样的

```c#
void Update()
{  
	_material.mainTextureOffset += new Vector2(Speed.x, Speed.y) * Time.deltaTime;
}
```

本来我以为，是因为手机帧率不稳定导致的，但是 `Time.deltaTime` 和刷新率的不稳定，最多造成轻微的误差或者卡顿，绝对不会出现手机端比编辑器模式快两三倍的问题，所以这个原因被排除了。

直到我发现

```c#
void Start()
{
    _material = gameObject.GetComponent<Renderer>().GetMaterial();
}
```

我们的material并不是通过 `.material` 获得的，所以我跟进 `GetMaterial()` 方法，发现

```c#
public static Material GetMaterial(this Renderer renderer)
{
#if UNITY_EDITOR
	return renderer.material;
#else
	return renderer.sharedMaterial;
#endif
}
```

原来我在编辑器下和在手机端使用的material是不同的，难道问题出在这？

没错，问题就在这里， `material` 和 `sharedMaterial` 

>sharedMaterial 是共用的 Material，称为共享材质。修改共享材质会改变所用使用该材质的物体，并且编辑器中的材质设置也会改变。
>
>material 是独立的 Material，返回分配给渲染器的第一个材质。修改材质仅会改变该物体的材质。如果该材质被其他的渲染器使用，将克隆该材质并用于当前的渲染器。

这样，快两三倍的原因就找到了...

因为offset的改变是通过 `.mainTextureOffset += value` 的方式实现的，所以在手机端同场景中，材质球被复用一次，offset的累加就会快一倍，复用两次，offset的累加就会快两倍

解决办法也比较简单

```c#
void Update()
{
    _offsetX += Time.deltaTime * Speed.x;
	_offsetY += Time.deltaTime * Speed.y;
	_material.mainTextureOffset = new Vector2(_offsetX, _offsetY);
}
```

使用直接赋值的方式就能够解决 `.mainTextureOffset += value` 的累加重复问题

__注：这种方法只能用于复用的材质纹理移动速度统一且同步的状态，所以一定要和技美约定好材质球的使用方式才行！__



>#### 使用 material 时的内存泄漏问题
>
>每一次引用 Renderer.material 的时候，都会生成一个新的 material 到内存中去，销毁物体的时候需要我们手动去销毁该material，否则会一直存在内存中。
>
>官方文档说：
>
>This function automatically instantiates the materials and makes them unique to this renderer. It is your responsibility to destroy the materials when the game object is being destroyed. Resources.UnloadUnusedAssets also destroys the materials but it is usually only called when loading a new level.
>
>此方法自动实例化该材质并且使其成为该渲染器独有的材质。当该游戏物体被删除时，你应该手动删除该材质。当替换场景调用 Resources.UnloadUnusedAssets 也可以删除该材质。
>
>网上的解决方案如下：
>
><http://www.xuanyusong.com/archives/2530>
>
>编辑器下使用 material， 其他平台使用 sharedMaterial
>
>```c#
>public static Material GetMaterial(Renderer render)  
>{  
>#if UNITY_EDITOR  
>	return render.material;  
>#else  
>	return render.sharedMaterial;  
>#endif  
>}
>```
>
><http://www.jianshu.com/p/ababf547d992>
>
>如果是主角这一类gameobject身上需要修改材质的属性或者shader属性比较多的时候，可以第一次使用material，这样可以动态的生成一个material实例，然后再使用sharedmaterial，动态的修改这个新生成的material，而且不会创建新的material。
>
><https://blog.uwa4d.com/archives/optimzation_memory_2.html>
>
>一般情况下，资源属性的改变情况都是固定的，并非随机出现。比如，假设GameObject受到攻击时，其Material属性改变随攻击类型的不同而有三种不同的参数设置。那么，对于这种需求，我们建议你直接制作三种不同的Material，在Runtime情况下通过代码直接替换对应GameObject的Material，而非改变其Material的属性。这样，你会发现，成百上千的instance Material在内存中消失了，取而代之的，则是这三个不同的Material资源。