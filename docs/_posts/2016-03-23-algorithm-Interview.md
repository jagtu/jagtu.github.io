---
layout: post
title: 常用算法面试题
categories: interview
description: 真傻还是假聪明，这是个问题。
keywords: 算法, 题目
---

[toc]



# 冒泡排序

<details>
<summary>查看答案</summary>
[冒泡排序](/2015/06/01/bubble-sort.md)
</details>



# 选择排序

<details>
<summary>查看答案</summary>
[选择排序](八大排序算法/选择排序.md)
</details>

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

<details>
<summary>查看答案</summary>

[希尔排序图解](八大排序算法/希尔排序.md)
</details>

# 选择堆排序
<details>
<summary>查看答案</summary>

[选择堆排序](八大排序算法/选择堆排序.md)
</details>

# 归并排序
<details>
<summary>查看答案</summary>

[归并排序](八大排序算法/归并排序.md)
</details>

# 基数排序
<details>
<summary>查看答案</summary>

[基数排序](八大排序算法/桶排序.md)
</details>

# 给一个整型数组和一个目标值，判断数组中是否有两个数字之和等于目标值

<details>
<summary>查看答案</summary>


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

</details>

# 给定一个整型数组中有且仅有两个数字之和等于目标值，求两个数字在数组中的序号

<details>
<summary>查看答案</summary>

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

</details>

# 给一个链表和一个值 x，要求只保留链表中所有小于 x 的值，原链表的节点顺序不能变

<details>
<summary>查看答案</summary>

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
</details>

# 求二叉树的最大深度

<details>
<summary>查看答案</summary>

```swift
// 计算树的最大深度
func maxDepth(root: TreeNode?) -> Int {
  guard let root = root else {
    return 0
  }
  return max(maxDepth(root.left), maxDepth(root.right)) + 1
}
```

</details>

# 如果判断一个二叉树是二叉搜索树

<details>
<summary>查看答案</summary>

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

</details>

