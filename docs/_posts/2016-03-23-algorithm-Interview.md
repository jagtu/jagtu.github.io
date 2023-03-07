---
layout: post
title: 常用算法面试题
categories: interview
description: 真傻还是假聪明，这是个问题。
keywords: 算法, interview
---



[TOC]



# 冒泡排序

- 中文名：**冒泡排序**（Bubble Sort）
- 时间复杂度：**O(n²)**
- 空间复杂度：**O(1)**
- 稳定性：**稳定**

冒泡排序对比两个元素的结果，之后根据大小交换位置的一种排序方法。

### 冒泡排序的图解

![image-20200326111633108](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326111633108.png)

**第一次冒泡排序**

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

**第二次冒泡排序**

3和6对比，小于6，位置不变

![image-20200326112153845](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326112153845.png)

![image-20200326112111462](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326112111462.png)

6和8对比，小于8，则位置不变

![image-20200326112249577](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326112249577.png)

![image-20200326112111462](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326112111462-20200326112258433.png)

8和1对比，大于1，则交换位置

![image-20200326112331744](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326112331744.png)

![image-20200326112358661](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326112358661.png)

**第三次冒泡排序**

3和6对比，小于6，位置不变

![image-20200326112450701](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326112450701.png)

![image-20200326112504459](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326112504459.png)

6和1对比，大于1，交换位置

![image-20200326112534010](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326112534010.png)

![image-20200326112556953](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326112556953.png)

**第四次冒泡排序**

3和1对比，大于1，交换位置

![image-20200326112633072](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326112633072.png)

![image-20200326112658086](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326112658086.png)

剩余一个元素排序完毕

### 示例代码

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




# 选择排序

- 中文名：**选择排序**（Selection Sort）
- 时间复杂度：**O(n^2)
- 稳定性：**不稳定**

选择排序是第一次从第一个和最后一个找出最小值和第一个交换，第二次从第二个到最后一个找出最小值和第二个交换，直到交换完毕。



### 选择排序的图解

![image-20200325151628128](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325151628128-20200325154219267.png)

**第一次循环(默认最小值为8)**

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

**第二次循环(默认值3)**

1. 12个3对比大于3，则当前最小值为3

   ![image-20200325155349567](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325155349567.png)

2. 45和3对比，大于3，则此时最小值为3

   ![image-20200325155433518](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325155433518.png)

3. 8和3对比，大于3，此时最小值为3

   ![image-20200325155521219](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325155521219.png)

4. 此时查出最小值为3，这位置不变

   ![image-20200325155147676](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325155147676.png)

---

**第三次循环(最小值默认为12)**

1. 45和12对比，大于12，此时的最小值为12

   ![image-20200325155722325](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325155722325.png)

2. 8和12对比，小于12，此时最小值为8

   ![image-20200325155812268](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325155812268.png)

3. 查出最小值为8，我们把12和8对调位置

   ![image-20200325155907043](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325155907043.png)

4. 排序之后

   ![image-20200325155942222](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325155942222.png)

---

**第四次循环(最小值默认为45)**

1. 12和45对比小于45，当前最小值为12

   ![image-20200325161908552](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325161908552.png)

2. 12和45进行交换

   ![image-20200325161951981](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325161951981.png)

   
---

此时已经排序完成

### 示例代码

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


# 快速排序算法

- 中文名：**快速排序**（Quick Sort）

- 时间复杂度：**O(nlog2n)**
- 空间复杂度：**O(log2n)**
- 稳定性：**不稳定**

快速排序是通过一个值，一个指针从左到右找出大于基数的值，一个指针从右到左找出小于基数的值，之后交换位置。当两个指针到大同一个位置，交换基数和查找中间位置。

### 快速排序图解

![image-20200326113843953](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326113843953.png)

**第一次快排，8作为中间值**

左侧大于8第一个是12 右侧小于8是1 8和1交换位置

![image-20200326144153919](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326144153919.png)

![image-20200326144219036](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326144219036.png)

指针继续查找则已经错过，将基数和刚才查找最后最小值交换，8和1互换

![image-20200326144416846](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326144416846.png)

![image-20200326144503851](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326144503851.png)

**第二次快排前面1 3 6基数为1**

大于1第一个数是3小于1 小于1没有，则保持不变

![image-20200326144908170](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326144908170.png)

**第三次快排基数8后面的12**

因为只有一个元素，则不需要排序

