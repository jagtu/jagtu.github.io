---
layout: post
title: Flutter在Android端的热更新方案
categories: flutter
description: Flutter在Android端的热更新方案
keywords: flutter, flutter module
---



### 分析

##### Flutter在android端启动流程

1、核心关注源代码：　`io.flutter.embedding.android.FlutterActivity`

​	Flutter工程默认的mainActivity是继承自FlutterActivity，它主要是执行了FlutterActivityAndFragmentDelegate.host这个代理类的方法。

2、关注Flutter 项目的打包产物：

<img src="/assets/images/image-20220121180036960.png" alt="image-20220121180036960" style="zoom:50%;" />

​											dart的核心代码主要在libapp.so这个so文件里

思路：在APP启动的时候动态替换这个so文件就可以达到热修复。



**Flutter源代码分析**



flutter默认工程会配置一个io.flutter.app.FlutterApplication作为我们的application，这个类中只做了一件事，加载flutter相关的环境。



**所以我们需要关注的就是FlutterInjector这个类了**



我们注意到它持有了FlutterLoader这个类的实例，并调用方法startInitialization初始化，方法注释是：

Starts initialization of the native system.

This loads the Flutter engine's native library to enable subsequent JNI calls. This also starts locating and unpacking Dart resources packaged in the app's APK.

Calling this method multiple times has no effect.

Params:

applicationContext – The Android application context.

settings – Configuration settings.



说明这个就是加载应用程序的APK中的Dart资源的地方。



热更新的实现，可以参考这部分代码。



Android启动加载：



—> FlutterActivity.onCreate()

—> FlutterActivityAndFragmentDelegate.onAttach()

—> FlutterActivityAndFragmentDelegate.setupFlutterEngine(); 

—>

{

​	// 设置加载内容

​	flutterLoader.startInitialization(context.getApplicationContext());

​		—>赋值：**flutterApplicationInfo** = ApplicationInfoLoader.*load*(appContext);

​				—>return FlutterApplicationInfo()见附2

​	//这个方法拼接shell命令给dartVM启动我们的程序

​	flutterLoader.ensureInitializationComplete(context, dartVmArgs);

​	///这里dartVmArgs = FlutterActivity.getFlutterShellArgs()

}



flutterLoader.ensureInitializationComplete拼接好后的关键内容是 -- aot-shared-library-name=libapp.so --aot-shared-library-name=nativeLibraryDir/libapp.so,



我已我们就仅需要将我们下发的补丁so文件路径替换成FlutterApplicationInfo.aotSharedLibraryName字段的值，那么flutter启动后就是执行我们下发的内容了。



由于FlutterLoader中的FlutterApplicationInfo是私有的并且FlutterApplicationInfo中的aotSharedLibraryName是final修饰的，所以我们不可能通过常规手段去修改FlutterApplicationInfo.aotSharedLibraryName的值，只能通过反射去修改了





​	

附2、FlutterApplicationInfo.load

*// XML Attribute keys supported in AndroidManifest.xml*

//	FlutterLoader.**aot-shared-library-name**

//	FlutterLoader.**flutter-assets-dir**

FlutterLoader.**class**.getName() + **'.'** + FlutterLoader.***AOT_SHARED_LIBRARY_NAME\***;

FlutterLoader.**class**.getName() + **'.'** + FlutterLoader.***FLUTTER_ASSETS_DIR_KEY\***;



即为：FlutterApplicationInfo类存放了libapp.so及资源文件等路径信息



​	aotSharedLibraryName：**libapp.so**



​	vmSnapshotData：**vm_snapshot_data**

​	isolateSnapshotData：**isolate_snapshot_data**

​	flutterAssetsDir：**flutter_assets**

​	domainNetworkPolicy

​	nativeLibraryDir





——

**构建一个发布版（release）APK**

本节介绍如何构建发布版（release）APK。如果您完成了前一节中的签名步骤，则会对APK进行签名。

使用命令行:

1. cd <app dir> (<app dir> 为您的工程目录).
2. 运行flutter build apk (flutter build 默认会包含 --release选项).

打包好的发布APK位于<app dir>/build/app/outputs/apk/app-release.apk。

 \3. 解压unzip build/app/outputs/flutter-apk/app-release.apk -d build/app/outputs/flutter-apk/appunzip





##### 参考文章

- 从Activity入手的方案：[ ](fhttps://juejin.cn/post/6844903984939941901#heading-7)https://juejin.cn/post/6844903984939941901#heading-7

https://juejin.cn/post/6937943901344890888

- **从Application入手的方案：**https://juejin.cn/post/6921280711462748167

- 从FlutterMain类入手：https://juejin.cn/post/6844903984939941901#heading-4

​		FlutterMain 这个类中，会传递指定资源路径，提供给 Dart VM 进行初始化。但是在最新的Flutter版本中，这个类已经Deprecated

