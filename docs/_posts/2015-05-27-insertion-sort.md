---
layout: post
title: 插入排序
categories: interview
description:算法插入排序了解
keywords:排序算法, 插入排序, Insertion sort
---

- 中文名：**插入排序**（Insertion sort）
- 别  名：直接插入排序

- 分  类：排序方法
- 时间复杂度：**O(N^(1-2)) **，最差情况 O(![img](https://bkimg.cdn.bcebos.com/formula/c937d30f3cd06da1cd53133d8a3b4887.svg))
- 空间复杂度：**O(1)**
- 稳定性：稳定

插入排序是将需要排序的元素插入到已经排序好的数组里面，对于数组第一个元素是算作一个已经排序好的数组。所以我们只需要从第二个元素开始，和之前的元素对比插入即可。

### 插入排序的图解过程

> 为什么不用动图 因为看动图可能没看明白就切换了 我深有体会所以做的图解 
> 因为图解比较麻烦 所以就用了五个数作为例子[8,3,12,45,1]

![](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/ME0jgt.jpg)

---

**从第二个元素3开始进行排序**

  1. 3和8对比，小于8则交换位置

     ![image-20200325115837523](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325115837523-20200325121530502.png)

  2. 插入之后

     ![image-20200325120003806](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325120003806-20200325121546930.png)
     
---

**第三个元素12开始进行排序**

1. 12和3对比，大于3，则不交换位置

    ![image-20200325120737715](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325120737715.png)

2. 插入之后

   ![image-20200325120003806](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325120003806-20200325120755865-20200325121649210.png)
   
3. 12和8对比，大于8，则不交换位置

    ![image-20200325121247125](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325121247125-20200325122009852.png)

4. 插入之后

    ![image-20200325120003806](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325120003806-20200325120755865-20200325121649210-20200325122032254.png)

---

**第四个元素45开始进行排序**

1. 45和3对比，大于3，则不交换位置

   ![image-20200325122331398](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325122331398.png)

2. 插入之后

   ![image-20200325120003806](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325120003806-20200325120755865-20200325121649210-20200325122032254-20200325122345546.png)

3. 45和8对比，大于8，则不交换位置

   ![image-20200325122443098](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325122443098.png)

4. 插入之后

   ![image-20200325120003806](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325120003806-20200325120755865-20200325121649210-20200325122032254-20200325122345546.png)

5. 45和12对比，大于12，则不交换位置

   ![image-20200325122601143](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325122601143.png)

6. 插入之后

   ![image-20200325120003806](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325120003806-20200325120755865-20200325121649210-20200325122032254-20200325122345546-20200325122612265.png)

---

第五个元素1开始排序

1. 1和3对比，小于3，则交换位置

   ![image-20200325123142482](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325123142482.png)

2. 插入之后

3. 

   ![image-20200325122800931](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325122800931.png)

4. 3和8对比，小于8，则交换位置

   ![image-20200325122903343](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325122903343.png)

5. 插入之后

   ![image-20200325122938877](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325122938877.png)

6. 8和12对比小于12，则交换位置

   ![image-20200325123028564](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325123028564.png)

7. 插入之后

   ![image-20200325140736107](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325140736107.png)

8. 12和45对比小于45 交换位置

   ![image-20200325140904544](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325140904544.png)

9. 插入之后

   ![image-20200325141107008](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325141107008.png)

到此插入排序已经排序完成

## 代码实现

```swift
/// 插入排序
func insertSort(_ numbers:inout [Int]) {
  guard numbers.count > 1 else {
    return
  }
  for i in 1 ..< numbers.count {
    for j in 0 ..< i {
      let n1 = numbers[i]
      let n2 = numbers[j]
      if n1 < n2 {
        numbers[j] = n1
        numbers[i] = n2
      }
    }
  }
}
```

   

### 时间复杂度

在插入排序中，当待排序数组是有序时，是最优的情况，只需当前数跟前一个数比较一下就可以了，这时一共需要比较N- 1次，时间复杂度为

![img](https://bkimg.cdn.bcebos.com/formula/01c1755ab0dfc73295291de2245131af.svg)

最坏的情况是待排序数组是逆序的，此时需要比较次数最多，总次数记为：1+2+3+…+N-1，所以，插入排序最坏情况下的时间复杂度为

![img](https://bkimg.cdn.bcebos.com/formula/f990c5e20c6033ea5c9c4a9be4ff8951.svg)

 

平均来说，A[1..j-1]中的一半元素小于A[j]，一半元素大于A[j]。插入排序在平均情况运行时间与最坏情况运行时间一样，是输入规模的二次函数 [1] 。



### 空间复杂度

插入排序的空间复杂度为常数阶

![img](https://bkimg.cdn.bcebos.com/formula/59f30c9e66f5f15028bde1e8e08c8bd8.svg)