![image-20200326144956278](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326144956278.png)

**第四次排序基数1右边大集合3和6 基数为3**

查找大于3第一个数是6，查找小于3没有，则保持不变

![image-20200326145151960](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326145151960.png)

**第四次排序基数3右边大集合6 基数为6**

因为只有一个元素，则保持不变

![image-20200326145236416](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326145236416.png)

到底排序完成

### 示例代码

```swift
class Solution {
    //快速排序入口
    func quickSort(_ numbers:inout [Int]) {
        quickSubSort(&numbers, 0, (numbers.count - 1))
    }
    //一趟快速排序
    func quickSubSort(_ numbers:inout [Int], _ start:Int, _ end:Int) {
        let lenght = end - start + 1
        guard lenght > 1 else {
            return
        }
        var leftIndex:Int = start + 1
        var rightIndex:Int = end
        var numberIndex = start
        let number = numbers[numberIndex]
        while true {
            guard leftIndex <= end, rightIndex >= start else {
                break
            }
            var minIndex:Int?
            var maxIndex:Int?
            for i in ((start + 1) ... rightIndex).reversed() {
                if numbers[i] < number {
                    minIndex = i
                    break
                }
            }
            guard let _minIndex = minIndex else {
                break
            }
            for j in (leftIndex ... end) {
                if numbers[j] >= number {
                    maxIndex = j
                    break
                }
            }
            if let _maxIndex = maxIndex, _maxIndex < _minIndex {
                self.swap(&numbers, _minIndex, _maxIndex)
            } else {
                self.swap(&numbers, start, _minIndex)
                numberIndex = _minIndex
                break
            }
            leftIndex += 1
            rightIndex -= 1
        }
        quickSubSort(&numbers, start, numberIndex - 1)
        quickSubSort(&numbers, (numberIndex + 1), end)
    }
    func swap(_ numbers:inout [Int], _ left:Int, _ right:Int) {
        guard numbers.count > 2, left != right, numbers.count > left, numbers.count > right else {
            return
        }
        let temp = numbers[left]
        numbers[left] = numbers[right]
        numbers[right] = temp
    }
}
```






# 插入排序
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



### **代码实现 ** 

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

   

**时间复杂度**

在插入排序中，当待排序数组是有序时，是最优的情况，只需当前数跟前一个数比较一下就可以了，这时一共需要比较N- 1次，时间复杂度为

