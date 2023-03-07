---
layout: post
title: 常用算法面试题
categories: interview
description: 真傻还是假聪明，这是个问题。
keywords: 算法, 题目

---



# 冒泡排序

<details>
<summary>查看答案</summary>
[冒泡排序](八大排序算法/冒泡排序.md)
</details>

# 选择排序
<details>
<summary>查看答案</summary>

[选择排序](八大排序算法/选择排序.md)
</details>

# 快速排序算法
<details>
<summary>查看答案</summary>

[快速排序](八大排序算法/快速排序.md)
</details>


# 插入排序
<details>
<summary>查看答案</summary>
[插入排序图解](八大排序算法/插入排序.md)
</details>

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

