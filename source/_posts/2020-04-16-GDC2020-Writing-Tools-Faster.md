---
title: GDC 2020 - Writing Tools Faster
date: 2020-04-16 09:33:58
tags:
typora-root-url: 2020-04-16-GDC2020-Writing-Tools-Faster
---

> 这是GDC2020上的一个分享，主题是“更快速地开发工具，加速工具开发的设计决策”
>
> 分享嘉宾是Niklas Gray，Our Machinery CTO
>
> 源视频地址 <https://www.youtube.com/watch?v=yYq_dviv1B0>

## 首先是分享嘉宾的自我介绍

我叫Niklas Gray，我是写游戏引擎的；参与开发的引擎如下：

- Diesel ，游戏开发商Grin 内部用引擎，代表作有Ghost Recon(幽灵行动）和PayDay（收获日）
- Bitsquid ，一款商业引擎，代表作有Vermintide (末日鼠疫)和Helldivers（绝地战兵）
- Stingray ，AutoDesk在2014年收购来Bitsquid，Stingray是Bitsquid的更名版本
- The Machinery ，Niklas从2017年开始开发的新引擎，特性是Lightweight、Flexible、Modular and extensible

## 本次主题分享内容

- 为什么工具开发这么难？
- 我们能够做些什么

## 工具开发失败简史

### Bitsquid 1.0 

- 引擎只定义文件格式，工具完全由用户自己开发，但用户其实并不想开发工具

### Bitsquid 2.0 

- 决定使用WinForms快速的开发工具
- 但WinFroms的UI非常丑陋
- 由于没有清晰的规划，随着时间累积项目难以维护(不过对用户来说还是很好用的)

### Bitsquid 3.0 

- 决定做出改变，在排序了几个UI库后，最终选择了看起来还不错的WPF
- 但没想到工具开发时间变长了
- WPF和WinForms相比门槛变高，因为要了解WPF、XAML等...
- 设想用WPF完全重写WinFroms工具，最终未能完成

### Stingray 

- 希望开发平台无关的编辑器，这意味着不能使用WPF；考虑到可以购买一些Web技术和不少人有Web开发经验；最终决定开发一款Web编辑器
- 但该技术选型的开发栈很长，涉及到C#, Lua, C++, WPF,WinForms, Chromium,Qt, JavaScript,Angular, WebSockets 
- 没想到开发工具的时间更长了
- 基于web重写工具的设想又落空来
- 最终结果是WinForms、WPF、Web各种工具同时存在，情况变得更加复杂

### Bitsquid/Stingray 的核心问题

- 不断更改开发框架
- 工具开发耗时长以至于无法完成
- 工具运行时性能差劲

**结果就是：非常差的工具！**

那么我们是如何做出改变的？请继续往下看。

### 为什么改变开发框架

- 某种程度上，是决策错误
- 某种程度上，是因为技术在不断变迁；继续使用过时的技术会非常痛苦

### 为什么工具开发会如此耗时

- 每一个小工具都要有UI，都要经过设计、编码和测试
- Undo, copy/paste, serialize, drag-and-drop 等特性实现起来比较耗时
- 技术栈太长，难以理解

  - 当出现bug时，bug发生在Angular, JavaScript, WebSocket, Chromium, C#, Lua 还是C++? 
  - 总之一切都很复杂
- 工具开发只能由某些人来做，当引擎工程师想要加个功能时，因为太复杂而无法独自完成

### 为什么会遇到性能问题

- 传统的web开发经验并不适用
  - 常规Web开发一般不太关心性能
  - 游戏场景通常比较复杂，编辑器开发会更复杂
- 刚开始东西少，常规的web并没有遇到什么问题；但随着东西越来越多，很快性能就是要急需解决的问题。但这个时候已经不是通过profile找到热点、解决热点函数那么简单了。往往是需要对某个系统进行重写。
- 过长的开发栈使得查问题变得非常困难

### 我们如何解决这些问题

- 基于良好设计的数据模型，自动进行undo、redo、copy、paste
- 简化技术栈
  - 让事情更加明显、容易理解
  - 避免更改开发框架
  - 控制好性能
- 基于数据自动生成UI并重用UI
  - Properties, Tree, Graph 等
  - 不用再为创建UI做很多工作

## 新的数据模型

### The Truth

- 用统一的方式描述所有数据

- 基于数据模型定义Undo这样的操作

- 每一个对象都有类型和属性

  ![1587267799367](/1587267799367.png)

  > 注：the truth是Machinery引擎底层数据模型。[here](https://ourmachinery.com/post/the-story-behind-the-truth-designing-a-data-model/)可以了解背后故事

### 多线程无锁访问

- 改变数据分成两个步骤：write、commit

- 写时复制

- 读时不必加锁

  ![1587268067315](/1587268067315.png)
  

### Undo
- On Commit时，将新旧object保存在undo scope

- On Undo时，恢复旧object

- undo scope可以保存不同object的多次更改

  ![1587275528768](/1587275528768.png)

### Prefabs/Prototypes 

![1587275643566](/1587275606065.png)

### Live Collaboration 

支持多人实时协作同一个工程

### The Truth: Pros & Cons 

优点

- 提供了很多基础功能，如undo、redo、copy、paste
- 甚至一些高级功能，如实时协作和原型设计

缺点

- 有些数据并不适合用key-value format
- 这套系统很复杂且处于中心位置
  - 没有简单的方式提供给其他系统使用
  - 修改会导致复杂度骤增

## 简化技术栈

### 我们的技术栈

- 所有代码都用C语言编写
- 外部依赖非常少

![1587273271561](/1587273271561.png)

### Draw 2D: 2DUI库

- 函数：stroke_rect(), fill_rect()
- 直接将数据写入到vertex buffer和index buffer
- 所有UI在一个drawcall内绘制，详细介绍可参考[here](https://ourmachinery.com/post/ui-rendering-using-primitive-buffers/ )

### Draw 2D: Clipping 

- 裁剪矩形被写入到vertex buffer
- 实现了gpu裁剪，在PS中基于裁剪矩形做裁剪

### Draw 2D: Overlays 

![1587277585876](/1587277585876.png)

### UI

- 立即模式UI，没有UI对象的创建和释放
- 所有UI和交互处理都在一个drawcall内完成
- 所有控件都会在每帧绘制一次
- 控件不会被永久保存，通过ID识别不同控件

### IMGUI: Pros & Cons 

- More straightforward code flow (debugging, profiling)
- No need to synchronize state
- Redraw every frame -- expensive?
  - Viewport typically wants to render every frame anyway
  - Can do it just on mouse/keyboard events
  - Easy to match performance to what is shown on screen
- New mindset: no objects to talk to
    - Can usually find ways around it 

（接下来一部分介绍来IMGUI模式遇到的问题以及他们的解决思路，这里略去细节...）

### 小结

优点

- 开发栈完全可控，更易于理解和调试
- 所有地方都使用相同的API，这样工具程序和其他程序之间就不存在什么障碍壁垒；任何人都可以把事情做好

缺点

- 你需要从零开始构建(大概6人月的工作量)
  - 前期投入很快就会收回
  - 你也可以直接使用Dear IMGUI
- 需要考虑很多设计决策
- IMGUI需要新的思维模式

## UI生成

Motivation 

- 减少创建UI的工作

(Niklas给了几个参考图和一些示例代码，就不在这里罗列了,详见视频...)

## 结论

- 创建UI更快了，进度不再被UI任务阻塞
- 引擎开发由两个人在两年内完成
- 数据模型看起来很酷但也可怕，因为没增加一项就会导致更复杂
- Aspects 用来自定义数据行为，是一种非常棒的方式
- 独自一人去实现这些功能是一项非常大的工作
- 创建工具集需要很多"functional design" 
- 我们也牺牲来一些特性，如文字自右向左排列

**All-in-all we're happy with the direction .**

## 观后感

1. 从历史中总结经验教训，使用IMGUI提高了Niklas他们的工具开发效率
2. The Truth数据模型在Machinery引擎处于中心位置，为工具开发提供来很多遍历功能；但同时因增加项而变得更加复杂；进而导致难以维护
3. 视频中提到的很多历史问题，其实我们也都遇到过
4. 要向大佬们学习，善于总结和改进
5. IMGUI和RMGUI的争论在网上就有很多，抛开具体场景谈好坏都是耍流氓
6. Machinery引擎还在开发中，目前刚发布了beta版本；但仅限于内测。为进一步学习和了解，两天前我发邮件申请加入内测，暂未得到回复