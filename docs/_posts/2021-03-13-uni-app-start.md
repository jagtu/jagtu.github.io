---
layout: post
title: uni-app快速指南
categories: uni-app
description: 一个跨端开发框架uni-app，基于前端框架vue扩展而来。
keywords: uni-app,跨端开发
---



## uni-app框架介绍与入门

`uni-app` 是一个跨端开发框架，一套代码运行多个平台，并可以使用条件编译实现不同平台的特性编码



![img](https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-uni-app-doc/87a0a0d0-60aa-11eb-8ff1-d5dcf8779628.png)



## 快速入门

`uni-app`支持通过可视化界面，HBuilderX内置相关环境，开箱即用

HBuilderX是通用的前端开发工具，但为`uni-app`做了特别强化。

`uni-app` 使用vue的语法+小程序的标签和API。

## 目录结构

一个uni-app工程，默认包含如下目录及文件：

```
│─components            符合vue组件规范的uni-app组件目录
│  └─comp-a.vue         可复用的a组件
├─hybrid                App端存放本地html文件的目录，详见
├─platforms             存放各平台专用页面的目录，详见
├─pages                 业务页面文件存放的目录
│  ├─index
│  │  └─index.vue       index页面
│  └─list
│     └─list.vue        list页面
├─static                存放应用引用的本地静态资源（如图片、视频等）的目录，注意：静态资源只能存放于此
├─common                自建的通用目录，可以存放一些公用css\less\scss、js等文件
├─store                 数据state共享store目录，通常我们引入vuex使用的目录
│  ├─index.js
├─api                   自建的api目录，可以存放封装服务端API接口、通用网络请求的js等文件
├─uni_modules           存放[uni_module](/uni_modules)规范的插件。
├─node_modules          存放npm导入的第三方包
├─wxcomponents          存放小程序组件的目录，详见
├─main.js               Vue初始化入口文件
├─App.vue               应用配置，用来配置App全局样式以及监听 应用生命周期
├─manifest.json         配置应用名称、appid、logo、版本等打包信息，详见
└─pages.json            配置页面路由、导航条、选项卡等页面类信息，详见
    
```

**Tips**

- 编译到任意平台时，`static` 目录下的文件均会被完整打包进去，且不会编译。非 `static` 目录下的文件（vue、js、css 等）只有被引用到才会被打包编译进去。
- `static` 目录下的 `js` 文件不会被编译，如果里面有 `es6` 的代码，不经过转换直接运行，在手机设备上会报错。

### 配置说明

`pages.json` 文件用来对 uni-app 进行全局配置，决定页面文件的路径、窗口样式、原生的导航栏、底部的原生tabbar 等

`manifest.json` 文件是应用的配置文件，用于指定应用的名称、图标、权限等。HBuilderX 创建的工程此文件在根目录，CLI 创建的工程此文件在 src 目录。

`uni.scss`文件的用途是为了方便整体控制应用的风格。比如按钮颜色、边框风格，`uni.scss`文件里预置了一批scss变量预置。

`App.vue`是uni-app的主组件，所有页面都是在`App.vue`下进行切换的，是页面入口文件。但`App.vue`本身不是页面，这里不能编写视图元素。

这个文件的作用包括：调用应用生命周期函数、配置全局样式、配置全局的存储globalData

应用生命周期仅可在`App.vue`中监听

`main.js`是uni-app的入口文件，主要作用是初始化`vue`实例、定义全局组件、使用需要的插件如vuex。

详细参考：https://uniapp.dcloud.io/collocation/pages





## 单页面程序

