- Introducing SwiftUI

  SwiftUI是一种为任何苹果平台声明用户界面的现代方式。比以往更快地创建漂亮、动态的应用程序。

  这部分知识内容分为以下4部分：

  1. [SwiftUI Essentials](https://developer.apple.com/tutorials/SwiftUI#swiftui-essentials)
  2. [Drawing and Animation](https://developer.apple.com/tutorials/SwiftUI#drawing-and-animation)
  3. [App Design and Layout](https://developer.apple.com/tutorials/SwiftUI#app-design-and-layout)
  4. [Framework Integration](https://developer.apple.com/tutorials/SwiftUI#framework-integration)
  5. [Resources](https://developer.apple.com/tutorials/SwiftUI#resources)

  

  The `environmentObject(_:)` modifier：在视图层次结构中向下传递数据，其视图层次结构中更靠下的视图可以读取通过环境向下传递的数据对象

  `@EnvironmentObject` attribute：在视图层次结构中较低的视图中使用此属性，以从较高的视图接收数据

  

  #### 注意要点

  * 绑定：使用`$` 绑定控制值的存储，因此您可以将数据传递到需要读取或写入它的不同视图

  * 状态管理：在声明的开头使用 `@State `属性，表示将值作为状态来管理，并注意将属性声明为私有，并为其赋予默认值

    e .g： `@State private var showFavoritesOnly = false`

  
