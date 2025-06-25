---
title: Auto Layout 小记
tag: ios
date: 2018-05-01 09:33:32
---

## 什么是 Auto Layout
在 iOS 开发过程，我们总是认为通过托拽产生的界面是 Auto Layout，然而并不仅仅是这样的。 并不是仅仅在 storyboard 中通过 Interface Builder（IB）得到的才是 Auto Layout。其本质上是一种新的创建用户界面的方式，通过约束来描述视图与内容相互之间的关系。其有以下几个原则[1]：

* Auto Layout 是声明性的（通过向系统添加规则来表达界面布局）
* Auto Layout 最小化了计算（指定布局，而不是特定的点和像素）
* Auto Layout 设计是非直接的，但是它也很灵活（视图为已改变的窗口集合请求更少的更新，而且它们很容易分解为可维护的布局组件）
* Auto Layout 是几何驱动的（它的基本属于都是自然的几何属性，例如edge、center和size）
* Auto Layout 聚焦于关系（使用几何上的等量和非等量关系，将视图相互关联起来，从绝对关系切换到相对关系）
* Auto Layout 允许甚至鼓励冲突的规则（其组件可以携带不同的优先级，可以添加高优先级的边界限制和低优先级的回退条件）
* Auto Layout 表现自然内容（内在视图内容驱动尺寸设置和对齐，让内容成为布局中的一个关键角色）
* Auto Layout 寻找最佳方案
* Auto Layout 是分布式的

## Auto Layout 布局约束法则
既然上面已经提到，其主要是通过约束来描述视图间关系来形成界面的，所以我们需要了解一些Auto Layout 布局约束的基本法则：

* 约束是有优先级的，相同优先级的约束同时考虑，如果需要优先处理某个约束，就要赋予该约束更高的优先级。
* 布局约束是关系，没有方向。
* 布局约束可以取近似值
* 布局约束可以循环，也即可以交叉引用。
* 可以冗余
* 可以引用兄弟视图
* 约束必须至少引用一个视图。

## IB 基本元素介绍
![2016121583524Screen Shot 2016-12-15 at 9.35.56 PM.png](http://of9yc6abb.bkt.clouddn.com/2016121583524Screen Shot 2016-12-15 at 9.35.56 PM.png)
![2016121523802Screen Shot 2016-12-15 at 9.36.05 PM.png](http://of9yc6abb.bkt.clouddn.com/2016121523802Screen Shot 2016-12-15 at 9.36.05 PM.png)
![2016121551112Screen Shot 2016-12-15 at 9.36.14 PM.png](http://of9yc6abb.bkt.clouddn.com/2016121551112Screen Shot 2016-12-15 at 9.36.14 PM.png)
![2016121533105Screen Shot 2016-12-15 at 9.36.24 PM.png](http://of9yc6abb.bkt.clouddn.com/2016121533105Screen Shot 2016-12-15 at 9.36.24 PM.png)
![2016121588295Screen Shot 2016-12-15 at 9.36.34 PM.png](http://of9yc6abb.bkt.clouddn.com/2016121588295Screen Shot 2016-12-15 at 9.36.34 PM.png)
![2016121553770Screen Shot 2016-12-15 at 9.36.44 PM.png](http://of9yc6abb.bkt.clouddn.com/2016121553770Screen Shot 2016-12-15 at 9.36.44 PM.png)

## 使用 IB 添加约束
### 托拽
### 对齐
## 处理约束冲突

> [呼呼呼山]()(http://code4fun.me)
> 2019-05-01 09:33:32