![img](https://bkimg.cdn.bcebos.com/formula/01c1755ab0dfc73295291de2245131af.svg)

最坏的情况是待排序数组是逆序的，此时需要比较次数最多，总次数记为：1+2+3+…+N-1，所以，插入排序最坏情况下的时间复杂度为

![img](https://bkimg.cdn.bcebos.com/formula/f990c5e20c6033ea5c9c4a9be4ff8951.svg)

平均来说，A[1..j-1]中的一半元素小于A[j]，一半元素大于A[j]。插入排序在平均情况运行时间与最坏情况运行时间一样，是输入规模的二次函数 [1] 。

**空间复杂度**

插入排序的空间复杂度为常数阶

![img](https://bkimg.cdn.bcebos.com/formula/59f30c9e66f5f15028bde1e8e08c8bd8.svg)



# 希尔排序

- 中文名：**希尔排序**（Shell's Sort）
- 别  名：直接插入排序

- 分  类：排序方法
- 时间复杂度：**O(![img](https://bkimg.cdn.bcebos.com/formula/eb39d4c67a9cabbd3a2690a2151ee6cc.svg))**
- 空间复杂度：**O(1)**
- 稳定性：**不稳定**

希尔排序是插入排序的变种，是通过根据一个gap值分割数组为gap个区间，分别对区间进行插入排序。之后缩小gap值，再分割多个区间进行插入排序，当gap=1时候再整体进行插入排序。

### 希尔排序的图解(设置gap为数组一半)

![image-20200325151628128](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325151628128.png)

**当gap为5/2=2时候**

![image-20200325151818203](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325151818203.png)

数组分为了分为了两组元素，一组为[8,12,1]一组为[3,45]

**对于[8,12,1]进行快排**

1. 12个8对比，大于8，则不交换位置

   ![image-20200325152121579](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325152121579.png)

2. 插入之后

   ![image-20200325151628128](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325151628128.png)

3. 1和8对比，小于8，交换位置

   ![image-20200325152309958](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325152309958.png)

4. 插入之后

   ![image-20200325152353556](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325152353556.png)

5. 8和12对比小于12，交换位置

   ![image-20200325152456441](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325152456441.png)

6. 插入之后

   ![image-20200325152606814](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325152606814.png)

**对[3,45]元素进行快排**

1. 45和3对比，大于3，则不需要交换位置

   ![image-20200325152743746](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325152743746.png)

2. 插入之后

   ![image-20200325152606814](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325152606814.png)



**当gap = 5/2 - 1 = 1时候希尔排序结束了。**

当前的数组虽然没有排序完成，但是整体已经从小到大排序了，我们最后将整体的进行插入排序。

**整体的插入排序**

> 插入排序的图解请参考[插入排序]([八大排序算法](八大排序算法)/插入排序.md)

排序之后

![image-20200325153239755](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200325153239755.png)

### 示例代码

```swift
class Solution {
    /// 希尔排序
    func shellSort(_ numbers:inout [Int]) {
        guard numbers.count > 1 else {
            return
        }
        var gap:Int = numbers.count / 2
        while gap > 1 {
            for i in gap ..< numbers.count {
                var j = i % gap
                while j < i {
                    let n1 = numbers[i]
                    let n2 = numbers[j]
                    if n1 < n2 {
                        numbers[j] = n1
                        numbers[i] = n2
                    }
                    print(numbers)
                    j += gap
                }
            }
            gap -= 1
        }
        insertSort(&numbers)
    }
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
}
```





# 选择堆排序

---

- 中文名：**堆排序**（Heapsort Sort）

- 时间复杂度：**O(n)+O(nlgn) ~ O(nlgn)**
- 空间复杂度：**O(1)**
- 稳定性：**不稳定**

堆排序是选择排序的一种变种，大顶堆是节点大于或者等于左右节点的完全二叉树，小顶堆是节点小于或者等于左右节点的完全二叉树。升序排序用大顶堆，降序用小顶堆。

### 堆排序图解

![image-20200326085732775](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326085732775.png)

此时的二叉树对应的图为

![image-20200326090707661](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326090707661.png)

一个二叉树对应的父节点可以通过公式`(lenght/2 - 1) ~ 0`,我们一共有`6`个元素，对应的父节点索引就是`2~0`也就是`12` `8` `3`,便利节点是从下到上，从右到左的原则。

**第一次构建大顶堆**

1. 便利节点12，对应的左节点是6，没有右节点，因为12比6大，所以不需要进行互换操作

   ![image-20200326091405329](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326091405329.png)

2. 便利节点8，左侧节点是45，右侧节点是1，最大节点是45，45比8大，需要交换位置。

   ![image-20200326091547613](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326091547613.png)

   ![image-20200326091629136](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326091629136.png)

3. 便利节点3，左侧节点45，右侧节点12。最大节点45，45比3大，所以45和3交换。

   ![image-20200326091759669](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326091759669.png)

   ![image-20200326091840187](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326091840187.png)

4. 查询节点12，只有左侧节点6，小于12，则不交换位置

   ![image-20200326093248334](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326093248334.png)

5. 查询节点3，左侧节点8，右侧节点1，最大节点8，则8大于3交换位置

   ![image-20200326093341850](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326093341850.png)

   ![image-20200326094025783](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326094025783.png)

6. 查询节点12，左侧节点6，不需要交换

   ![image-20200326094200618](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326094200618.png)

7. 到此第一次大顶堆排序完成

   ![image-20200326094314900](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326094314900.png)

8. 我们将收尾进行互换

   ![image-20200326095347775](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326095347775.png)

   ![image-20200326095416523](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326095416523.png)
   
   **对剩余的五个元素进行大顶堆排序**

![image-20200326095546825](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326095546825.png)

我们通过上面进行大顶堆排序之后

![image-20200326100132784](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326100132784.png)

**第二次大顶堆排序完成**

![image-20200326100229588](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326100229588.png)

将首尾进行互换

![image-20200326100324560](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326100324560.png)

![image-20200326100358741](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326100358741.png)

**将剩余的四个元素进行大顶堆排序**

![image-20200326100443589](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326100443589.png)

进行大顶堆排序完毕

![image-20200326110003342](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326110003342.png)

![image-20200326110031314](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326110031314.png)

将首尾进行位置的互换

![image-20200326110058850](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326110058850.png)

![image-20200326110152825](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326110152825.png)

**将剩下的元素进行大顶堆排序**

![image-20200326110216266](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326110216266.png)

大顶堆排序之后

![image-20200326110236640](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326110236640.png)

![image-20200326110257631](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326110257631.png)

   将首尾互换

![image-20200326110315484](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326110315484.png)

![image-20200326110349948](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326110349948.png)

**将剩余的两个元素进行大顶堆排序**

![image-20200326110406023](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326110406023.png)

大顶堆排序完毕之后

![image-20200326101207774](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326101207774.png)

![image-20200326101226598](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326101226598.png)

首尾互换

![image-20200326101253316](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326101253316.png)

![image-20200326101315692](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200326101315692.png)

剩余一个元素不需要再次排序，则排序完毕。

### 示例代码

```swift
class Solution {
    func heapSort(_ numbers:inout [Int], _ lenght:Int = 0) {
        let _lenght = lenght > 0 ? lenght : numbers.count
        guard _lenght > 1 else {
            return
        }
        let count = _lenght / 2 - 1
        var index = 0
        while count >= index {
            for i in (index ... count).reversed() {
                let leftIndex = 2 * i + 1
                let leftN = numbers[leftIndex]
                let nodeN = numbers[i]
                var swapIndex = i
                if leftN > nodeN {
                    swapIndex = leftIndex
                }
                let rightIndex = 2 * i + 2
                if _lenght > rightIndex {
                    let rightN = numbers[rightIndex]
                    if rightN > leftN {
                        swapIndex = rightIndex
                    }
                }
                if swapIndex != i {
                    self.swap(&numbers, i, right: swapIndex)
                }
            }
            index += 1
        }
        swap(&numbers, 0, right: (_lenght - 1))
        heapSort(&numbers, _lenght - 1)
    }
    func swap(_ numbers:inout [Int], _ left:Int, right:Int) {
        guard numbers.count > left, numbers.count > right else {
            return
        }
        let temp = numbers[left]
        numbers[left] = numbers[right]
        numbers[right] = temp
    }
}
```



# 归并排序

- 中文名：**归并排序**（Merge Sort）

- 时间复杂度：**O(n log n)**
- 空间复杂度：**T(n)**
- 稳定性：**稳定**

归并排序是将一组序列分解按照等分依次分解，最后成为只有一个元素的小数组，最后将数组合并之后排序成为整体的数组的一种排序方法。

### 归并排序图解

![image-20200327094824842](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327094824842.png)

**分解数组**

将数组【8 3 6 12 1】按照平均数划分

![image-20200327100428478](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327100428478.png)

将数组【8 3】和数组【6 12 1】进行平分

![image-20200327100549132](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327100549132.png)

将数组【12 1】进行平分

![image-20200327100639480](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327100639480.png)

**合并数组**

将数组【8】和数组【3】进行合并

取出第一个元素8和第一个元素3对比，将3放入到新数组

![image-20200327100840835](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327100840835.png)

数组【3】完毕，将第一个数组全部放在新数组中

![image-20200327100942384](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327100942384.png)

将数组【12】和数组【1】进行合并

取出第一个元素12和1对比，将1放到新数组中

![image-20200327101121299](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327101121299.png)

数组【1】完成，将第一个数组全部放在新数组中

![image-20200327101209833](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327101209833.png)

将数组【6】和数组【1 12】合并

取出第一个元素6和第一个元素1对比，将1放入新数组

![image-20200327101347613](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327101347613.png)

取出第一个元素6和第二个元素12对比，将6放入到新数组中

![image-20200327101444249](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327101444249.png)

数组【6】完毕，将第二个数组剩余全部放在新数组中

![image-20200327101553975](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327101553975.png)

将数组【3 8】和数组 【1 6 12】进行合并

取出第一个元素3和第一个元素1对比，将1加入到新数组中

![image-20200327101740642](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327101740642.png)

将第一个元素3和第二个元素6对比，将3加入到新数组中

![image-20200327101824296](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327101824296.png)

将第二个元素8和第二个元素6对比，将6添加到新数组中

![image-20200327101906271](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327101906271.png)

将第二个元素8和第三个元素12对比，将8加入到新数组

![image-20200327102000773](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327102000773.png)

数组【3 8】完毕将第二个数组全部加入到新数组中

![image-20200327102036706](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327102036706.png)

到底归并排序完成

### 示例代码

```swift
class Solution {
    func mergeSort(_ numbers:[Int]) -> [Int] {
        guard numbers.count > 1 else {
            return numbers
        }
        guard let cuts = cutList(numbers) else {
            return numbers
        }
        return mergeList(cuts.0, cuts.1)
    }
    func cutList(_ numbers:[Int]) -> ([Int],[Int])? {
        guard numbers.count > 1 else {
            return nil
        }
        let cut:Int = numbers.count / 2
        let leftNumbers = mergeSort(Array(numbers[0 ..< cut]))
        let rightNumber = mergeSort(Array(numbers[cut...]))
        return (leftNumbers, rightNumber)
    }
    
    func mergeList(_ left:[Int], _ right:[Int]) -> [Int] {
        guard left.count > 0 else {
            return left
        }
        guard right.count > 0 else {
            return left
        }
        var list:[Int] = []
        var i = 0
        var j = 0
        while true {
            guard left.count > i || right.count > j else {
                break
            }
            if left.count <= i {
                list.append(right[j])
                j += 1
            } else if right.count <= j {
                list.append(left[i])
                i += 1
            } else {
                let n1 = left[i]
                let n2 = right[j]
                if n1 < n2 {
                    list.append(n1)
                    i += 1
                } else {
                    list.append(n2)
                    j += 1
                }
            }
        }
        return list
    }
}
```



# 基数排序

---

- 中文名：**基数排序（Radix Sort）**

- 时间复杂度：**O (nlog(r)m)**，其中r为所采取的基数，而m为堆数
- 空间复杂度：**O(n+k)**，其中k为桶的数量
- 稳定性：**稳定**

基数排序是首先求出最大数的个数，然后从低位到高位分别放到对应0-9的桶中，按照先进先出的规则

### 基数排序的图解

![image-20200327111005882](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327111005882.png)

找到最大数是321，个数是3。

**按照个位数进行桶排序**

![image-20200327111052227](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327111052227.png)

![image-20200327111126333](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327111126333.png)

**按照十位数进行桶排序**

![image-20200327111654402](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327111654402.png)

![image-20200327111719109](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327111719109.png)

**按照百分位进行桶排序**

![image-20200327111836251](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327111836251.png)

![image-20200327111917343](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200327111917343.png)

此时已经排序完成

### 示例代码

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


# 其他题目

### 给一个整型数组和一个目标值，判断数组中是否有两个数字之和等于目标值

```swift
func sum(_ nums:[Int], _ target:Int) -> Bool {
    var dic:[Int:Int] = [:]
    for num in nums {
        guard let _ = dic[num] else {
            dic[target - num] = num
            continue
        }
        return true
    }
    return false
}
```

 因为既然数组有两个数之后等于目标值，那么这两个值一定在数组里面。我们按照顺序，查询剩余的值是否存在即可。



### 给定一个整型数组中有且仅有两个数字之和等于目标值，求两个数字在数组中的序号


```swift
func sum(_ nums:[Int], _ target:Int) -> (Int,Int)? {
    var dic:[Int:Int] = [:]
    for item in nums.enumerated() {
        guard let index = dic[item.element] else {
            dic[target - item.element] = item.offset
            continue
        }
        return (index,item.offset)
    }
    return nil
}
```

### 给一个链表和一个值 x，要求只保留链表中所有小于 x 的值，原链表的节点顺序不能变


```swift
func getLeftList(_ node:ListNode?, _ x:Int) -> ListNode? {
    guard let head = node else {
        return node
    }
    if head.val >= x  {
        return getLeftList(head.next, x)
    } else {
        head.next = getLeftList(head.next, x)
        return head
    }
}
```
### 求二叉树的最大深度


```swift
// 计算树的最大深度
func maxDepth(root: TreeNode?) -> Int {
  guard let root = root else {
    return 0
  }
  return max(maxDepth(root.left), maxDepth(root.right)) + 1
}
```



### 如果判断一个二叉树是二叉搜索树


```swift
// 判断一颗二叉树是否为二叉查找树
func isValidBST(root: TreeNode?) -> Bool {
  return _helper(root, nil, nil)
}

private func _helper(node: TreeNode?, _ min: Int?, _ max: Int?) -> Bool {
  guard let node = node else {
    return true
  }
  // 所有右子节点都必须大于根节点
  if let min = min, node.val <= min {
    return false
  }
  // 所有左子节点都必须小于根节点
  if let max = max, node.val >= max {
    return false
  }

  return _helper(node.left, min, node.val) && _helper(node.right, node.val, max)
}
```

