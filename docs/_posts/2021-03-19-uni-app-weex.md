---
layout: post
title: 利用 XXL-JOB 实现灵活控制的分片处理
categories: Java
description: 通过 XXL-JOB 的分片广播机制，实现灵活控制的并发数控制。
keywords: XXL-JOB, Java, 分片广播
---

本文讲述uni-app框架关于week页面的渲染原理。

###  Weex 页面的渲染



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



## 参考

- [详解 Weex 页面的渲染过程][1]

[1]: https://segmentfault.com/a/1190000010415641
