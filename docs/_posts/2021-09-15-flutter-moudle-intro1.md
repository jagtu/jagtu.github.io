---
layout: post
title: 原生工程与Flutter混合开发
categories: flutter
description: 将 Flutter 集成到现有工程
keywords: flutter, flutter module
---



### **将 Flutter 集成到现有应用**

用 Flutter 一次性重写现有的应用是不切实际的。对于这些情况，将Flutter 可以作为一个库或模块，集成进现有的应用当中是我们的唯一方案。

模块引入到您的 Android 或 iOS 应用中，以使用 Flutter 来渲染一部分的 UI，完成一部分可运行于多平台共享的业务模块。



目前仍有以下限制：

- 目前尚未支持将多个 Flutter 库打包到同一个应用中
- 插件需要基于 [`FlutterPlugin`](https://api.flutter-io.cn/javadoc/io/flutter/embedding/engine/plugins/FlutterPlugin.html) 开发
- Flutter 模块仅支持 Android 平台中的 AndroidX 应用

可实现适用场景：

- 导航到原生页面和 Flutter 页面的混合栈应用
- 一个页面有原生视图和 Flutter 视图的情况



## 创建 Flutter module

为了将 Flutter 集成到你的既有应用里，第一步要创建一个 Flutter module。

在命令行中执行：

```shell
cd some/path/
flutter create --template module my_flutter
```

Flutter module 会创建在 `some/path/my_flutter/` 目录。

在这个目录中，你可以像在其它 Flutter 项目中一样，执行 `flutter` 命令。比如 `flutter run --debug` 或者 `flutter build ios`。同样，你也可以通过 [Android Studio/IntelliJ](https://flutter.cn/docs/development/tools/android-studio) 或者 [VS Code](https://flutter.cn/docs/development/tools/vs-code) 中的 Flutter 和 Dart 插件运行这个模块，在集成到现有应用前，这个项目在 Flutter module 中包含了一个单视图的示例代码，对 Flutter 侧代码的测试会有帮助。

### 模块组织

在 `my_flutter` 模块，目录结构和普通 Flutter 应用类似：

```
my_flutter/
├── .ios/
│   ├── Runner.xcworkspace
│   └── Flutter/podhelper.rb
├── lib/
│   └── main.dart
├── test/
└── pubspec.yaml

```

添加你的 Dart 代码到 `lib/` 目录

添加 Flutter 依赖到 `my_flutter/pubspec.yaml`，包括 Flutter packages 和 plugins

`.ios/` 隐藏文件夹包含了一个 Xcode workspace，用于单独运行你的 Flutter module。它是一个独立启动 Flutter 代码的壳工程，并且包含了一个帮助脚本，用于编译 framewroks 或者使用 CocoaPods 将 Flutter module 集成到你的既有应用。

**注**：*iOS 代码要添加到你的既有应用或者 Flutter plugin中，而不是 Flutter module 的 `.ios/` 目录下。 `.ios/` 下的改变不会集成到你的既有应用，并且这有可能被 Flutter 重写。*

*由于 `.ios/` 目录是自动生成的，因此请勿对其进行版本控制。在新机器上构建 module 时，请在使用 Flutter module 构建 iOS 项目之前，先于 `my_flutter` 目录运行 `flutter pub get` 以生成 `.ios/` 目录。*



## 在iOS中集成Flutter

Flutter 以 framework 框架的形式添加到 iOS 应用中

有两种方式可以将 Flutter 集成到你的既有应用中。

1. 使用 CocoaPods 依赖管理和已安装的 Flutter SDK 。（推荐）
2. 把 Flutter engine 、你的 dart 代码和所有 Flutter plugin 编译成 framework 。用 Xcode 手动集成到你的应用中，并更新编译设置。

### 使用 CocoaPods 和 Flutter SDK 集成

注：本地需安装 Flutter SDK

假设iOS应用和 Flutter module 在相邻目录，否则请根据实际情况适配到对应的路径

```
some/path/
├── my_flutter/
│   └── .ios/
│       └── Flutter/
│         └── podhelper.rb
└── MyApp/
    └── Podfile
```



1. 在 `Podfile` 中添加下面代码：

   ```bash
   flutter_application_path = '../my_flutter'
   load File.join(flutter_application_path, '.ios', 'Flutter', 'podhelper.rb')
   ```

2. 每个需要集成 Flutter 的 **Podfile target**，执行 `install_all_flutter_pods(flutter_application_path)`，示例：

   ```shell
   target 'MyApp' do
     install_all_flutter_pods(flutter_application_path)
   end
   ```

3. 运行 `pod install`

`podhelper.rb` 脚本会把你的 plugins， `Flutter.framework`，和 `App.framework` 集成到你的项目中。



### 添加单个 Flutter 页面

#### 启动 FlutterEngine 和FlutterViewController

在iOS应用中，`FlutterEngine` 充当 Dart VM 和 Flutter 运行时的主机； `FlutterViewController` 依附于 `FlutterEngine`，给 Flutter 传递 UIKit 的输入事件，并展示被 `FlutterEngine` 渲染的每一帧画面。

`FlutterEngine` 的寿命可能与 `FlutterViewController` 相同，也可能超过 `FlutterViewController`，通常建议为您的应用预热一个“long-lived”的 `FlutterEngine`  because:

- 当展示 `FlutterViewController` 时，第一帧画面将会更快展现；
- 你的 Flutter 和 Dart 状态将比一个`FlutterViewController` 存活更久；
- 在展示 UI 前，你的应用和 plugins 可以与 Flutter 和 Dart 逻辑交互。

##### 创建一个 FlutterEngine

创建 `FlutterEngine` 的合适位置取决于您的应用。作为示例，我们将在应用启动的 app delegate 中创建一个 `FlutterEngine`，并作为属性暴露给外界

**在 `AppDelegate.h`:**

```objective-c
@import UIKit;
@import Flutter;

@interface AppDelegate : FlutterAppDelegate // More on the FlutterAppDelegate below.
@property (nonatomic,strong) FlutterEngine *flutterEngine;
@end

```

**在 `AppDelegate.m`:**

```objective-c
// Used to connect plugins (only if you have plugins with iOS platform code).
#import <FlutterPluginRegistrant/GeneratedPluginRegistrant.h>

#import "AppDelegate.h"

@implementation AppDelegate

- (BOOL)application:(UIApplication *)application
    didFinishLaunchingWithOptions:(NSDictionary<UIApplicationLaunchOptionsKey, id> *)launchOptions {
  self.flutterEngine = [[FlutterEngine alloc] initWithName:@"my flutter engine"];
  // Runs the default Dart entrypoint with a default Flutter route.
  // 你的默认 Dart 库的默认入口函数 main()，将会在 AppDelegate 创建 FlutterEngine 并调用 run 方法时调用。
  [self.flutterEngine run];
  // Used to connect plugins (only if you have plugins with iOS platform code).
  [GeneratedPluginRegistrant registerWithRegistry:self.flutterEngine];
  return [super application:application didFinishLaunchingWithOptions:launchOptions];
}

@end

```

##### 使用 FlutterEngine 展示 FlutterViewController

下面的例子展示了一个普通的 `ViewController`，包含一个能跳转到 [`FlutterViewController`](https://api.flutter-io.cn/objcdoc/Classes/FlutterViewController.html) 的 `UIButton`，这个`FlutterViewController` 使用在 `AppDelegate` 中创建的 Flutter 引擎 (`FlutterEngine`)。

```objective-c
@import Flutter;
#import "AppDelegate.h"
#import "ViewController.h"

@implementation ViewController
- (void)viewDidLoad {
    [super viewDidLoad];

    // Make a button to call the showFlutter function when pressed.
    UIButton *button = [UIButton buttonWithType:UIButtonTypeCustom];
    [button addTarget:self
               action:@selector(showFlutter)
     forControlEvents:UIControlEventTouchUpInside];
    [button setTitle:@"Show Flutter!" forState:UIControlStateNormal];
    button.backgroundColor = UIColor.blueColor;
    button.frame = CGRectMake(80.0, 210.0, 160.0, 40.0);
    [self.view addSubview:button];
}

- (void)showFlutter {
    FlutterEngine *flutterEngine =
        ((AppDelegate *)UIApplication.sharedApplication.delegate).flutterEngine;
    FlutterViewController *flutterViewController =
        [[FlutterViewController alloc] initWithEngine:flutterEngine nibName:nil bundle:nil];
    [self presentViewController:flutterViewController animated:YES completion:nil];
}
@end

```

##### 使用隐式 FlutterEngine 创建 FlutterViewController

上一个示例还有另一个选择，你可以让 `FlutterViewController` 隐式创建它自己的 `FlutterEngine`，而不用提前预热 engine。

不过不建议这样做，因为按需创建`FlutterEngine` 的话，在 `FlutterViewController` 被 present 出来之后，第一帧图像渲染完之前，将会引入明显的延迟。但是当 Flutter 页面很少被展示时，当对决定何时启动 Dart VM 没有好的启发时，当 Flutter 无需在页面（view controller）之间保持状态时，此方式可能会有用。

为了不使用已经存在的 `FlutterEngine` 来展现 `FlutterViewController`，省略 `FlutterEngine` 的创建步骤，并且在创建 `FlutterViewController` 时，去掉 engine 的引用。

```objective-c
// Existing code omitted.
// 省略已经存在的代码
- (void)showFlutter {
  FlutterViewController *flutterViewController =
      [[FlutterViewController alloc] initWithProject:nil nibName:nil bundle:nil];
  [self presentViewController:flutterViewController animated:YES completion:nil];
}
@end

```

#### 使用 FlutterAppDelegate

推荐让你应用的 `UIApplicationDelegate` 继承 `FlutterAppDelegate`，但不是必须的。

`FlutterAppDelegate` 有这些功能：

- 传递应用的回调，例如 [`openURL`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1623112-application) 到 Flutter 的插件 —— [local_auth](https://pub.flutter-io.cn/packages/local_auth)。
- 传递状态栏点击（这只能在 AppDelegate 中检测）到 Flutter 的点击置顶行为。

如果你的 app delegate 不能直接继承 `FlutterAppDelegate`，让你的 app delegate 实现 `FlutterAppLifeCycleProvider`协议，来确保 Flutter plugins 接收到必要的回调。否则，依赖这些事件的 plugins 将会有无法预估的行为。

例如：

```objective-c
@import UIKit;
@import FlutterPluginRegistrant;

@interface AppDelegate : UIResponder <UIApplicationDelegate, FlutterAppLifeCycleProvider>
@property (strong, nonatomic) UIWindow *window;
@property (nonatomic,strong) FlutterEngine *flutterEngine;
@end
```

在具体实现中，应该最大化地委托给 `FlutterPluginAppLifeCycleDelegate`：

```objective-c
@interface AppDelegate ()
@property (nonatomic, strong) FlutterPluginAppLifeCycleDelegate* lifeCycleDelegate;
@end

@implementation AppDelegate

- (instancetype)init {
    if (self = [super init]) {
        _lifeCycleDelegate = [[FlutterPluginAppLifeCycleDelegate alloc] init];
    }
    return self;
}

- (BOOL)application:(UIApplication*)application
didFinishLaunchingWithOptions:(NSDictionary<UIApplicationLaunchOptionsKey, id>*))launchOptions {
    self.flutterEngine = [[FlutterEngine alloc] initWithName:@"io.flutter" project:nil];
    [self.flutterEngine runWithEntrypoint:nil];
    [GeneratedPluginRegistrant registerWithRegistry:self.flutterEngine];
    return [_lifeCycleDelegate application:application didFinishLaunchingWithOptions:launchOptions];
}

// Returns the key window's rootViewController, if it's a FlutterViewController.
// Otherwise, returns nil.
- (FlutterViewController*)rootFlutterViewController {
    UIViewController* viewController = [UIApplication sharedApplication].keyWindow.rootViewController;
    if ([viewController isKindOfClass:[FlutterViewController class]]) {
        return (FlutterViewController*)viewController;
    }
    return nil;
}

- (void)touchesBegan:(NSSet*)touches withEvent:(UIEvent*)event {
    [super touchesBegan:touches withEvent:event];

    // Pass status bar taps to key window Flutter rootViewController.
    if (self.rootFlutterViewController != nil) {
        [self.rootFlutterViewController handleStatusBarTouches:event];
    }
}

- (void)application:(UIApplication*)application
didRegisterUserNotificationSettings:(UIUserNotificationSettings*)notificationSettings {
    [_lifeCycleDelegate application:application
didRegisterUserNotificationSettings:notificationSettings];
}

- (void)application:(UIApplication*)application
didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken {
    [_lifeCycleDelegate application:application
didRegisterForRemoteNotificationsWithDeviceToken:deviceToken];
}

- (void)application:(UIApplication*)application
didReceiveRemoteNotification:(NSDictionary*)userInfo
fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))completionHandler {
    [_lifeCycleDelegate application:application
       didReceiveRemoteNotification:userInfo
             fetchCompletionHandler:completionHandler];
}

- (BOOL)application:(UIApplication*)application
            openURL:(NSURL*)url
            options:(NSDictionary<UIApplicationOpenURLOptionsKey, id>*)options {
    return [_lifeCycleDelegate application:application openURL:url options:options];
}

- (BOOL)application:(UIApplication*)application handleOpenURL:(NSURL*)url {
    return [_lifeCycleDelegate application:application handleOpenURL:url];
}

- (BOOL)application:(UIApplication*)application
            openURL:(NSURL*)url
  sourceApplication:(NSString*)sourceApplication
         annotation:(id)annotation {
    return [_lifeCycleDelegate application:application
                                   openURL:url
                         sourceApplication:sourceApplication
                                annotation:annotation];
}

- (void)application:(UIApplication*)application
performActionForShortcutItem:(UIApplicationShortcutItem*)shortcutItem
  completionHandler:(void (^)(BOOL succeeded))completionHandler NS_AVAILABLE_IOS(9_0) {
    [_lifeCycleDelegate application:application
       performActionForShortcutItem:shortcutItem
                  completionHandler:completionHandler];
}

- (void)application:(UIApplication*)application
handleEventsForBackgroundURLSession:(nonnull NSString*)identifier
  completionHandler:(nonnull void (^)(void))completionHandler {
    [_lifeCycleDelegate application:application
handleEventsForBackgroundURLSession:identifier
                  completionHandler:completionHandler];
}

- (void)application:(UIApplication*)application
performFetchWithCompletionHandler:(void (^)(UIBackgroundFetchResult result))completionHandler {
    [_lifeCycleDelegate application:application performFetchWithCompletionHandler:completionHandler];
}

- (void)addApplicationLifeCycleDelegate:(NSObject<FlutterPlugin>*)delegate {
    [_lifeCycleDelegate addDelegate:delegate];
}
@end
```



#### Local Network Privacy Permissions

在iOS 14及更高版本上，为了能够使用热重载来调试Flutter模块的代码，需要开启本地网络访问权限。

注意，只能在Debug环境启用热重载，否则应用上架App Store时，可能被拒绝。

 1. 将应用程序的Info.plist重命名为Info-Debug.plist。复制一个名为Info-Release.plist的副本，并将其添加到Xcode项目中。

    ![Info-Debug.plist and Info-Release.plist in Xcode](https://flutter.cn/assets/images/docs/development/add-to-app/ios/project-setup/info-plists.png)

 2. In **Info-Debug.plist** *only* add the key `NSBonjourServices` and set the value to an array with the string `_dartobservatory._tcp`. Note Xcode will display this as “Bonjour services”.

    Optionally, add the key `NSLocalNetworkUsageDescription` set to your desired customized permission dialog text.

    ```xml
    <key>NSBonjourServices</key>
        <array>
            <string>_dartobservatory._tcp</string>
        </array>
        <key>NSLocalNetworkUsageDescription</key>
        <string>Allow Flutter tools on your computer to connent and debug</string>
    ```

 3. In your target’s build settings, change the **Info.plist File** (`INFOPLIST_FILE`) setting path from `path/to/Info.plist` to `path/to/Info-$(CONFIGURATION).plist`.

    This will resolve to the path **Info-Debug.plist** in **Debug** and **Info-Release.plist** in **Release**.

    ![Resolved INFOPLIST_FILE build setting](https://flutter.cn/assets/images/docs/development/add-to-app/ios/project-setup/plist-build-setting.png)

 4. If the **Info-Release.plist** copy is in your target’s **Build Settings > Build Phases > Copy Bundle** Resources build phase, remove it.



### 启动选项

例子中展示了使用默认启动选项运行 Flutter。

为了定制化你的 Flutter 运行时，你也可以置顶 Dart 入口、库和路由。

#### Dart 入口

在 `FlutterEngine` 上调用 `run`，默认将会调用你的 `lib/main.dart` 文件里的 `main()` 函数。

你也可以使用另一个入口方法 [`runWithEntrypoint`](https://api.flutter-io.cn/objcdoc/Classes/FlutterEngine.html#/c:objc(cs)FlutterEngine(im)runWithEntrypoint:)，并使用 `NSString` 字符串指定一个不同的 Dart 入口。

 **提示**：*使用 `main()` 以外的 Dart 入口函数，必须使用下面的注解，防止被 [tree-shaken](https://en.wikipedia.org/wiki/Tree_shaking) 优化掉，而没有编译。*

```dart
  @pragma('vm:entry-point')
  void myOtherEntrypoint() { ... };
```

#### Dart 库

另外，在指定 Dart 函数时，你可以指定特定文件的特定函数。

下面的例子使用 `lib/other_file.dart` 文件的 `myOtherEntrypoint()` 函数取代 `lib/main.dart` 的 `main()` 函数：

```objective-c
[flutterEngine runWithEntrypoint:@"myOtherEntrypoint" libraryURI:@"other_file.dart"];
```

#### 路由

当构建 engine 时，可以为你的 Flutter [`WidgetsApp`](https://api.flutter-io.cn/flutter/widgets/WidgetsApp-class.html) 设置一个初始路由。

```
FlutterEngine *flutterEngine =
    [[FlutterEngine alloc] initWithName:@"my flutter engine"];
[[flutterEngine navigationChannel] invokeMethod:@"setInitialRoute"
                                      arguments:@"/onboarding"];
[flutterEngine run];
```

这段代码使用 `"/onboarding"` 取代 `"/"`，作为你的 `dart:ui` 的 [`window.defaultRouteName`](https://api.flutter-io.cn/flutter/dart-ui/Window/defaultRouteName.html)

```objective-c
FlutterViewController* flutterViewController =
      [[FlutterViewController alloc] initWithProject:nil
                                        initialRoute:@"/onboarding"
                                             nibName:nil
                                              bundle:nil];
```

 **小提示**

如果在 `FlutterEngine` 启动后，迫切得需要在平台侧改变你当前的 Flutter 路由，可以使用 `FlutterViewController` 里的 [`pushRoute()`](https://api.flutter-io.cn/objcdoc/Classes/FlutterViewController.html#/c:objc(cs)FlutterViewController(im)pushRoute:) 或者 [`popRoute()`](https://api.flutter-io.cn/objcdoc/Classes/FlutterViewController.html#/c:objc(cs)FlutterViewController(im)popRoute)。

在 Flutter 侧推出 iOS 路由，调用 [`SystemNavigator.pop()`](https://api.flutter-io.cn/flutter/services/SystemNavigator/pop.html)。

查看文档：[路由和导航](https://flutter.cn/docs/development/ui/navigation) 了解更多 Flutter 路由的内容。



## 集成到 Android 项目

Flutter 可以作为 Gradle 子项目源码或者 AAR 嵌入到现有的 Android 应用程序中。

### 使用 Android Studio

在 Android Studio 中，你可以在一个项目中同时编写 Android 代码和 Flutter 代码，

这种也是**以 Gradle 子项目源码的方式集成**。

![img](https://flutter.cn/assets/images/docs/development/add-to-app/android/project-setup/ide-new-module.png)

在 Android Studio 打开现有的 Android 项目并点击菜单按钮 **File > New > New Module…** ，这样就可以创建出一个可以集成的新 Flutter 模块，或者选择导入已有的 Flutter 模块。

此时，Android Studio 插件就会自动为这个 Android 项目配置添加 Flutter 模块作为依赖项，这时集成应用就已准备好进行下一步的构建。

### 手动集成

#### 创建 Flutter 模块

假设你在 `some/path/MyApp` 路径下已有一个 Android 应用，并且你希望 Flutter 项目作为同级项目：

```shell
cd some/path/
flutter create -t module --org com.example my_flutter
```

这会创建一个 `some/path/my_flutter/` 的 Flutter 模块项目，其中包含一些 Dart 代码来帮助你入门以及一个隐藏的子文件夹 `.android/`。 `.android` 文件夹包含一个 Android 项目，该项目不仅可以帮助你通过 `flutter run` 运行这个 Flutter 模块的独立应用，而且还可以作为封装程序来帮助引导 Flutter 模块作为可嵌入的 Android 库。

#### 引入 Java 8

lutter Android 引擎需要使用到 Java 8 中的新特性，故请先确保宿主 Android 应用的 build.gradle 文件的 `android { }` 块中声明了以下源兼容性，例如：

```java
android {
  //...
  compileOptions {
    sourceCompatibility 1.8
    targetCompatibility 1.8
  }
}
```

#### 将 Flutter module 作为依赖项

主要有两种方法实现:

- AAR 机制可以为每个 Flutter 模块创建 Android AAR 作为依赖媒介
- 将 Flutter 模块的源码作为子项目的依赖（需要另外安装 Flutter SDK）

##### 方案 A - 依赖 Android Archive (AAR)

这种方式会将 Flutter 库打包成由 AAR 和 POM artifacts 组成的本地 Maven 存储库。这种方案可以使你的团队不需要安装 Flutter SDK 即可编译宿主应用。之后，你可以从本地或远程存储库中分发更新 artifacts。

假设你在 `some/path/my_flutter` 下构建 Flutter 模块，执行如下命令：

```shell
cd some/path/my_flutter
flutter build aar
```

然后，根据屏幕上的提示完成集成操作

![img](https://flutter.cn/assets/images/docs/development/add-to-app/android/project-setup/build-aar-instructions.png)

详细地说，该命令应用于创建（默认情况下创建 debug/profile/release 所有模式）本地存储库，主要包含以下文件：

```xml
build/host/outputs/repo
└── com
    └── example
        └── my_flutter
            ├── flutter_release
            │   ├── 1.0
            │   │   ├── flutter_release-1.0.aar
            │   │   ├── flutter_release-1.0.aar.md5
            │   │   ├── flutter_release-1.0.aar.sha1
            │   │   ├── flutter_release-1.0.pom
            │   │   ├── flutter_release-1.0.pom.md5
            │   │   └── flutter_release-1.0.pom.sha1
            │   ├── maven-metadata.xml
            │   ├── maven-metadata.xml.md5
            │   └── maven-metadata.xml.sha1
            ├── flutter_profile
            │   ├── ...
            └── flutter_debug
                └── ...
```

要依赖 AAR，宿主应用必须能够找到这些文件。

为此，需要在宿主应用程序中修改 `app/build.gradle` 文件，使其包含本地存储库和上述依赖项：

```shell
android {
  // ...
}

repositories {
  maven {
    url 'some/path/my_flutter/build/host/outputs/repo'
    // This is relative to the location of the build.gradle file
    // if using a relative path.
  }
  maven {
    url 'https://storage.flutter-io.cn/download.flutter.io'
  }
}

dependencies {
  // ...
  debugImplementation 'com.example.flutter_module:flutter_debug:1.0'
  profileImplementation 'com.example.flutter_module:flutter_profile:1.0'
  releaseImplementation 'com.example.flutter_module:flutter_release:1.0'
}
```

##### 方案 B - 依赖模块的源码

该方式可以使你的 Android 项目和 Flutter 项目能够同步一键式构建。当你需要同时在这两个项目中进行快速迭代时，这种方案非常方便，但是此时，你的团队必须安装 Flutter SDK 才能构建宿主应用程序。

将 Flutter 模块作为子项目添加到宿主应用的 `settings.gradle` 中：

```shell
// Include the host app project.
include ':app'                                    // assumed existing content
setBinding(new Binding([gradle: this]))
evaluate(new File(
  settingsDir.parentFile,
  'my_flutter/.android/include_flutter.groovy'
))
```

假设 `my_flutter` 和 `MyApp` 是同级目录。

binding 和 evaluation 脚本可以使 Flutter 模块将其自身（如 `:flutter`）和该模块使用的所有 Flutter 插件（如 `:package_info`，`:video_player` 等）都包含在 `settings.gradle` 的评估的上下文中。

在你的应用中引入对 Flutter 模块的依赖：

```
dependencies {
  implementation project(':flutter')
}
```

此时，你的应用程序已将 Flutter 模块添加为依赖项，下面，你可以按照[向 Android 应用中添加 Flutter 页面]中的后续步骤继续操作。



### 向 Android 应用中添加 Flutter 页面

#### 步骤 1：在 AndroidManifest.xml 中添加 FlutterActivity

Flutter 提供了 [`FlutterActivity`](https://api.flutter-io.cn/javadoc/io/flutter/embedding/android/FlutterActivity.html)，用于在 Android 应用内部展示一个 Flutter 的交互界面。和其他的 [`Activity`](https://developer.android.com/reference/android/app/Activity) 一样，`FlutterActivity` 必须在项目的 `AndroidManifest.xml` 文件中注册。将下边的 XML 代码添加到你的 `AndroidManifest.xml` 文件中的 `application` 标签内：

```xml
<activity
  android:name="io.flutter.embedding.android.FlutterActivity"
  android:theme="@style/LaunchTheme"
  android:configChanges="orientation|keyboardHidden|keyboard|screenSize|locale|layoutDirection|fontScale|screenLayout|density|uiMode"
  android:hardwareAccelerated="true"
  android:windowSoftInputMode="adjustResize"
  />
```

上述代码中的 `@style/LaunchTheme` 可以替换为想要在你的 `FlutterActivity` 中使用的其他 Android 主题。主题的选择决定 Android 系统展示框架所使用的颜色，例如 Android 的导航栏，以及 Flutter UI 自身的第一次渲染前 `FlutterActivity` 的背景色。

### 

#### 步骤 2：加载 FlutterActivity

在你的清单文件中注册了 `FlutterActivity` 之后，根据需要，你可以在应用中的任意位置添加打开 `FlutterActivity` 的代码。下边的代码展示了如何在 `OnClickListener` 的点击事件中打开 `FlutterActivity`。

```kotlin
import io.flutter.embedding.android.FlutterActivity;

myButton.setOnClickListener {
  startActivity(
    FlutterActivity.createDefaultIntent(this)
  )
}

//or 上述的例子假定了你的 Dart 代码入口是调用 main()，并且你的 Flutter 初始路由是 ‘/’。 Dart 代码入口不能通过 Intent 改变，但是初始路由可以通过 Intent 来修改。下面的例子讲解了如何打开一个自定义 Flutter 初始路由的 FlutterActivity。

myButton.setOnClickListener {
  startActivity(
    FlutterActivity
      .withNewEngine()
      .initialRoute("/my_route")
      .build(this)
  )
}

```

可以用你想要的初始路由替换掉 `"/my_route"`。

工厂方法 `withNewEngine()` 可以用于配置一个 `FlutterActivity`，它会在内部创建一个属于自己的 `FlutterEngine` 实例，这会有一个明显的初始化时间。另外一种可选的做法是让 `FlutterActivity` 使用一个预热且缓存的 `FlutterEngine`，这可以最小化 Flutter 初始化的时间。这种方式接下来会讨论到。

#### 步骤 3：（可选）使用缓存的 FlutterEngine

每一个 `FlutterActivity` 默认会创建它自己的 `FlutterEngine`。每一个 `FlutterEngine` 会有一个明显的预热时间。这意味着加载一个标准的 `FlutterActivity` 时，在你的 Flutter 交互页面可见之前会有一个短暂的延迟。想要最小化这个延迟时间，你可以在抵达你的 `FlutterActivity` 之前，初始化一个 `FlutterEngine`，然后使用这个已经预热好的 `FlutterEngine`。

要预热一个 `FlutterEngine`，可以在你的应用中找一个合理的地方实例化一个 `FlutterEngine`。下面的这个例子是在 `Application` 类中预先初始化一个 `FlutterEngine`：

```kotlin
class MyApplication : Application() {
  lateinit var flutterEngine : FlutterEngine

  override fun onCreate() {
    super.onCreate()

    // Instantiate a FlutterEngine.
    flutterEngine = FlutterEngine(this)

    // Start executing Dart code to pre-warm the FlutterEngine.
    flutterEngine.dartExecutor.executeDartEntrypoint(
      DartExecutor.DartEntrypoint.createDefault()
    )

    // Cache the FlutterEngine to be used by FlutterActivity.
    FlutterEngineCache
      .getInstance()
      .put("my_engine_id", flutterEngine)
  }
}

```

传给 [`FlutterEngineCache`](https://api.flutter-io.cn/javadoc/io/flutter/embedding/engine/FlutterEngineCache.html) 的 ID 可以是你想要的任何名称。确保 `FlutterActivity` 或 [`FlutterFragment`](https://api.flutter-io.cn/javadoc/io/flutter/embedding/android/FlutterFragment.html) 在使用缓存的`FlutterEngine` 时，传递了同样的 ID。基于缓存的 `FlutterEngine` 来使用 `FlutterActivity` 会在后续讨论到.



## 平台通道

消息使用平台通道在客户端（UI）和宿主（平台）之间传递，如下图所示：

![Platform channels architecture](https://flutter.cn/assets/images/docs/PlatformChannels.png)

消息和响应以异步的形式进行传递，以确保用户界面能够保持响应。

**注意：**  ` Flutter 是通过 Dart 异步发送消息的。即便如此，当你调用一个平台方法时，也需要在主线程上做调用。`

参考：https://flutter.cn/docs/development/platform-integration/platform-channels

参考：https://flutter.cn/docs/development/add-to-app





### 混合导航栈

![img](https://pic2.zhimg.com/80/v2-8bc5ba8268a87f01f1a7ef4ee670fd79_1440w.jpg)

- 原生页面跳转Flutter页面
- Flutter页面跳转到原生页面
  - Flutter页面打开新的原生页面
  - Flutter页面回退到上一个原生页面

通过method channel实现：

![img](https://pic3.zhimg.com/80/v2-7641a2f9de9f62827af114d8ce90c88a_1440w.jpg)



混合导航栈示例的代码流程：

![img](https://pic2.zhimg.com/80/v2-8fea8686e18edbbba52975adadaa7c5d_1440w.jpg)



### 网络请求

原生工程完成网络模块的封装，Flutter业务模块通过Method Channel来调用。

