---
layout:     post
title:      leetcode-题解
subtitle:   
date:       2022-02-25
author:     FishRedLeaf
header-img: iu_img/my_iu_11.jpg
catalog: true
tags:
    - DSA与刷题/leetcode

---

题解

# [5253. 找到指定长度的回文数](https://leetcode-cn.com/problems/find-palindrome-with-fixed-length/)


给你一个整数数组 `queries` 和一个 **正** 整数 `intLength` ，请你返回一个数组 `answer` ，其中 `answer[i]` 是长度为 `intLength` 的 **正回文数** 中第`queries[i]` 小的数字，如果不存在这样的回文数，则为 `-1` 。

**回文数** 指的是从前往后和从后往前读一模一样的数字。回文数不能有前导 0 。

**示例 1：**

```
输入：queries = [1,2,3,4,5,90], intLength = 3
输出：[101,111,121,131,141,999]
解释：
长度为 3 的最小回文数依次是：
101, 111, 121, 131, 141, 151, 161, 171, 181, 191, 201, ...
第 90 个长度为 3 的回文数是 999 。
```

**示例 2：**

```
输入：queries = [2,4,6], intLength = 4
输出：[1111,1331,1551]
解释：
长度为 4 的前 6 个回文数是：
1001, 1111, 1221, 1331, 1441 和 1551 。
```

**提示：**

*   `1 <= queries.length <= 5 * 10<sup>4</sup>`
*   `1 <= queries[i] <= 10<sup>9</sup>`
*   `1 <= intLength <= 15`

**思路**：

1.   边界情况：intLength = 1 or 2

- `intLength = 1`所有符合条件的回文数为`[1, 2, 3, 4, 5, 6, 7, 8, 9]`
- `intLength = 2`所有符合条件的回文数为`[11, 22, 33, 44, 55, 66, 77, 88, 99]`
- 根据query索引从中选择即可

2.   考虑一个例子，`intLength = 1`如何生成`intLength = 3`

- 在`intLength = 1`的结果中加入0，得到基底`base = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]`
- 遍历`i = 1~9`，在`base`中的每个数前后都加上i，例如`i=1`，得到`[101, 111, 121, 131, 141, 151, 161, 171, 181, 191]`
- 因此可以归纳出一般情况
    - `intLength = 2 * n + 1`的回文数个数为`9 * (10 ** n)`
        - `1, 3, ..., 2 * n - 1`生成带上前导0的回文数，个数分别为`10, 100, ..., 10 ** n`
        - `2 * n + 1`生成不带前导0的回文数，个数为`9 * (10 ** n)`
    - 同理，`intLength = 2 * n`的回文数个数为`9 * (10 ** (n - 1))`

3.   `intLength`分为奇数和偶数，考虑递推关系

- 奇数由1逐渐生成，偶数由2逐渐生成，带前导0的基底分别为
    - 奇数：`base = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]`
    - 偶数：`base = [00, 11, 22, 33, 44, 55, 66, 77, 88, 99]`
- 以`intLength`是奇数为例，假设`intLength = 2 * n + 1`，考虑递推关系
    - 对于`intLength = 1 ~ 2 * n - 1`，计算包含前导0的所有回文数；对于`2 * n + 1`，计算不包含前导0的所有回文数
    - `2 * n - 3` -> `2 * n - 1`
        - 假设所有长度为`2 * n - 3`符合条件的回文数为`lst`
        - 所有长度为`2 * n - 1`符合条件的回文数为，遍历`i = 0~9`，在`lst`中的每个数前后都加上i
    - `2 * n - 1` -> `2 * n + 1`
        - 假设所有长度为`2 * n - 1`符合条件的回文数为`lst`
        - 所有长度为`2 * n + 1`符合条件的回文数为，遍历`i = 1~9`，在`lst`中的每个数前后都加上i

4.   以`query = 20, intLength = 5`为例，分析解法

- 问题1：`query = 20`时，从`intLength = 3`变为`intLength = 5`，前后加的数是多少
    - 假设所有长度为3的回文数为`a1, a2, ..., al`，根据前文的分析，易知`l = 100`
        - 那么`intLength = 5`的第`1 ~ 100`个数为 `int( str(1) + str(ai) + str(1) ), i = 1, ..., l`
        - 第`101 ~ 200`个数为 `int( str(2) + str(ai) + str(2) ), i = 1, ..., l`
        - ...
        - 第`801 ~ 900`个数为 `int( str(9) + str(ai) + str(9) ), i = 1, ..., l`
    - 只要找出第`query`个数落在第几个区间块即可
        - 当前区间块的长度为 `block_len = 10 ** ((5-1) // 2) = 100`
        - 前后加的数为 `int((query - 1) / block_len) + 1 = 1`, `res.append(1)`
        - 因此确定`query = 20, intLength = 5`的数为`1xxx1`，中间三位数需要后续进一步确定
    - `query`如何更新？
        - 长度为5的回文数中的第20个数，其中间的三个数对应 长度为3的带前导0的回文数中的 第多少个数
        - 很显然想到是余数，即`query = (query - 1) % block_len + 1 = 20`
- 问题2：`query = 20`时，从`intLength = 1`变为`intLength = 3`，前后加的数是多少
    - 假设所有长度为1的回文数为`a1, a2, ..., al`，根据前文的分析，易知`l = 10`
        - 那么`intLength = 3`的第`1 ~ 10`个数为 `int( str(0) + str(ai) + str(0) ), i = 1, ..., l`
        - 第`101 ~ 200`个数为 `int( str(1) + str(ai) + str(1) ), i = 1, ..., l`
        - ...
        - 第`901 ~ 1000`个数为 `int( str(9) + str(ai) + str(9) ), i = 1, ..., l`
    - 只要找出第`query`个数落在第几个区间块即可
        - 当前区间块的长度为 `block_len = 10 ** ((3-1) // 2) = 10`
        - 前后加的数为 `int((query - 1) / block_len) + 1 = 2`, `res.append(2)`
        - 因此确定`query = 20, intLength = 3`的数为`12x21`，中间三位数需要后续进一步确定
    - `query`如何更新？
        - 长度为5的回文数中的第20个数，其中间的三个数对应 长度为3的带前导0的回文数中的 第多少个数
        - 很显然想到是余数，即`query = (query - 1) % block_len + 1 = 10` 
- 偶数遍历到 `intLength = 4`，奇数遍历到`intLength = 3`，根据此时的query从base中取数
- 最终结果为 `res + [base[query-1]] + res[::-1]`



**代码**：

```python
class Solution:
    def kthPalindrome(self, queries: List[int], intLength: int) -> List[int]:
        if intLength == 1:
            res = [str(i) for i in range(1, 10)]
            return [int(res[q-1]) if q <= len(res) else -1 for q in queries]
        if intLength == 2:
            res = [str(i) + str(i) for i in range(1, 10)]
            return [int(res[q-1]) if q <= len(res) else -1 for q in queries]
        
        base = [str(i) for i in range(10)]
        if intLength % 2 == 0:
            base = [str(i) + str(i) for i in range(10)]
        
        def gen_num(q):
            res = []
            for i in range(intLength, 2, -2):
                block_len = 10 ** ((i-1) // 2)
                prefix = str(int((q - 1) / block_len) + 1)
                if i != intLength:
                    prefix = str(int((q - 1) / block_len))
                res.append(prefix)
                q = (q - 1) % block_len + 1
            ans = res + [base[q-1]] + res[::-1]
            return int("".join(ans))
        
        cur_len = 9 * (10 ** ((intLength - 1) // 2))
        return [gen_num(q) if q <= cur_len else -1 for q in queries]
                

```
