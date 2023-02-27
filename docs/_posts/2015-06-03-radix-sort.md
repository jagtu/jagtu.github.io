---
layout: post
title: 基数排序
categories: algorithm
description:算法归并排序了解
keywords:排序算法, 基数排序, Radix Sort, bucket sort, distribution sort

---

- 中文名：**基数排序（Radix Sort）**

- 时间复杂度：**O (nlog(r)m)**，其中r为所采取的基数，而m为堆数
- 空间复杂度：**O(n+k)**，其中k为桶的数量
- 稳定性：**稳定**

基数排序是首先求出最大数的个数，然后从低位到高位分别放到对应0-9的桶中，按照先进先出的规则

## 基数排序的图解

![image-20200327111005882](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327111005882.png)

找到最大数是321，个数是3。

### 按照个位数进行桶排序

![image-20200327111052227](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327111052227.png)

![image-20200327111126333](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327111126333.png)

### 按照十位数进行桶排序

![image-20200327111654402](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327111654402.png)

![image-20200327111719109](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327111719109.png)

按照百分位进行桶排序

![image-20200327111836251](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327111836251.png)

![image-20200327111917343](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327111917343.png)

此时已经排序完成

## 示例代码

```swift
class Solution {
    func baseSort(_ numbers:[Int]) -> [Int] {
        guard numbers.count > 1 else {
            return numbers
        }
        var maxValue:Int = 0
        for i in 0 ..< numbers.count {
            maxValue = max(numbers[i], maxValue)
        }
        let maxLenght = "\(maxValue)".count
        var list:[Int] = numbers
        for i in 0 ..< maxLenght {
            list = bucketSort(list, i)
        }
        return list
    }
    func bucketSort(_ numbers:[Int], _ base:Int) -> [Int] {
        var buckets:[[Int]] = Array(repeating: [], count: 9)
        for i in 0 ..< numbers.count {
            let n = "\(numbers[i])"
            if n.count > base, let baseValue = Int(String(n[n.index(n.endIndex, offsetBy: (-1 - base))])) {
                var list = buckets[baseValue]
                list.append(numbers[i])
                buckets[baseValue] = list
            } else {
                var list = buckets[0]
                list.append(numbers[i])
                buckets[0] = list
            }
        }
        var list:[Int] = []
        for i in 0 ..< buckets.count {
            list += buckets[i]
        }
        return list
    }
}
```

