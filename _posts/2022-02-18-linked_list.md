---
layout:     post   				    # 使用的布局（不需要改）
title:      leetcode linked list 				# 标题 
subtitle:    #副标题
date:       2022-02-18 				# 时间
author:     FishRedLeaf 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - leetcode
---

[链表知识点题库 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/tag/linked-list/problemset/)

```python
# Definition for singly-linked list.
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next
```



# 2.两数相加

[https://leetcode-cn.com/problems/add-two-numbers](https://leetcode-cn.com/problems/add-two-numbers) 
## 原题
给你两个  **非空** 的链表，表示两个非负的整数。它们每位数字都是按照  **逆序**  的方式存储的，并且每个节点只能存储  **一位**  数字。

请你将两个数相加，并以相同形式返回一个表示和的链表。

你可以假设除了数字 0 之外，这两个数都不会以 0 开头。

 

 **示例 1：** 
<img alt="" src="https://tva1.sinaimg.cn/large/e6c9d24egy1gzlapjg569j20df09i74g.jpg" style="width: 483px; height: 342px;" />
```

输入：l1 = [2,4,3], l2 = [5,6,4]
输出：[7,0,8]
解释：342 + 465 = 807.

```
 **示例 2：** 

```

输入：l1 = [0], l2 = [0]
输出：[0]

```
 **示例 3：** 

```

输入：l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
输出：[8,9,9,9,0,0,0,1]

```


 **提示：** 
- 每个链表中的节点数在范围 `[1, 100]` 内
-  `0 <= Node.val <= 9` 
- 题目数据保证列表表示的数字不含前导零

**标签**
`递归` `链表` `数学` 

## solution1 递归

- 如果`l1.val + l2.val < 10`，不需要考虑进位，加和直接作为和链表当前节点的值，且下一个节点为`addTwoNumbers(l1.next, l2.next)`
- 如果`l1.val + l2.val >= 10`，需要考虑进位，且进位只可能为1。加和减去10作为和链表当前节点的值，且下一个节点为三个节点的值相加：构造一个值为1的`ListNode`表示进位，`l1.next`和`l2.next`，通过两次调用递归函数得到`addTwoNumbers(addTwoNumbers(l1.next, carry), l2.next)`

```python
class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        if not l1:
            return l2
        if not l2:
            return l1
        cur_val = l1.val + l2.val
        if cur_val < 10:
            l3 = ListNode(cur_val)
            l3.next = self.addTwoNumbers(l1.next, l2.next)
        else:
            l3 = ListNode(cur_val - 10)
            carry = ListNode(1)
            l3.next = self.addTwoNumbers(self.addTwoNumbers(l1.next, carry), l2.next)
        return l3
```

## solution2 迭代

- 定义一个进位节点carry
- `l1`, `l2`和`carry`只要有一个节点不为空，就继续迭代
- 当前节点的值为`(l1.val + l2.val + carry.val) mod 10`，如果节点为空，则值定义为0。如果`(l1.val + l2.val + carry.val) >= 10`，则进位节点的值为`(l1.val + l2.val + carry.val) // 10`，否则进位节点置为None
- 更新`l1`和`l2`

```python
class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        dummy = ListNode(-1)
        res = dummy
        carry = None
        while l1 or l2 or carry:
            val1 = l1.val if l1 else 0
            val2 = l2.val if l2 else 0
            cval = carry.val if carry else 0
            cur_val = val1 + val2 + cval
            carry = None if cur_val < 10 else ListNode(1)
            res.next = ListNode(cur_val % 10)
            res = res.next
            l1 = l1.next if l1 else None
            l2 = l2.next if l2 else None
        return dummy.next
```



>
# 19.删除链表的倒数第 N 个结点
[https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list) 
## 原题
给你一个链表，删除链表的倒数第 `n` 个结点，并且返回链表的头结点。

 

 **示例 1：** 
<img alt="" src="https://assets.leetcode.com/uploads/2020/10/03/remove_ex1.jpg" style="width: 542px; height: 222px;" />
```

输入：head = [1,2,3,4,5], n = 2
输出：[1,2,3,5]

```
 **示例 2：** 

```

输入：head = [1], n = 1
输出：[]

```
 **示例 3：** 

```

输入：head = [1,2], n = 1
输出：[1]

```


 **提示：** 
- 链表中结点的数目为 `sz` 
-  `1 <= sz <= 30` 
-  `0 <= Node.val <= 100` 
-  `1 <= n <= sz` 


 **进阶：** 你能尝试使用一趟扫描实现吗？


**标签**
`链表` `双指针` 

## solution 快慢指针

- 删除倒数第n个节点，需要找到倒数第n+1个节点p，然后通过`p.next = p.next.next`就可以删掉倒数第n个节点`p.next`了。
- 寻找倒数第n+1个节点p的方法——快慢指针，以`1->2->3->4->5`，n=2为例
  - 第一步，定义一个dummy节点指向head，即`0->1->2->3->4->5`
  - 第二步，找到正数第n个节点，从0出发，向右跑n步，定义为fast节点，即2
  - 第三步，slow节点从0出发，fast节点从第n个节点(即2)出发，一起向右跑。fast节点右边还有length-n(5-2=3)个节点，那么当fast节点跑到最后一个节点5时，slow节点从0往右跑了length-n步，正数length-n个节点就是倒数第n+1个节点
  - 注意dummy节点不计入正数和倒数的索引中

```python
class Solution:
    def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
        dummy = ListNode(0, head)
        fast = dummy
        for _ in range(n):
            fast = fast.next
        slow = dummy
        while fast.next:
            fast = fast.next
            slow = slow.next
        slow.next = slow.next.next
        return dummy.next
```

>
# 21.合并两个有序链表
[https://leetcode-cn.com/problems/merge-two-sorted-lists](https://leetcode-cn.com/problems/merge-two-sorted-lists) 
## 原题
将两个升序链表合并为一个新的 **升序** 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

 

 **示例 1：** 
<img alt="" src="https://assets.leetcode.com/uploads/2020/10/03/merge_ex1.jpg" style="width: 662px; height: 302px;" />
```

输入：l1 = [1,2,4], l2 = [1,3,4]
输出：[1,1,2,3,4,4]

```
 **示例 2：** 

```

输入：l1 = [], l2 = []
输出：[]

```
 **示例 3：** 

```

输入：l1 = [], l2 = [0]
输出：[0]

```


 **提示：** 
- 两个链表的节点数目范围是 `[0, 50]` 
-  `-100 <= Node.val <= 100` 
-  `l1` 和 `l2` 均按 **非递减顺序** 排列

**标签**
`递归` `链表` 

这题和[2. 两数相加](https://leetcode-cn.com/problems/add-two-numbers/)的处理方式很相似，比较简单，直接看代码即可。

## solution1 递归

```python
class Solution:
    def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
        if not l1:
            return l2
        if not l2:
            return l1
        if l1.val < l2.val:
            l3 = ListNode(l1.val)
            l3.next = self.mergeTwoLists(l1.next, l2)
        else:
            l3 = ListNode(l2.val)
            l3.next = self.mergeTwoLists(l1, l2.next)
        return l3
```

## solution2 迭代

```python
class Solution:
    def mergeTwoLists2(self, l1: ListNode, l2: ListNode) -> ListNode:
        dummy = ListNode(-1)
        res = dummy
        while l1 and l2:
            if l1.val < l2.val:
                res.next = l1
                l1 = l1.next
            else:
                res.next = l2
                l2 = l2.next
            res = res.next
        if l1:
            res.next = l1
        if l2:
            res.next = l2
        return dummy.next
```



>
# 23.合并K个升序链表
[https://leetcode-cn.com/problems/merge-k-sorted-lists](https://leetcode-cn.com/problems/merge-k-sorted-lists) 
## 原题
给你一个链表数组，每个链表都已经按升序排列。

请你将所有链表合并到一个升序链表中，返回合并后的链表。

 

 **示例 1：** 

```
输入：lists = [[1,4,5],[1,3,4],[2,6]]
输出：[1,1,2,3,4,4,5,6]
解释：链表数组如下：
[
  1->4->5,
  1->3->4,
  2->6
]
将它们合并到一个有序链表中得到。
1->1->2->3->4->4->5->6

```
 **示例 2：** 

```
输入：lists = []
输出：[]

```
 **示例 3：** 

```
输入：lists = [[]]
输出：[]

```


 **提示：** 
-  `k == lists.length` 
-  `0 <= k <= 10^4` 
-  `0 <= lists[i].length <= 500` 
-  `-10^4 <= lists[i][j] <= 10^4` 
-  `lists[i]` 按 **升序** 排列
-  `lists[i].length` 的总和不超过 `10^4` 

**标签**
`链表` `分治` `堆（优先队列）` `归并排序` 

## solution1 合并两个有序链表+归并排序

```python
class Solution:
    def mergeKLists(self, lists):
        def mergeTwoLists(l1, l2):
            if not l1:
                return l2
            if not l2:
                return l1
            if l1.val <= l2.val:
                l3 = ListNode(l1.val)
                l3.next = mergeTwoLists(l1.next, l2)
            else:
                l3 = ListNode(l2.val)
                l3.next = mergeTwoLists(l2.next, l1)
            return l3

        n = len(lists)
        if n == 0:
            return None
        elif n == 1:
            return lists[0]
        elif n == 2:
            return mergeTwoLists(lists[0], lists[1])
        
        mid = n // 2
        l1 = self.mergeKLists(lists[:mid])
        l2 = self.mergeKLists(lists[mid:])
        return mergeTwoLists(l1, l2)
```



## solution2 最小堆

- 因为每个链表都是升序的，因此当前的最小值必定是所有链表head节点值中的最小值。维护一个大小为`len(lists)`的最小堆，堆中的每个元素表示一个链表当前的head节点。
- 初始化：heappush每个链表的head节点，其中节点值用于比较大小，链表索引用于找到该节点
- 迭代过程：每次得到一个当前的最小值
  - 从heap中heappop出值最小的节点，即链表`lists[index]`的head节点，作为当前节点p的下一个节点
  - 当前节点移到下一个节点，链表`lists[index]`的head节点移到下一个节点
  - 如果链表`lists[index]`head节点不为空，那么将其heappush到堆中

```python
class Solution:
    def mergeKLists(self, lists: List[ListNode]) -> ListNode:
        import heapq
        heap = []
        for i in range(len(lists)):
            if lists[i]:
                heapq.heappush(heap, (lists[i].val, i))
        
        dummy = ListNode(-1)
        p = dummy
        while heap:
            _, index = heapq.heappop(heap)
            p.next = lists[index]
            p = p.next
            lists[index] = lists[index].next
            if lists[index]:
                heapq.heappush(heap, (lists[index].val, index))
        return dummy.next
```



>
# 24.两两交换链表中的节点
[https://leetcode-cn.com/problems/swap-nodes-in-pairs](https://leetcode-cn.com/problems/swap-nodes-in-pairs) 
## 原题
给你一个链表，两两交换其中相邻的节点，并返回交换后链表的头节点。你必须在不修改节点内部的值的情况下完成本题（即，只能进行节点交换）。

 

 **示例 1：** 
<img alt="" src="https://assets.leetcode.com/uploads/2020/10/03/swap_ex1.jpg" style="width: 422px; height: 222px;" />
```

输入：head = [1,2,3,4]
输出：[2,1,4,3]

```
 **示例 2：** 

```

输入：head = []
输出：[]

```
 **示例 3：** 

```

输入：head = [1]
输出：[1]

```


 **提示：** 
- 链表中节点的数目在范围 `[0, 100]` 内
-  `0 <= Node.val <= 100` 

**标签**
`递归` `链表` 

## solution

![avatar](E:/learning_home/leetcode/markdown_files/leetcode_cn/images/leetcode24.png)

```python
class Solution:
    def swapPairs(self, head: ListNode) -> ListNode:
        if not head or not head.next:
            return head
        dummy = ListNode(-1)
        pre, p = dummy, head
        # pre指向p的前一个节点，交换p和p.next
        while p and p.next:
            pre.next = p.next
            p.next = p.next.next
            pre.next.next = p
            pre = p
            p = p.next
        return dummy.next
```



>
# 25.K 个一组翻转链表
[https://leetcode-cn.com/problems/reverse-nodes-in-k-group](https://leetcode-cn.com/problems/reverse-nodes-in-k-group) 
## 原题
给你一个链表，每  *k * 个节点一组进行翻转，请你返回翻转后的链表。

 *k * 是一个正整数，它的值小于或等于链表的长度。

如果节点总数不是  *k * 的整数倍，那么请将最后剩余的节点保持原有顺序。

 **进阶：** 
- 你可以设计一个只使用常数额外空间的算法来解决此问题吗？
-  **你不能只是单纯的改变节点内部的值** ，而是需要实际进行节点交换。


 **示例 1：** 
<img alt="" src="https://assets.leetcode.com/uploads/2020/10/03/reverse_ex1.jpg" style="width: 542px; height: 222px;" />
```

输入：head = [1,2,3,4,5], k = 2
输出：[2,1,4,3,5]

```
 **示例 2：** 
<img alt="" src="https://assets.leetcode.com/uploads/2020/10/03/reverse_ex2.jpg" style="width: 542px; height: 222px;" />
```

输入：head = [1,2,3,4,5], k = 3
输出：[3,2,1,4,5]

```
 **示例 3：** 

```

输入：head = [1,2,3,4,5], k = 1
输出：[1,2,3,4,5]

```
 **示例 4：** 

```

输入：head = [1], k = 1
输出：[1]

```
 **提示：** 
- 列表中节点的数量在范围 `sz` 内
-  `1 <= sz <= 5000` 
-  `0 <= Node.val <= 1000` 
-  `1 <= k <= sz` 

**标签**
`递归` `链表` 

## solution

- 设置dummy节点
- 关键在于反转前先确认好 上一段的end和本段的start链接，本段的end和下一段的start链接

![avatar](E:/learning_home/leetcode/markdown_files/leetcode_cn/images/leetcode25.png)

```python
class Solution:
    def reverseKGroup(self, head: ListNode, k: int) -> ListNode:
        if not head or not k:
            return head
        length = 0
        p = head
        while p:
            p = p.next
            length += 1
        loops = length // k
        dummy = ListNode(-1)
        dummy.next = head
        pre, p = None, head
        last_end, cur_end = dummy, head
        for _ in range(loops):
            pre = None
            for _ in range(k):
                tmp = p.next
                p.next = pre
                pre = p
                p = tmp
            # after reverse cur part, pre is cur_start, p is next_end
            # last_end.next = cur_start
            last_end.next = pre
            # cur_end.next = next_end
            cur_end.next = p
            last_end, cur_end = cur_end, p
        return dummy.next
```



>
# 61.旋转链表
[https://leetcode-cn.com/problems/rotate-list](https://leetcode-cn.com/problems/rotate-list) 
## 原题
给你一个链表的头节点 `head` ，旋转链表，将链表每个节点向右移动  `k` * * 个位置。

 

 **示例 1：** 
<img alt="" src="https://assets.leetcode.com/uploads/2020/11/13/rotate1.jpg" style="width: 600px; height: 254px;" />
```

输入：head = [1,2,3,4,5], k = 2
输出：[4,5,1,2,3]

```
 **示例 2：** 
<img alt="" src="https://assets.leetcode.com/uploads/2020/11/13/roate2.jpg" style="width: 472px; height: 542px;" />
```

输入：head = [0,1,2], k = 4
输出：[2,0,1]

```


 **提示：** 
- 链表中节点的数目在范围 `[0, 500]` 内
-  `-100 <= Node.val <= 100` 
-  `0 <= k <= 2 * 10^9` 

**标签**
`链表` `双指针` 

## solution

```python

```
>
# 82.删除排序链表中的重复元素 II
[https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii) 
## 原题
给定一个已排序的链表的头 `head` ， *删除原始链表中所有重复数字的节点，只留下不同的数字* 。返回 *已排序的链表* 。

 

 **示例 1：** 
<img alt="" src="https://assets.leetcode.com/uploads/2021/01/04/linkedlist1.jpg" style="height: 142px; width: 500px;" />
```

输入：head = [1,2,3,3,4,4,5]
输出：[1,2,5]

```
 **示例 2：** 
<img alt="" src="https://assets.leetcode.com/uploads/2021/01/04/linkedlist2.jpg" style="height: 164px; width: 400px;" />
```

输入：head = [1,1,1,2,3]
输出：[2,3]

```


 **提示：** 
- 链表中节点数目在范围 `[0, 300]` 内
-  `-100 <= Node.val <= 100` 
- 题目数据保证链表已经按升序 **排列** 

**标签**
`链表` `双指针` 


##
```python

```
>
# 83.删除排序链表中的重复元素
[https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list) 
## 原题
给定一个已排序的链表的头<meta charset="UTF-8" /> `head` ， *删除所有重复的元素，使每个元素只出现一次* 。返回 *已排序的链表* 。

 

 **示例 1：** 
<img alt="" src="https://assets.leetcode.com/uploads/2021/01/04/list1.jpg" style="height: 160px; width: 200px;" />
```

输入：head = [1,1,2]
输出：[1,2]

```
 **示例 2：** 
<img alt="" src="https://assets.leetcode.com/uploads/2021/01/04/list2.jpg" style="height: 123px; width: 300px;" />
```

输入：head = [1,1,2,3,3]
输出：[1,2,3]

```


 **提示：** 
- 链表中节点数目在范围 `[0, 300]` 内
-  `-100 <= Node.val <= 100` 
- 题目数据保证链表已经按升序 **排列** 

**标签**
`链表` 


##
```python

```
>
# 86.分隔链表
[https://leetcode-cn.com/problems/partition-list](https://leetcode-cn.com/problems/partition-list) 
## 原题
给你一个链表的头节点 `head` 和一个特定值 `x` ，请你对链表进行分隔，使得所有 **小于** `x` 的节点都出现在 **大于或等于** `x` 的节点之前。

你应当 **保留** 两个分区中每个节点的初始相对位置。

 

 **示例 1：** 
<img alt="" src="https://assets.leetcode.com/uploads/2021/01/04/partition.jpg" style="width: 662px; height: 222px;" />
```

输入：head = [1,4,3,2,5,2], x = 3
输出：[1,2,2,4,3,5]

```
 **示例 2：** 

```

输入：head = [2,1], x = 2
输出：[1,2]

```


 **提示：** 
- 链表中节点的数目在范围 `[0, 200]` 内
-  `-100 <= Node.val <= 100` 
-  `-200 <= x <= 200` 

**标签**
`链表` `双指针` 


##
```python

```
>
# 92.反转链表 II
[https://leetcode-cn.com/problems/reverse-linked-list-ii](https://leetcode-cn.com/problems/reverse-linked-list-ii) 
## 原题
给你单链表的头指针 `head` 和两个整数  `left` 和 `right` ，其中  `left <= right` 。请你反转从位置 `left` 到位置 `right` 的链表节点，返回 **反转后的链表** 。


 **示例 1：** 
<img alt="" src="https://assets.leetcode.com/uploads/2021/02/19/rev2ex2.jpg" style="width: 542px; height: 222px;" />
```

输入：head = [1,2,3,4,5], left = 2, right = 4
输出：[1,4,3,2,5]

```
 **示例 2：** 

```

输入：head = [5], left = 1, right = 1
输出：[5]

```


 **提示：** 
- 链表中节点数目为 `n` 
-  `1 <= n <= 500` 
-  `-500 <= Node.val <= 500` 
-  `1 <= left <= right <= n` 


 **进阶：** 你可以使用一趟扫描完成反转吗？


**标签**
`链表` 


##
```python

```
>
# 109.有序链表转换二叉搜索树
[https://leetcode-cn.com/problems/convert-sorted-list-to-binary-search-tree](https://leetcode-cn.com/problems/convert-sorted-list-to-binary-search-tree) 
## 原题
给定一个单链表，其中的元素按升序排序，将其转换为高度平衡的二叉搜索树。

本题中，一个高度平衡二叉树是指一个二叉树 *每个节点* 的左右两个子树的高度差的绝对值不超过 1。

 **示例:** 

```
给定的有序链表： [-10, -3, 0, 5, 9],

一个可能的答案是：[0, -3, 9, -10, null, 5], 它可以表示下面这个高度平衡二叉搜索树：

      0
     / \
   -3   9
   /   /
 -10  5

```

**标签**
`树` `二叉搜索树` `链表` `分治` `二叉树` 


##
```python

```
>
# 114.二叉树展开为链表
[https://leetcode-cn.com/problems/flatten-binary-tree-to-linked-list](https://leetcode-cn.com/problems/flatten-binary-tree-to-linked-list) 
## 原题
给你二叉树的根结点 `root` ，请你将它展开为一个单链表：
- 展开后的单链表应该同样使用 `TreeNode` ，其中 `right` 子指针指向链表中下一个结点，而左子指针始终为 `null` 。
- 展开后的单链表应该与二叉树 <a href="https://baike.baidu.com/item/%E5%85%88%E5%BA%8F%E9%81%8D%E5%8E%86/6442839?fr=aladdin" target="_blank"> **先序遍历** </a> 顺序相同。


 **示例 1：** 
<img alt="" src="https://assets.leetcode.com/uploads/2021/01/14/flaten.jpg" style="width: 500px; height: 226px;" />
```

输入：root = [1,2,5,3,4,null,6]
输出：[1,null,2,null,3,null,4,null,5,null,6]

```
 **示例 2：** 

```

输入：root = []
输出：[]

```
 **示例 3：** 

```

输入：root = [0]
输出：[0]

```


 **提示：** 
- 树中结点数在范围 `[0, 2000]` 内
-  `-100 <= Node.val <= 100` 


 **进阶：** 你可以使用原地算法（ `O(1)` 额外空间）展开这棵树吗？


**标签**
`栈` `树` `深度优先搜索` `链表` `二叉树` 


##
```python

```
>
# 116.填充每个节点的下一个右侧节点指针
[https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node](https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node) 
## 原题
给定一个 **完美二叉树** ，其所有叶子节点都在同一层，每个父节点都有两个子节点。二叉树定义如下：

```

struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```
填充它的每个 next 指针，让这个指针指向其下一个右侧节点。如果找不到下一个右侧节点，则将 next 指针设置为 `NULL` 。

初始状态下，所有 next 指针都被设置为 `NULL` 。

 

 **示例 1：** 

<img alt="" src="https://assets.leetcode.com/uploads/2019/02/14/116_sample.png" style="height: 171px; width: 500px;" />

```

输入：root = [1,2,3,4,5,6,7]
输出：[1,#,2,3,#,4,5,6,7,#]
解释：给定二叉树如图 A 所示，你的函数应该填充它的每个 next 指针，以指向其下一个右侧节点，如图 B 所示。序列化的输出按层序遍历排列，同一层节点由 next 指针连接，'#' 标志着每一层的结束。

```
<meta charset="UTF-8" />

 **示例 2:** 

```

输入：root = []
输出：[]

```


 **提示：** 
- 树中节点的数量在<meta charset="UTF-8" /> `[0, 2^12 - 1]` 范围内
-  `-1000 <= node.val <= 1000` 


 **进阶：** 
- 你只能使用常量级额外空间。
- 使用递归解题也符合要求，本题中递归程序占用的栈空间不算做额外的空间复杂度。

**标签**
`树` `深度优先搜索` `广度优先搜索` `链表` `二叉树` 


##
```python

```
>
# 117.填充每个节点的下一个右侧节点指针 II
[https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node-ii](https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node-ii) 
## 原题
给定一个二叉树

```

struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```
填充它的每个 next 指针，让这个指针指向其下一个右侧节点。如果找不到下一个右侧节点，则将 next 指针设置为 `NULL` 。

初始状态下，所有 next 指针都被设置为 `NULL` 。

 

 **进阶：** 
- 你只能使用常量级额外空间。
- 使用递归解题也符合要求，本题中递归程序占用的栈空间不算做额外的空间复杂度。


 **示例：** 

<img alt="" src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/02/15/117_sample.png" style="height: 218px; width: 640px;" />

```

输入：root = [1,2,3,4,5,null,7]
输出：[1,#,2,3,#,4,5,7,#]
解释：给定二叉树如图 A 所示，你的函数应该填充它的每个 next 指针，以指向其下一个右侧节点，如图 B 所示。序列化输出按层序遍历顺序（由 next 指针连接），'#' 表示每层的末尾。
```


 **提示：** 
- 树中的节点数小于 `6000` 
-  `-100 <= node.val <= 100` 


**标签**
`树` `深度优先搜索` `广度优先搜索` `链表` `二叉树` 


##
```python

```
>
# 138.复制带随机指针的链表
[https://leetcode-cn.com/problems/copy-list-with-random-pointer](https://leetcode-cn.com/problems/copy-list-with-random-pointer) 
## 原题
给你一个长度为 `n` 的链表，每个节点包含一个额外增加的随机指针 `random` ，该指针可以指向链表中的任何节点或空节点。

构造这个链表的 **<a href="https://baike.baidu.com/item/深拷贝/22785317?fr=aladdin" target="_blank">深拷贝</a>** 。 深拷贝应该正好由 `n` 个 **全新** 节点组成，其中每个新节点的值都设为其对应的原节点的值。新节点的 `next` 指针和 `random` 指针也都应指向复制链表中的新节点，并使原链表和复制链表中的这些指针能够表示相同的链表状态。 **复制链表中的指针都不应指向原链表中的节点** 。

例如，如果原链表中有 `X` 和 `Y` 两个节点，其中 `X.random --> Y` 。那么在复制链表中对应的两个节点 `x` 和 `y` ，同样有 `x.random --> y` 。

返回复制链表的头节点。

用一个由 `n` 个节点组成的链表来表示输入/输出中的链表。每个节点用一个 `[val, random_index]` 表示：
-  `val` ：一个表示 `Node.val` 的整数。
-  `random_index` ：随机指针指向的节点索引（范围从 `0` 到 `n-1` ）；如果不指向任何节点，则为 `null` 。
你的代码 **只** 接受原链表的头节点 `head` 作为传入参数。

 

 **示例 1：** 

<img alt="" src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/09/e1.png" style="height: 142px; width: 700px;" />

```

输入：head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
输出：[[7,null],[13,0],[11,4],[10,2],[1,0]]

```
 **示例 2：** 

<img alt="" src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/09/e2.png" style="height: 114px; width: 700px;" />

```

输入：head = [[1,1],[2,1]]
输出：[[1,1],[2,1]]

```
 **示例 3：** 

 **<img alt="" src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/09/e3.png" style="height: 122px; width: 700px;" />** 

```

输入：head = [[3,null],[3,0],[3,null]]
输出：[[3,null],[3,0],[3,null]]

```


 **提示：** 
-  `0 <= n <= 1000` <meta charset="UTF-8" />
-  `-10^4 <= Node.val <= 10^4` 
-  `Node.random` 为 `null` 或指向链表中的节点。

**标签**
`哈希表` `链表` 


##
```python

```
>
# 141.环形链表
[https://leetcode-cn.com/problems/linked-list-cycle](https://leetcode-cn.com/problems/linked-list-cycle) 
## 原题
给你一个链表的头节点 `head` ，判断链表中是否有环。

如果链表中有某个节点，可以通过连续跟踪 `next` 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 `pos` 来表示链表尾连接到链表中的位置（索引从 0 开始）。 **注意： `pos` 不作为参数进行传递** 。仅仅是为了标识链表的实际情况。

 *如果链表中存在环* ，则返回 `true` 。 否则，返回 `false` 。

 

 **示例 1：** 

<img alt="" src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist.png" />

```

输入：head = [3,2,0,-4], pos = 1
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。

```
 **示例 2：** 

<img alt="" src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test2.png" />

```

输入：head = [1,2], pos = 0
输出：true
解释：链表中有一个环，其尾部连接到第一个节点。

```
 **示例 3：** 

<img alt="" src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test3.png" />

```

输入：head = [1], pos = -1
输出：false
解释：链表中没有环。

```


 **提示：** 
- 链表中节点的数目范围是 `[0, 10^4]` 
-  `-10^5 <= Node.val <= 10^5` 
-  `pos` 为 `-1` 或者链表中的一个 **有效索引** 。


 **进阶：** 你能用 `O(1)` （即，常量）内存解决此问题吗？


**标签**
`哈希表` `链表` `双指针` 


##
```python

```
>
# 143.重排链表
[https://leetcode-cn.com/problems/reorder-list](https://leetcode-cn.com/problems/reorder-list) 
## 原题
给定一个单链表 `L` 的头节点 `head` ，单链表 `L` 表示为：

```

L0 → L1 → … → Ln - 1 → Ln

```
请将其重新排列后变为：

```

L0 → Ln → L1 → Ln - 1 → L2 → Ln - 2 → …
```
不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

 

 **示例 1：** 

<img alt="" src="https://pic.leetcode-cn.com/1626420311-PkUiGI-image.png" style="width: 240px; " />

```

输入：head = [1,2,3,4]
输出：[1,4,2,3]
```
 **示例 2：** 

<img alt="" src="https://pic.leetcode-cn.com/1626420320-YUiulT-image.png" style="width: 320px; " />

```

输入：head = [1,2,3,4,5]
输出：[1,5,2,4,3]
```


 **提示：** 
- 链表的长度范围为 `[1, 5 * 10^4]` 
-  `1 <= node.val <= 1000` 

**标签**
`栈` `递归` `链表` `双指针` 


##
```python

```
>
# 146.LRU 缓存
[https://leetcode-cn.com/problems/lru-cache](https://leetcode-cn.com/problems/lru-cache) 
## 原题
请你设计并实现一个满足  <a href="https://baike.baidu.com/item/LRU" target="_blank">LRU (最近最少使用) 缓存</a> 约束的数据结构。

实现 `LRUCache` 类：
-  `LRUCache(int capacity)` 以 **正整数** 作为容量 `capacity` 初始化 LRU 缓存
-  `int get(int key)` 如果关键字 `key` 存在于缓存中，则返回关键字的值，否则返回 `-1` 。
-  `void put(int key, int value)` 如果关键字 `key` 已经存在，则变更其数据值 `value` ；如果不存在，则向缓存中插入该组 `key-value` 。如果插入操作导致关键字数量超过 `capacity` ，则应该 **逐出** 最久未使用的关键字。
函数 `get` 和 `put` 必须以 `O(1)` 的平均时间复杂度运行。


 **示例：** 

```

输入
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
输出
[null, null, null, 1, null, -1, null, -1, 3, 4]

解释
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // 缓存是 {1=1}
lRUCache.put(2, 2); // 缓存是 {1=1, 2=2}
lRUCache.get(1);    // 返回 1
lRUCache.put(3, 3); // 该操作会使得关键字 2 作废，缓存是 {1=1, 3=3}
lRUCache.get(2);    // 返回 -1 (未找到)
lRUCache.put(4, 4); // 该操作会使得关键字 1 作废，缓存是 {4=4, 3=3}
lRUCache.get(1);    // 返回 -1 (未找到)
lRUCache.get(3);    // 返回 3
lRUCache.get(4);    // 返回 4

```


 **提示：** 
-  `1 <= capacity <= 3000` 
-  `0 <= key <= 10000` 
-  `0 <= value <= 10^5` 
- 最多调用 `2 * 10^5` 次 `get` 和 `put` 

**标签**
`设计` `哈希表` `链表` `双向链表` 


##
```python

```
>
# 147.对链表进行插入排序
[https://leetcode-cn.com/problems/insertion-sort-list](https://leetcode-cn.com/problems/insertion-sort-list) 
## 原题
给定单个链表的头<meta charset="UTF-8" /> `head` ，使用 **插入排序** 对链表进行排序，并返回 *排序后链表的头* 。

 **插入排序** 算法的步骤:
- 插入排序是迭代的，每次只移动一个元素，直到所有元素可以形成一个有序的输出列表。
- 每次迭代中，插入排序只从输入数据中移除一个待排序的元素，找到它在序列中适当的位置，并将其插入。
- 重复直到所有输入数据插入完为止。
下面是插入排序算法的一个图形示例。部分排序的列表(黑色)最初只包含列表中的第一个元素。每次迭代时，从输入数据中删除一个元素(红色)，并就地插入已排序的列表中。

对链表进行插入排序。

<img alt="" src="https://upload.wikimedia.org/wikipedia/commons/0/0f/Insertion-sort-example-300px.gif" />

 

 **示例 1：** 

<img alt="" src="https://assets.leetcode.com/uploads/2021/03/04/sort1linked-list.jpg" />

```

输入: head = [4,2,1,3]
输出: [1,2,3,4]
```
 **示例 2：** 

<img alt="" src="https://assets.leetcode.com/uploads/2021/03/04/sort2linked-list.jpg" />

```

输入: head = [-1,5,3,4,0]
输出: [-1,0,3,4,5]
```


 **提示：** 

<meta charset="UTF-8" />
- 列表中的节点数在 `[1, 5000]` 范围内
-  `-5000 <= Node.val <= 5000` 

**标签**
`链表` `排序` 


##
```python

```
>
# 148.排序链表
[https://leetcode-cn.com/problems/sort-list](https://leetcode-cn.com/problems/sort-list) 
## 原题
给你链表的头结点 `head` ，请将其按 **升序** 排列并返回 **排序后的链表** 。


 **示例 1：** 
<img alt="" src="https://assets.leetcode.com/uploads/2020/09/14/sort_list_1.jpg" style="width: 450px;" />
```

输入：head = [4,2,1,3]
输出：[1,2,3,4]

```
 **示例 2：** 
<img alt="" src="https://assets.leetcode.com/uploads/2020/09/14/sort_list_2.jpg" style="width: 550px;" />
```

输入：head = [-1,5,3,4,0]
输出：[-1,0,3,4,5]

```
 **示例 3：** 

```

输入：head = []
输出：[]

```


<b>提示：</b>
- 链表中节点的数目在范围 `[0, 5 * 10^4]` 内
-  `-10^5 <= Node.val <= 10^5` 


<b>进阶：</b>你可以在 `O(n log n)` 时间复杂度和常数级空间复杂度下，对链表进行排序吗？


**标签**
`链表` `双指针` `分治` `排序` `归并排序` 


##
```python

```
>
# 160.相交链表
[https://leetcode-cn.com/problems/intersection-of-two-linked-lists](https://leetcode-cn.com/problems/intersection-of-two-linked-lists) 
## 原题
给你两个单链表的头节点 `headA` 和 `headB` ，请你找出并返回两个单链表相交的起始节点。如果两个链表不存在相交节点，返回 `null` 。

图示两个链表在节点 `c1` 开始相交 **：** 

<a href="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png" target="_blank"><img alt="" src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png" style="height: 130px; width: 400px;" /></a>

题目数据 **保证** 整个链式结构中不存在环。

 **注意** ，函数返回结果后，链表必须 **保持其原始结构** 。

 **自定义评测：** 

 **评测系统** 的输入如下（你设计的程序 **不适用** 此输入）：
-  `intersectVal` - 相交的起始节点的值。如果不存在相交节点，这一值为 `0` 
-  `listA` - 第一个链表
-  `listB` - 第二个链表
-  `skipA` - 在 `listA` 中（从头节点开始）跳到交叉节点的节点数
-  `skipB` - 在 `listB` 中（从头节点开始）跳到交叉节点的节点数
评测系统将根据这些输入创建链式数据结构，并将两个头节点 `headA` 和 `headB` 传递给你的程序。如果程序能够正确返回相交节点，那么你的解决方案将被 **视作正确答案** 。

 

 **示例 1：** 

<a href="https://assets.leetcode.com/uploads/2018/12/13/160_example_1.png" target="_blank"><img alt="" src="https://assets.leetcode.com/uploads/2021/03/05/160_example_1_1.png" style="height: 130px; width: 400px;" /></a>

```

输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,6,1,8,4,5], skipA = 2, skipB = 3
输出：Intersected at '8'
解释：相交节点的值为 8 （注意，如果两个链表相交则不能为 0）。
从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,6,1,8,4,5]。
在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。

```
 **示例 2：** 

<a href="https://assets.leetcode.com/uploads/2018/12/13/160_example_2.png" target="_blank"><img alt="" src="https://assets.leetcode.com/uploads/2021/03/05/160_example_2.png" style="height: 136px; width: 350px;" /></a>

```

输入：intersectVal = 2, listA = [1,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
输出：Intersected at '2'
解释：相交节点的值为 2 （注意，如果两个链表相交则不能为 0）。
从各自的表头开始算起，链表 A 为 [1,9,1,2,4]，链表 B 为 [3,2,4]。
在 A 中，相交节点前有 3 个节点；在 B 中，相交节点前有 1 个节点。

```
 **示例 3：** 

<a href="https://assets.leetcode.com/uploads/2018/12/13/160_example_3.png" target="_blank"><img alt="" src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_example_3.png" style="height: 126px; width: 200px;" /></a>

```

输入：intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
输出：null
解释：从各自的表头开始算起，链表 A 为 [2,6,4]，链表 B 为 [1,5]。
由于这两个链表不相交，所以 intersectVal 必须为 0，而 skipA 和 skipB 可以是任意值。
这两个链表不相交，因此返回 null 。

```


 **提示：** 
-  `listA` 中节点数目为 `m` 
-  `listB` 中节点数目为 `n` 
-  `1 <= m, n <= 3 * 10^4` 
-  `1 <= Node.val <= 10^5` 
-  `0 <= skipA <= m` 
-  `0 <= skipB <= n` 
- 如果 `listA` 和 `listB` 没有交点， `intersectVal` 为 `0` 
- 如果 `listA` 和 `listB` 有交点， `intersectVal == listA[skipA] == listB[skipB]` 


 **进阶：** 你能否设计一个时间复杂度 `O(m + n)` 、仅用 `O(1)` 内存的解决方案？


**标签**
`哈希表` `链表` `双指针` 


##
```python

```
>
# 203.移除链表元素
[https://leetcode-cn.com/problems/remove-linked-list-elements](https://leetcode-cn.com/problems/remove-linked-list-elements) 
## 原题
给你一个链表的头节点 `head` 和一个整数 `val` ，请你删除链表中所有满足 `Node.val == val` 的节点，并返回 **新的头节点** 。


 **示例 1：** 
<img alt="" src="https://assets.leetcode.com/uploads/2021/03/06/removelinked-list.jpg" style="width: 500px; height: 142px;" />
```

输入：head = [1,2,6,3,4,5,6], val = 6
输出：[1,2,3,4,5]

```
 **示例 2：** 

```

输入：head = [], val = 1
输出：[]

```
 **示例 3：** 

```

输入：head = [7,7,7,7], val = 7
输出：[]

```


 **提示：** 
- 列表中的节点数目在范围 `[0, 10^4]` 内
-  `1 <= Node.val <= 50` 
-  `0 <= val <= 50` 

**标签**
`递归` `链表` 


##
```python

```
>
# 206.反转链表
[https://leetcode-cn.com/problems/reverse-linked-list](https://leetcode-cn.com/problems/reverse-linked-list) 
## 原题
给你单链表的头节点 `head` ，请你反转链表，并返回反转后的链表。


 **示例 1：** 
<img alt="" src="https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg" style="width: 542px; height: 222px;" />
```

输入：head = [1,2,3,4,5]
输出：[5,4,3,2,1]

```
 **示例 2：** 
<img alt="" src="https://assets.leetcode.com/uploads/2021/02/19/rev1ex2.jpg" style="width: 182px; height: 222px;" />
```

输入：head = [1,2]
输出：[2,1]

```
 **示例 3：** 

```

输入：head = []
输出：[]

```


 **提示：** 
- 链表中节点的数目范围是 `[0, 5000]` 
-  `-5000 <= Node.val <= 5000` 


 **进阶：** 链表可以选用迭代或递归方式完成反转。你能否用两种方法解决这道题？

**标签**
`递归` `链表` 


##
```python

```
>
# 234.回文链表
[https://leetcode-cn.com/problems/palindrome-linked-list](https://leetcode-cn.com/problems/palindrome-linked-list) 
## 原题
给你一个单链表的头节点 `head` ，请你判断该链表是否为回文链表。如果是，返回 `true` ；否则，返回 `false` 。

 

 **示例 1：** 
<img alt="" src="https://assets.leetcode.com/uploads/2021/03/03/pal1linked-list.jpg" style="width: 422px; height: 62px;" />
```

输入：head = [1,2,2,1]
输出：true

```
 **示例 2：** 
<img alt="" src="https://assets.leetcode.com/uploads/2021/03/03/pal2linked-list.jpg" style="width: 182px; height: 62px;" />
```

输入：head = [1,2]
输出：false

```


 **提示：** 
- 链表中节点数目在范围 `[1, 10^5]` 内
-  `0 <= Node.val <= 9` 


 **进阶：** 你能否用 `O(n)` 时间复杂度和 `O(1)` 空间复杂度解决此题？


**标签**
`栈` `递归` `链表` `双指针` 


##
```python

```
>
# 237.删除链表中的节点
[https://leetcode-cn.com/problems/delete-node-in-a-linked-list](https://leetcode-cn.com/problems/delete-node-in-a-linked-list) 
## 原题
请编写一个函数，用于 **删除单链表中某个特定节点** 。在设计函数时需要注意，你无法访问链表的头节点 `head` ，只能直接访问 **要被删除的节点** 。

题目数据保证需要删除的节点 **不是末尾节点** 。

 

 **示例 1：** 
<img alt="" src="https://assets.leetcode.com/uploads/2020/09/01/node1.jpg" style="width: 450px; height: 322px;" />
```

输入：head = [4,5,1,9], node = 5
输出：[4,1,9]
解释：指定链表中值为 5 的第二个节点，那么在调用了你的函数之后，该链表应变为 4 -> 1 -> 9

```
 **示例 2：** 
<img alt="" src="https://assets.leetcode.com/uploads/2020/09/01/node2.jpg" style="width: 450px; height: 354px;" />
```

输入：head = [4,5,1,9], node = 1
输出：[4,5,9]
解释：指定链表中值为 1 的第三个节点，那么在调用了你的函数之后，该链表应变为 4 -> 5 -> 9
```
 **示例 3：** 

```

输入：head = [1,2,3,4], node = 3
输出：[1,2,4]

```
 **示例 4：** 

```

输入：head = [0,1], node = 0
输出：[1]

```
 **示例 5：** 

```

输入：head = [-3,5,-99], node = -3
输出：[5,-99]

```


 **提示：** 
- 链表中节点的数目范围是 `[2, 1000]` 
-  `-1000 <= Node.val <= 1000` 
- 链表中每个节点的值都是唯一的
- 需要删除的节点 `node` 是 **链表中的一个有效节点** ，且 **不是末尾节点** 

**标签**
`链表` 


##
```python

```
>
# 328.奇偶链表
[https://leetcode-cn.com/problems/odd-even-linked-list](https://leetcode-cn.com/problems/odd-even-linked-list) 
## 原题
给定单链表的头节点 `head` ，将所有索引为奇数的节点和索引为偶数的节点分别组合在一起，然后返回重新排序的列表。

 **第一个** 节点的索引被认为是 **奇数** ， **第二个** 节点的索引为 **偶数** ，以此类推。

请注意，偶数组和奇数组内部的相对顺序应该与输入时保持一致。

你必须在 `O(1)` 的额外空间复杂度和 `O(n)` 的时间复杂度下解决这个问题。

 

 **示例 1:** 

<img src="https://assets.leetcode.com/uploads/2021/03/10/oddeven-linked-list.jpg" style="height: 123px; width: 300px;" />

```

输入: head = [1,2,3,4,5]
输出: [1,3,5,2,4]
```
 **示例 2:** 

<img src="https://assets.leetcode.com/uploads/2021/03/10/oddeven2-linked-list.jpg" style="height: 142px; width: 500px;" />

```

输入: head = [2,1,3,5,6,4,7]
输出: [2,3,6,7,1,5,4]
```


 **提示:** 
-  `n ==` 链表中的节点数
-  `0 <= n <= 10^4` 
-  `-10^6 <= Node.val <= 10^6` 

**标签**
`链表` 


##
```python

```
>
# 355.设计推特
[https://leetcode-cn.com/problems/design-twitter](https://leetcode-cn.com/problems/design-twitter) 
## 原题
设计一个简化版的推特(Twitter)，可以让用户实现发送推文，关注/取消关注其他用户，能够看见关注人（包括自己）的最近 `10` 条推文。

实现 `Twitter` 类：
-  `Twitter()` 初始化简易版推特对象
-  `void postTweet(int userId, int tweetId)` 根据给定的 `tweetId` 和 `userId` 创建一条新推文。每次调用此函数都会使用一个不同的 `tweetId` 。
-  `List<Integer> getNewsFeed(int userId)` 检索当前用户新闻推送中最近 `10` 条推文的 ID 。新闻推送中的每一项都必须是由用户关注的人或者是用户自己发布的推文。推文必须 **按照时间顺序由最近到最远排序** 。
-  `void follow(int followerId, int followeeId)` ID 为 `followerId` 的用户开始关注 ID 为 `followeeId` 的用户。
-  `void unfollow(int followerId, int followeeId)` ID 为 `followerId` 的用户不再关注 ID 为 `followeeId` 的用户。


 **示例：** 

```

输入
["Twitter", "postTweet", "getNewsFeed", "follow", "postTweet", "getNewsFeed", "unfollow", "getNewsFeed"]
[[], [1, 5], [1], [1, 2], [2, 6], [1], [1, 2], [1]]
输出
[null, null, [5], null, null, [6, 5], null, [5]]

解释
Twitter twitter = new Twitter();
twitter.postTweet(1, 5); // 用户 1 发送了一条新推文 (用户 id = 1, 推文 id = 5)
twitter.getNewsFeed(1);  // 用户 1 的获取推文应当返回一个列表，其中包含一个 id 为 5 的推文
twitter.follow(1, 2);    // 用户 1 关注了用户 2
twitter.postTweet(2, 6); // 用户 2 发送了一个新推文 (推文 id = 6)
twitter.getNewsFeed(1);  // 用户 1 的获取推文应当返回一个列表，其中包含两个推文，id 分别为 -> [6, 5] 。推文 id 6 应当在推文 id 5 之前，因为它是在 5 之后发送的
twitter.unfollow(1, 2);  // 用户 1 取消关注了用户 2
twitter.getNewsFeed(1);  // 用户 1 获取推文应当返回一个列表，其中包含一个 id 为 5 的推文。因为用户 1 已经不再关注用户 2
```


 **提示：** 
-  `1 <= userId, followerId, followeeId <= 500` 
-  `0 <= tweetId <= 10^4` 
- 所有推特的 ID 都互不相同
-  `postTweet` 、 `getNewsFeed` 、 `follow` 和 `unfollow` 方法最多调用 `3 * 10^4` 次

**标签**
`设计` `哈希表` `链表` `堆（优先队列）` 


##
```python

```
>
# 369.给单链表加一
[https://leetcode-cn.com/problems/plus-one-linked-list](https://leetcode-cn.com/problems/plus-one-linked-list) 
## 原题


**标签**
`链表` `数学` 


##
```python

```
>
# 379.电话目录管理系统
[https://leetcode-cn.com/problems/design-phone-directory](https://leetcode-cn.com/problems/design-phone-directory) 
## 原题


**标签**
`设计` `队列` `数组` `哈希表` `链表` 


##
```python

```
>
# 382.链表随机节点
[https://leetcode-cn.com/problems/linked-list-random-node](https://leetcode-cn.com/problems/linked-list-random-node) 
## 原题
给你一个单链表，随机选择链表的一个节点，并返回相应的节点值。每个节点 **被选中的概率一样** 。

实现 `Solution` 类：
-  `Solution(ListNode head)` 使用整数数组初始化对象。
-  `int getRandom()` 从链表中随机选择一个节点并返回该节点的值。链表中所有节点被选中的概率相等。


 **示例：** 
<img alt="" src="https://assets.leetcode.com/uploads/2021/03/16/getrand-linked-list.jpg" style="width: 302px; height: 62px;" />
```

输入
["Solution", "getRandom", "getRandom", "getRandom", "getRandom", "getRandom"]
[[[1, 2, 3]], [], [], [], [], []]
输出
[null, 1, 3, 2, 2, 3]

解释
Solution solution = new Solution([1, 2, 3]);
solution.getRandom(); // 返回 1
solution.getRandom(); // 返回 3
solution.getRandom(); // 返回 2
solution.getRandom(); // 返回 2
solution.getRandom(); // 返回 3
// getRandom() 方法应随机返回 1、2、3中的一个，每个元素被返回的概率相等。
```


 **提示：** 
- 链表中的节点数在范围 `[1, 10^4]` 内
-  `-10^4 <= Node.val <= 10^4` 
- 至多调用 `getRandom` 方法 `10^4` 次


 **进阶：** 
- 如果链表非常大且长度未知，该怎么处理？
- 你能否在不使用额外空间的情况下解决此问题？

**标签**
`水塘抽样` `链表` `数学` `随机化` 


##
```python

```
>
# 426.将二叉搜索树转化为排序的双向链表
[https://leetcode-cn.com/problems/convert-binary-search-tree-to-sorted-doubly-linked-list](https://leetcode-cn.com/problems/convert-binary-search-tree-to-sorted-doubly-linked-list) 
## 原题


**标签**
`栈` `树` `深度优先搜索` `二叉搜索树` `链表` `二叉树` `双向链表` 


##
```python

```
>
# 430.扁平化多级双向链表
[https://leetcode-cn.com/problems/flatten-a-multilevel-doubly-linked-list](https://leetcode-cn.com/problems/flatten-a-multilevel-doubly-linked-list) 
## 原题
你会得到一个双链表，其中包含的节点有一个下一个指针、一个前一个指针和一个额外的 **子指针** 。这个子指针可能指向一个单独的双向链表，也包含这些特殊的节点。这些子列表可以有一个或多个自己的子列表，以此类推，以生成如下面的示例所示的 **多层数据结构** 。

给定链表的头节点 <font color="#c7254e"><font face="Menlo, Monaco, Consolas, Courier New, monospace"><span style="font-size:12.6px"><span style="background-color:#f9f2f4">head</span></span></font></font> ，将链表 **扁平化** ，以便所有节点都出现在单层双链表中。让 `curr` 是一个带有子列表的节点。子列表中的节点应该出现在 **扁平化列表** 中的 `curr` **之后** 和 `curr.next` **之前** 。

返回 *扁平列表的 `head` 。列表中的节点必须将其 **所有** 子指针设置为 `null` 。* 

 

 **示例 1：** 

<img src="https://assets.leetcode.com/uploads/2021/11/09/flatten11.jpg" style="height:339px; width:700px" />

```

输入：head = [1,2,3,4,5,6,null,null,null,7,8,9,10,null,null,11,12]
输出：[1,2,3,7,8,11,12,9,10,4,5,6]
解释：输入的多级列表如上图所示。
扁平化后的链表如下图：
<img src="https://assets.leetcode.com/uploads/2021/11/09/flatten12.jpg" style="height:69px; width:1000px" />

```
 **示例 2：** 

<img src="https://assets.leetcode.com/uploads/2021/11/09/flatten2.1jpg" style="height:200px; width:200px" />

```

输入：head = [1,2,null,3]
输出：[1,3,2]
解释：输入的多级列表如上图所示。
扁平化后的链表如下图：
<img src="https://assets.leetcode.com/uploads/2021/11/24/list.jpg" style="height:87px; width:300px" />

```
 **示例 3：** 

```

输入：head = []
输出：[]
说明：输入中可能存在空列表。

```


 **提示：** 
- 节点数目不超过 `1000` 
-  `1 <= Node.val <= 10^5` 


 **如何表示测试用例中的多级链表？** 

以 **示例 1** 为例：

```

 1---2---3---4---5---6--NULL
         |
         7---8---9---10--NULL
             |
             11--12--NULL
```
序列化其中的每一级之后：

```

[1,2,3,4,5,6,null]
[7,8,9,10,null]
[11,12,null]

```
为了将每一级都序列化到一起，我们需要每一级中添加值为 null 的元素，以表示没有节点连接到上一级的上级节点。

```

[1,2,3,4,5,6,null]
[null,null,7,8,9,10,null]
[null,11,12,null]

```
合并所有序列化结果，并去除末尾的 null 。

```

[1,2,3,4,5,6,null,null,null,7,8,9,10,null,null,11,12]

```

**标签**
`深度优先搜索` `链表` `双向链表` 


##
```python

```
>
# 432.全 O(1) 的数据结构
[https://leetcode-cn.com/problems/all-oone-data-structure](https://leetcode-cn.com/problems/all-oone-data-structure) 
## 原题
请你设计一个用于存储字符串计数的数据结构，并能够返回计数最小和最大的字符串。

实现 `AllOne` 类：
-  `AllOne()` 初始化数据结构的对象。
-  `inc(String key)` 字符串 `key` 的计数增加 `1` 。如果数据结构中尚不存在 `key` ，那么插入计数为 `1` 的 `key` 。
-  `dec(String key)` 字符串 `key` 的计数减少 `1` 。如果 `key` 的计数在减少后为 `0` ，那么需要将这个 `key` 从数据结构中删除。测试用例保证：在减少计数前， `key` 存在于数据结构中。
-  `getMaxKey()` 返回任意一个计数最大的字符串。如果没有元素存在，返回一个空字符串 `""` 。
-  `getMinKey()` 返回任意一个计数最小的字符串。如果没有元素存在，返回一个空字符串 `""` 。


 **示例：** 

```

输入
["AllOne", "inc", "inc", "getMaxKey", "getMinKey", "inc", "getMaxKey", "getMinKey"]
[[], ["hello"], ["hello"], [], [], ["leet"], [], []]
输出
[null, null, null, "hello", "hello", null, "hello", "leet"]

解释
AllOne allOne = new AllOne();
allOne.inc("hello");
allOne.inc("hello");
allOne.getMaxKey(); // 返回 "hello"
allOne.getMinKey(); // 返回 "hello"
allOne.inc("leet");
allOne.getMaxKey(); // 返回 "hello"
allOne.getMinKey(); // 返回 "leet"

```


 **提示：** 
-  `1 <= key.length <= 10` 
-  `key` 由小写英文字母组成
- 测试用例保证：在每次调用 `dec` 时，数据结构中总存在 `key` 
- 最多调用 `inc` 、 `dec` 、 `getMaxKey` 和 `getMinKey` 方法 `5 * 10^4` 次

**标签**
`设计` `哈希表` `链表` `双向链表` 


##
```python

```
>
# 445.两数相加 II
[https://leetcode-cn.com/problems/add-two-numbers-ii](https://leetcode-cn.com/problems/add-two-numbers-ii) 
## 原题
给你两个 **非空** 链表来代表两个非负整数。数字最高位位于链表开始位置。它们的每个节点只存储一位数字。将这两数相加会返回一个新的链表。

你可以假设除了数字 0 之外，这两个数字都不会以零开头。

 

 **示例1：** 

<img alt="" src="https://pic.leetcode-cn.com/1626420025-fZfzMX-image.png" style="width: 302px; " />

```

输入：l1 = [7,2,4,3], l2 = [5,6,4]
输出：[7,8,0,7]

```
 **示例2：** 

```

输入：l1 = [2,4,3], l2 = [5,6,4]
输出：[8,0,7]

```
 **示例3：** 

```

输入：l1 = [0], l2 = [0]
输出：[0]

```


 **提示：** 
- 链表的长度范围为 `[1, 100]` 
-  `0 <= node.val <= 9` 
- 输入数据保证链表代表的数字无前导 0


 **进阶：** 如果输入链表不能翻转该如何解决？


**标签**
`栈` `链表` `数学` 


##
```python

```
>
# 460.LFU 缓存
[https://leetcode-cn.com/problems/lfu-cache](https://leetcode-cn.com/problems/lfu-cache) 
## 原题
请你为 <a href="https://baike.baidu.com/item/%E7%BC%93%E5%AD%98%E7%AE%97%E6%B3%95">最不经常使用（LFU）</a>缓存算法设计并实现数据结构。

实现 `LFUCache` 类：
-  `LFUCache(int capacity)` - 用数据结构的容量 `capacity` 初始化对象
-  `int get(int key)` - 如果键 `key` 存在于缓存中，则获取键的值，否则返回 `-1` 。
-  `void put(int key, int value)` - 如果键 `key` 已存在，则变更其值；如果键不存在，请插入键值对。当缓存达到其容量 `capacity` 时，则应该在插入新项之前，移除最不经常使用的项。在此问题中，当存在平局（即两个或更多个键具有相同使用频率）时，应该去除 **最近最久未使用** 的键。
为了确定最不常使用的键，可以为缓存中的每个键维护一个 **使用计数器** 。使用计数最小的键是最久未使用的键。

当一个键首次插入到缓存中时，它的使用计数器被设置为 `1` (由于 put 操作)。对缓存中的键执行 `get` 或 `put` 操作，使用计数器的值将会递增。

函数 `get` 和 `put` 必须以 `O(1)` 的平均时间复杂度运行。

 

 **示例：** 

```

输入：
["LFUCache", "put", "put", "get", "put", "get", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [3], [4, 4], [1], [3], [4]]
输出：
[null, null, null, 1, null, -1, 3, null, -1, 3, 4]

解释：
// cnt(x) = 键 x 的使用计数
// cache=[] 将显示最后一次使用的顺序（最左边的元素是最近的）
LFUCache lfu = new LFUCache(2);
lfu.put(1, 1);   // cache=[1,_], cnt(1)=1
lfu.put(2, 2);   // cache=[2,1], cnt(2)=1, cnt(1)=1
lfu.get(1);      // 返回 1
                 // cache=[1,2], cnt(2)=1, cnt(1)=2
lfu.put(3, 3);   // 去除键 2 ，因为 cnt(2)=1 ，使用计数最小
                 // cache=[3,1], cnt(3)=1, cnt(1)=2
lfu.get(2);      // 返回 -1（未找到）
lfu.get(3);      // 返回 3
                 // cache=[3,1], cnt(3)=2, cnt(1)=2
lfu.put(4, 4);   // 去除键 1 ，1 和 3 的 cnt 相同，但 1 最久未使用
                 // cache=[4,3], cnt(4)=1, cnt(3)=2
lfu.get(1);      // 返回 -1（未找到）
lfu.get(3);      // 返回 3
                 // cache=[3,4], cnt(4)=1, cnt(3)=3
lfu.get(4);      // 返回 4
                 // cache=[3,4], cnt(4)=2, cnt(3)=3
```


 **提示：** 
-  `0 <= capacity <= 10^4` 
-  `0 <= key <= 10^5` 
-  `0 <= value <= 10^9` 
- 最多调用 `2 * 10^5` 次 `get` 和 `put` 方法

**标签**
`设计` `哈希表` `链表` `双向链表` 


##
```python

```
>
# 622.设计循环队列
[https://leetcode-cn.com/problems/design-circular-queue](https://leetcode-cn.com/problems/design-circular-queue) 
## 原题
设计你的循环队列实现。 循环队列是一种线性数据结构，其操作表现基于 FIFO（先进先出）原则并且队尾被连接在队首之后以形成一个循环。它也被称为“环形缓冲器”。

循环队列的一个好处是我们可以利用这个队列之前用过的空间。在一个普通队列里，一旦一个队列满了，我们就不能插入下一个元素，即使在队列前面仍有空间。但是使用循环队列，我们能使用这些空间去存储新的值。

你的实现应该支持如下操作：
-  `MyCircularQueue(k)` : 构造器，设置队列长度为 k 。
-  `Front` : 从队首获取元素。如果队列为空，返回 -1 。
-  `Rear` : 获取队尾元素。如果队列为空，返回 -1 。
-  `enQueue(value)` : 向循环队列插入一个元素。如果成功插入则返回真。
-  `deQueue()` : 从循环队列中删除一个元素。如果成功删除则返回真。
-  `isEmpty()` : 检查循环队列是否为空。
-  `isFull()` : 检查循环队列是否已满。


 **示例：** 

```
MyCircularQueue circularQueue = new MyCircularQueue(3); // 设置长度为 3
circularQueue.enQueue(1);  // 返回 true
circularQueue.enQueue(2);  // 返回 true
circularQueue.enQueue(3);  // 返回 true
circularQueue.enQueue(4);  // 返回 false，队列已满
circularQueue.Rear();  // 返回 3
circularQueue.isFull();  // 返回 true
circularQueue.deQueue();  // 返回 true
circularQueue.enQueue(4);  // 返回 true
circularQueue.Rear();  // 返回 4
```


 **提示：** 
- 所有的值都在 0 至 1000 的范围内；
- 操作数将在 1 至 1000 的范围内；
- 请不要使用内置的队列库。

**标签**
`设计` `队列` `数组` `链表` 


##
```python

```
>
# 641.设计循环双端队列
[https://leetcode-cn.com/problems/design-circular-deque](https://leetcode-cn.com/problems/design-circular-deque) 
## 原题
设计实现双端队列。

实现 `MyCircularDeque` 类:
-  `MyCircularDeque(int k)` ：构造函数,双端队列最大为 `k` 。
-  `boolean insertFront()` ：将一个元素添加到双端队列头部。 如果操作成功返回 `true` ，否则返回 `false` 。
-  `boolean insertLast()` ：将一个元素添加到双端队列尾部。如果操作成功返回 `true` ，否则返回 `false` 。
-  `boolean deleteFront()` ：从双端队列头部删除一个元素。 如果操作成功返回 `true` ，否则返回 `false` 。
-  `boolean deleteLast()` ：从双端队列尾部删除一个元素。如果操作成功返回 `true` ，否则返回 `false` 。
-  `int getFront()` )：从双端队列头部获得一个元素。如果双端队列为空，返回 `-1` 。
-  `int getRear()` ：获得双端队列的最后一个元素。 如果双端队列为空，返回 `-1` 。
-  `boolean isEmpty()` ：若双端队列为空，则返回 `true` ，否则返回 `false` 。
-  `boolean isFull()` ：若双端队列满了，则返回 `true` ，否则返回 `false` 。


 **示例 1：** 

```

输入
["MyCircularDeque", "insertLast", "insertLast", "insertFront", "insertFront", "getRear", "isFull", "deleteLast", "insertFront", "getFront"]
[[3], [1], [2], [3], [4], [], [], [], [4], []]
输出
[null, true, true, true, false, 2, true, true, true, 4]

解释
MyCircularDeque circularDeque = new MycircularDeque(3); // 设置容量大小为3
circularDeque.insertLast(1);			        // 返回 true
circularDeque.insertLast(2);			        // 返回 true
circularDeque.insertFront(3);			        // 返回 true
circularDeque.insertFront(4);			        // 已经满了，返回 false
circularDeque.getRear();  				// 返回 2
circularDeque.isFull();				        // 返回 true
circularDeque.deleteLast();			        // 返回 true
circularDeque.insertFront(4);			        // 返回 true
circularDeque.getFront();				// 返回 4
 
```


 **提示：** 
-  `1 <= k <= 1000` 
-  `0 <= value <= 1000` 
-  `insertFront` , `insertLast` , `deleteFront` , `deleteLast` , `getFront` , `getRear` , `isEmpty` , `isFull` 调用次数不大于 `2000` 次

**标签**
`设计` `队列` `数组` `链表` 


##
```python

```
>
# 705.设计哈希集合
[https://leetcode-cn.com/problems/design-hashset](https://leetcode-cn.com/problems/design-hashset) 
## 原题
不使用任何内建的哈希表库设计一个哈希集合（HashSet）。

实现 `MyHashSet` 类：
-  `void add(key)` 向哈希集合中插入值 `key` 。
-  `bool contains(key)` 返回哈希集合中是否存在这个值 `key` 。
-  `void remove(key)` 将给定值 `key` 从哈希集合中删除。如果哈希集合中没有这个值，什么也不做。

 

 **示例：** 

```

输入：
["MyHashSet", "add", "add", "contains", "contains", "add", "contains", "remove", "contains"]
[[], [1], [2], [1], [3], [2], [2], [2], [2]]
输出：
[null, null, null, true, false, null, true, null, false]

解释：
MyHashSet myHashSet = new MyHashSet();
myHashSet.add(1);      // set = [1]
myHashSet.add(2);      // set = [1, 2]
myHashSet.contains(1); // 返回 True
myHashSet.contains(3); // 返回 False ，（未找到）
myHashSet.add(2);      // set = [1, 2]
myHashSet.contains(2); // 返回 True
myHashSet.remove(2);   // set = [1]
myHashSet.contains(2); // 返回 False ，（已移除）
```


 **提示：** 
-  `0 <= key <= 10^6` 
- 最多调用 `10^4` 次 `add` 、 `remove` 和 `contains` 

**标签**
`设计` `数组` `哈希表` `链表` `哈希函数` 


##
```python

```
>
# 706.设计哈希映射
[https://leetcode-cn.com/problems/design-hashmap](https://leetcode-cn.com/problems/design-hashmap) 
## 原题
不使用任何内建的哈希表库设计一个哈希映射（HashMap）。

实现 `MyHashMap` 类：
-  `MyHashMap()` 用空映射初始化对象
-  `void put(int key, int value)` 向 HashMap 插入一个键值对 `(key, value)` 。如果 `key` 已经存在于映射中，则更新其对应的值 `value` 。
-  `int get(int key)` 返回特定的 `key` 所映射的 `value` ；如果映射中不包含 `key` 的映射，返回 `-1` 。
-  `void remove(key)` 如果映射中存在 `key` 的映射，则移除 `key` 和它所对应的 `value` 。


 **示例：** 

```

输入：
["MyHashMap", "put", "put", "get", "get", "put", "get", "remove", "get"]
[[], [1, 1], [2, 2], [1], [3], [2, 1], [2], [2], [2]]
输出：
[null, null, null, 1, -1, null, 1, null, -1]

解释：
MyHashMap myHashMap = new MyHashMap();
myHashMap.put(1, 1); // myHashMap 现在为 [[1,1]]
myHashMap.put(2, 2); // myHashMap 现在为 [[1,1], [2,2]]
myHashMap.get(1);    // 返回 1 ，myHashMap 现在为 [[1,1], [2,2]]
myHashMap.get(3);    // 返回 -1（未找到），myHashMap 现在为 [[1,1], [2,2]]
myHashMap.put(2, 1); // myHashMap 现在为 [[1,1], [2,1]]（更新已有的值）
myHashMap.get(2);    // 返回 1 ，myHashMap 现在为 [[1,1], [2,1]]
myHashMap.remove(2); // 删除键为 2 的数据，myHashMap 现在为 [[1,1]]
myHashMap.get(2);    // 返回 -1（未找到），myHashMap 现在为 [[1,1]]

```


 **提示：** 
-  `0 <= key, value <= 10^6` 
- 最多调用 `10^4` 次 `put` 、 `get` 和 `remove` 方法

**标签**
`设计` `数组` `哈希表` `链表` `哈希函数` 


##
```python

```
>
# 707.设计链表
[https://leetcode-cn.com/problems/design-linked-list](https://leetcode-cn.com/problems/design-linked-list) 
## 原题
设计链表的实现。您可以选择使用单链表或双链表。单链表中的节点应该具有两个属性： `val` 和 `next` 。 `val` 是当前节点的值， `next` 是指向下一个节点的指针/引用。如果要使用双向链表，则还需要一个属性 `prev` 以指示链表中的上一个节点。假设链表中的所有节点都是 0-index 的。

在链表类中实现这些功能：
- get(index)：获取链表中第 `index` 个节点的值。如果索引无效，则返回 `-1` 。
- addAtHead(val)：在链表的第一个元素之前添加一个值为 `val` 的节点。插入后，新节点将成为链表的第一个节点。
- addAtTail(val)：将值为 `val` 的节点追加到链表的最后一个元素。
- addAtIndex(index,val)：在链表中的第 `index` 个节点之前添加值为 `val` 的节点。如果 `index` 等于链表的长度，则该节点将附加到链表的末尾。如果 `index` 大于链表长度，则不会插入节点。如果 `index` 小于0，则在头部插入节点。
- deleteAtIndex(index)：如果索引 `index` 有效，则删除链表中的第 `index` 个节点。


 **示例：** 

```
MyLinkedList linkedList = new MyLinkedList();
linkedList.addAtHead(1);
linkedList.addAtTail(3);
linkedList.addAtIndex(1,2);   //链表变为1-> 2-> 3
linkedList.get(1);            //返回2
linkedList.deleteAtIndex(1);  //现在链表是1-> 3
linkedList.get(1);            //返回3

```


 **提示：** 
- 所有 `val` 值都在 `[1, 1000]` 之内。
- 操作次数将在 `[1, 1000]` 之内。
- 请不要使用内置的 LinkedList 库。

**标签**
`设计` `链表` 


##
```python

```
>
# 708.循环有序列表的插入
[https://leetcode-cn.com/problems/insert-into-a-sorted-circular-linked-list](https://leetcode-cn.com/problems/insert-into-a-sorted-circular-linked-list) 
## 原题


**标签**
`链表` 


##
```python

```
>
# 716.最大栈
[https://leetcode-cn.com/problems/max-stack](https://leetcode-cn.com/problems/max-stack) 
## 原题


**标签**
`栈` `设计` `链表` `双向链表` `有序集合` 


##
```python

```
>
# 725.分隔链表
[https://leetcode-cn.com/problems/split-linked-list-in-parts](https://leetcode-cn.com/problems/split-linked-list-in-parts) 
## 原题
给你一个头结点为 `head` 的单链表和一个整数 `k` ，请你设计一个算法将链表分隔为 `k` 个连续的部分。

每部分的长度应该尽可能的相等：任意两部分的长度差距不能超过 1 。这可能会导致有些部分为 null 。

这 `k` 个部分应该按照在链表中出现的顺序排列，并且排在前面的部分的长度应该大于或等于排在后面的长度。

返回一个由上述 `k` 部分组成的数组。


 **示例 1：** 
<img alt="" src="https://assets.leetcode.com/uploads/2021/06/13/split1-lc.jpg" style="width: 400px; height: 134px;" />
```

输入：head = [1,2,3], k = 5
输出：[[1],[2],[3],[],[]]
解释：
第一个元素 output[0] 为 output[0].val = 1 ，output[0].next = null 。
最后一个元素 output[4] 为 null ，但它作为 ListNode 的字符串表示是 [] 。

```
 **示例 2：** 
<img alt="" src="https://assets.leetcode.com/uploads/2021/06/13/split2-lc.jpg" style="width: 600px; height: 60px;" />
```

输入：head = [1,2,3,4,5,6,7,8,9,10], k = 3
输出：[[1,2,3,4],[5,6,7],[8,9,10]]
解释：
输入被分成了几个连续的部分，并且每部分的长度相差不超过 1 。前面部分的长度大于等于后面部分的长度。

```


 **提示：** 
- 链表中节点的数目在范围 `[0, 1000]` 
-  `0 <= Node.val <= 1000` 
-  `1 <= k <= 50` 

**标签**
`链表` 


##
```python

```
>
# 817.链表组件
[https://leetcode-cn.com/problems/linked-list-components](https://leetcode-cn.com/problems/linked-list-components) 
## 原题
给定链表头结点 `head` ，该链表上的每个结点都有一个 **唯一的整型值** 。同时给定列表 `nums` ，该列表是上述链表中整型值的一个子集。

返回列表 `nums` 中组件的个数，这里对组件的定义为：链表中一段最长连续结点的值（该值必须在列表 `nums` 中）构成的集合。

 

 **示例 1：** 

<img src="https://assets.leetcode.com/uploads/2021/07/22/lc-linkedlistcom1.jpg" />

```

输入: head = [0,1,2,3], nums = [0,1,3]
输出: 2
解释: 链表中,0 和 1 是相连接的，且 nums 中不包含 2，所以 [0, 1] 是 nums 的一个组件，同理 [3] 也是一个组件，故返回 2。
```
 **示例 2：** 

 **** <img src="https://assets.leetcode.com/uploads/2021/07/22/lc-linkedlistcom2.jpg" />

```

输入: head = [0,1,2,3,4], nums = [0,3,1,4]
输出: 2
解释: 链表中，0 和 1 是相连接的，3 和 4 是相连接的，所以 [0, 1] 和 [3, 4] 是两个组件，故返回 2。
```


 **提示：** 
- 链表中节点数为 `n` 
-  `1 <= n <= 10^4` 
-  `0 <= Node.val < n` 
-  `Node.val` 中所有值 **不同** 
-  `1 <= nums.length <= n` 
-  `0 <= nums[i] < n` 
-  `nums` 中所有值 **不同** 

**标签**
`哈希表` `链表` 


##
```python

```
>
# 876.链表的中间结点
[https://leetcode-cn.com/problems/middle-of-the-linked-list](https://leetcode-cn.com/problems/middle-of-the-linked-list) 
## 原题
给定一个头结点为 `head`  的非空单链表，返回链表的中间结点。

如果有两个中间结点，则返回第二个中间结点。

 

 **示例 1：** 

```

输入：[1,2,3,4,5]
输出：此列表中的结点 3 (序列化形式：[3,4,5])
返回的结点值为 3 。 (测评系统对该结点序列化表述是 [3,4,5])。
注意，我们返回了一个 ListNode 类型的对象 ans，这样：
ans.val = 3, ans.next.val = 4, ans.next.next.val = 5, 以及 ans.next.next.next = NULL.

```
 **示例 2：** 

```

输入：[1,2,3,4,5,6]
输出：此列表中的结点 4 (序列化形式：[4,5,6])
由于该列表有两个中间结点，值分别为 3 和 4，我们返回第二个结点。

```


 **提示：** 
- 给定链表的结点数介于  `1`  和  `100`  之间。

**标签**
`链表` `双指针` 


##
```python

```
>
# 1019.链表中的下一个更大节点
[https://leetcode-cn.com/problems/next-greater-node-in-linked-list](https://leetcode-cn.com/problems/next-greater-node-in-linked-list) 
## 原题
给定一个长度为 `n` 的链表 `head` 

对于列表中的每个节点，查找下一个 **更大节点** 的值。也就是说，对于每个节点，找到它旁边的第一个节点的值，这个节点的值 **严格大于** 它的值。

返回一个整数数组 `answer` ，其中 `answer[i]` 是第 `i` 个节点( **从1开始** )的下一个更大的节点的值。如果第 `i` 个节点没有下一个更大的节点，设置 `answer[i] = 0` 。

 

 **示例 1：** 

<img src="https://assets.leetcode.com/uploads/2021/08/05/linkedlistnext1.jpg" />

```

输入：head = [2,1,5]
输出：[5,5,0]

```
 **示例 2：** 

<img src="https://assets.leetcode.com/uploads/2021/08/05/linkedlistnext2.jpg" />

```

输入：head = [2,7,4,3,5]
输出：[7,0,5,5,0]

```


 **提示：** 
- 链表中节点数为 `n` 
-  `1 <= n <= 10^4` 
-  `1 <= Node.val <= 10^9` 

**标签**
`栈` `数组` `链表` `单调栈` 


##
```python

```
>
# 1171.从链表中删去总和值为零的连续节点
[https://leetcode-cn.com/problems/remove-zero-sum-consecutive-nodes-from-linked-list](https://leetcode-cn.com/problems/remove-zero-sum-consecutive-nodes-from-linked-list) 
## 原题
给你一个链表的头节点 `head` ，请你编写代码，反复删去链表中由 **总和** 值为 `0` 的连续节点组成的序列，直到不存在这样的序列为止。

删除完毕后，请你返回最终结果链表的头节点。

 

你可以返回任何满足题目要求的答案。

（注意，下面示例中的所有序列，都是对 `ListNode` 对象序列化的表示。）

 **示例 1：** 

```
输入：head = [1,2,-3,3,1]
输出：[3,1]
提示：答案 [1,2,1] 也是正确的。

```
 **示例 2：** 

```
输入：head = [1,2,3,-3,4]
输出：[1,2,4]

```
 **示例 3：** 

```
输入：head = [1,2,3,-3,-2]
输出：[1]

```


 **提示：** 
- 给你的链表中可能有 `1` 到 `1000` 个节点。
- 对于链表中的每个节点，节点的值： `-1000 <= node.val <= 1000` .

**标签**
`哈希表` `链表` 


##
```python

```
>
# 1206.设计跳表
[https://leetcode-cn.com/problems/design-skiplist](https://leetcode-cn.com/problems/design-skiplist) 
## 原题
不使用任何库函数，设计一个 **跳表** 。

 **跳表** 是在 `O(log(n))` 时间内完成增加、删除、搜索操作的数据结构。跳表相比于树堆与红黑树，其功能与性能相当，并且跳表的代码长度相较下更短，其设计思想与链表相似。

例如，一个跳表包含 `[30, 40, 50, 60, 70, 90]` ，然后增加 `80` 、 `45` 到跳表中，以下图的方式操作：

<img alt="" src="https://assets.leetcode.com/uploads/2019/09/27/1506_skiplist.gif" /><br />
<small>Artyom Kalinin [CC BY-SA 3.0], via <a href="https://commons.wikimedia.org/wiki/File:Skip_list_add_element-en.gif" target="_blank" title="Artyom Kalinin [CC BY-SA 3.0 (https://creativecommons.org/licenses/by-sa/3.0)], via Wikimedia Commons">Wikimedia Commons</a></small>

跳表中有很多层，每一层是一个短的链表。在第一层的作用下，增加、删除和搜索操作的时间复杂度不超过 `O(n)` 。跳表的每一个操作的平均时间复杂度是 `O(log(n))` ，空间复杂度是 `O(n)` 。

了解更多 : <a href="https://en.wikipedia.org/wiki/Skip_list" target="_blank">https://en.wikipedia.org/wiki/Skip_list</a>

在本题中，你的设计应该要包含这些函数：
-  `bool search(int target)` : 返回target是否存在于跳表中。
-  `void add(int num)` : 插入一个元素到跳表。
-  `bool erase(int num)` : 在跳表中删除一个值，如果 `num` 不存在，直接返回false. 如果存在多个 `num` ，删除其中任意一个即可。
注意，跳表中可能存在多个相同的值，你的代码需要处理这种情况。

 

 **示例 1:** 

```

输入
["Skiplist", "add", "add", "add", "search", "add", "search", "erase", "erase", "search"]
[[], [1], [2], [3], [0], [4], [1], [0], [1], [1]]
输出
[null, null, null, null, false, null, true, false, true, false]

解释
Skiplist skiplist = new Skiplist();
skiplist.add(1);
skiplist.add(2);
skiplist.add(3);
skiplist.search(0);   // 返回 false
skiplist.add(4);
skiplist.search(1);   // 返回 true
skiplist.erase(0);    // 返回 false，0 不在跳表中
skiplist.erase(1);    // 返回 true
skiplist.search(1);   // 返回 false，1 已被擦除

```


 **提示:** 
-  `0 <= num, target <= 2 * 10^4` 
- 调用 `search` , `add` , `erase` 操作次数不大于 `5 * 10^4` 

**标签**
`设计` `链表` 


##
```python

```
>
# 1265.逆序打印不可变链表
[https://leetcode-cn.com/problems/print-immutable-linked-list-in-reverse](https://leetcode-cn.com/problems/print-immutable-linked-list-in-reverse) 
## 原题


**标签**
`栈` `递归` `链表` `双指针` 


##
```python

```
>
# 1290.二进制链表转整数
[https://leetcode-cn.com/problems/convert-binary-number-in-a-linked-list-to-integer](https://leetcode-cn.com/problems/convert-binary-number-in-a-linked-list-to-integer) 
## 原题
给你一个单链表的引用结点 `head` 。链表中每个结点的值不是 0 就是 1。已知此链表是一个整数数字的二进制表示形式。

请你返回该链表所表示数字的 **十进制值** 。

 

 **示例 1：** 

<img alt="" src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/12/15/graph-1.png" style="height: 108px; width: 426px;">

```
输入：head = [1,0,1]
输出：5
解释：二进制数 (101) 转化为十进制数 (5)

```
 **示例 2：** 

```
输入：head = [0]
输出：0

```
 **示例 3：** 

```
输入：head = [1]
输出：1

```
 **示例 4：** 

```
输入：head = [1,0,0,1,0,0,1,1,1,0,0,0,0,0,0]
输出：18880

```
 **示例 5：** 

```
输入：head = [0,0]
输出：0

```


 **提示：** 
- 链表不为空。
- 链表的结点总数不超过 `30` 。
- 每个结点的值不是 `0` 就是 `1` 。

**标签**
`链表` `数学` 


##
```python

```
>
# 1367.二叉树中的列表
[https://leetcode-cn.com/problems/linked-list-in-binary-tree](https://leetcode-cn.com/problems/linked-list-in-binary-tree) 
## 原题
给你一棵以 `root` 为根的二叉树和一个 `head` 为第一个节点的链表。

如果在二叉树中，存在一条一直向下的路径，且每个点的数值恰好一一对应以 `head` 为首的链表中每个节点的值，那么请你返回 `True` ，否则返回 `False` 。

一直向下的路径的意思是：从树中某个节点开始，一直连续向下的路径。

 

 **示例 1：** 

 **<img alt="" src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/02/29/sample_1_1720.png" style="height: 280px; width: 220px;">** 

```
输入：head = [4,2,8], root = [1,4,4,null,2,2,null,1,null,6,8,null,null,null,null,1,3]
输出：true
解释：树中蓝色的节点构成了与链表对应的子路径。

```
 **示例 2：** 

 **<img alt="" src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/02/29/sample_2_1720.png" style="height: 280px; width: 220px;">** 

```
输入：head = [1,4,2,6], root = [1,4,4,null,2,2,null,1,null,6,8,null,null,null,null,1,3]
输出：true

```
 **示例 3：** 

```
输入：head = [1,4,2,6,8], root = [1,4,4,null,2,2,null,1,null,6,8,null,null,null,null,1,3]
输出：false
解释：二叉树中不存在一一对应链表的路径。

```


 **提示：** 
- 二叉树和链表中的每个节点的值都满足 `1 <= node.val <= 100` 。
- 链表包含的节点数目在 `1` 到 `100` 之间。
- 二叉树包含的节点数目在 `1` 到 `2500` 之间。

**标签**
`树` `深度优先搜索` `广度优先搜索` `链表` `二叉树` 


##
```python

```
>
# 1472.设计浏览器历史记录
[https://leetcode-cn.com/problems/design-browser-history](https://leetcode-cn.com/problems/design-browser-history) 
## 原题
你有一个只支持单个标签页的 **浏览器** ，最开始你浏览的网页是 `homepage` ，你可以访问其他的网站 `url` ，也可以在浏览历史中后退 `steps` 步或前进 `steps` 步。

请你实现 `BrowserHistory` 类：
-  `BrowserHistory(string homepage)` ，用 `homepage` 初始化浏览器类。
-  `void visit(string url)` 从当前页跳转访问 `url` 对应的页面  。执行此操作会把浏览历史前进的记录全部删除。
-  `string back(int steps)` 在浏览历史中后退 `steps` 步。如果你只能在浏览历史中后退至多 `x` 步且 `steps > x` ，那么你只后退 `x` 步。请返回后退 **至多** `steps` 步以后的 `url` 。
-  `string forward(int steps)` 在浏览历史中前进 `steps` 步。如果你只能在浏览历史中前进至多 `x` 步且 `steps > x` ，那么你只前进 `x` 步。请返回前进 **至多** `steps` 步以后的 `url` 。


 **示例：** 

```
输入：
["BrowserHistory","visit","visit","visit","back","back","forward","visit","forward","back","back"]
[["leetcode.com"],["google.com"],["facebook.com"],["youtube.com"],[1],[1],[1],["linkedin.com"],[2],[2],[7]]
输出：
[null,null,null,null,"facebook.com","google.com","facebook.com",null,"linkedin.com","google.com","leetcode.com"]

解释：
BrowserHistory browserHistory = new BrowserHistory("leetcode.com");
browserHistory.visit("google.com");       // 你原本在浏览 "leetcode.com" 。访问 "google.com"
browserHistory.visit("facebook.com");     // 你原本在浏览 "google.com" 。访问 "facebook.com"
browserHistory.visit("youtube.com");      // 你原本在浏览 "facebook.com" 。访问 "youtube.com"
browserHistory.back(1);                   // 你原本在浏览 "youtube.com" ，后退到 "facebook.com" 并返回 "facebook.com"
browserHistory.back(1);                   // 你原本在浏览 "facebook.com" ，后退到 "google.com" 并返回 "google.com"
browserHistory.forward(1);                // 你原本在浏览 "google.com" ，前进到 "facebook.com" 并返回 "facebook.com"
browserHistory.visit("linkedin.com");     // 你原本在浏览 "facebook.com" 。 访问 "linkedin.com"
browserHistory.forward(2);                // 你原本在浏览 "linkedin.com" ，你无法前进任何步数。
browserHistory.back(2);                   // 你原本在浏览 "linkedin.com" ，后退两步依次先到 "facebook.com" ，然后到 "google.com" ，并返回 "google.com"
browserHistory.back(7);                   // 你原本在浏览 "google.com"， 你只能后退一步到 "leetcode.com" ，并返回 "leetcode.com"

```


 **提示：** 
-  `1 <= homepage.length <= 20` 
-  `1 <= url.length <= 20` 
-  `1 <= steps <= 100` 
-  `homepage` 和 `url` 都只包含 ';.'; 或者小写英文字母。
- 最多调用 `5000` 次 `visit` ， `back` 和 `forward` 函数。

**标签**
`栈` `设计` `数组` `链表` `数据流` `双向链表` 


##
```python

```
>
# 1474.删除链表 M 个节点之后的 N 个节点
[https://leetcode-cn.com/problems/delete-n-nodes-after-m-nodes-of-a-linked-list](https://leetcode-cn.com/problems/delete-n-nodes-after-m-nodes-of-a-linked-list) 
## 原题


**标签**
`链表` 


##
```python

```
>
# 1634.求两个多项式链表的和
[https://leetcode-cn.com/problems/add-two-polynomials-represented-as-linked-lists](https://leetcode-cn.com/problems/add-two-polynomials-represented-as-linked-lists) 
## 原题


**标签**
`链表` `数学` `双指针` 


##
```python

```
>
# 1669.合并两个链表
[https://leetcode-cn.com/problems/merge-in-between-linked-lists](https://leetcode-cn.com/problems/merge-in-between-linked-lists) 
## 原题
给你两个链表 `list1` 和 `list2` ，它们包含的元素分别为 `n` 个和 `m` 个。

请你将 `list1` 中下标从 `a` 到 `b` 的全部节点都删除，并将 `list2` 接在被删除节点的位置。

下图中蓝色边和节点展示了操作后的结果：
<img alt="" src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/11/28/fig1.png" style="height: 130px; width: 504px;" />
请你返回结果链表的头指针。

 

 **示例 1：** 

<img alt="" src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/11/28/merge_linked_list_ex1.png" style="width: 406px; height: 140px;" />

```

输入：list1 = [0,1,2,3,4,5], a = 3, b = 4, list2 = [1000000,1000001,1000002]
输出：[0,1,2,1000000,1000001,1000002,5]
解释：我们删除 list1 中下标为 3 和 4 的两个节点，并将 list2 接在该位置。上图中蓝色的边和节点为答案链表。

```
 **示例 2：** 
<img alt="" src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/11/28/merge_linked_list_ex2.png" style="width: 463px; height: 140px;" />
```

输入：list1 = [0,1,2,3,4,5,6], a = 2, b = 5, list2 = [1000000,1000001,1000002,1000003,1000004]
输出：[0,1,1000000,1000001,1000002,1000003,1000004,6]
解释：上图中蓝色的边和节点为答案链表。

```


 **提示：** 
-  `3 <= list1.length <= 10^4` 
-  `1 <= a <= b < list1.length - 1` 
-  `1 <= list2.length <= 10^4` 

**标签**
`链表` 


##
```python

```
>
# 1670.设计前中后队列
[https://leetcode-cn.com/problems/design-front-middle-back-queue](https://leetcode-cn.com/problems/design-front-middle-back-queue) 
## 原题
请你设计一个队列，支持在前，中，后三个位置的 `push`  和 `pop`  操作。

请你完成  `FrontMiddleBack`  类：
-  `FrontMiddleBack()`  初始化队列。
-  `void pushFront(int val)` 将  `val`  添加到队列的 **最前面**  。
-  `void pushMiddle(int val)` 将  `val`  添加到队列的 **正中间**  。
-  `void pushBack(int val)`  将  `val`  添加到队里的 **最后面**  。
-  `int popFront()`  将 **最前面** 的元素从队列中删除并返回值，如果删除之前队列为空，那么返回 `-1`  。
-  `int popMiddle()` 将 <b>正中间</b> 的元素从队列中删除并返回值，如果删除之前队列为空，那么返回 `-1`  。
-  `int popBack()` 将 **最后面** 的元素从队列中删除并返回值，如果删除之前队列为空，那么返回 `-1`  。
请注意当有  **两个**  中间位置的时候，选择靠前面的位置进行操作。比方说：
- 将 `6`  添加到  `[1, 2, 3, 4, 5]`  的中间位置，结果数组为  `[1, 2, **6** , 3, 4, 5]`  。
- 从  `[1, 2, **3** , 4, 5, 6]`  的中间位置弹出元素，返回  `3`  ，数组变为  `[1, 2, 4, 5, 6]`  。


 **示例 1：** 

```

输入：
["FrontMiddleBackQueue", "pushFront", "pushBack", "pushMiddle", "pushMiddle", "popFront", "popMiddle", "popMiddle", "popBack", "popFront"]
[[], [1], [2], [3], [4], [], [], [], [], []]
输出：
[null, null, null, null, null, 1, 3, 4, 2, -1]

解释：
FrontMiddleBackQueue q = new FrontMiddleBackQueue();
q.pushFront(1);   // [1]
q.pushBack(2);    // [1, 2]
q.pushMiddle(3);  // [1, 3, 2]
q.pushMiddle(4);  // [1, 4, 3, 2]
q.popFront();     // 返回 1 -> [4, 3, 2]
q.popMiddle();    // 返回 3 -> [4, 2]
q.popMiddle();    // 返回 4 -> [2]
q.popBack();      // 返回 2 -> []
q.popFront();     // 返回 -1 -> [] （队列为空）

```


 **提示：** 
-  `1 <= val <= 10^9` 
- 最多调用  `1000`  次  `pushFront` ，  `pushMiddle` ，  `pushBack` ，  `popFront` ，  `popMiddle`  和  `popBack` 。

**标签**
`设计` `队列` `数组` `链表` `数据流` 


##
```python

```
>
# 1721.交换链表中的节点
[https://leetcode-cn.com/problems/swapping-nodes-in-a-linked-list](https://leetcode-cn.com/problems/swapping-nodes-in-a-linked-list) 
## 原题
给你链表的头节点 `head` 和一个整数 `k` 。

 **交换** 链表正数第 `k` 个节点和倒数第 `k` 个节点的值后，返回链表的头节点（链表 **从 1 开始索引** ）。

 

 **示例 1：** 
<img alt="" src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/01/10/linked1.jpg" style="width: 722px; height: 202px;" />
```

输入：head = [1,2,3,4,5], k = 2
输出：[1,4,3,2,5]

```
 **示例 2：** 

```

输入：head = [7,9,6,6,7,8,3,0,9,5], k = 5
输出：[7,9,6,6,8,7,3,0,9,5]

```
 **示例 3：** 

```

输入：head = [1], k = 1
输出：[1]

```
 **示例 4：** 

```

输入：head = [1,2], k = 1
输出：[2,1]

```
 **示例 5：** 

```

输入：head = [1,2,3], k = 2
输出：[1,2,3]

```


 **提示：** 
- 链表中节点的数目是 `n` 
-  `1 <= k <= n <= 10^5` 
-  `0 <= Node.val <= 100` 

**标签**
`链表` `双指针` 


##
```python

```
>
# 1836.从未排序的链表中移除重复元素
[https://leetcode-cn.com/problems/remove-duplicates-from-an-unsorted-linked-list](https://leetcode-cn.com/problems/remove-duplicates-from-an-unsorted-linked-list) 
## 原题


**标签**
`哈希表` `链表` 


##
```python

```
>
# 2046.给按照绝对值排序的链表排序
[https://leetcode-cn.com/problems/sort-linked-list-already-sorted-using-absolute-values](https://leetcode-cn.com/problems/sort-linked-list-already-sorted-using-absolute-values) 
## 原题


**标签**
`链表` `双指针` `排序` 


##
```python

```
>
# 2058.找出临界点之间的最小和最大距离
[https://leetcode-cn.com/problems/find-the-minimum-and-maximum-number-of-nodes-between-critical-points](https://leetcode-cn.com/problems/find-the-minimum-and-maximum-number-of-nodes-between-critical-points) 
## 原题
链表中的 **临界点** 定义为一个 **局部极大值点** **或** **局部极小值点 。** 

如果当前节点的值 **严格大于** 前一个节点和后一个节点，那么这个节点就是一个 **局部极大值点** 。

如果当前节点的值 **严格小于** 前一个节点和后一个节点，那么这个节点就是一个 **局部极小值点** 。

注意：节点只有在同时存在前一个节点和后一个节点的情况下，才能成为一个 **局部极大值点 / 极小值点** 。

给你一个链表 `head` ，返回一个长度为 2 的数组 `[minDistance, maxDistance]` ，其中 `minDistance` 是任意两个不同临界点之间的最小距离， `maxDistance` 是任意两个不同临界点之间的最大距离。如果临界点少于两个，则返回 `[-1，-1]` 。

 

 **示例 1：** 

<img alt="" src="https://assets.leetcode.com/uploads/2021/10/13/a1.png" style="width: 148px; height: 55px;" />

```

输入：head = [3,1]
输出：[-1,-1]
解释：链表 [3,1] 中不存在临界点。

```
 **示例 2：** 

<img alt="" src="https://assets.leetcode.com/uploads/2021/10/13/a2.png" style="width: 624px; height: 46px;" />

```

输入：head = [5,3,1,2,5,1,2]
输出：[1,3]
解释：存在三个临界点：
- [5,3,1,2,5,1,2]：第三个节点是一个局部极小值点，因为 1 比 3 和 2 小。
- [5,3,1,2,5,1,2]：第五个节点是一个局部极大值点，因为 5 比 2 和 1 大。
- [5,3,1,2,5,1,2]：第六个节点是一个局部极小值点，因为 1 比 5 和 2 小。
第五个节点和第六个节点之间距离最小。minDistance = 6 - 5 = 1 。
第三个节点和第六个节点之间距离最大。maxDistance = 6 - 3 = 3 。

```
 **示例 3：** 

<img alt="" src="https://assets.leetcode.com/uploads/2021/10/14/a5.png" style="width: 624px; height: 39px;" />

```

输入：head = [1,3,2,2,3,2,2,2,7]
输出：[3,3]
解释：存在两个临界点：
- [1,3,2,2,3,2,2,2,7]：第二个节点是一个局部极大值点，因为 3 比 1 和 2 大。
- [1,3,2,2,3,2,2,2,7]：第五个节点是一个局部极大值点，因为 3 比 2 和 2 大。
最小和最大距离都存在于第二个节点和第五个节点之间。
因此，minDistance 和 maxDistance 是 5 - 2 = 3 。
注意，最后一个节点不算一个局部极大值点，因为它之后就没有节点了。

```
 **示例 4：** 

<img alt="" src="https://assets.leetcode.com/uploads/2021/10/13/a4.png" style="width: 345px; height: 52px;" />

```

输入：head = [2,3,3,2]
输出：[-1,-1]
解释：链表 [2,3,3,2] 中不存在临界点。

```


 **提示：** 
- 链表中节点的数量在范围 `[2, 10^5]` 内
-  `1 <= Node.val <= 10^5` 

**标签**
`链表` 


##
```python

```
>
# 2074.反转偶数长度组的节点
[https://leetcode-cn.com/problems/reverse-nodes-in-even-length-groups](https://leetcode-cn.com/problems/reverse-nodes-in-even-length-groups) 
## 原题
给你一个链表的头节点 `head` 。

链表中的节点 **按顺序** 划分成若干 **非空** 组，这些非空组的长度构成一个自然数序列（ `1, 2, 3, 4, ...` ）。一个组的 **长度** 就是组中分配到的节点数目。换句话说：
- 节点 `1` 分配给第一组
- 节点 `2` 和 `3` 分配给第二组
- 节点 `4` 、 `5` 和 `6` 分配给第三组，以此类推
注意，最后一组的长度可能小于或者等于 `1 + 倒数第二组的长度` 。

 **反转** 每个 **偶数** 长度组中的节点，并返回修改后链表的头节点 `head` 。

 

 **示例 1：** 

<img alt="" src="https://assets.leetcode.com/uploads/2021/10/25/eg1.png" style="width: 699px; height: 124px;" />

```

输入：head = [5,2,6,3,9,1,7,3,8,4]
输出：[5,6,2,3,9,1,4,8,3,7]
解释：
- 第一组长度为 1 ，奇数，没有发生反转。
- 第二组长度为 2 ，偶数，节点反转。
- 第三组长度为 3 ，奇数，没有发生反转。
- 最后一组长度为 4 ，偶数，节点反转。

```
 **示例 2：** 

<img alt="" src="https://assets.leetcode.com/uploads/2021/10/25/eg2.png" style="width: 284px; height: 114px;" />

```

输入：head = [1,1,0,6]
输出：[1,0,1,6]
解释：
- 第一组长度为 1 ，没有发生反转。
- 第二组长度为 2 ，节点反转。
- 最后一组长度为 1 ，没有发生反转。

```
 **示例 3：** 

<img alt="" src="https://assets.leetcode.com/uploads/2021/10/28/eg3.png" style="width: 139px; height: 114px;" />

```

输入：head = [2,1]
输出：[2,1]
解释：
- 第一组长度为 1 ，没有发生反转。
- 最后一组长度为 1 ，没有发生反转。

```
 **示例 4：** 

```

输入：head = [8]
输出：[8]
解释：只有一个长度为 1 的组，没有发生反转。

```


 **提示：** 
- 链表中节点数目范围是 `[1, 10^5]` 
-  `0 <= Node.val <= 10^5` 

**标签**
`链表` 


##
```python

```
>
# 2095.删除链表的中间节点
[https://leetcode-cn.com/problems/delete-the-middle-node-of-a-linked-list](https://leetcode-cn.com/problems/delete-the-middle-node-of-a-linked-list) 
## 原题
给你一个链表的头节点 `head` 。 **删除** 链表的 **中间节点** ，并返回修改后的链表的头节点 `head` 。

长度为 `n` 链表的中间节点是从头数起第 `⌊n / 2⌋` 个节点（下标从 **0** 开始），其中 `⌊x⌋` 表示小于或等于 `x` 的最大整数。
- 对于 `n` = `1` 、 `2` 、 `3` 、 `4` 和 `5` 的情况，中间节点的下标分别是 `0` 、 `1` 、 `1` 、 `2` 和 `2` 。


 **示例 1：** 

<img alt="" src="https://assets.leetcode.com/uploads/2021/11/16/eg1drawio.png" style="width: 500px; height: 77px;" />

```

输入：head = [1,3,4,7,1,2,6]
输出：[1,3,4,1,2,6]
解释：
上图表示给出的链表。节点的下标分别标注在每个节点的下方。
由于 n = 7 ，值为 7 的节点 3 是中间节点，用红色标注。
返回结果为移除节点后的新链表。 

```
 **示例 2：** 

<img alt="" src="https://assets.leetcode.com/uploads/2021/11/16/eg2drawio.png" style="width: 250px; height: 43px;" />

```

输入：head = [1,2,3,4]
输出：[1,2,4]
解释：
上图表示给出的链表。
对于 n = 4 ，值为 3 的节点 2 是中间节点，用红色标注。

```
 **示例 3：** 

<img alt="" src="https://assets.leetcode.com/uploads/2021/11/16/eg3drawio.png" style="width: 150px; height: 58px;" />

```

输入：head = [2,1]
输出：[2]
解释：
上图表示给出的链表。
对于 n = 2 ，值为 1 的节点 1 是中间节点，用红色标注。
值为 2 的节点 0 是移除节点 1 后剩下的唯一一个节点。
```


 **提示：** 
- 链表中节点的数目在范围 `[1, 10^5]` 内
-  `1 <= Node.val <= 10^5` 

**标签**
`链表` `双指针` 


##
```python

```
>
# 2130
##
```python

```
>

