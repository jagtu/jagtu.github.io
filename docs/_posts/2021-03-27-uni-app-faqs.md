---
layout: post
title: uni-app注意点摘录
categories: uni-app
description: uni-app常见问题
keywords: uni-app, nvue, vue
---

**注意**

- **关于nvue页面**
  - nvue 暂不支持在 main.js 注册全局组件
  - nvue 页面中只支持`this.$store` ，其它通过`Vue.prototype.$**`定义的全局变量无法访问
- **关于vuex**
  - uni-app不支持直接引入store使用，会导致跟界面的`this.$store`是两个值；可以使用`mapState` `mapGetters` `mapMutations` `mapActions`等辅助方法或者直接使用`this.$store`
  - 

### APP端开发注意事项

#### 1、运行原理

`uni-app` 在非H5端运行时，从架构上分为逻辑层和视图层两个部分。逻辑层负责执行业务逻辑，也就是运行js代码，视图层负责页面渲染。

虽然开发者在一个vue页面里写js和css，但其实，编译时就已经将它们拆分。

**逻辑层**

运行在一个独立的jscore里的，它不依赖于本机的webview

	* 无法运行window、document、navigator、localstorage等浏览器专用的js API
	* `jscore`就是一个标准js引擎，标准js是可以正常运行的，uni-app的App端和小程序端的js引擎，其实是在jscore上补充了一批手机端常用的JS API

<img src="https://img.cdn.aliyun.dcloud.net.cn/uni-app/jscore.jpg" alt="img" style="zoom:40%;" />

**视图层**

h5和小程序平台，以及app-vue，视图层是webview。而app-nvue的视图层是基于weex改造的原生渲染视图。

关于webview，iOS上默认是WKWebview，Android在APP端默认是Android system webview，因此app-vue会有手机浏览器的css兼容问题



**逻辑层和视图层分离的利与弊**

* 窗体动画稳
  * js运算不卡渲染
* 两层互相通信，有通信损耗
  * 不管小程序还是app，不管app-vue还是app-nvue，都有这个两层通信损耗的问题
* 解决方案
  * webview渲染的视图层，提供了一种运行于视图层的专属js，详见：[renderjs](https://uniapp.dcloud.io/frame?id=renderjs)
  * 原生渲染的视图层，weex提供了一套[bindingx](https://uniapp.dcloud.io/nvue-api?id=nvue-里使用-bindingx)机制，可以在js里一次性传一个表达式给原生层
  * 在app-vue和小程序，频繁更新数据的区域做成组件，以防止少量数据更新导致整个页面更新

**优化建议**

* 使用nvue代替vue
* 使用v3编译模式，fast启动模式
* 避免使用大量大图
* 优化数据更新，定义在data里面的数据尽量是视图层需要的
* 减少一次性渲染的节点数量，比如分批加载数据
* 减少组件数量、减少节点嵌套层级
  * 组件的设计策略：复用，频繁数据更新
* 避免视图层和逻辑层频繁进行通讯
  * 减少 scroll-view 组件的 scroll 事件监听，当监听 scroll-view 的滚动事件时，视图层会频繁的向逻辑层发送数据；
  * 监听 scroll-view 组件的滚动事件时，不要实时的改变 scroll-top/scroll-left 属性，因为监听滚动时，视图层向逻辑层通讯，改变 scroll-top/scroll-left 时，逻辑层又向视图层通讯，这样就可能造成通讯卡顿。
  * 注意 onPageScroll 的使用，onPageScroll 进行监听时，视图层会频繁的向逻辑层发送数据；
  * 多使用css动画，而不是通过js的定时器操作界面做动画
  * 如需在canvas里做跟手操作，app端建议使用renderjs，小程序端建议使用web-view组件。web-view里的页面没有逻辑层和视图层分离的概念，自然也不会有通信折损。
* 优化页面切换动画
  * 页面初始化时若存在大量图片或原生组件渲染和大量数据通讯，建议延时100ms~300ms
  * App-nvue和H5，还支持页面预载`uni.preloadPage





#### 2、事件监听与页面通讯

##### uni.$emit(eventName,OBJECT)

触发全局的自定事件。附加参数都会	传给监听器回调。

| 属性      | 类型   | 描述                   |
| --------- | ------ | ---------------------- |
| eventName | String | 事件名                 |
| OBJECT    | Object | 触发事件携带的附加参数 |

**代码示例**

```javascript
    uni.$emit('update',{msg:'页面更新'})
```

##### uni.$on(eventName,callback)

监听全局的自定义事件。事件可以由 uni.$emit 触发，回调函数会接收所有传入事件触发函数的额外参数。

##### uni.$once(eventName,callback)

监听全局的自定义事件。事件可以由 uni.$emit 触发，但是只触发一次，在第一次触发之后移除监听器。

| 属性      | 类型     | 描述           |
| --------- | -------- | -------------- |
| eventName | String   | 事件名         |
| callback  | Function | 事件的回调函数 |

**代码示例**

```javascript
    uni.$on('update',function(data){
        console.log('监听到事件来自 update ，携带参数 msg 为：' + data.msg);
    })

		uni.$once('login',function(data){
        console.log('监听到事件来自 login (只触发一次)，携带参数 msg 为：' + data.msg);
    })
```

##### uni.$off([eventName, callback\])

移除全局自定义事件监听器。

| 属性      | 类型            | 描述           |
| --------- | --------------- | -------------- |
| eventName | Array＜String＞ | 事件名         |
| callback  | Function        | 事件的回调函数 |

**Tips**

- 如果没有提供参数，则移除所有的事件监听器；
- 如果只提供了事件，则移除该事件所有的监听器；
- 如果同时提供了事件与回调，则只移除这个回调的监听器；
- 提供的回调必须跟$on的回调为同一个才能移除这个回调的监听器；

##### uni.onUserCaptureScreen(CALLBACK)

监听用户主动截屏事件，用户使用系统截屏按键截屏时触发此事件。

**平台差异说明**

| App  |  H5  | 微信小程序 | 支付宝小程序 | 百度小程序 | 字节跳动小程序 | QQ小程序 |
| :--: | :--: | :--------: | :----------: | :--------: | :------------: | :------: |
|  x   |  x   |     √      |      √       |     √      |       √        |    √     |

##### uni.onNetworkStatusChange(CALLBACK)

监听网络状态变化。可使用`uni.offNetworkStatusChange`取消监听。

**uni.offNetworkStatusChange(CALLBACK)**

取消监听网络状态变化。

**CALLBACK 返回参数**

| 参数        | 类型    | 说明               | 平台差异说明         |
| :---------- | :------ | :----------------- | :------------------- |
| isConnected | Boolean | 当前是否有网络连接 | 字节跳动小程序不支持 |
| networkType | String  | 网络类型           |                      |





#### 3、[关于 App 的调试debug](https://uniapp.dcloud.io/snippet?id=关于-app-的调试debug)

- APP调试器
- console.log打印日志
- 同步断点到调试器代码调试
- 如果是调试`App`的界面和常规API，推荐编译到H5端

