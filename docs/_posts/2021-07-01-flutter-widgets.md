---
layout: post
title: Flutter常用widget
categories: Flutter
description: Flutter的一些常用widget
keywords: Flutter, widget
---



### 经典控件

**Text**

> 比如Android中的TextView，iOS中的UILabel

Text参数分类：

- **控制整体文本布局的参数**
  - textAlign
  - textDirection
  - maxLines
  - overflow
  - ...
- **控制文本展示样式的参数**
  - fontFamily
  - fontSize
  - color
  - shadows

通过TextSpan来对Text继续分片样式处理。

**Image**

> 比如Android中的ImageView，iOS里的UIImageView

- 加载本地资源图片
- 加载本地图片
- 加载网络图片

- FadeInImage（支持占位图、加载动态等）
- CacheNetworkImage（支持缓存到文件系统，更加强大的加载过程占位和加载错误占位）

**按钮**

- FloatingActionButton（圆形的按钮）
- FlatButton（扁平化的按钮）
- RaisedButton（凸起的按钮）

两个最重要的参数：

- onPressed（用于设置点击回调）
- child（用于设置按钮的内容）



#### 滚动视图

**ListView**

- ListView.builder：适用于很多子 Widget 场景，需要时创建子Widget
- ListView.separated：单独设置分割线的样式

**CustomScrollView**

解决多 ListView 嵌套滚动一致性问题，在 CustomScrollView 中，这些彼此独立的、可滚动的 Widget 被统称为 Sliver

- SliverList
- SliverAppBar

**ScrollController**

- 在 State 的初始化方法里，创建了 ScrollController，并通过 _controller.addListener 注册了滚动监听方法回调
- 在视图构建方法 build 中，我们将 ScrollController 对象与 ListView 进行了关联

**ScrollNotification**

- 通过 NotificationListener（一个 Widget） 来实现，从而感知 ListView 的各类滚动事件

```Dart
Widget build(BuildContext context) {
  return MaterialApp(
    title: 'ScrollController Demo',
    home: Scaffold(
      appBar: AppBar(title: Text('ScrollController Demo')),
      body: NotificationListener<ScrollNotification>(//添加NotificationListener作为父容器
        onNotification: (scrollNotification) {//注册通知回调
          if (scrollNotification is ScrollStartNotification) {//滚动开始
            print('Scroll Start');
          } else if (scrollNotification is ScrollUpdateNotification) {//滚动位置更新
            print('Scroll Update');
          } else if (scrollNotification is ScrollEndNotification) {//滚动结束
            print('Scroll End');
          }
        },
        child: ListView.builder(
          itemCount: 30,//列表元素个数
          itemBuilder: (context, index) => ListTile(title: Text("Index : $index")),//列表项创建方法
        ),
      )	
    )
  );
}
```

