---
layout: post
title: Flutter跨平台方案介绍
categories: flutter
description: Flutter区别于其他方案的关键技术是什么
keywords: flutter, 跨平台
---



### 浅述跨平台开发的背景

移动开发技术可分为纯原生开发与跨平台开发，**纯原生开发**使用相应平台(iOS和Android)支持的开发工具和语言，并直接调用系统提供的SDK API。主要优势可访问平台全部功能、速度快、性能高、可以实现复杂动画及绘制，整体用户体验好。不足在于动态化和开发成本两个问题，为了节约成本和时间，提高开发效率，诞生了一些**跨平台开发框架**。

#### 跨平台开发的三种方案

根据实现方式的不同，业内常见的观点是将主流的跨平台方案划分为三种方案。

* **Web 容器**（H5+原生Webview）：基于 Web 相关技术通过浏览器组件来实现界面及功能，典型的框架包括 Cordova(PhoneGap)、Ionic 和早期的微信小程序。

  ![img](https://static001.geekbang.org/resource/image/6d/79/6d4035e44b4af68b7588a750fec06c79.png)

  * 优点是动态内容是H5，web技术栈，社区及资源丰富，
  * 缺点是性能不好，对于复杂用户界面或动画，WebView不堪重任

* **泛 Web 容器**（JavaScript开发+原生渲染）：采用类 Web 标准进行开发，但在运行时把绘制和渲染交由原生系统接管的技术，代表框架有 React Native、uni-app、Weex、小程序和快应用，广义的还包括天猫的 Virtual View 等。

  ![img](https://static001.geekbang.org/resource/image/3a/0b/3af2c590f42c0b924a22bb135134380b.png)

  * 优点：Web技术栈、原生渲染，性能相比H5提高很多、支持热更新
  * 缺点：JS与原生有通信损耗、JIT(Just In Time)执行效率、不同平台原生控件渲染能力有差异带来的问题

* **自绘引擎**（自绘UI+原生）：自带渲染引擎，客户端仅提供一块画布即可获得从业务逻辑到功能呈现的多端高度一致的渲染体验。Flutter，是为数不多的代表。

  ![img](https://static001.geekbang.org/resource/image/85/2c/85dfb3f8a803a0bf573f3fce63ddc92c.png)

  * 优点：性能高与原生相近、组件库易维护，布局灵活，UI外观保真度和一致性高

  * 不足：使用全新的技术栈

    

### Flutter 是怎么运转的？

Flutter重写了一套跨平台的 UI 框架，包括渲染逻辑、开发语言。

* 渲染引擎依靠跨平台的 Skia 图形库来实现，Skia 引擎会将使用 Dart 构建的抽象的视图结构数据加工成 GPU 数据，交由 OpenGL 最终提供给 GPU 渲染，至此完成渲染闭环，因此可以在最大程度上保证一款应用在不同平台、不同设备上的体验一致性。
* 开发语言选用的是同时支持 JIT（Just-in-Time，即时编译）和 AOT（Ahead-of-Time，预编译）的 Dart，不仅保证了开发效率，更提升了执行效率（比使用 JavaScript 开发的泛 Web 容器方案要高得多）

#### 硬件绘图基本原理

在计算机系统中，图像的显示需要 CPU、GPU 和显示器一起配合完成：CPU 负责图像数据计算，GPU 负责图像数据渲染，而显示器则负责最终图像显示。

CPU 把计算好的、需要显示的内容交给 GPU，由 GPU 完成渲染后放入帧缓冲区，随后视频控制器根据垂直同步信号（VSync）以每秒 60 次的速度，从帧缓冲区读取帧数据交由显示器完成图像显示。

通常操作系统会封装一套操作显示器硬件命令的API供操作系统之上的应用调用，但是对于应用开发者来说，直接调用这些操作系统提供的API是比较复杂和低效的，因为操作系统提供的API往往比较基础，直接调用需要了解API的很多细节。正是因为这个原因，几乎所有用于开发GUI程序的编程语言都会在操作系统之上再封装一层，将操作系统原生API封装在一个编程框架和模型中，然后定义一种简单的开发规则来开发GUI应用程序，而这一层抽象，正是我们所说的“UI”系统，如Android SDK正是封装了Android操作系统API，提供了一个“UI描述文件XML+Java操作DOM”的UI系统，而iOS的UIKit 对View的抽象也是一样的，他们都将操作系统API抽象成一个基础对象（如用于2D图形绘制的Canvas），然后再定义一套规则来描述UI，如UI树结构，UI操作的单线程原则等

![iOS](/Users/Jagtu/Documents/个人资料/学习文档/笔记/Flutter/渲染图.png)

#### Flutter是怎么运转的

Flutter 作为跨平台开发框架也采用了这种底层方案。下面有一张更为详尽的示意图来解释 Flutter 的绘制原理

![img](https://static001.geekbang.org/resource/image/95/2a/95cb258c9103e05398f9c97a1113072a.png)

可以看到，Flutter 关注如何尽可能快地在两个硬件时钟的 VSync 信号之间计算并合成视图数据，然后通过 Skia 交给 GPU 渲染：UI 线程使用 Dart 来构建视图结构数据，这些数据会在 GPU 线程进行图层合成，随后交给 Skia 引擎加工成 GPU 数据，而这些数据会通过 OpenGL 最终提供给 GPU 渲染。

**Skia 是什么？**

要想了解 Flutter，你必须先了解它的底层图像**渲染引擎 Skia**。因为，Flutter 只关心如何向 GPU 提供视图数据，而 Skia 就是它向 GPU 提供视图数据的好帮手。

Skia 是一款用 C++ 开发的、性能彪悍的 2D 图像绘制引擎，其前身是一个向量绘图软件。2005 年被 Google 公司收购后，因为其出色的绘制表现被广泛应用在 Chrome 和 Android 等核心产品上。Skia 在图形转换、文字渲染、位图渲染方面都表现卓越，并提供了开发者友好的 API。

因此，架构于 Skia 之上的 Flutter，也因此拥有了彻底的跨平台渲染能力。通过与 Skia 的深度定制及优化，Flutter 可以最大限度地抹平平台差异，提高渲染效率与性能。

底层渲染能力统一了，上层开发接口和功能体验也就随即统一了，开发者再也不用操心平台相关的渲染特性了。也就是说，Skia 保证了同一套代码调用在 Android 和 iOS 平台上的渲染效果是完全一致的。

#### Flutter 的架构

![Flutter 架构图](https://static001.geekbang.org/resource/image/ac/2f/ac7d1cec200f7ea7cb6cbab04eda252f.png)

Flutter 架构采用分层设计，从下到上分为三层，依次为：Embedder、Engine、Framework。

* Embedder 是操作系统适配层，实现了渲染 Surface 设置，线程设置，以及平台插件等平台相关特性的适配。从这里我们可以看到，Flutter 平台相关特性并不多，这就使得从框架层面保持跨端一致性的成本相对较低。
* Engine 层主要包含 Skia、Dart 和 Text，实现了 Flutter 的渲染引擎、文字排版、事件处理和 Dart 运行时等功能。Skia 和 Text 为上层接口提供了调用底层渲染和排版的能力，Dart 则为 Flutter 提供了运行时调用 Dart 和渲染引擎的能力。而 Engine 层的作用，则是将它们组合起来，从它们生成的数据中实现视图渲染。
* Framework 层则是一个用 Dart 实现的 UI SDK，包含了动画、图形绘制和手势识别等功能。为了在绘制控件等固定样式的图形时提供更直观、更方便的接口，Flutter 还基于这些基础能力，根据 Material 和 Cupertino 两种视觉设计风格封装了一套 UI 组件库。我们在开发 Flutter 的时候，可以直接使用这些组件库。			

#### 界面渲染过程

事实上，Flutter 的核心设计思想便是**“一切皆 Widget”**

![图14-0](https://book.flutterchina.club/assets/img/14-0.bd5c2d9d.png)

* Widget：“描述一个UI元素的配置数据”，包括布局、渲染属性、事件响应信息等
  * 不可变性，视图渲染的配置信息发生变化时，Flutter 会选择重建 Widget 树的方式进行数据更新，以数据驱动 UI 构建的方式简单高效
* Element：Widget的实例化，一个Widget对象可以对应多个`Element`对象
* RenderObject：负责实现视图渲染的对象，完成布局和绘制，所有的RenderObject构成一个渲染树

Flutter 通过控件树（Widget 树）中的每个控件（Widget）创建不同类型的渲染对象，组成渲染对象树。

而渲染对象树在 Flutter 的展示过程分为四个阶段，即**布局、绘制、合成和渲染**。

**布局和绘制**在 RenderObject 中完成，Flutter 采用深度优先机制遍历渲染对象树，确定树中各个对象的位置和尺寸，并把它们绘制到不同的图层上。绘制完毕后，**合成和渲染**的工作则交给 Skia 搞定。

##### 布局

Flutter 采用深度优先机制遍历渲染对象树，决定渲染对象树中各渲染对象在屏幕上的位置和尺寸。在布局过程中，渲染对象树中的每个渲染对象都会接收父对象的布局约束参数，决定自己的大小，然后父对象按照控件逻辑决定各个子对象的位置，完成布局过程。

![img](https://static001.geekbang.org/resource/image/f9/00/f9e6bbf06231fbad54ed11ef291e8d00.png)



为了防止因子节点发生变化而导致整个控件树重新布局，Flutter 加入了一个机制——布局边界（Relayout Boundary），可以在某些节点自动或手动地设置布局边界，当边界内的任何对象发生重新布局时，不会影响边界外的对象，反之亦然。

![img](https://static001.geekbang.org/resource/image/42/de/42961a84ecc8181d1afe7ffbaa1a55de.png)



##### 绘制

布局完成后，渲染对象树中的每个节点都有了明确的尺寸和位置。Flutter 会把所有的渲染对象绘制到不同的图层上。与布局过程一样，绘制过程也是深度优先遍历，而且总是先绘制自身，再绘制子节点。以下图为例：节点 1 在绘制完自身后，会再绘制节点 2，然后绘制它的子节点 3、4 和 5，最后绘制节点 6。

![img](https://static001.geekbang.org/resource/image/8c/b8/8c1d612990d9ada0508c5a41c9e4cab8.png)

可以看到，由于一些其他原因（比如，视图手动合并）导致 2 的子节点 5 与它的兄弟节点 6 处于了同一层，这样会导致当节点 2 需要重绘的时候，与其无关的节点 6 也会被重绘，带来性能损耗。

为了解决这一问题，Flutter 提出了与布局边界对应的机制——重绘边界（Repaint Boundary）。在重绘边界内，Flutter 会强制切换新的图层，这样就可以避免边界内外的互相影响，避免无关内容置于同一图层引起不必要的重绘。

![img](https://static001.geekbang.org/resource/image/da/ee/da430de8f265f444c4801758902a8bee.png)



重绘边界的一个典型场景是 Scrollview。ScrollView 滚动的时候需要刷新视图内容，从而触发内容重绘。而当滚动内容重绘时，一般情况下其他内容是不需要重绘的，这时候重绘边界就派上用场了。

##### 合成和渲染

终端设备的页面越来越复杂，因此 Flutter 的渲染树层级通常很多，直接交付给渲染引擎进行多图层渲染，可能会出现大量渲染内容的重复绘制，所以还需要先进行一次图层合成，即将所有的图层根据大小、层级、透明度等规则计算出最终的显示效果，将相同的图层归类合并，简化渲染树，提高渲染效率。







**为什么是 Dart？**

* Dart 同时支持即时编译 JIT 和事前编译 AOT
* Dart 作为一门现代化语言，集百家之长
* Dart 单线程模型，避免了抢占式调度和共享内存



### 问题

1. 如果没有widget层，单靠`Element`层是否可以搭建起一个可用的UI框架？如果可以应该是什么样子？
2. Flutter UI框架能不做成响应式吗？

