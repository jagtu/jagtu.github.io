---
layout: post
title: 归并排序
categories: 算法
description:算法归并排序了解
keywords:排序算法, 冒泡排序, Bubble Sort
---

- 中文名：**冒泡排序**（Bubble Sort）

- 时间复杂度：**O(n²)**
- 空间复杂度：**O(1)**
- 稳定性：**稳定**

冒泡排序对比两个元素的结果，之后根据大小交换位置的一种排序方法。

## 冒泡排序的图解

![image-20200326111633108](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326111633108.png)

### 第一次冒泡排序

8和3对比，8大于3交换位置

![image-20200326111729817](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326111729817.png)

![image-20200326111754527](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326111754527.png)

8和6对比，8大于6，交换位置

![image-20200326111855780](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326111855780.png)

![image-20200326111919801](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326111919801.png)

8和12对比，小于12，则位置不变

![image-20200326111952714](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326111952714.png)

![image-20200326111919801](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326111919801.png)

12和1对比，大于1，交换位置

![image-20200326112047807](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326112047807.png)

![image-20200326112111462](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326112111462.png)

第二次冒泡排序

3和6对比，小于6，位置不变

![image-20200326112153845](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326112153845.png)

![image-20200326112111462](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326112111462.png)

6和8对比，小于8，则位置不变

![image-20200326112249577](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326112249577.png)

![image-20200326112111462](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326112111462-20200326112258433.png)

8和1对比，大于1，则交换位置

![image-20200326112331744](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326112331744.png)

![image-20200326112358661](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326112358661.png)

第三次冒泡排序

3和6对比，小于6，位置不变

![image-20200326112450701](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326112450701.png)

![image-20200326112504459](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326112504459.png)

6和1对比，大于1，交换位置

![image-20200326112534010](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326112534010.png)

![image-20200326112556953](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326112556953.png)

第四次冒泡排序

3和1对比，大于1，交换位置

![image-20200326112633072](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326112633072.png)

![image-20200326112658086](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326112658086.png)

剩余一个元素排序完毕

## 示例代码

```swift
class Solution {
    func bubble(_ numbers:inout [Int]) {
        guard numbers.count > 1 else {
            return
        }
        var count = numbers.count
        while count > 1 {
            for i in 0 ..< (count - 1) {
                let j = i + 1
                let n1 = numbers[i]
                let n2 = numbers[j]
                if n2 < n1 {
                    numbers[i] = n2
                    numbers[j] = n1
                }
                print(numbers)
            }
            count -= 1
        }
    }
}
```