`uni-app` 约定页面文件遵循 [Vue 单文件组件 (SFC) 规范](https://vue-loader.vuejs.org/zh/spec.html)，采用简洁的模板语法来声明式地将数据渲染进 DOM 的系统

```css
<template>
	<view class="container">
		{{message}}
	</view>
</template>

<script>
	export default {
		data() {
			return {
				message: 'Hello World',
			}
		}
	}
</script>
<style>
  .container{
    background-color: #FFF;
  }
</style>

```

### 模版语法

Vue.js 使用了基于 HTML 的模板语法，允许开发者声明式地将 DOM 绑定至底层 Vue 实例的数据。所有 Vue.js 的模板都是合法的 HTML，所以能被遵循规范的浏览器和 HTML 解析器解析。

在底层的实现上，Vue 将模板编译成虚拟 DOM 渲染函数。结合响应系统，Vue 能够智能地计算出最少需要重新渲染多少组件，并把 DOM 操作次数减到最少。

模板语法，包含插值、指令

#### 插值

**文本**，使用双大括号：`<span>Message: {{ msg }}</span>`

**原始HTML**，使用v-html指令：`<p>Using v-html directive: <span v-html="rawHtml"></span></p>`

**使用 JavaScript 表达式**，支持JavaScript 表达式：`{{ number + 1 }}`

#### 指令

v-on与v-bind是常用的指令，用于绑定属性设置事件

**Attribute**，使用v-bind 指令（缩写可以直接省略v-bind）：`<button v-bind:disabled="isButtonDisabled">Button</button>`

**事件**，使用 v-on 指令（缩写@）：`<a @click="doSomething">...</a>`

**条件与循环**，`v-if`	`v-else-if`     `v-else`   `v-for`

### 计算属性与侦听器

**计算属性**

计算属性是基于它们的响应式依赖进行缓存的

```
computed: {
  msg: function () {
    return this.message
  }
}
```

**侦听属性**

当你有一些数据需要随着其它数据变动而变动时，用 `watch`

```
watch: {
    // 如果 `question` 发生改变，这个函数就会运行
    question: function (newQuestion, oldQuestion) {
      this.answer = 'Waiting for you to stop typing...'
      this.debouncedGetAnswer()
    }
  }
```

### class与style绑定

通过动态绑定 style、class 属性来控制组件的样式。

- style：静态的样式统一写到 class 中。style 接收动态的样式，在运行时会进行解析，请尽量避免将静态的样式写进 style 中，以免影响渲染速度。

  ```
  <view :style="{color:color, fontSize: fontSize + 'px'}" />
  ```

- 动态绑定 class

  ```
  <view class="normal_view" />
  ```

   [对象语法](https://cn.vuejs.org/v2/guide/class-and-style.html#对象语法-1)：

  ```
  <view :class="{ active: isActive }"></view>
  ```

  上面的语法表示 `active` 这个 class 存在与否将取决于数据  `isActive` （nvue页面不支持）

   [数组语法](https://cn.vuejs.org/v2/guide/class-and-style.html#数组语法-1)：

  ```
  <view v-bind:class="[activeClass, errorClass]"></view>
  data: {
    activeClass: 'active',
    errorClass: 'text-danger'
  }
  ```

**关于选择器注意：**

- 在 `uni-app` 中不能使用 `*` 选择器。

- 目前vue支持的选择器有： `.class`	 `#id  ` 	`element` 	`::after` 	`::before

- nvue页面、微信小程序自定义组件中仅支持 class 选择器

  

### 资源路径说明

#### 模板内引入静态资源

> `template`内引入静态资源，如`image`、`video`等标签的`src`属性时，可以使用相对路径或者绝对路径，形式如下

```html
<!-- 绝对路径，/static指根目录下的static目录，在cli项目中/static指src目录下的static目录 -->
<image class="logo" src="/static/logo.png"></image>
<image class="logo" src="@/static/logo.png"></image>
<!-- 相对路径 -->
<image class="logo" src="../../static/logo.png"></image>
```

**注意**

- `@`开头的绝对路径以及相对路径会经过base64转换规则校验
- 引入的静态资源在非h5平台，均不转为base64。
- H5平台，小于4kb的资源会被转换成base64，其余不转。
- 自`HBuilderX 2.6.6`起`template`内支持`@`开头路径引入静态资源，旧版本不支持此方式
- App平台自`HBuilderX 2.6.9`起`template`节点中引用静态资源文件时（如：图片），调整查找策略为【基于当前文件的路径搜索】，与其他平台保持一致
- 支付宝小程序组件内 image 标签不可使用相对路径

#### js文件引入

> `js`文件或`script`标签内（包括renderjs等）引入`js`文件时，可以使用相对路径和绝对路径，形式如下

```js
// 绝对路径，@指向项目根目录，在cli项目中@指向src目录
import add from '@/common/add.js'
// 相对路径
import add from '../../common/add.js'
```

**注意**

- js文件不支持使用`/`开头的方式引入

#### css引入静态资源

> `css`文件或`style标签`内引入`css`文件时（scss、less文件同理），可以使用相对路径或绝对路径（`HBuilderX 2.6.6`）

```css
/* 绝对路径 */
@import url('/common/uni.css');
@import url('@/common/uni.css');
/* 相对路径 */
@import url('../../common/uni.css');
```

**注意**

- 自`HBuilderX 2.6.6`起支持绝对路径引入静态资源，旧版本不支持此方式

> `css`文件或`style标签`内引用的图片路径可以使用相对路径也可以使用绝对路径，需要注意的是，有些小程序端css文件不允许引用本地文件（请看注意事项）。

```css
/* 绝对路径 */
background-image: url(/static/logo.png);
background-image: url(@/static/logo.png);
/* 相对路径 */
background-image: url(../../static/logo.png);
```

**Tips**

- 引入字体图标请参考，[字体图标](https://uniapp.dcloud.io/frame?id=字体图标)

- `@`开头的绝对路径以及相对路径会经过base64转换规则校验

- 不支持本地图片的平台，小于40kb，一定会转base64。（共四个平台mp-weixin, mp-qq, mp-toutiao, app v2）

- h5平台，小于4kb会转base64，超出4kb时不转。

- 其余平台不会转base64

  

## 生命周期

### 应用生命周期

`uni-app` 支持如下应用生命周期函数：

| 函数名               | 说明                                                         |
| :------------------- | :----------------------------------------------------------- |
| onLaunch             | 当`uni-app` 初始化完成时触发（全局只触发一次）               |
| onShow               | 当 `uni-app` 启动，或从后台进入前台显示                      |
| onHide               | 当 `uni-app` 从前台进入后台                                  |
| onError              | 当 `uni-app` 报错时触发                                      |
| onUniNViewMessage    | 对 `nvue` 页面发送的数据进行监听，可参考 [nvue 向 vue 通讯](https://uniapp.dcloud.io/nvue-api?id=communication) |
| onUnhandledRejection | 对未处理的 Promise 拒绝事件监听函数（2.8.1+）                |
| onPageNotFound       | 页面不存在监听函数                                           |
| onThemeChange        | 监听系统主题变化                                             |

**注意**

- 应用生命周期仅可在`App.vue`中监听，在其它页面监听无效。

  **示例代码**

  ```html
  <script>
      // 只能在App.vue里监听应用的生命周期
      export default {
          onLaunch: function() {
              console.log('App Launch')
          },
          onShow: function() {
              console.log('App Show')
          },
          onHide: function() {
              console.log('App Hide')
          }
      }
  </script>
  ```



### 页面生命周期

`uni-app` 支持如下页面生命周期函数：

| 函数名                              | 说明                                                         | 平台差异说明                                                 | 最低版本 |
| :---------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- | :------- |
| onInit                              | 监听页面初始化，其参数同 onLoad 参数，为上个页面传递的数据，参数类型为 Object（用于页面传参），触发时机早于 onLoad | 百度小程序                                                   | 3.1.0+   |
| onLoad                              | 监听页面加载，其参数为上个页面传递的数据，参数类型为 Object（用于页面传参），参考[示例](https://uniapp.dcloud.io/api/router?id=navigateto) |                                                              |          |
| onShow                              | 监听页面显示。页面每次出现在屏幕上都触发，包括从下级页面点返回露出当前页面 |                                                              |          |
| onReady                             | 监听页面初次渲染完成。注意如果渲染速度快，会在页面进入动画完成前触发 |                                                              |          |
| onHide                              | 监听页面隐藏                                                 |                                                              |          |
| onUnload                            | 监听页面卸载                                                 |                                                              |          |
| onResize                            | 监听窗口尺寸变化                                             | App、微信小程序                                              |          |
| onPullDownRefresh                   | 监听用户下拉动作，一般用于下拉刷新，参考[示例](https://uniapp.dcloud.io/api/ui/pulldown) |                                                              |          |
| onReachBottom                       | 页面滚动到底部的事件（不是scroll-view滚到底），常用于下拉下一页数据。具体见下方注意事项 |                                                              |          |
| onTabItemTap                        | 点击 tab 时触发，参数为Object，具体见下方注意事项            | 微信小程序、支付宝小程序、百度小程序、H5、App（自定义组件模式） |          |
| onShareAppMessage                   | 用户点击右上角分享                                           | 微信小程序、百度小程序、字节跳动小程序、支付宝小程序         |          |
| onPageScroll                        | 监听页面滚动，参数为Object                                   | nvue暂不支持                                                 |          |
| onNavigationBarButtonTap            | 监听原生标题栏按钮点击事件，参数为Object                     | App、H5                                                      |          |
| onBackPress                         | 监听页面返回，返回 event = {from:backbutton、 navigateBack} ，backbutton 表示来源是左上角返回按钮或 android 返回键；navigateBack表示来源是 uni.navigateBack ；详细说明及使用：[onBackPress 详解](http://ask.dcloud.net.cn/article/35120)。支付宝小程序只有真机能触发，只能监听非navigateBack引起的返回，不可阻止默认行为。 | app、H5、支付宝小程序                                        |          |
| onNavigationBarSearchInputChanged   | 监听原生标题栏搜索输入框输入内容变化事件                     | App、H5                                                      | 1.6.0    |
| onNavigationBarSearchInputConfirmed | 监听原生标题栏搜索输入框搜索事件，用户点击软键盘上的“搜索”按钮时触发。 | App、H5                                                      | 1.6.0    |
| onNavigationBarSearchInputClicked   | 监听原生标题栏搜索输入框点击事件                             | App、H5                                                      | 1.6.0    |
| onShareTimeline                     | 监听用户点击右上角转发到朋友圈                               | 微信小程序                                                   | 2.8.1+   |
| onAddToFavorites                    | 监听用户点击右上角收藏                                       | 微信小程序                                                   | 2.8.1+   |



## 全局变量

### globalData

在 App.vue 可以定义 globalData ，globalData支持vue和nvue共享数据

定义：App.vue

```html
<script>  
    export default {  
        globalData: {  
            text: 'text'  
        },  
        onLaunch: function() {  
            console.log('App Launch')  
        },  
        onShow: function() {  
            console.log('App Show')  
        },  
        onHide: function() {  
            console.log('App Hide')  
        }  
    }  
</script>  

<style>  
    /*每个页面公共css */  
</style>  
```

js中操作globalData的方式如下：

赋值：`getApp().globalData.text = 'test'`

取值：`console.log(getApp().globalData.text) // 'test'`

如果需要把globalData的数据绑定到页面上，可在页面的onshow声明周期里进行变量重赋值。



### vue渲染原理



![data](https://cn.vuejs.org/images/data.png)

* 声明式响应，在初始化实例前声明所有根级响应式 property

* Vue 在更新 DOM 时是**异步**执行的

### vue的状态管理模式 

![状态管理](https://cn.vuejs.org/images/state.png)

### 使用vuex管理共享状态

![vuex](https://vuex.vuejs.org/vuex.png)

### vuex-persistedstate

持久化



## npm支持

*npm* 是 JavaScript 世界的包管理工具。通过 *npm* 可以安装、共享、分发代码,管理项目依赖关系。





## 路由

`uni-app`页面路由为框架统一管理，开发者需要在[pages.json](https://uniapp.dcloud.io/collocation/pages?id=pages)里配置每个路由页面的路径及页面样式，pages 节点接收一个数组，数组每个项都是一个对象，其属性值如下：

| 属性  | 类型   | 默认值 | 描述                                                         |
| :---- | :----- | :----- | :----------------------------------------------------------- |
| path  | String |        | 配置页面路径                                                 |
| style | Object |        | 配置页面窗口表现，配置项参考下方 [pageStyle](https://uniapp.dcloud.io/collocation/pages?id=style) |

**Tips：**

- pages节点的第一项为应用入口页（即首页）

**style**：

用于设置每个页面的状态栏、导航条、标题、窗口背景色等。

页面中配置项会覆盖 [globalStyle](https://uniapp.dcloud.io/collocation/pages?id=globalstyle) 中相同的配置项

| 属性                         | 类型     | 默认值  | 描述                                                         | 平台差异说明                                   |
| :--------------------------- | :------- | :------ | :----------------------------------------------------------- | :--------------------------------------------- |
| navigationBarBackgroundColor | HexColor | #000000 | 导航栏背景颜色（同状态栏背景色），如"#000000"                |                                                |
| navigationBarTextStyle       | String   | white   | 导航栏标题颜色及状态栏前景颜色，仅支持 black/white           |                                                |
| navigationBarTitleText       | String   |         | 导航栏标题文字内容                                           |                                                |
| navigationBarShadow          | Object   |         | 导航栏阴影，配置参考下方 [导航栏阴影](https://uniapp.dcloud.io/collocation/pages?id=navigationbarshadow) |                                                |
| navigationStyle              | String   | default | 导航栏样式，仅支持 default/custom。custom即取消默认的原生导航栏，需看[使用注意](https://uniapp.dcloud.io/collocation/pages?id=customnav) | 微信小程序 7.0+、百度小程序、H5、App（2.0.3+） |
| disableScroll                | Boolean  | false   | 设置为 true 则页面整体不能上下滚动（bounce效果），只在页面配置中有效，在globalStyle中设置无效 | 微信小程序（iOS）、百度小程序（iOS）           |
| backgroundColor              | HexColor | #ffffff | 窗口的背景色                                                 | 微信小程序、百度小程序、字节跳动小程序         |
| backgroundTextStyle          | String   | dark    | 下拉 loading 的样式，仅支持 dark/light                       |                                                |
| enablePullDownRefresh        | Boolean  | false   | 是否开启下拉刷新，详见[页面生命周期](https://uniapp.dcloud.io/collocation/frame/lifecycle?id=页面生命周期)。 |                                                |
| onReachBottomDistance        | Number   | 50      | 页面上拉触底事件触发时距页面底部距离，单位只支持px，详见[页面生命周期](https://uniapp.dcloud.io/collocation/frame/lifecycle?id=页面生命周期) |                                                |
| backgroundColorTop           | HexColor | #ffffff | 顶部窗口的背景色（bounce回弹区域）                           | 仅 iOS 平台                                    |
| backgroundColorBottom        | HexColor | #ffffff | 底部窗口的背景色（bounce回弹区域）                           | 仅 iOS 平台                                    |
| titleImage                   | String   |         | 导航栏图片地址（替换当前文字标题），支付宝小程序内必须使用https的图片链接地址 | 支付宝小程序、H5                               |
| transparentTitle             | String   | none    | 导航栏透明设置。支持 always 一直透明 / auto 滑动自适应 / none 不透明 | 支付宝小程序、H5、APP                          |
| titlePenetrate               | String   | NO      | 导航栏点击穿透                                               | 支付宝小程序、H5                               |
| app-plus                     | Object   |         | 设置编译到 App 平台的特定样式，配置项参考下方 [app-plus](https://uniapp.dcloud.io/collocation/pages?id=app-plus) | App                                            |
| h5                           | Object   |         | 设置编译到 H5 平台的特定样式，配置项参考下方 [H5](https://uniapp.dcloud.io/collocation/pages?id=h5) | H5                                             |
| mp-alipay                    | Object   |         | 设置编译到 mp-alipay 平台的特定样式，配置项参考下方 [MP-ALIPAY](https://uniapp.dcloud.io/collocation/pages?id=mp-alipay) | 支付宝小程序                                   |
| mp-weixin                    | Object   |         | 设置编译到 mp-weixin 平台的特定样式                          | 微信小程序                                     |
| mp-baidu                     | Object   |         | 设置编译到 mp-baidu 平台的特定样式                           | 百度小程序                                     |
| mp-toutiao                   | Object   |         | 设置编译到 mp-toutiao 平台的特定样式                         | 字节跳动小程序                                 |
| mp-qq                        | Object   |         | 设置编译到 mp-qq 平台的特定样式                              | QQ小程序                                       |
| usingComponents              | Object   |         | 引用小程序组件，参考 [小程序组件](https://uniapp.dcloud.io/frame?id=小程序组件支持) | App、微信小程序、支付宝小程序、百度小程序      |
| leftWindow                   | Boolean  | true    | 当存在 leftWindow时，当前页面是否显示 leftWindow             | H5                                             |
| topWindow                    | Boolean  | true    | 当存在 topWindow 时，当前页面是否显示 topWindow              | H5                                             |
| rightWindow                  | Boolean  | true    | 当存在 rightWindow时，当前页面是否显示 rightWindow           | H5                                             |
| maxWidth                     | Number   | 1190    | 单位px，当浏览器可见区域宽度大于maxWidth时，两侧留白，当小于等于maxWidth时，页面铺满；不同页面支持配置不同的maxWidth；maxWidth = leftWindow(可选)+page(页面主体)+rightWindow(可选) | H5（2.9.9+）                                   |



### 路由跳转

`uni-app` 有两种页面路由跳转方式：使用[navigator](https://uniapp.dcloud.io/component/navigator)组件跳转、调用[API](https://uniapp.dcloud.io/api/router)跳转。

**navigator**:

页面跳转组件类似HTML中的`<a>`组件，只能跳转本地页面。目标页面必须在pages.json中注册。

*示例*

```html
<template>
    <view>
        <view class="page-body">
            <view class="btn-area">
                <navigator url="navigate/navigate?title=navigate" hover-class="navigator-hover">
                    <button type="default">跳转到新页面</button>
                </navigator>
                <navigator url="redirect/redirect?title=redirect" open-type="redirect" hover-class="other-navigator-hover">
                    <button type="default">在当前页打开</button>
                </navigator>
                <navigator url="/pages/tabBar/extUI/extUI" open-type="switchTab" hover-class="other-navigator-hover">
                    <button type="default">跳转tab页面</button>
                </navigator>
            </view>
        </view>
    </view>
```



**API**：

`uni.navigateTo(OBJECT)` 保留当前页面，跳转到应用内的某个页面，使用uni.navigateBack可以返回到原页面。

`uni.redirectTo(OBJECT)` 关闭当前页面，跳转到应用内的某个页面。

`uni.reLaunch(OBJECT)` 关闭所有页面，打开到应用内的某个页面。

`uni.switchTab(OBJECT)` 跳转到 tabBar 页面，并关闭其他所有非 tabBar 页面。

`uni.navigateBack(OBJECT)` 关闭当前页面，返回上一页面或多级页面。

### 页面栈

框架以栈的形式管理当前所有页面， 当发生路由切换的时候，页面栈的表现如下：

| 路由方式   | 页面栈表现                        | 触发时机                                                     |
| ---------- | --------------------------------- | ------------------------------------------------------------ |
| 初始化     | 新页面入栈                        | uni-app 打开的第一个页面                                     |
| 打开新页面 | 新页面入栈                        | 调用 API  [uni.navigateTo](https://uniapp.dcloud.io/api/router?id=navigateto) 、使用组件<navigator open-type="navigate"/> |
| 页面重定向 | 当前页面出栈，新页面入栈          | 调用 API  [uni.redirectTo](https://uniapp.dcloud.io/api/router?id=redirectto) 、使用组件<navigator open-type="redirectTo"/> |
| 页面返回   | 页面不断出栈，直到目标返回页      | 调用 API  [uni.navigateBack](https://uniapp.dcloud.io/api/router?id=navigateback)  、使用组件<navigator open-type="navigateBack"/>、用户按左上角返回按钮、安卓用户点击物理back按键 |
| Tab 切换   | 页面全部出栈，只留下新的 Tab 页面 | 调用 API  [uni.switchTab](https://uniapp.dcloud.io/api/router?id=switchtab) 、使用组件<navigator open-type="switchTab"/>、用户切换 Tab |
| 重加载     | 页面全部出栈，只留下新的页面      | 调用 API  [uni.reLaunch](https://uniapp.dcloud.io/api/router?id=relaunch) 、使用组件<navigator open-type="reLaunch"/> |



## 运行环境判断

### 开发环境和生产环境

```javascript
if(process.env.NODE_ENV === 'development'){
    console.log('开发环境')
}else{
    console.log('生产环境')
}
```

注意：

- 在HBuilderX 中，点击“运行”编译出来的代码是开发环境，点击“发行”编译出来的代码是生产环境
- cli模式下，是通行的编译环境处理方式。
- 如果你需要自定义更多环境，比如测试环境：
  - 假设只需要对单一平台配置，可以 package.json 中配置，在HBuilderX的运行和发行菜单里会多一个出来。https://uniapp.dcloud.io/collocation/package
  - 如果是针对所有平台配置，可以在 vue-config.js 中配置。https://uniapp.dcloud.io/collocation/vue-config
- HBuilderX 中敲入代码块 `uEnvDev`、`uEnvProd` 可以快速生成对应 `development`、`production` 的运行环境判定代码。

### 判断平台

平台判断有2种场景，一种是在编译期判断，一种是在运行期判断。

- 编译期判断，即条件编译，不同平台在编译出包后已经是不同的代码。

  以 #ifdef 或 #ifndef 加 **%PLATFORM%** 开头，以 #endif 结尾。

  | 值                      | 平台                                                         |
  | :---------------------- | :----------------------------------------------------------- |
  | APP-PLUS                | App                                                          |
  | APP-PLUS-NVUE或APP-NVUE | App nvue                                                     |
  | H5                      | H5                                                           |
  | MP-WEIXIN               | 微信小程序                                                   |
  | MP-ALIPAY               | 支付宝小程序                                                 |
  | MP-BAIDU                | 百度小程序                                                   |
  | MP-TOUTIAO              | 字节跳动小程序                                               |
  | MP-QQ                   | QQ小程序                                                     |
  | MP-360                  | 360小程序                                                    |
  | MP                      | 微信小程序/支付宝小程序/百度小程序/字节跳动小程序/QQ小程序/360小程序 |
  | QUICKAPP-WEBVIEW        | 快应用通用(包含联盟、华为)                                   |
  | QUICKAPP-WEBVIEW-UNION  | 快应用联盟                                                   |
  | QUICKAPP-WEBVIEW-HUAWEI | 快应用华为                                                   |

```javascript
// #ifdef H5
    alert("只有h5平台才有alert方法")
// #endif
```

如上代码只会编译到H5的发行包里，其他平台的包不会包含如上代码。

- 运行期判断 运行期判断是指代码已经打入包中，仍然需要在运行期判断平台，此时可使用 `uni.getSystemInfoSync().platform` 判断客户端环境是 Android、iOS 还是小程序开发工具（在百度小程序开发工具、微信小程序开发工具、支付宝小程序开发工具中使用 `uni.getSystemInfoSync().platform` 返回值均为 devtools）。

```javascript
switch(uni.getSystemInfoSync().platform){
    case 'android':
       console.log('运行Android上')
       break;
    case 'ios':
       console.log('运行iOS上')
       break;
    default:
       console.log('运行在开发者工具上')
       break;
}
```

如有必要，也可以在条件编译里自己定义一个变量，赋不同值。在后续运行代码中动态判断环境。





## 页面样式与布局

uni-app的css与web的css基本一致。

uni-app有vue页面和nvue页面。vue页面是webview渲染的、app端的nvue页面是原生渲染的

### 尺寸单位

`uni-app` 支持的通用 css 单位包括 px、rpx

- px 即屏幕像素
- rpx 即响应式px，一种根据屏幕宽度自适应的动态单位。以750宽的屏幕为基准，750rpx恰好为屏幕宽度。屏幕变宽，rpx 实际显示效果会等比放大，但在 App 端和 H5 端屏幕宽度达到 960px 时，默认将按照 375px 的屏幕宽度进行计算
- uni-app 2.9+ 新增 leftWindow, topWindow, rightWindow 配置。用于解决宽屏适配问题。

**nvue还不支持百分比单位**

vue页面支持下面这些普通H5单位，但在nvue里不支持：

- rem 根字体大小可以通过 [page-meta](https://uniapp.dcloud.io/component/page-meta?id=page-meta) 配置
- vh viewpoint height，视窗高度，1vh等于视窗高度的1%
- vw viewpoint width，视窗宽度，1vw等于视窗宽度的1%

App端，在 pages.json 里的 titleNView 或页面里写的 plus api 中涉及的单位，只支持 px。**注意此时不支持 rpx**

nvue中，uni-app 模式（[nvue 不同编译模式介绍](https://ask.dcloud.net.cn/article/36074)）可以使用 px 、rpx，表现与 vue 中一致。weex 模式目前遵循weex的单位，它的单位比较特殊：

- px:，以750宽的屏幕为基准动态计算的长度单位，与 vue 页面中的 rpx 理念相同。（一定要注意 weex 模式的 px，和 vue 里的 px 逻辑不一样。）
- wx：与设备屏幕宽度无关的长度单位，与 vue 页面中的 px 理念相同

下面对 `rpx` 详细说明：

设计师在只提供一个分辨率的图，严格按设计图标注的 px 做开发，在不同宽度的手机上界面的宽度很容易变形。

 `rpx` 动态宽度单位，就是为了解决这个问题。`uni-app` 在 App 端、H5 端都支持了 `rpx`

在Pages.json的globalStyle中可以配置不同屏幕宽度的计算方式，具体参考：

| 属性                   | 默认值 | 描述                                                         | 平台兼容 |
| ---------------------- | ------ | ------------------------------------------------------------ | -------- |
| rpxCalcMaxDeviceWidth  | 960    | rpx 计算所支持的最大设备宽度，单位 px                        | App、H5  |
| rpxCalcBaseDeviceWidth | 375    | rpx 计算使用的基准设备宽度，设备实际宽度超出 rpx 计算所支持的最大设备宽度时将按基准宽度计算，单位 px | App、H5  |
| rpxCalcIncludeWidth    | 750    | rpx 计算特殊处理的值，始终按实际的设备宽度计算，单位 rpx     | App、H5  |
| maxWidth               | 1190   | 单位px，当浏览器可见区域宽度大于maxWidth时，两侧留白，当小于等于maxWidth时，页面铺满；不同页面支持配置不同的maxWidth；maxWidth = leftWindow(可选)+page(页面主体)+rightWindow(可选) | H5       |

rpx 是相对于基准宽度的单位，可以根据屏幕宽度进行自适应。`uni-app` 规定屏幕基准宽度 750rpx，页面元素宽度在 `uni-app` 中的宽度计算公式：

```
宽度rpx = 750 * 元素在设计稿中的宽度 / 设计稿基准宽度
```

**比如：** 若设计稿宽度为 750px，元素 A 在设计稿上的宽度为 100px，那么元素 A 在 `uni-app` 里面的宽度应该设为：`750 * 100 / 750`，结果为：100rpx。



**Tips**

- 注意 rpx 是和宽度相关的单位，屏幕越宽，该值实际像素越大。如不想根据屏幕宽度缩放，则应该使用 px 单位。
- 如果开发者在字体或高度中也使用了 rpx ，那么需注意这样的写法意味着随着屏幕变宽，字体会变大、高度会变大。如果你需要固定高度，则应该使用 px 。
- rpx不支持动态横竖屏切换计算，使用rpx建议锁定屏幕方向
- 设计师可以用 iPhone6 作为视觉稿的标准。
- 如果设计稿不是750px，HBuilderX提供了自动换算的工具，详见：https://ask.dcloud.net.cn/article/35445。
- App端，在 pages.json 里的 titleNView 或页面里写的 plus api 中涉及的单位，只支持 px，不支持 rpx。
- 早期 uni-app 提供了 upx ，目前已经推荐统一改为 rpx 了，[详见](http://ask.dcloud.net.cn/article/36130)

### 样式导入

使用`@import`语句可以导入外联样式表，`@import`后跟需要导入的外联样式表的相对路径，用`;`表示语句结束。

**示例代码：**

```
<style>
    @import "../../common/uni.css";

    .uni-card {
        box-shadow: none;
    }
</style>
```



### 全局样式与局部样式

定义在 App.vue 中的样式为全局样式，作用于每一个页面。在 pages 目录下 的 vue 文件中定义的样式为局部样式，只作用在对应的页面，并会覆盖 App.vue 中相同的选择器。

**注意：**

- App.vue 中通过 `@import` 语句可以导入外联样式，一样作用于每一个页面。
- nvue页面暂不支持全局样式

### CSS变量

uni-app 提供内置 CSS 变量

| CSS变量             | 描述                   | App                                                          | 小程序 | H5                   |
| :------------------ | :--------------------- | :----------------------------------------------------------- | :----- | :------------------- |
| --status-bar-height | 系统状态栏高度         | [系统状态栏高度](http://www.html5plus.org/doc/zh_cn/navigator.html#plus.navigator.getStatusbarHeight)、nvue页面不支持 | 25px   | 0                    |
| --window-top        | 内容区域距离顶部的距离 | 0                                                            | 0      | NavigationBar 的高度 |
| --window-bottom     | 内容区域距离底部的距离 | 0                                                            | 0      | TabBar 的高度        |

**注意：**

- `var(--status-bar-height)` 此变量在微信小程序环境为固定 `25px`，在 App 里为手机实际状态栏高度。
- 当设置 `"navigationStyle":"custom"` 取消原生导航栏后，由于窗体为沉浸式，占据了状态栏位置。此时可以使用一个高度为 `var(--status-bar-height)` 的 view 放在页面顶部，避免页面内容出现在状态栏。
- 由于在H5端，不存在原生导航栏和tabbar，也是前端div模拟。如果设置了一个固定位置的居底view，在小程序和App端是在tabbar上方，但在H5端会与tabbar重叠。此时可使用`--window-bottom`，不管在哪个端，都是固定在tabbar上方。
- 目前 nvue 在App端，还不支持 `--status-bar-height`变量，替代方案是在页面onLoad时通过uni.getSystemInfoSync().statusBarHeight获取状态栏高度，然后通过style绑定方式给占位view设定高度。下方提供了示例代码

**代码块**

快速书写css变量的方法是：在css中敲hei，在候选助手中即可看到3个css变量。（HBuilderX 1.9.6以上支持）

示例1 - 普通页面使用css变量：

```html
<template>
    <!-- HBuilderX 2.6.3+ 新增 page-meta, 详情：https://uniapp.dcloud.io/component/page-meta -->
    <page-meta>
        <navigation-bar />
    </page-meta>
    <view>
        <view class="status_bar">
            <!-- 这里是状态栏 -->
        </view>
        <view> 状态栏下的文字 </view>
    </view>
</template>    
<style>
    .status_bar {
        height: var(--status-bar-height);
        width: 100%;
    }
</style>
<template>
    <view>
        <view class="toTop">
            <!-- 这里可以放一个向上箭头，它距离底部tabbar上浮10px-->
        </view>
    </view>
</template>    
<style>
    .toTop {
        bottom: calc(var(--window-bottom) + 10px)
    }
</style>
```

示例2 - nvue页面获取状态栏高度

```html
<template>
    <view class="content">
        <view :style="{ height: iStatusBarHeight + 'px'}"></view>
    </view>
</template>

<script>
    export default {
        data() {
            return {
                iStatusBarHeight:0
            }
        },
        onLoad() {
            this.iStatusBarHeight = uni.getSystemInfoSync().statusBarHeight
        }
    }
</script>
```

### 固定值

`uni-app` 中以下组件的高度是固定的，不可修改：

| 组件          | 描述       | App                      | H5   |
| :------------ | :--------- | :----------------------- | :--- |
| NavigationBar | 导航栏     | 44px                     | 44px |
| TabBar        | 底部选项卡 | 50px（可以自主更改高度） | 50px |

各小程序平台，包括同小程序平台的iOS和Android的高度也不一样。

### Flex布局

为支持跨平台，框架建议使用Flex布局，关于Flex布局可以参考外部文档[阮一峰的flex教程](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)等。



### 背景图片

`uni-app` 支持使用在 css 里设置背景图片，使用方式与普通 `web` 项目大体相同，但需要注意以下几点：

- 支持 base64 格式图片。

- 支持网络路径图片。

- 小程序不支持在css中使用本地文件，包括本地的背景图和字体文件。需以base64方式方可使用。App端在v3模式以前，也有相同限制。v3编译模式起支持直接使用本地背景图和字体。

- 使用本地路径背景图片需注意：

  1. 为方便开发者，在背景图片小于 40kb 时，`uni-app` 编译到不支持本地背景图的平台时，会自动将其转化为 base64 格式；

  2. 图片大于等于 40kb，会有性能问题，不建议使用太大的背景图，如开发者必须使用，则需自己将其转换为 base64 格式使用，或将其挪到服务器上，从网络地址引用。

  3. 本地背景图片的引用路径推荐使用以 ~@ 开头的绝对路径。

     ```css
      .test2 {
          background-image: url('~@/static/logo.png');
      }
     ```

**注意**

- 微信小程序不支持相对路径（真机不支持，开发工具支持）

### 字体图标

`uni-app` 支持使用字体图标，使用方式与普通 `web` 项目相同，需要注意以下几点：

- 支持 base64 格式字体图标。

- 支持网络路径字体图标。

- 小程序不支持在css中使用本地文件，包括本地的背景图和字体文件。需以base64方式方可使用。App端在v3模式以前，也有相同限制。v3编译模式起支持直接使用本地背景图和字体。

- 网络路径必须加协议头 `https`。

- 从 [http://www.iconfont.cn](http://www.iconfont.cn/) 上拷贝的代码，默认是没加协议头的。

- 从 [http://www.iconfont.cn](http://www.iconfont.cn/) 上下载的字体文件，都是同名字体（字体名都叫iconfont，安装字体文件时可以看到），在nvue内使用时需要注意，此字体名重复可能会显示不正常，可以使用工具修改。

- 使用本地路径图标字体需注意：

  1. 为方便开发者，在字体文件小于 40kb 时，`uni-app` 会自动将其转化为 base64 格式；

  2. 字体文件大于等于 40kb，仍转换为 base64 方式使用的话可能有性能问题，如开发者必须使用，则需自己将其转换为 base64 格式使用，或将其挪到服务器上，从网络地址引用；

  3. 字体文件的引用路径推荐使用以 ~@ 开头的绝对路径。

     ```css
      @font-face {
          font-family: test1-icon;
          src: url('~@/static/iconfont.ttf');
      }
     ```

`nvue`中不可直接使用css的方式引入字体文件，需要使用以下方式在js内引入。nvue内不支持本地路径引入字体，请使用网络链接或者`base64`形式。**`src`字段的`url`的括号内一定要使用单引号。**

```js
var domModule = weex.requireModule('dom');
domModule.addRule('fontFace', {
  'fontFamily': "fontFamilyName",
  'src': "url('https://...')"
})
```

**示例：**

```html
<template>
    <view>
        <view>
            <text class="test">&#xe600;</text>
            <text class="test">&#xe687;</text>
            <text class="test">&#xe60b;</text>
        </view>
    </view>
</template>
<style>
    @font-face {
        font-family: 'iconfont';
        src: url('https://at.alicdn.com/t/font_865816_17gjspmmrkti.ttf') format('truetype');
    }
    .test {
        font-family: iconfont;
        margin-left: 20rpx;
    }
</style>
```



### nvue页面的样式注意事项

- nvue的css**仅支持flex布局**
- class 进行绑定时只支持数组语法。
- 不能使用百分比布局，如`width：100%`
- 不支持在css里写背景图`background-image`，但可以使用image组件和层级来实现类似web中的背景效果。因为原生开发本身也没有web这种背景图概念
- `nvue `的各组件在安卓端默认是透明的，如果不设置`background-color`，可能会导致出现重影的问题
- 文字内容，必须只能在`text`组件下，`text`组件不能换行写内容，否则会出现无法去除的周边空白
- 只有`text`标签可以设置字体大小，字体颜色
- 使用`image`标签，支持使用base64
- 不支持媒体查询，不能在 style 中引入字体文件
- 新增 `nvueStyleCompiler` 配置，支持组合选择器（相邻兄弟选择器、普通兄弟选择器、子选择器、后代选择器）。[详见](https://ask.dcloud.net.cn/article/38751)

- nvue的`uni-app`编译模式下，App.vue 中的样式，会编译到每个 nvue文件。对于共享样式，如果有不合法属性控制台会给出警告，可以通过[条件编译](https://uniapp.dcloud.io/platform)`APP-PLUS-NVUE`屏蔽 App 中的警告。
- nvue布局模型基于 CSS Flexbox，**是默认且唯一的布局模型，不需要手动为元素添加** `display: flex;` 
- 基于原生引擎的渲染，虽然还是前端技术栈，但和web开发肯定是有区别的。
  1. nvue 页面控制显隐只可以使用`v-if`不可以使用`v-show`
  2. nvue 页面只能使用`flex`布局，不支持其他布局方式。页面开发前，首先想清楚这个页面的纵向内容有什么，哪些是要滚动的，然后每个纵向内容的横轴排布有什么，按 flex 布局设计好界面。
  3. nvue 页面的布局排列方向默认为竖排（`column`），如需改变布局方向，可以在 `manifest.json` -> `app-plus` -> `nvue` -> `flex-direction` 节点下修改，仅在 uni-app 模式下生效。[详情](https://uniapp.dcloud.io/collocation/manifest?id=nvue)。
  4. nvue页面编译为H5、小程序时，会做一件css默认值对齐的工作。因为weex渲染引擎只支持flex，并且默认flex方向是垂直。而H5和小程序端，使用web渲染，默认不是flex，并且设置`display:flex`后，它的flex方向默认是水平而不是垂直的。所以nvue编译为H5、小程序时，会自动把页面默认布局设为flex、方向为垂直。当然开发者手动设置后会覆盖默认设置。
  5. 文字内容，必须、只能在`<text>`组件下。不能在`<div>`、`<view>`的`text`区域里直接写文字。否则即使渲染了，也无法绑定js里的变量。
  6. 只有`text`标签可以设置字体大小，字体颜色。
  7. 布局不能使用百分比、没有媒体查询。
  8. nvue 切换横竖屏时可能导致样式出现问题，建议有 nvue 的页面锁定手机方向。
  9. 支持的css有限，不过并不影响布局出你需要的界面，`flex`还是非常强大的。详见
  10. 不支持背景图。但可以使用`image`组件和层级来实现类似web中的背景效果。因为原生开发本身也没有web这种背景图概念
  11. css选择器支持的比较少，只能使用 class 选择器。[详见](https://uniapp.dcloud.io/nvue-css)
  12. nvue 的各组件在安卓端默认是透明的，如果不设置`background-color`，可能会导致出现重影的问题。
  13. `class` 进行绑定时只支持数组语法。
  14. Android端在一个页面内使用大量圆角边框会造成性能问题，尤其是多个角的样式还不一样的话更耗费性能。应避免这类使用。
  15. nvue页面没有`bounce`回弹效果，只有几个列表组件有`bounce`效果，包括 `list`、`recycle-list`、`waterfall`。
  16. 原生开发没有页面滚动的概念，页面内容高过屏幕高度并不会自动滚动，只有部分组件可滚动（`list`、`waterfall`、`scroll-view/scroller`），要滚得内容需要套在可滚动组件下。这不符合前端开发的习惯，所以在 nvue 编译为 uni-app模式时，给页面外层自动套了一个 `scroller`，页面内容过高会自动滚动。（组件不会套，页面有`recycle-list`时也不会套）。后续会提供配置，可以设置不自动套。
  17. 在 App.vue 中定义的全局js变量不会在 nvue 页面生效。`globalData`和`vuex`是生效的。
  18. App.vue 中定义的全局css，对nvue和vue页面同时生效。如果全局css中有些css在nvue下不支持，编译时控制台会报警，建议把这些不支持的css包裹在[条件编译](https://uniapp.dcloud.io/platform)里，`APP-PLUS-NVUE`
  19. 不能在 `style` 中引入字体文件，nvue 中字体图标的使用参考：[加载自定义字体](https://uniapp.dcloud.io/nvue-api?id=addrule)。如果是本地字体，可以用`plus.io`的API转换路径。
  20. 目前不支持在 nvue 页面使用 `typescript/ts`。
  21. nvue 页面关闭原生导航栏时，想要模拟状态栏，可以[参考文章](https://ask.dcloud.net.cn/article/35111)。但是，仍然强烈建议在nvue页面使用原生导航栏。nvue的渲染速度再快，也没有原生导航栏快。原生排版引擎解析`json`绘制原生导航栏耗时很少，而解析nvue的js绘制整个页面的耗时要大的多，尤其在新页面进入动画期间，对于复杂页面，没有原生导航栏会在动画期间产生整个屏幕的白屏或闪屏。



## API

[API](https://uniapp.dcloud.io/api/README)

- 接口能力（JS API）靠近微信小程序规范，但需将前缀 `wx` 替换为 `uni`，详见[uni-app接口规范](https://uniapp.dcloud.io/api/README)

### 网络请求

uni-app封装了发起网络请求以及拦截器的API ：

**发起请求：**  **`uni.request(OBJECT)`**   [详情](https://uniapp.dcloud.io/api/request/request?id=request)

**拦截器：** `uni.addInterceptor(STRING, OBJECT)`



### Flyio

[Fly.js](https://github.com/wendux/fly) 一个基于Promise的、强大的、支持多种JavaScript运行时的http请求库

它在浏览器、微信小程序、Weex、Node、React Native、快应用中都能正常运行

特点：

1. 提供统一的 Promise API。
2. 浏览器环境下，**轻量且非常轻量**。
3. 支持多种JavaScript 运行环境
4. 支持请求／响应拦截器。
5. 自动转换 JSON 数据。
6. **支持切换底层 Http Engine，可轻松适配各种运行环境**。
7. **浏览器端支持全局Ajax拦截 。**
8. **H5页面内嵌到原生 APP 中时，支持将 http 请求转发到 Native。支持直接请求图片**。

#### 引入fly

```javascript
let Fly
//微信小程序
//#ifdef MP-WEIXIN
Fly = require('flyio/dist/npm/wx')
//#endif

//APP版本
//#ifdef APP-PLUS
Fly = require("flyio/dist/npm/weex")
//#endif

//支付宝小程序
//#ifdef MP-ALIPAY
Fly = require('flyio/dist/npm/ap')
//#endif

// H5版本
// #ifdef H5
Fly = require('flyio/dist/npm/fly')
//#endif

let fly = new Fly()
```

#### 在uni-app中使用

在[uni-app](https://uniapp.dcloud.io/) 中您也可以将fly实例挂在vue原型上，这样就可以在任何组件中通过`this`方便的调用：

```javascript
var Fly=require("flyio/dist/npm/wx") 
var fly=new Fly
... //添加全局配置、拦截器等
Vue.prototype.$request=fly //将fly实例挂在vue原型上
```

在组件中您可以方便的使用：

```javascript
this.$request.get("/test",{xx:6}).then((d)=>{
      //输出请求数据
      console.log(d.data)
      //输出响应头
      console.log(d.header)
    }).catch(err=>{
      console.log(err.status,err.message)
    })
```

#### 拦截器封装

Fly支持请求／响应拦截器，可以通过它在请求发起之前和收到响应数据之后做一些预处理

```javascript
//添加请求拦截器
fly.interceptors.request.use((request)=>{
    //给所有请求添加自定义header
    request.headers["X-Tag"]="flyio";
  	//打印出请求体
  	console.log(request.body)
  	//终止请求
  	//var err=new Error("xxx")
  	//err.request=request
  	//return Promise.reject(new Error(""))
  
    //可以显式返回request, 也可以不返回，没有返回值时拦截器中默认返回request
    return request;
})
 
//添加响应拦截器，响应拦截器会在then/catch处理之前执行
fly.interceptors.response.use(
    (response) => {
        //只将请求结果的data字段返回
        return response.data
    },
    (err) => {
        //发生网络错误后会走到这里
        //return Promise.resolve("ssss")
    }
)
```



参考：https://www.npmjs.com/package/flyio

------



# APP端开发注意事项

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

  

## Weex 页面的渲染

参考：[详解 Weex 页面的渲染过程](https://segmentfault.com/a/1190000010415641)



![图片描述](https://segmentfault.com/img/bVRRJu?w=1608&h=1608)



图中画出了 Weex SDK 的部分内容。其中 `weex-vue-framework` 和 `Vue.js` 是对等的，语法和内部机制都是一样的，只不过 `Vue.js` 最终创建的是 DOM 元素，而 `weex-vue-framework` 则是向原生端发送渲染指令，最终渲染生成的是原生组件。Weex Runtime 用来对接上层前端框架（如 Vue.js 和 Rax）并且负责和原生端之间的通信。Render Engine 就是针对各个端开发的原生渲染器，包含了 Weex 内置组件和模块的实现，可扩展。



### Patch



![图片描述](https://segmentfault.com/img/bVRRJB?w=2445&h=1240)

在 Vue.js 内部，Web 平台和 Weex 平台中的 `patch` 方法是不同的，但是都是由 `createPatchFunction` 这个方法生成的，它支持传递 `nodeOps` 参数，在其中代理了所有 DOM 操作。在 Web 平台中 `nodeOps` 背后调用的都是 Web API，在 Weex 平台中则调用的是 Weex Runtime 提供的 [Native DOM API](http://weex.apache.org/cn/references/native-dom-api.html)。触发 DOM 渲染的入口一致，但是不同平台的实现方式不同。



![图片描述](https://segmentfault.com/img/bVRRJG?w=2657&h=1559)

上层前端框架调用了 Weex 平台提供的 Native DOM API 之后，Weex Runtime 会构建一个用于渲染的节点树，并将操作转换成渲染指令发送给客户端



目前来说渲染指令是基于 JSON 描述的，具体格式大致如下所示：

```
{
  module: 'dom',
  method: 'createBody',
  args: [{
    ref: '_root',
    type: 'div',
    style: { justifyContent: 'center' }
  }]
}
```





原生渲染器接收上层传来的渲染指令，并且逐步将其渲染成原生组件。

![图片描述](https://segmentfault.com/img/bVRRJ9?w=2162&h=1620)



- https://zh.wikipedia.org/zh-hans/%E7%BA%BF%E7%A8%8B%E5%AE%89%E5%85%A8)
