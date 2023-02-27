---
layout: post
title: 单项链表反转
categories: CPlusPlus
description: 单项链表反转
keywords: 链表
---

如何将一个单项链表进行原地反转

## 循环图解

![image-20200327121737159](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327121737159.png)

![image-20200327144818277](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327144818277.png)

声明指针`Pre`指向头节点

![image-20200327144945196](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327144945196.png)

声明指针`Cur`指向头节点的下一个节点

![image-20200327145022324](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327145022324.png)

声明指针`Temp`指向头部下一个节点的下一个节点

![image-20200327145307413](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327145307413.png)

### 循环

#### 第一次循环

指针`Temp`指向`Cur`的下一个节点

![image-20200327145059042](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327145059042.png)

指针`Cur`的下一个节点指向指针`Pre`

![image-20200327145407230](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327145407230.png)

指针`Pre`指向`Cur`

![image-20200327145431026](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327145431026.png)

指针`Cur`指向`Temp`

![image-20200327145506438](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327145506438.png)

#### 第二次循环

指针`Temp`指向`Cur`的下一个节点

![image-20200327145640160](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327145640160.png)

指针`Cur`的下一个节点指向`Pre`

![image-20200327145743785](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327145743785.png)

指针`Pre`指向指针`Cur`

![image-20200327145818001](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327145818001.png)

指针`Cur`指向指针`Temp`

![image-20200327145900473](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327145900473.png)

#### 第三次循环

指针`Temp`指向指针`Cur`的下一个节点

![image-20200327150247646](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327150247646.png)

指针`Cur`的下一个节点指向`Pre`

![image-20200327150136690](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327150136690.png)

指针`Pre`指向指针`Cur`

![image-20200327150206861](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327150206861.png)

指针`Cur`指向指针`Temp`

![image-20200327150309892](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327150309892.png)

#### 第四次循环

指针`Temp`指向`Cur`下一个指针

![image-20200327150353629](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327150353629.png)

指针`Cur`的下一个指针指向指针`Pre`

![image-20200327150553983](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327150553983.png)

指针`Pre`指向指针`Cur`

![image-20200327150656405](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327150656405.png)

指针`Cur`指向指针`Temp`空指针，退出。

设置`Head`下一个指针为空指针

![image-20200327150805145](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327150805145.png)

到此位置单链表反转完毕

### 示例代码

```swift
func reverseNode(_ node:Node?) -> Node? {
    guard let head = node, let next = node?.next else {
        return node
    }
    var pre:Node? = head
    var cur:Node? = next
    var temp:Node? = next.next
    while cur != nil {
        temp = cur?.next
        cur?.next = pre
        pre = cur
        cur = temp
    }
    head.next = nil
    return pre
}
```

```swift
func reverseNode(_ node:Node?) -> Node? {
    guard let head = node, let next = node?.next else {
        return node
    }
    let newNode = reverseNode(next)
    head.next?.next = head
    head.next = nil
    return newNode
}
```
