---
layout: post
title: 选择排序
categories: algorithm
description:选择排序算法
keywords:排序算法, 选择排序, Selection Sort
---



- 中文名：**选择排序**（Selection Sort）

- 时间复杂度：**O(n^2)
- 稳定性：**不稳定**

选择排序是第一次从第一个和最后一个找出最小值和第一个交换，第二次从第二个到最后一个找出最小值和第二个交换，直到交换完毕。

## 选择排序的图解

![image-20200325151628128](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325151628128-20200325154219267.png)

### 第一次循环(默认最小值为8)

1. 3和8对比，3小于，此时最小值为3

   ![image-20200325154731873](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325154731873.png)

   

2. 12个3对比，12大于3，此时最小值为3

   ![image-20200325154835942](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325154835942.png)

3. 45和3对比，45大于3，此时最小值为3

   ![image-20200325154916788](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325154916788.png)

4. 1和3对比，1小于3，此时最小值为1

   ![image-20200325155005041](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325155005041.png)

5. 此时我们把8和1的位置互换

   ![image-20200325155114725](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325155114725.png)

6. 排序之后

   ![image-20200325155147676](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325155147676.png)

---

## 第二次循环(默认值3)

1. 12个3对比大于3，则当前最小值为3

   ![image-20200325155349567](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325155349567.png)

2. 45和3对比，大于3，则此时最小值为3

   ![image-20200325155433518](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325155433518.png)

3. 8和3对比，大于3，此时最小值为3

   ![image-20200325155521219](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325155521219.png)

4. 此时查出最小值为3，这位置不变

   ![image-20200325155147676](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325155147676.png)

---

### 第三次循环(最小值默认为12)

1. 45和12对比，大于12，此时的最小值为12

   ![image-20200325155722325](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325155722325.png)

2. 8和12对比，小于12，此时最小值为8

   ![image-20200325155812268](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325155812268.png)

3. 查出最小值为8，我们把12和8对调位置

   ![image-20200325155907043](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325155907043.png)

4. 排序之后

   ![image-20200325155942222](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325155942222.png)

---

### 第四次循环(最小值默认为45)

1. 12和45对比小于45，当前最小值为12

   ![image-20200325161908552](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325161908552.png)

2. 12和45进行交换

   ![image-20200325161951981](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325161951981.png)

   
---

此时已经排序完成

## 示例代码

```swift
class Solution {
    /// 选择排序
    func selectSort(_ numbers:inout [Int]) {
        guard numbers.count > 1 else {
            return
        }
        for i in 0 ..< (numbers.count - 1) {
            var min = numbers[i]
            var minIndex = i
            var j = i + 1
            while j < numbers.count {
                let n1 = numbers[j]
                if n1 < min {
                    min = n1
                    minIndex = j
                }
                j += 1
            }
            numbers[minIndex] = numbers[i]
            numbers[i] = min
        }
    }
}
```







