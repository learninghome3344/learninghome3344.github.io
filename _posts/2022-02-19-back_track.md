---
layout:     post   				    # 使用的布局（不需要改）
title:      leetcode backtrack 				# 标题 
subtitle:    #副标题
date:       2022-02-19 				# 时间
author:     FishRedLeaf 						# 作者
header-img: img/post-bg-ios10.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - leetcode
---

# 套路

[C++ 总结了回溯问题类型 带你搞懂回溯算法(大量例题) - 子集 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/subsets/solution/c-zong-jie-liao-hui-su-wen-ti-lei-xing-dai-ni-gao-/)

[剑指 Offer 38. 字符串的排列 题解 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/zi-fu-chuan-de-pai-lie-lcof/solution/c-zong-jie-liao-hui-su-wen-ti-lei-xing-dai-ni-ga-4/)

| 类型       | 题目链接                                                     |
| ---------- | ------------------------------------------------------------ |
| 子集、组合 | 子集、子集 II、组合、组合总和、组合总和 II、电话号码的字母组合 |
| 全排列     | 全排列、全排列 II、字符串的全排列、字母大小写全排列          |
| 搜索       | 解数独、单词搜索、N皇后、分割回文串、二进制手表              |

- 子集、组合类问题，关键是用一个 start 参数来控制选择列表！！最后回溯六步走：
  ①画出递归树，找到状态变量(回溯函数的参数)，这一步非常重要※
  ②根据题意，确立结束条件
  ③找准选择列表(与函数参数相关),与第一步紧密关联※
  ④判断是否需要剪枝
  ⑤作出选择，递归调用，进入下一层
  ⑥撤销选择
- “排列”类型问题和“子集、组合”问题不同在于：“排列”问题使用used数组来标识选择列表，而“子集、组合”问题则使用start参数。另外还需注意两种问题的判重剪枝！！

https://leetcode-cn.com/tag/backtracking/

# 17.电话号码的字母组合

[https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number) 
## 原题
给定一个仅包含数字 `2-9` 的字符串，返回所有它能表示的字母组合。答案可以按 **任意顺序** 返回。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

<img src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/11/09/200px-telephone-keypad2svg.png" style="width: 200px;" />

 

 **示例 1：** 

```

输入：digits = "23"
输出：["ad","ae","af","bd","be","bf","cd","ce","cf"]

```
 **示例 2：** 

```

输入：digits = ""
输出：[]

```
 **示例 3：** 

```

输入：digits = "2"
输出：["a","b","c"]

```


 **提示：** 
-  `0 <= digits.length <= 4` 
-  `digits[i]` 是范围 `['2', '9']` 的一个数字。

**标签**
`哈希表` `字符串` `回溯` 

## solution

```python
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        def dfs(digits, path, start):
            if len(digits) == len(path):
                res.append("".join(path))
                return
            for i in range(start, len(digits)):
                for a in i2a[digits[i]]:
                    dfs(digits, path + [a], i+1)

        res = []
        if not digits:
            return res
        i2a = {'2': "abc", '3': "def", 
            '4': "ghi", '5': "jkl", '6': "mno", 
            '7': "pqrs", '8': "tuv", '9': "wxyz"}
        
        dfs(digits, [], 0)
        return res
```
>
# 22.括号生成
[https://leetcode-cn.com/problems/generate-parentheses](https://leetcode-cn.com/problems/generate-parentheses) 
## 原题
数字 `n` 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 **有效的** 括号组合。

 

 **示例 1：** 

```

输入：n = 3
输出：["((()))","(()())","(())()","()(())","()()()"]

```
 **示例 2：** 

```

输入：n = 1
输出：["()"]

```


 **提示：** 
-  `1 <= n <= 8` 

**标签**
`字符串` `动态规划` `回溯` 

## solution

```python
class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        def dfs(n, left_num, right_num, path):
            if left_num == n and right_num == n:
                res.append("".join(path))
                return
            if left_num < n:
                dfs(n, left_num + 1, right_num, path + ["("])
            if right_num < left_num <= n:
                dfs(n, left_num, right_num + 1, path + [")"])

        res = []
        dfs(n, 0, 0, [])
        return res
```
>
# 37.解数独
[https://leetcode-cn.com/problems/sudoku-solver](https://leetcode-cn.com/problems/sudoku-solver) 
## 原题
编写一个程序，通过填充空格来解决数独问题。

数独的解法需 **遵循如下规则** ：
- 数字  `1-9`  在每一行只能出现一次。
- 数字  `1-9`  在每一列只能出现一次。
- 数字  `1-9`  在每一个以粗实线分隔的  `3x3`  宫内只能出现一次。（请参考示例图）
数独部分空格内已填入了数字，空白格用  `'.'`  表示。


 **示例：** 
<img src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/04/12/250px-sudoku-by-l2g-20050714svg.png" style="height:250px; width:250px" />
```

输入：board = [["5","3",".",".","7",".",".",".","."],["6",".",".","1","9","5",".",".","."],[".","9","8",".",".",".",".","6","."],["8",".",".",".","6",".",".",".","3"],["4",".",".","8",".","3",".",".","1"],["7",".",".",".","2",".",".",".","6"],[".","6",".",".",".",".","2","8","."],[".",".",".","4","1","9",".",".","5"],[".",".",".",".","8",".",".","7","9"]]
输出：[["5","3","4","6","7","8","9","1","2"],["6","7","2","1","9","5","3","4","8"],["1","9","8","3","4","2","5","6","7"],["8","5","9","7","6","1","4","2","3"],["4","2","6","8","5","3","7","9","1"],["7","1","3","9","2","4","8","5","6"],["9","6","1","5","3","7","2","8","4"],["2","8","7","4","1","9","6","3","5"],["3","4","5","2","8","6","1","7","9"]]
解释：输入的数独如上图所示，唯一有效的解决方案如下所示：

```

<img src=" https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/04/12/250px-sudoku-by-l2g-20050714_solutionsvg.png" style="height:250px; width:250px" /> 

**提示：** 

-  `board.length == 9` 
-  `board[i].length == 9` 
-  `board[i][j]` 是一位数字或者 `'.'` 
- 题目数据 **保证** 输入数独仅有一个解

**标签**
`数组` `回溯` `矩阵` 

## solution

- 一个位置可选的`num`集合为三个部分的交集，行剩余元素，列剩余元素和方块剩余元素
- 所有需要补充的位置加入`dot_pos`中，递归调用，对第一个需要填充的位置调用`dfs`函数，函数体内判断成功时自动对下一个位置调用`dfs`函数
- 方块的保存方式要记录下，在涉及二维数组的题目时是一种通用的方式，`i // 3 * 3 + j // 3`

```python
class Solution:
    def solveSudoku(self, board: List[List[str]]) -> None:
        """
        Do not return anything, modify board in-place instead.
        """
        def dfs(board, dot_pos, row, col, block, start):
            if start == len(dot_pos):
                return True
            i, j = dot_pos[start]
            cur_set = row[i] & col[j] & block[i // 3 * 3 + j // 3]
            if not cur_set:
                return False
            for c in cur_set:
                tmp = board[i][j]
                board[i][j] = c
                row[i].remove(c)
                col[j].remove(c)
                block[i // 3 * 3+ j // 3].remove(c)
                if dfs(board, dot_pos, row, col, block, start+1):
                    return True
               	board[i][j] = tmp
                row[i].add(c)
                col[j].add(c)
                block[i // 3 * 3+ j // 3].add(c)
                        
        m, n = len(board), len(board[0])
        row = [set([str(i) for i in range(1, 10)]) for _ in range(9)]
        col = [set([str(i) for i in range(1, 10)]) for _ in range(9)]
        block = [set([str(i) for i in range(1, 10)]) for _ in range(9)]
        dot_pos = []
        for i in range(m):
            for j in range(n):
                if board[i][j] == ".":
                    dot_pos.append((i, j))
                else:
                    row[i].remove(board[i][j])
                    col[j].remove(board[i][j])
                    block[i // 3 * 3+ j // 3].remove(board[i][j])
        dfs(board, dot_pos, row, col, block, 0)

```
>
# 39.组合总和
[https://leetcode-cn.com/problems/combination-sum](https://leetcode-cn.com/problems/combination-sum) 
## 原题
给你一个 **无重复元素** 的整数数组 `candidates` 和一个目标整数 `target` ，找出 `candidates` 中可以使数字和为目标数 `target` 的 *所有* **不同组合** ，并以列表形式返回。你可以按 **任意顺序** 返回这些组合。

 `candidates` 中的 **同一个** 数字可以 **无限制重复被选取** 。如果至少一个数字的被选数量不同，则两种组合是不同的。 

对于给定的输入，保证和为 `target` 的不同组合数少于 `150` 个。

 

 **示例 1：** 

```

输入：candidates = [2,3,6,7], target = 7
输出：[[2,2,3],[7]]
解释：
2 和 3 可以形成一组候选，2 + 2 + 3 = 7 。注意 2 可以使用多次。
7 也是一个候选， 7 = 7 。
仅有这两种组合。
```
 **示例 2：** 

```

输入: candidates = [2,3,5], target = 8
输出: [[2,2,2,2],[2,3,3],[3,5]]
```
 **示例 3：** 

```

输入: candidates = [2], target = 1
输出: []

```


 **提示：** 
-  `1 <= candidates.length <= 30` 
-  `1 <= candidates[i] <= 200` 
-  `candidate` 中的每个元素都 **互不相同** 
-  `1 <= target <= 500` 

**标签**
`数组` `回溯` 

## solution

```python
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        def dfs(candidates, target, path, sum_, start):
            if sum_ == target:
                res.append(path[:])
            if sum_ < target:
                for i in range(start, len(candidates)):
                    dfs(candidates, target, path + [candidates[i]], sum_ + candidates[i], i)

        res = []
        dfs(candidates, target, [], 0, 0)
        return res
```
>
# 40.组合总和 II
[https://leetcode-cn.com/problems/combination-sum-ii](https://leetcode-cn.com/problems/combination-sum-ii) 
## 原题
给定一个候选人编号的集合 `candidates` 和一个目标数 `target` ，找出 `candidates` 中所有可以使数字和为 `target` 的组合。

 `candidates` 中的每个数字在每个组合中只能使用 **一次** 。

 **注意：** 解集不能包含重复的组合。 

 

 **示例 1:** 

```

输入: candidates = [10,1,2,7,6,1,5], target = 8,
输出:
[
[1,1,6],
[1,2,5],
[1,7],
[2,6]
]
```
 **示例 2:** 

```

输入: candidates = [2,5,2,1,2], target = 5,
输出:
[
[1,2,2],
[5]
]
```


 **提示:** 
-  `1 <= candidates.length <= 100` 
-  `1 <= candidates[i] <= 50` 
-  `1 <= target <= 30` 

**标签**
`数组` `回溯` 

## solution

```python
class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        def dfs(candidates, target, path, sum_, start):
            if sum_ == target:
                res.append(path[:])
            if sum_ < target:
                for i in range(start, len(candidates)):
                    if i != start and candidates[i] == candidates[i-1]:
                        continue
                    dfs(candidates, target, path + [candidates[i]], sum_ + candidates[i], i+1)
        
        candidates.sort()
        res = []
        dfs(candidates, target, [], 0, 0)
        return res
```
>
# 46.全排列
[https://leetcode-cn.com/problems/permutations](https://leetcode-cn.com/problems/permutations) 
## 原题
给定一个不含重复数字的数组 `nums` ，返回其 *所有可能的全排列* 。你可以 **按任意顺序** 返回答案。

 

 **示例 1：** 

```

输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]

```
 **示例 2：** 

```

输入：nums = [0,1]
输出：[[0,1],[1,0]]

```
 **示例 3：** 

```

输入：nums = [1]
输出：[[1]]

```


 **提示：** 
-  `1 <= nums.length <= 6` 
-  `-10 <= nums[i] <= 10` 
-  `nums` 中的所有整数 **互不相同** 

**标签**
`数组` `回溯` 

## solution

```python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        def dfs(nums, used, path):
            if len(nums) == len(path):
                res.append(path[:])
                return
            for i in range(len(nums)):
                if not used[i]:
                    used[i] = 1
                    dfs(nums, used, path + [nums[i]])
                    used[i] = 0
        
        res = []
        dfs(nums, [0] * len(nums), [])
        return res
```
>
# 47.全排列 II
[https://leetcode-cn.com/problems/permutations-ii](https://leetcode-cn.com/problems/permutations-ii) 
## 原题
给定一个可包含重复数字的序列 `nums` ， ***按任意顺序*** 返回所有不重复的全排列。

 

 **示例 1：** 

```

输入：nums = [1,1,2]
输出：
[[1,1,2],
 [1,2,1],
 [2,1,1]]

```
 **示例 2：** 

```

输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]

```


 **提示：** 
-  `1 <= nums.length <= 8` 
-  `-10 <= nums[i] <= 10` 

**标签**
`数组` `回溯` 

## solution

```python
class Solution:
    def permuteUnique(self, nums: List[int]) -> List[List[int]]:
        def dfs(nums, used, path):
            if len(nums) == len(path):
                res.append(path[:])
                return
            for i in range(len(nums)):
                if not used[i]:
                    if i > 0 and nums[i] == nums[i-1] and not used[i-1]:
                        continue
                    used[i] = 1
                    dfs(nums, used, path + [nums[i]])
                    used[i] = 0
        
        nums.sort()
        res = []
        dfs(nums, [0] * len(nums), [])
        return res
```
>
# 51.N 皇后
[https://leetcode-cn.com/problems/n-queens](https://leetcode-cn.com/problems/n-queens) 
## 原题
 **n 皇后问题** 研究的是如何将 `n` 个皇后放置在 `n×n` 的棋盘上，并且使皇后彼此之间不能相互攻击。

给你一个整数 `n` ，返回所有不同的 **n皇后问题** 的解决方案。
每一种解法包含一个不同的 **n 皇后问题** 的棋子放置方案，该方案中 `'Q'` 和 `'.'` 分别代表了皇后和空位。

 

 **示例 1：** 
<img alt="" src="https://assets.leetcode.com/uploads/2020/11/13/queens.jpg" style="width: 600px; height: 268px;" />
```

输入：n = 4
输出：[[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
解释：如上图所示，4 皇后问题存在两个不同的解法。

```
 **示例 2：** 

```

输入：n = 1
输出：[["Q"]]

```


 **提示：** 
-  `1 <= n <= 9` 

**标签**
`数组` `回溯` 

## solution

```python
class Solution:
    def solveNQueens(self, n: int) -> List[List[str]]:
        # def dfs(path, path_idx, p):
        def dfs(path, path_idx):
            if len(path) == n:
                res.append(path[:])
                return

            can_use = [True] * n
            # 计算前面行选定的皇后，对当前行的限制
            for i, j in enumerate(path_idx):
                can_use[j] = False
                step = len(path) - i
                if 0 <= j-step < n:
                    can_use[j-step] = False
                if 0 <= j+step < n:
                    can_use[j+step] = False

            for i in range(n):
                if not can_use[i]:
                    continue
                tmp = "." * i + "Q" + "." * (n - i - 1)
                # print(p, len(path), i, path+[tmp], can_use)
                # dfs(path + [tmp], path_idx + [i], p+"-")
                dfs(path + [tmp], path_idx + [i])

        res = []
        # dfs([], [], "-")
        dfs([], [])
        return res
```
>
# 52.N皇后 II
[https://leetcode-cn.com/problems/n-queens-ii](https://leetcode-cn.com/problems/n-queens-ii) 
## 原题
 **n 皇后问题** 研究的是如何将 `n` 个皇后放置在 `n × n` 的棋盘上，并且使皇后彼此之间不能相互攻击。

给你一个整数 `n` ，返回 **n 皇后问题** 不同的解决方案的数量。


 **示例 1：** 
<img alt="" src="https://assets.leetcode.com/uploads/2020/11/13/queens.jpg" style="width: 600px; height: 268px;" />
```

输入：n = 4
输出：2
解释：如上图所示，4 皇后问题存在两个不同的解法。

```
 **示例 2：** 

```

输入：n = 1
输出：1

```


 **提示：** 
-  `1 <= n <= 9` 

**标签**
`回溯` 

## solution

```python
class Solution:
    def totalNQueens(self, n: int) -> int:
        def dfs(path, path_idx):
            if len(path) == n:
                self.count += 1
                return

            can_use = [True] * n
            # 计算前面行选定的皇后，对当前行的限制
            for i, j in enumerate(path_idx):
                can_use[j] = False
                step = len(path) - i
                if 0 <= j-step < n:
                    can_use[j-step] = False
                if 0 <= j+step < n:
                    can_use[j+step] = False

            for i in range(n):
                if not can_use[i]:
                    continue
                tmp = "." * i + "Q" + "." * (n - i - 1)
                # print(p, len(path), i, path+[tmp], can_use)
                dfs(path + [tmp], path_idx + [i])

        self.count = 0
        dfs([], [])
        return self.count
```
>
# 77.组合
[https://leetcode-cn.com/problems/combinations](https://leetcode-cn.com/problems/combinations) 
## 原题
给定两个整数 `n` 和 `k` ，返回范围 `[1, n]` 中所有可能的 `k` 个数的组合。

你可以按 **任何顺序** 返回答案。

 

 **示例 1：** 

```

输入：n = 4, k = 2
输出：
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```
 **示例 2：** 

```

输入：n = 1, k = 1
输出：[[1]]
```


 **提示：** 
-  `1 <= n <= 20` 
-  `1 <= k <= n` 

**标签**
`数组` `回溯` 

## solution

```python
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        def dfs(n, k, path, start):
            if len(path) == k:
                res.append(path[:])
            for i in range(start, n+1):
                dfs(n, k, path + [i], i+1)
        
        res = []
        dfs(n, k, [], 1)
        return res
```
>
# 78.子集
[https://leetcode-cn.com/problems/subsets](https://leetcode-cn.com/problems/subsets) 
## 原题
给你一个整数数组  `nums` ，数组中的元素 **互不相同** 。返回该数组所有可能的子集（幂集）。

解集 **不能** 包含重复的子集。你可以按 **任意顺序** 返回解集。

 

 **示例 1：** 

```

输入：nums = [1,2,3]
输出：[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]

```
 **示例 2：** 

```

输入：nums = [0]
输出：[[],[0]]

```


 **提示：** 
-  `1 <= nums.length <= 10` 
-  `-10 <= nums[i] <= 10` 
-  `nums` 中的所有元素 **互不相同** 

**标签**
`位运算` `数组` `回溯` 

## solution

```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        def dfs(nums, path, start):
            res.append(path[:])
            for i in range(start, len(nums)):
                path.append(nums[i])
                dfs(nums, path, i+1)
                path.pop()
        res = []
        dfs(nums, [], 0)
        return res
```
>
# 79.单词搜索
[https://leetcode-cn.com/problems/word-search](https://leetcode-cn.com/problems/word-search) 
## 原题
给定一个  `m x n` 二维字符网格  `board` 和一个字符串单词  `word` 。如果  `word` 存在于网格中，返回 `true` ；否则，返回 `false` 。

单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。

 

 **示例 1：** 
<img alt="" src="https://assets.leetcode.com/uploads/2020/11/04/word2.jpg" style="width: 322px; height: 242px;" />
```

输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
输出：true

```
 **示例 2：** 
<img alt="" src="https://assets.leetcode.com/uploads/2020/11/04/word-1.jpg" style="width: 322px; height: 242px;" />
```

输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "SEE"
输出：true

```
 **示例 3：** 
<img alt="" src="https://assets.leetcode.com/uploads/2020/10/15/word3.jpg" style="width: 322px; height: 242px;" />
```

输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCB"
输出：false

```


 **提示：** 
-  `m == board.length` 
-  `n = board[i].length` 
-  `1 <= m, n <= 6` 
-  `1 <= word.length <= 15` 
-  `board` 和 `word` 仅由大小写英文字母组成


 **进阶：** 你可以使用搜索剪枝的技术来优化解决方案，使其在 `board` 更大的情况下可以更快解决问题？


**标签**
`数组` `回溯` `矩阵` 

## solution

```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        if not board:
            return False

        def dfs(i, j, idx, visited):
            if idx == len(word) - 1:
                self.res = True
                return
            visited[i][j] = True
            for di, dj in directions:
                x, y = i + di, j + dj
                if 0 <= x < m and 0 <= y < n and board[x][y] == word[idx + 1] and not visited[x][y]:
                    dfs(x, y, idx + 1, visited)
            visited[i][j] = False

        m, n = len(board), len(board[0])
        directions = [(-1, 0), (1, 0), (0, 1), (0, -1)]
        visited = [[False] * n for _ in range(m)]
        self.res = False
        for i in range(m):
            for j in range(n):
                if board[i][j] == word[0]:
                    dfs(i, j, 0, visited)
                    if self.res:
                        break
        return self.res
```
>
# 89.格雷编码
[https://leetcode-cn.com/problems/gray-code](https://leetcode-cn.com/problems/gray-code) 
## 原题
 **n 位格雷码序列** 是一个由 `2^n` 个整数组成的序列，其中：

- 每个整数都在范围 `[0, 2^n - 1]` 内（含 `0` 和 `2^n - 1` ）
- 第一个整数是 `0` 
- 一个整数在序列中出现 **不超过一次** 
- 每对 **相邻** 整数的二进制表示 **恰好一位不同** ，且
-  **第一个** 和 **最后一个** 整数的二进制表示 **恰好一位不同** 
给你一个整数 `n` ，返回任一有效的 **n 位格雷码序列** 。

 

 **示例 1：** 

```

输入：n = 2
输出：[0,1,3,2]
解释：
[0,1,3,2] 的二进制表示是 [00,01,11,10] 。
- 00 和 01 有一位不同
- 01 和 11 有一位不同
- 11 和 10 有一位不同
- 10 和 00 有一位不同
[0,2,3,1] 也是一个有效的格雷码序列，其二进制表示是 [00,10,11,01] 。
- 00 和 10 有一位不同
- 10 和 11 有一位不同
- 11 和 01 有一位不同
- 01 和 00 有一位不同

```
 **示例 2：** 

```

输入：n = 1
输出：[0,1]

```


 **提示：** 
-  `1 <= n <= 16` 

**标签**
`位运算` `数学` `回溯` 

## solution1 回溯

- 对上一个元素`res[-1]`，任意改变它的一位数字，改变第`k`位数字的方式是通过`res[-1]`和`1 << i`异或得到，如果未被使用过，则这个异或结果可以作为下一个元素
- 补充一个python的知识，global，nonlocal
  - 全局变量`x`想在函数体内使用，在函数体内加一个`global x`
  - 非全局变量`y`想在函数体内使用，在函数体内加一个`nonlocal y`，本题`res`是定义在类成员函数中的非全局变量，因此想在函数`dfs`内使用，需要加`nonlocal`

```python
class Solution:
    def grayCode(self, n: int) -> List[int]:
        def dfs(n):
            nonlocal res, used
            if len(res) == (1 << n):
                return True
            for i in range(n):
                cur = res[-1] ^ (1 << i)
                if cur in used:
                    continue
                used.add(cur)
                res.append(cur)
                if dfs(n):
                    return True
                res.pop()
                used.remove(cur)

        res = [0]
        used = set(res)
        dfs(n)
        return res
```

## solution2 递推规律

[Gray Code （镜像反射法，图解） - 格雷编码 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/gray-code/solution/gray-code-jing-xiang-fan-she-fa-by-jyd/)

从`G(n)`到`G(n+1)`，将`G(n)`每个元素前面加上0，合并上`G(n)`反转后每个元素前面加上1

![img](https://pic.leetcode-cn.com/28acf6d5b1fae0fb2dddbedd7ac92ffeee8902cd28233bdfb08b52af411a9bb2-Picture4.png)

```python
class Solution:
    def grayCode(self, n: int) -> List[int]:
        res, head = [0], 1
        for i in range(n):
            length = len(res)
            for j in range(length-1, -1, -1):
                res.append(head + res[j])
            head <<= 1
        return res
```

## solution3 二进制码生成格雷码

$$
\begin{align*}
(n)_{2} &=\underbrace{\ldots}_{记为x} 0 \underbrace{11 \ldots 11}_{k个}
\end{align*}
$$

$$
\begin{align*}
(n+1)_{2} &=\underbrace{\ldots}_{记为x} 1 \underbrace{00 \ldots 00}_{k个} \\
\end{align*}
$$

$$
G(n)=n \oplus\left\lfloor\frac{n}{2}\right\rfloor \\
G(n+1)=(n+1) \oplus\left\lfloor\frac{n+1}{2}\right\rfloor
$$

- $(n)_{2}$和$(n+1)_{2}$是二进制码，`G(n)`和`G(n+1)`是对应的格雷码
- `n` 和 `n+1` 的区别，把 `n` 加 1 相当于把 `n` 末位连接的 1 全部取反，然后最低位 0 变成 1
- 证明`G(n)`和`G(n+1)`之间只有一位不同

```
# 为了方便说明，假设高位的x为三位的0/1，以abc表示。将n拆成三部分看，高位的abc，中间的第k+1位为0，后面的k位都是1
abc  0  11...11 —— n
0ab  c  01...11 -- n>>1
xyz 0^c 10...00 -- G(n)

abc  1  00...00 -- n+1
0ab  c  10...00 -- (n+1)>>1
xyz 1^c 10...00 -- G(n+1)
```

因此`G(n)`和`G(n+1)`之间只有第`k+1`位不同。

```python
class Solution:
    def grayCode(self, n: int) -> List[int]:
        res = []
        for i in range(1 << n):
            res.append(i ^ (i >> 1))
        return res
```
>
# 90.子集 II
[https://leetcode-cn.com/problems/subsets-ii](https://leetcode-cn.com/problems/subsets-ii) 
## 原题
给你一个整数数组 `nums` ，其中可能包含重复元素，请你返回该数组所有可能的子集（幂集）。

解集 **不能** 包含重复的子集。返回的解集中，子集可以按 **任意顺序** 排列。


 **示例 1：** 

```

输入：nums = [1,2,2]
输出：[[],[1],[1,2],[1,2,2],[2],[2,2]]

```
 **示例 2：** 

```

输入：nums = [0]
输出：[[],[0]]

```


 **提示：** 
-  `1 <= nums.length <= 10` 
-  `-10 <= nums[i] <= 10` 

**标签**
`位运算` `数组` `回溯` 

## solution

```python
class Solution:
    def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
        def dfs(nums, path, start):
            res.append(path[:])
            for i in range(start, len(nums)):
                if i > start and nums[i] == nums[i-1]:
                    continue
                path.append(nums[i])
                dfs(nums, path, i+1)
                path.pop()
        
        nums.sort()
        res = []
        dfs(nums, [], 0)
        return res
```
>
# 93.复原 IP 地址
[https://leetcode-cn.com/problems/restore-ip-addresses](https://leetcode-cn.com/problems/restore-ip-addresses) 
## 原题
 **有效 IP 地址** 正好由四个整数（每个整数位于 `0` 到 `255` 之间组成，且不能含有前导 `0` ），整数之间用 `'.'` 分隔。
- 例如： `"0.1.2.201"` 和 `"192.168.1.1"` 是 **有效** IP 地址，但是 `"0.011.255.245"` 、 `"192.168.1.312"` 和 `"192.168@1.1"` 是 **无效** IP 地址。
给定一个只包含数字的字符串 `s` ，用以表示一个 IP 地址，返回所有可能的 **有效 IP 地址** ，这些地址可以通过在 `s` 中插入 `'.'` 来形成。你 **不能** 重新排序或删除 `s` 中的任何数字。你可以按 **任何** 顺序返回答案。

 

 **示例 1：** 

```

输入：s = "25525511135"
输出：["255.255.11.135","255.255.111.35"]

```
 **示例 2：** 

```

输入：s = "0000"
输出：["0.0.0.0"]

```
 **示例 3：** 

```

输入：s = "101023"
输出：["1.0.10.23","1.0.102.3","10.1.0.23","10.10.2.3","101.0.2.3"]

```


 **提示：** 
-  `0 <= s.length <= 20` 
-  `s` 仅由数字组成

**标签**
`字符串` `回溯` 

## solution1 拿到回溯结果时判断解是否valid

```python
class Solution:
def restoreIpAddresses1(self, s: str) -> List[str]:
        def dfs(s, path, start):
            if len(path) == 4 and start == len(s) and isValid(path):
                res.append(".".join(path[:]))
                return
            for i in range(start, min(start+3, len(s))):
                dfs(s, path+[s[start:i+1]], i+1)

        def isValid(path):
            for p in path:
                if p[0] == "0" and len(p) > 1:
                    return False
                if int(p) > 255:
                    return False
            return True

        res = []
        dfs(s, [], 0)
        return res
```



## solution2 在回溯过程中剪枝

```python
class Solution:
    # 在回溯过程中剪枝，而不是最后判断valid
    def restoreIpAddresses(self, s: str) -> List[str]:
        def dfs(s, path, start):
            if len(path) == 4 and start == len(s):
                res.append(".".join(path[:]))
                return
            
            # 四段出来了字符串没用完，或者 字符串用完了四段没出来
            if len(path) == 4 or start == len(s):
                return
            
            if s[start] == "0":
                dfs(s, path+[s[start]], start+1)
            else:
                for i in range(start, min(start+3, len(s))):
                    cur = s[start:i+1]
                    if 0 <= int(cur) <= 255:
                        dfs(s, path+[cur], i+1)

        res = []
        dfs(s, [], 0)
        return res
```
>
# 95.不同的二叉搜索树 II
[https://leetcode-cn.com/problems/unique-binary-search-trees-ii](https://leetcode-cn.com/problems/unique-binary-search-trees-ii) 
## 原题
给你一个整数 `n` ，请你生成并返回所有由 `n` 个节点组成且节点值从 `1` 到 `n` 互不相同的不同 **二叉搜索树** 。可以按 **任意顺序** 返回答案。


 **示例 1：** 
<img alt="" src="https://assets.leetcode.com/uploads/2021/01/18/uniquebstn3.jpg" style="width: 600px; height: 148px;" />
```

输入：n = 3
输出：[[1,null,2,null,3],[1,null,3,2],[2,1,3],[3,1,null,null,2],[3,2,null,1]]

```
 **示例 2：** 

```

输入：n = 1
输出：[[1]]

```


 **提示：** 
-  `1 <= n <= 8` 

**标签**
`树` `二叉搜索树` `动态规划` `回溯` `二叉树` 


##
```python

```
>
# 113.路径总和 II
[https://leetcode-cn.com/problems/path-sum-ii](https://leetcode-cn.com/problems/path-sum-ii) 
## 原题
给你二叉树的根节点 `root` 和一个整数目标和 `targetSum` ，找出所有 **从根节点到叶子节点** 路径总和等于给定目标和的路径。

 **叶子节点** 是指没有子节点的节点。


 **示例 1：** 
<img alt="" src="https://assets.leetcode.com/uploads/2021/01/18/pathsumii1.jpg" style="width: 500px; height: 356px;" />
```

输入：root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
输出：[[5,4,11,2],[5,8,4,5]]

```
 **示例 2：** 
<img alt="" src="https://assets.leetcode.com/uploads/2021/01/18/pathsum2.jpg" style="width: 212px; height: 181px;" />
```

输入：root = [1,2,3], targetSum = 5
输出：[]

```
 **示例 3：** 

```

输入：root = [1,2], targetSum = 0
输出：[]

```


 **提示：** 
- 树中节点总数在范围 `[0, 5000]` 内
-  `-1000 <= Node.val <= 1000` 
-  `-1000 <= targetSum <= 1000` 

**标签**
`树` `深度优先搜索` `回溯` `二叉树` 


##
```python

```
>
# 126.单词接龙 II
[https://leetcode-cn.com/problems/word-ladder-ii](https://leetcode-cn.com/problems/word-ladder-ii) 
## 原题
按字典 `wordList` 完成从单词 `beginWord` 到单词 `endWord` 转化，一个表示此过程的 **转换序列** 是形式上像 `beginWord -> s<sub>1</sub> -> s<sub>2</sub> -> ... -> s<sub>k</sub>` 这样的单词序列，并满足：
- 每对相邻的单词之间仅有单个字母不同。
- 转换过程中的每个单词 `s<sub>i</sub>` （ `1 <= i <= k` ）必须是字典 `wordList` 中的单词。注意， `beginWord` 不必是字典 `wordList` 中的单词。
-  `s<sub>k</sub> == endWord` 
给你两个单词 `beginWord` 和 `endWord` ，以及一个字典 `wordList` 。请你找出并返回所有从 `beginWord` 到 `endWord` 的 **最短转换序列** ，如果不存在这样的转换序列，返回一个空列表。每个序列都应该以单词列表 `[beginWord, s<sub>1</sub>, s<sub>2</sub>, ..., s<sub>k</sub>]` 的形式返回。

 

 **示例 1：** 

```

输入：beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
输出：[["hit","hot","dot","dog","cog"],["hit","hot","lot","log","cog"]]
解释：存在 2 种最短的转换序列：
"hit" -> "hot" -> "dot" -> "dog" -> "cog"
"hit" -> "hot" -> "lot" -> "log" -> "cog"

```
 **示例 2：** 

```

输入：beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]
输出：[]
解释：endWord "cog" 不在字典 wordList 中，所以不存在符合要求的转换序列。

```


 **提示：** 
-  `1 <= beginWord.length <= 5` 
-  `endWord.length == beginWord.length` 
-  `1 <= wordList.length <= 5000` 
-  `wordList[i].length == beginWord.length` 
-  `beginWord` 、 `endWord` 和 `wordList[i]` 由小写英文字母组成
-  `beginWord != endWord` 
-  `wordList` 中的所有单词 **互不相同** 

**标签**
`广度优先搜索` `哈希表` `字符串` `回溯` 


##
```python

```
>
# 131.分割回文串
[https://leetcode-cn.com/problems/palindrome-partitioning](https://leetcode-cn.com/problems/palindrome-partitioning) 
## 原题
给你一个字符串 `s` ，请你将 `s` 分割成一些子串，使每个子串都是 **回文串** 。返回 `s` 所有可能的分割方案。

 **回文串** 是正着读和反着读都一样的字符串。

 

 **示例 1：** 

```

输入：s = "aab"
输出：[["a","a","b"],["aa","b"]]

```
 **示例 2：** 

```

输入：s = "a"
输出：[["a"]]

```


 **提示：** 
-  `1 <= s.length <= 16` 
-  `s` 仅由小写英文字母组成

**标签**
`字符串` `动态规划` `回溯` 


##
```python

```
>
# 140.单词拆分 II
[https://leetcode-cn.com/problems/word-break-ii](https://leetcode-cn.com/problems/word-break-ii) 
## 原题
给定一个字符串 `s` 和一个字符串字典<meta charset="UTF-8" /> `wordDict` ，在字符串<meta charset="UTF-8" /> `s` 中增加空格来构建一个句子，使得句子中所有的单词都在词典中。 **以任意顺序** 返回所有这些可能的句子。

 **注意：** 词典中的同一个单词可能在分段中被重复使用多次。

 

 **示例 1：** 

```

输入:s = "catsanddog", wordDict = ["cat","cats","and","sand","dog"]
输出:["cats and dog","cat sand dog"]

```
 **示例 2：** 

```

输入:s = "pineapplepenapple", wordDict = ["apple","pen","applepen","pine","pineapple"]
输出:["pine apple pen apple","pineapple pen apple","pine applepen apple"]
解释: 注意你可以重复使用字典中的单词。

```
 **示例 3：** 

```

输入:s = "catsandog", wordDict = ["cats","dog","sand","and","cat"]
输出:[]

```


 **提示：** 

<meta charset="UTF-8" />
-  `1 <= s.length <= 20` 
-  `1 <= wordDict.length <= 1000` 
-  `1 <= wordDict[i].length <= 10` 
-  `s` 和 `wordDict[i]` 仅有小写英文字母组成
-  `wordDict` 中所有字符串都 **不同** 

**标签**
`字典树` `记忆化搜索` `哈希表` `字符串` `动态规划` `回溯` 


##
```python

```
>
# 212.单词搜索 II
[https://leetcode-cn.com/problems/word-search-ii](https://leetcode-cn.com/problems/word-search-ii) 
## 原题
给定一个 `m x n` 二维字符网格 `board` **** 和一个单词（字符串）列表 `words` ， *返回所有二维网格上的单词* 。

单词必须按照字母顺序，通过 **相邻的单元格** 内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母在一个单词中不允许被重复使用。

 

 **示例 1：** 
<img alt="" src="https://assets.leetcode.com/uploads/2020/11/07/search1.jpg" />
```

输入：board = [["o","a","a","n"],["e","t","a","e"],["i","h","k","r"],["i","f","l","v"]], words = ["oath","pea","eat","rain"]
输出：["eat","oath"]

```
 **示例 2：** 
<img alt="" src="https://assets.leetcode.com/uploads/2020/11/07/search2.jpg" />
```

输入：board = [["a","b"],["c","d"]], words = ["abcb"]
输出：[]

```


 **提示：** 
-  `m == board.length` 
-  `n == board[i].length` 
-  `1 <= m, n <= 12` 
-  `board[i][j]` 是一个小写英文字母
-  `1 <= words.length <= 3 * 10^4` 
-  `1 <= words[i].length <= 10` 
-  `words[i]` 由小写英文字母组成
-  `words` 中的所有字符串互不相同

**标签**
`字典树` `数组` `字符串` `回溯` `矩阵` 


##
```python

```
>
# 216.组合总和 III
[https://leetcode-cn.com/problems/combination-sum-iii](https://leetcode-cn.com/problems/combination-sum-iii) 
## 原题
找出所有相加之和为 ***n*** 的 ** *k* ** 个数的组合 ** *。* ** 组合中只允许含有 1 - 9 的正整数，并且每种组合中不存在重复的数字。

 **说明：** 
- 所有数字都是正整数。
- 解集不能包含重复的组合。 
 **示例 1:** 

```
输入: k = 3, n = 7
输出: [[1,2,4]]

```
 **示例 2:** 

```
输入: k = 3, n = 9
输出: [[1,2,6], [1,3,5], [2,3,4]]

```

**标签**
`数组` `回溯` 

## solution

```python
class Solution:
    def combinationSum3(self, k: int, n: int) -> List[List[int]]:
        def dfs(nums, k, n, path, sum_, start):
            if len(path) == k and sum_ == n:
                res.append(path[:])
            if sum_ < n and len(path) < k:
                for i in range(start, len(nums)):
                    dfs(nums, k, n, path + [nums[i]], sum_ + nums[i], i+1)

        res = []
        nums = list(range(1, 10))
        dfs(nums, k, n, [], 0, 0)
        return res

```
>
# 254.因子的组合
[https://leetcode-cn.com/problems/factor-combinations](https://leetcode-cn.com/problems/factor-combinations) 
## 原题


**标签**
`数组` `回溯` 


##
```python

```
>
# 257.二叉树的所有路径
[https://leetcode-cn.com/problems/binary-tree-paths](https://leetcode-cn.com/problems/binary-tree-paths) 
## 原题
给你一个二叉树的根节点 `root` ，按 **任意顺序** ，返回所有从根节点到叶子节点的路径。

 **叶子节点** 是指没有子节点的节点。


 **示例 1：** 
<img alt="" src="https://assets.leetcode.com/uploads/2021/03/12/paths-tree.jpg" style="width: 207px; height: 293px;" />
```

输入：root = [1,2,3,null,5]
输出：["1->2->5","1->3"]

```
 **示例 2：** 

```

输入：root = [1]
输出：["1"]

```


 **提示：** 
- 树中节点的数目在范围 `[1, 100]` 内
-  `-100 <= Node.val <= 100` 

**标签**
`树` `深度优先搜索` `字符串` `回溯` `二叉树` 


##
```python

```
>
# 267.回文排列 II
[https://leetcode-cn.com/problems/palindrome-permutation-ii](https://leetcode-cn.com/problems/palindrome-permutation-ii) 
## 原题


**标签**
`哈希表` `字符串` `回溯` 


##
```python

```
>
# 282.给表达式添加运算符
[https://leetcode-cn.com/problems/expression-add-operators](https://leetcode-cn.com/problems/expression-add-operators) 
## 原题
给定一个仅包含数字 `0-9` 的字符串 `num` 和一个目标值整数 `target` ，在 `num` 的数字之间添加 **二元** 运算符（不是一元） `+` 、 `-` 或 `*` ，返回所有能够得到目标值的表达式。

 

 **示例 1:** 

```

输入: num = "123", target = 6
输出: ["1+2+3", "1*2*3"] 

```
 **示例 2:** 

```

输入: num = "232", target = 8
输出: ["2*3+2", "2+3*2"]
```
 **示例 3:** 

```

输入: num = "105", target = 5
输出: ["1*0+5","10-5"]
```
 **示例 4:** 

```

输入: num = "00", target = 0
输出: ["0+0", "0-0", "0*0"]

```
 **示例 5:** 

```

输入: num = "3456237490", target = 9191
输出: []
```


 **提示：** 
-  `1 <= num.length <= 10` 
-  `num` 仅含数字
-  `-2^31 <= target <= 2^31 - 1` 

**标签**
`数学` `字符串` `回溯` 


##
```python

```
>
# 291.单词规律 II
[https://leetcode-cn.com/problems/word-pattern-ii](https://leetcode-cn.com/problems/word-pattern-ii) 
## 原题


**标签**
`哈希表` `字符串` `回溯` 


##
```python

```
>
# 294.翻转游戏 II
[https://leetcode-cn.com/problems/flip-game-ii](https://leetcode-cn.com/problems/flip-game-ii) 
## 原题


**标签**
`记忆化搜索` `数学` `动态规划` `回溯` `博弈` 


##
```python

```
>
# 301.删除无效的括号
[https://leetcode-cn.com/problems/remove-invalid-parentheses](https://leetcode-cn.com/problems/remove-invalid-parentheses) 
## 原题
给你一个由若干括号和字母组成的字符串 `s` ，删除最小数量的无效括号，使得输入的字符串有效。

返回所有可能的结果。答案可以按 **任意顺序** 返回。

 

 **示例 1：** 

```

输入：s = "()())()"
输出：["(())()","()()()"]

```
 **示例 2：** 

```

输入：s = "(a)())()"
输出：["(a())()","(a)()()"]

```
 **示例 3：** 

```

输入：s = ")("
输出：[""]

```


 **提示：** 
-  `1 <= s.length <= 25` 
-  `s` 由小写英文字母以及括号 `'('` 和 `')'` 组成
-  `s` 中至多含 `20` 个括号

**标签**
`广度优先搜索` `字符串` `回溯` 


##
```python

```
>
# 306.累加数
[https://leetcode-cn.com/problems/additive-number](https://leetcode-cn.com/problems/additive-number) 
## 原题
 **累加数** 是一个字符串，组成它的数字可以形成累加序列。

一个有效的 **累加序列** 必须 **至少** 包含 3 个数。除了最开始的两个数以外，序列中的每个后续数字必须是它之前两个数字之和。

给你一个只包含数字 `'0'-'9'` 的字符串，编写一个算法来判断给定输入是否是 **累加数** 。如果是，返回 `true` ；否则，返回 `false` 。

 **说明：** 累加序列里的数，除数字 0 之外， **不会** 以 0 开头，所以不会出现 `1, 2, 03` 或者 `1, 02, 3` 的情况。

 

 **示例 1：** 

```

输入："112358"
输出：true 
解释：累加序列为: 1, 1, 2, 3, 5, 8 。1 + 1 = 2, 1 + 2 = 3, 2 + 3 = 5, 3 + 5 = 8

```
 **示例 2：** 

```

输入："199100199"
输出：true 
解释：累加序列为: 1, 99, 100, 199。1 + 99 = 100, 99 + 100 = 199
```


 **提示：** 
-  `1 <= num.length <= 35` 
-  `num` 仅由数字（ `0` - `9` ）组成


 **进阶：** 你计划如何处理由过大的整数输入导致的溢出?


**标签**
`字符串` `回溯` 


##
```python

```
>
# 320.列举单词的全部缩写
[https://leetcode-cn.com/problems/generalized-abbreviation](https://leetcode-cn.com/problems/generalized-abbreviation) 
## 原题


**标签**
`位运算` `字符串` `回溯` 


##
```python

```
>
# 351.安卓系统手势解锁
[https://leetcode-cn.com/problems/android-unlock-patterns](https://leetcode-cn.com/problems/android-unlock-patterns) 
## 原题


**标签**
`动态规划` `回溯` 


##
```python

```
>
# 357.计算各个位数不同的数字个数
[https://leetcode-cn.com/problems/count-numbers-with-unique-digits](https://leetcode-cn.com/problems/count-numbers-with-unique-digits) 
## 原题
给定一个 **非负** 整数 n，计算各位数字都不同的数字 x 的个数，其中 0 ≤ x < 10^n 。

 **示例:** 

```
输入: 2
输出: 91 
解释: 答案应为除去 11,22,33,44,55,66,77,88,99 外，在 [0,100) 区间内的所有数字。

```

**标签**
`数学` `动态规划` `回溯` 


##
```python

```
>
# 401.二进制手表
[https://leetcode-cn.com/problems/binary-watch](https://leetcode-cn.com/problems/binary-watch) 
## 原题
二进制手表顶部有 4 个 LED 代表 **小时（0-11）** ，底部的 6 个 LED 代表 **分钟（0-59）** 。每个 LED 代表一个 0 或 1，最低位在右侧。
- 例如，下面的二进制手表读取 `"3:25"` 。
<img src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/03/29/binary_clock_samui_moon.jpg" style="height: 300px; width" />

<small> *（图源：<a href="https://commons.m.wikimedia.org/wiki/File:Binary_clock_samui_moon.jpg">WikiMedia - Binary clock samui moon.jpg</a> ，许可协议：<a href="https://creativecommons.org/licenses/by-sa/3.0/deed.en">Attribution-ShareAlike 3.0 Unported (CC BY-SA 3.0)</a> ）* </small>

给你一个整数 `turnedOn` ，表示当前亮着的 LED 的数量，返回二进制手表可以表示的所有可能时间。你可以 **按任意顺序** 返回答案。

小时不会以零开头：
- 例如， `"01:00"` 是无效的时间，正确的写法应该是 `"1:00"` 。
分钟必须由两位数组成，可能会以零开头：
- 例如， `"10:2"` 是无效的时间，正确的写法应该是 `"10:02"` 。


 **示例 1：** 

```

输入：turnedOn = 1
输出：["0:01","0:02","0:04","0:08","0:16","0:32","1:00","2:00","4:00","8:00"]

```
 **示例 2：** 

```

输入：turnedOn = 9
输出：[]

```


 **提示：** 
-  `0 <= turnedOn <= 10` 

**标签**
`位运算` `回溯` 


##
```python

```
>
# 411.最短独占单词缩写
[https://leetcode-cn.com/problems/minimum-unique-word-abbreviation](https://leetcode-cn.com/problems/minimum-unique-word-abbreviation) 
## 原题


**标签**
`位运算` `字符串` `回溯` 


##
```python

```
>
# 425.单词方块
[https://leetcode-cn.com/problems/word-squares](https://leetcode-cn.com/problems/word-squares) 
## 原题


**标签**
`字典树` `数组` `字符串` `回溯` 


##
```python

```
>
# 465.最优账单平衡
[https://leetcode-cn.com/problems/optimal-account-balancing](https://leetcode-cn.com/problems/optimal-account-balancing) 
## 原题


**标签**
`数组` `回溯` 


##
```python

```
>
# 473.火柴拼正方形
[https://leetcode-cn.com/problems/matchsticks-to-square](https://leetcode-cn.com/problems/matchsticks-to-square) 
## 原题
你将得到一个整数数组 `matchsticks` ，其中 `matchsticks[i]` 是第 `i` 个火柴棒的长度。你要用 **所有的火柴棍** 拼成一个正方形。你 **不能折断** 任何一根火柴棒，但你可以把它们连在一起，而且每根火柴棒必须 **使用一次** 。

如果你能使这个正方形，则返回 `true` ，否则返回 `false` 。

 

 **示例 1:** 

<img src="https://assets.leetcode.com/uploads/2021/04/09/matchsticks1-grid.jpg" />

```

输入: matchsticks = [1,1,2,2,2]
输出: true
解释: 能拼成一个边长为2的正方形，每边两根火柴。

```
 **示例 2:** 

```

输入: matchsticks = [3,3,3,3,4]
输出: false
解释: 不能用所有火柴拼成一个正方形。

```


 **提示:** 
-  `1 <= matchsticks.length <= 15` 
-  `1 <= matchsticks[i] <= 10^8` 

**标签**
`位运算` `数组` `动态规划` `回溯` `状态压缩` 


##
```python

```
>
# 489.扫地机器人
[https://leetcode-cn.com/problems/robot-room-cleaner](https://leetcode-cn.com/problems/robot-room-cleaner) 
## 原题


**标签**
`回溯` `交互` 


##
```python

```
>
# 491.递增子序列
[https://leetcode-cn.com/problems/increasing-subsequences](https://leetcode-cn.com/problems/increasing-subsequences) 
## 原题
给你一个整数数组 `nums` ，找出并返回所有该数组中不同的递增子序列，递增子序列中 **至少有两个元素** 。你可以按 **任意顺序** 返回答案。

数组中可能含有重复元素，如出现两个整数相等，也可以视作递增序列的一种特殊情况。

 

 **示例 1：** 

```

输入：nums = [4,6,7,7]
输出：[[4,6],[4,6,7],[4,6,7,7],[4,7],[4,7,7],[6,7],[6,7,7],[7,7]]

```
 **示例 2：** 

```

输入：nums = [4,4,3,2,1]
输出：[[4,4]]

```


 **提示：** 
-  `1 <= nums.length <= 15` 
-  `-100 <= nums[i] <= 100` 

**标签**
`位运算` `数组` `哈希表` `回溯` 


##
```python

```
>
# 494.目标和
[https://leetcode-cn.com/problems/target-sum](https://leetcode-cn.com/problems/target-sum) 
## 原题
给你一个整数数组 `nums` 和一个整数 `target` 。

向数组中的每个整数前添加  `'+'` 或 `'-'` ，然后串联起所有整数，可以构造一个 **表达式** ：
- 例如， `nums = [2, 1]` ，可以在 `2` 之前添加 `'+'` ，在 `1` 之前添加 `'-'` ，然后串联起来得到表达式 `"+2-1"` 。
返回可以通过上述方法构造的、运算结果等于 `target` 的不同 **表达式** 的数目。

 

 **示例 1：** 

```

输入：nums = [1,1,1,1,1], target = 3
输出：5
解释：一共有 5 种方法让最终目标和为 3 。
-1 + 1 + 1 + 1 + 1 = 3
+1 - 1 + 1 + 1 + 1 = 3
+1 + 1 - 1 + 1 + 1 = 3
+1 + 1 + 1 - 1 + 1 = 3
+1 + 1 + 1 + 1 - 1 = 3

```
 **示例 2：** 

```

输入：nums = [1], target = 1
输出：1

```


 **提示：** 
-  `1 <= nums.length <= 20` 
-  `0 <= nums[i] <= 1000` 
-  `0 <= sum(nums[i]) <= 1000` 
-  `-1000 <= target <= 1000` 

**标签**
`数组` `动态规划` `回溯` 


##
```python

```
>
# 526.优美的排列
[https://leetcode-cn.com/problems/beautiful-arrangement](https://leetcode-cn.com/problems/beautiful-arrangement) 
## 原题
假设有从 1 到 n 的 n 个整数。用这些整数构造一个数组 `perm` （ **下标从 1 开始** ），只要满足下述条件 **之一** ，该数组就是一个 **优美的排列** ：
-  `perm[i]` 能够被 `i` 整除
-  `i` 能够被 `perm[i]` 整除
给你一个整数 `n` ，返回可以构造的 **优美排列** 的 **数量** 。

 

 **示例 1：** 

```

输入：n = 2
输出：2
解释：
第 1 个优美的排列是 [1,2]：
    - perm[1] = 1 能被 i = 1 整除
    - perm[2] = 2 能被 i = 2 整除
第 2 个优美的排列是 [2,1]:
    - perm[1] = 2 能被 i = 1 整除
    - i = 2 能被 perm[2] = 1 整除

```
 **示例 2：** 

```

输入：n = 1
输出：1

```


 **提示：** 
-  `1 <= n <= 15` 

**标签**
`位运算` `数组` `动态规划` `回溯` `状态压缩` 


##
```python

```
>
# 638.大礼包
[https://leetcode-cn.com/problems/shopping-offers](https://leetcode-cn.com/problems/shopping-offers) 
## 原题
在 LeetCode 商店中， 有 `n` 件在售的物品。每件物品都有对应的价格。然而，也有一些大礼包，每个大礼包以优惠的价格捆绑销售一组物品。

给你一个整数数组 `price` 表示物品价格，其中 `price[i]` 是第 `i` 件物品的价格。另有一个整数数组 `needs` 表示购物清单，其中 `needs[i]` 是需要购买第 `i` 件物品的数量。

还有一个数组 `special` 表示大礼包， `special[i]` 的长度为 `n + 1` ，其中 `special[i][j]` 表示第 `i` 个大礼包中内含第 `j` 件物品的数量，且 `special[i][n]` （也就是数组中的最后一个整数）为第 `i` 个大礼包的价格。

返回 **确切** 满足购物清单所需花费的最低价格，你可以充分利用大礼包的优惠活动。你不能购买超出购物清单指定数量的物品，即使那样会降低整体价格。任意大礼包可无限次购买。

 

 **示例 1：** 

```

输入：price = [2,5], special = [[3,0,5],[1,2,10]], needs = [3,2]
输出：14
解释：有 A 和 B 两种物品，价格分别为 ¥2 和 ¥5 。 
大礼包 1 ，你可以以 ¥5 的价格购买 3A 和 0B 。 
大礼包 2 ，你可以以 ¥10 的价格购买 1A 和 2B 。 
需要购买 3 个 A 和 2 个 B ， 所以付 ¥10 购买 1A 和 2B（大礼包 2），以及 ¥4 购买 2A 。
```
 **示例 2：** 

```

输入：price = [2,3,4], special = [[1,1,0,4],[2,2,1,9]], needs = [1,2,1]
输出：11
解释：A ，B ，C 的价格分别为 ¥2 ，¥3 ，¥4 。
可以用 ¥4 购买 1A 和 1B ，也可以用 ¥9 购买 2A ，2B 和 1C 。 
需要买 1A ，2B 和 1C ，所以付 ¥4 买 1A 和 1B（大礼包 1），以及 ¥3 购买 1B ， ¥4 购买 1C 。 
不可以购买超出待购清单的物品，尽管购买大礼包 2 更加便宜。
```


 **提示：** 
-  `n == price.length` 
-  `n == needs.length` 
-  `1 <= n <= 6` 
-  `0 <= price[i] <= 10` 
-  `0 <= needs[i] <= 10` 
-  `1 <= special.length <= 100` 
-  `special[i].length == n + 1` 
-  `0 <= special[i][j] <= 50` 

**标签**
`位运算` `记忆化搜索` `数组` `动态规划` `回溯` `状态压缩` 


##
```python

```
>
# 679.24 点游戏
[https://leetcode-cn.com/problems/24-game](https://leetcode-cn.com/problems/24-game) 
## 原题
给定一个长度为4的整数数组 `cards` 。你有 `4` 张卡片，每张卡片上都包含一个范围在 `[1,9]` 的数字。您应该使用运算符 `['+', '-', '*', '/']` 和括号 `'('` 和 `')'` 将这些卡片上的数字排列成数学表达式，以获得值24。

你须遵守以下规则:
- 除法运算符 `'/'` 表示实数除法，而不是整数除法。

- 例如， `4 /(1 - 2 / 3)= 4 /(1 / 3)= 12` 。
	
- 每个运算都在两个数字之间。特别是，不能使用 `“-”` 作为一元运算符。
	
- 例如，如果 `cards =[1,1,1,1]` ，则表达式 `“-1 -1 -1 -1”` 是 **不允许** 的。
	
- 你不能把数字串在一起
	
- 例如，如果 `cards =[1,2,1,2]` ，则表达式 `“12 + 12”` 无效。
	
	

如果可以得到这样的表达式，其计算结果为 `24` ，则返回 `true` ，否则返回 `false` 。

 

 **示例 1:** 

```

输入: cards = [4, 1, 8, 7]
输出: true
解释: (8-4) * (7-1) = 24

```
 **示例 2:** 

```

输入: cards = [1, 2, 1, 2]
输出: false

```


 **提示:** 
-  `cards.length == 4` 
-  `1 <= cards[i] <= 9` 

**标签**
`数组` `数学` `回溯` 


##
```python

```
>
# 691.贴纸拼词
[https://leetcode-cn.com/problems/stickers-to-spell-word](https://leetcode-cn.com/problems/stickers-to-spell-word) 
## 原题
我们有 `n` 种不同的贴纸。每个贴纸上都有一个小写的英文单词。

您想要拼写出给定的字符串 `target` ，方法是从收集的贴纸中切割单个字母并重新排列它们。如果你愿意，你可以多次使用每个贴纸，每个贴纸的数量是无限的。

返回你需要拼出 `target` 的最小贴纸数量。如果任务不可能，则返回 `-1` 。

 **注意：** 在所有的测试用例中，所有的单词都是从 `1000` 个最常见的美国英语单词中随机选择的，并且 `target` 被选择为两个随机单词的连接。

 

 **示例 1：** 

```

输入： stickers = ["with","example","science"], target = "thehat"
输出：3
解释：
我们可以使用 2 个 "with" 贴纸，和 1 个 "example" 贴纸。
把贴纸上的字母剪下来并重新排列后，就可以形成目标 “thehat“ 了。
此外，这是形成目标字符串所需的最小贴纸数量。

```
 **示例 2:** 

```

输入：stickers = ["notice","possible"], target = "basicbasic"
输出：-1
解释：我们不能通过剪切给定贴纸的字母来形成目标“basicbasic”。
```


 **提示:** 
-  `n == stickers.length` 
-  `1 <= n <= 50` 
-  `1 <= stickers[i].length <= 10` 
-  `1 <= target <= 15` 
-  `stickers[i]` 和 `target` 由小写英文单词组成

**标签**
`位运算` `动态规划` `回溯` `状态压缩` 


##
```python

```
>
# 698.划分为k个相等的子集
[https://leetcode-cn.com/problems/partition-to-k-equal-sum-subsets](https://leetcode-cn.com/problems/partition-to-k-equal-sum-subsets) 
## 原题
给定一个整数数组 `nums` 和一个正整数 `k` ，找出是否有可能把这个数组分成 `k` 个非空子集，其总和都相等。

 **示例 1：** 

```
输入： nums = [4, 3, 2, 3, 5, 2, 1], k = 4
输出： True
说明： 有可能将其分成 4 个子集（5），（1,4），（2,3），（2,3）等于总和。
```


 **提示：** 
-  `1 <= k <= len(nums) <= 16` 
-  `0 < nums[i] < 10000` 

**标签**
`位运算` `记忆化搜索` `数组` `动态规划` `回溯` `状态压缩` 


##
```python

```
>
# 784.字母大小写全排列
[https://leetcode-cn.com/problems/letter-case-permutation](https://leetcode-cn.com/problems/letter-case-permutation) 
## 原题
给定一个字符串 `S` ，通过将字符串 `S` 中的每个字母转变大小写，我们可以获得一个新的字符串。返回所有可能得到的字符串集合。

 

```
示例：
输入：S = "a1b2"
输出：["a1b2", "a1B2", "A1b2", "A1B2"]

输入：S = "3z4"
输出：["3z4", "3Z4"]

输入：S = "12345"
输出：["12345"]

```


 **提示：** 
-  `S` 的长度不超过 `12` 。
-  `S` 仅由数字和字母组成。

**标签**
`位运算` `字符串` `回溯` 


##
```python

```
>
# 797.所有可能的路径
[https://leetcode-cn.com/problems/all-paths-from-source-to-target](https://leetcode-cn.com/problems/all-paths-from-source-to-target) 
## 原题
给你一个有 `n` 个节点的 **有向无环图（DAG）** ，请你找出所有从节点 `0` 到节点 `n-1` 的路径并输出（ **不要求按特定顺序** ）

<meta charset="UTF-8" /> `graph[i]` 是一个从节点 `i` 可以访问的所有节点的列表（即从节点 `i` 到节点 `graph[i][j]` 存在一条有向边）。

 

 **示例 1：** 

<img alt="" src="https://assets.leetcode.com/uploads/2020/09/28/all_1.jpg" />

```

输入：graph = [[1,2],[3],[3],[]]
输出：[[0,1,3],[0,2,3]]
解释：有两条路径 0 -> 1 -> 3 和 0 -> 2 -> 3

```
 **示例 2：** 

<img alt="" src="https://assets.leetcode.com/uploads/2020/09/28/all_2.jpg" />

```

输入：graph = [[4,3,1],[3,2,4],[3],[4],[]]
输出：[[0,4],[0,3,4],[0,1,3,4],[0,1,2,3,4],[0,1,4]]

```


 **提示：** 
-  `n == graph.length` 
-  `2 <= n <= 15` 
-  `0 <= graph[i][j] < n` 
-  `graph[i][j] != i` （即不存在自环）
-  `graph[i]` 中的所有元素 **互不相同** 
- 保证输入为 **有向无环图（DAG）** 



**标签**
`深度优先搜索` `广度优先搜索` `图` `回溯` 


##
```python

```
>
# 816.模糊坐标
[https://leetcode-cn.com/problems/ambiguous-coordinates](https://leetcode-cn.com/problems/ambiguous-coordinates) 
## 原题
我们有一些二维坐标，如 `"(1, 3)"` 或 `"(2, 0.5)"` ，然后我们移除所有逗号，小数点和空格，得到一个字符串 `S` 。返回所有可能的原始字符串到一个列表中。

原始的坐标表示法不会存在多余的零，所以不会出现类似于"00", "0.0", "0.00", "1.0", "001", "00.01"或一些其他更小的数来表示坐标。此外，一个小数点前至少存在一个数，所以也不会出现“.1”形式的数字。

最后返回的列表可以是任意顺序的。而且注意返回的两个数字中间（逗号之后）都有一个空格。

 

```

示例 1:
输入: "(123)"
输出: ["(1, 23)", "(12, 3)", "(1.2, 3)", "(1, 2.3)"]

```
```

示例 2:
输入: "(00011)"
输出:  ["(0.001, 1)", "(0, 0.011)"]
解释: 
0.0, 00, 0001 或 00.01 是不被允许的。

```
```

示例 3:
输入: "(0123)"
输出: ["(0, 123)", "(0, 12.3)", "(0, 1.23)", "(0.1, 23)", "(0.1, 2.3)", "(0.12, 3)"]

```
```

示例 4:
输入: "(100)"
输出: [(10, 0)]
解释: 
1.0 是不被允许的。

```


 **提示:** 
-  `4 <= S.length <= 12` .
-  `S[0]` = "(", `S[S.length - 1]` = ")", 且字符串 `S` 中的其他元素都是数字。



**标签**
`字符串` `回溯` 


##
```python

```
>
# 842.将数组拆分成斐波那契序列
[https://leetcode-cn.com/problems/split-array-into-fibonacci-sequence](https://leetcode-cn.com/problems/split-array-into-fibonacci-sequence) 
## 原题
给定一个数字字符串 `num` ，比如 `"123456579"` ，我们可以将它分成「斐波那契式」的序列 `[123, 456, 579]` 。

形式上， **斐波那契式** 序列是一个非负整数列表 `f` ，且满足：
-  `0 <= f[i] < 2^31` ，（也就是说，每个整数都符合 **32 位** 有符号整数类型）
-  `f.length >= 3` 
- 对于所有的 `0 <= i < f.length - 2` ，都有 `f[i] + f[i + 1] = f[i + 2]` 
另外，请注意，将字符串拆分成小块时，每个块的数字一定不要以零开头，除非这个块是数字 `0` 本身。

返回从 `num` 拆分出来的任意一组斐波那契式的序列块，如果不能拆分则返回 `[]` 。

 

 **示例 1：** 

```

输入：num = "1101111"
输出：[11,0,11,11]
解释：输出[110,1,111]也可以。
```
 **示例 2：** 

```

输入: num = "112358130"
输出: []
解释: 无法拆分。

```
 **示例 3：** 

```

输入："0123"
输出：[]
解释：每个块的数字不能以零开头，因此 "01"，"2"，"3" 不是有效答案。

```


 **提示：** 
-  `1 <= num.length <= 200` 
-  `num` 中只含有数字

**标签**
`字符串` `回溯` 


##
```python

```
>
# 967.连续差相同的数字
[https://leetcode-cn.com/problems/numbers-with-same-consecutive-differences](https://leetcode-cn.com/problems/numbers-with-same-consecutive-differences) 
## 原题
返回所有长度为 `n` 且满足其每两个连续位上的数字之间的差的绝对值为 `k` 的 **非负整数** 。

请注意， **除了** 数字 `0` 本身之外，答案中的每个数字都 **不能** 有前导零。例如， `01` 有一个前导零，所以是无效的；但 `0` 是有效的。

你可以按 **任何顺序** 返回答案。

 

 **示例 1：** 

```

输入：n = 3, k = 7
输出：[181,292,707,818,929]
解释：注意，070 不是一个有效的数字，因为它有前导零。

```
 **示例 2：** 

```

输入：n = 2, k = 1
输出：[10,12,21,23,32,34,43,45,54,56,65,67,76,78,87,89,98]
```
 **示例 3：** 

```

输入：n = 2, k = 0
输出：[11,22,33,44,55,66,77,88,99]

```
 **示例 4：** 

```

输入：n = 2, k = 2
输出：[13,20,24,31,35,42,46,53,57,64,68,75,79,86,97]

```


 **提示：** 
-  `2 <= n <= 9` 
-  `0 <= k <= 9` 

**标签**
`广度优先搜索` `回溯` 


##
```python

```
>
# 980.不同路径 III
[https://leetcode-cn.com/problems/unique-paths-iii](https://leetcode-cn.com/problems/unique-paths-iii) 
## 原题
在二维网格 `grid` 上，有 4 种类型的方格：
-  `1` 表示起始方格。且只有一个起始方格。
-  `2` 表示结束方格，且只有一个结束方格。
-  `0` 表示我们可以走过的空方格。
-  `-1` 表示我们无法跨越的障碍。
返回在四个方向（上、下、左、右）上行走时，从起始方格到结束方格的不同路径的数目 **。** 

 **每一个无障碍方格都要通过一次，但是一条路径中不能重复通过同一个方格** 。

 

 **示例 1：** 

```
输入：[[1,0,0,0],[0,0,0,0],[0,0,2,-1]]
输出：2
解释：我们有以下两条路径：
1. (0,0),(0,1),(0,2),(0,3),(1,3),(1,2),(1,1),(1,0),(2,0),(2,1),(2,2)
2. (0,0),(1,0),(2,0),(2,1),(1,1),(0,1),(0,2),(0,3),(1,3),(1,2),(2,2)
```
 **示例 2：** 

```
输入：[[1,0,0,0],[0,0,0,0],[0,0,0,2]]
输出：4
解释：我们有以下四条路径： 
1. (0,0),(0,1),(0,2),(0,3),(1,3),(1,2),(1,1),(1,0),(2,0),(2,1),(2,2),(2,3)
2. (0,0),(0,1),(1,1),(1,0),(2,0),(2,1),(2,2),(1,2),(0,2),(0,3),(1,3),(2,3)
3. (0,0),(1,0),(2,0),(2,1),(2,2),(1,2),(1,1),(0,1),(0,2),(0,3),(1,3),(2,3)
4. (0,0),(1,0),(2,0),(2,1),(1,1),(0,1),(0,2),(0,3),(1,3),(1,2),(2,2),(2,3)
```
 **示例 3：** 

```
输入：[[0,1],[2,0]]
输出：0
解释：
没有一条路能完全穿过每一个空的方格一次。
请注意，起始和结束方格可以位于网格中的任意位置。

```


 **提示：** 
-  `1 <= grid.length * grid[0].length <= 20` 

**标签**
`位运算` `数组` `回溯` `矩阵` 


##
```python

```
>
# 996.正方形数组的数目
[https://leetcode-cn.com/problems/number-of-squareful-arrays](https://leetcode-cn.com/problems/number-of-squareful-arrays) 
## 原题
给定一个非负整数数组 `A` ，如果该数组每对相邻元素之和是一个完全平方数，则称这一数组为 *正方形* 数组。

返回 A 的正方形排列的数目。两个排列 `A1` 和 `A2` 不同的充要条件是存在某个索引 `i` ，使得 A1[i] != A2[i]。

 

 **示例 1：** 

```
输入：[1,17,8]
输出：2
解释：
[1,8,17] 和 [17,8,1] 都是有效的排列。

```
 **示例 2：** 

```
输入：[2,2,2]
输出：1

```


 **提示：** 
-  `1 <= A.length <= 12` 
-  `0 <= A[i] <= 1e9` 

**标签**
`位运算` `数组` `数学` `动态规划` `回溯` `状态压缩` 


##
```python

```
>
# 1066.校园自行车分配 II
[https://leetcode-cn.com/problems/campus-bikes-ii](https://leetcode-cn.com/problems/campus-bikes-ii) 
## 原题


**标签**
`位运算` `数组` `动态规划` `回溯` `状态压缩` 


##
```python

```
>
# 1079.活字印刷
[https://leetcode-cn.com/problems/letter-tile-possibilities](https://leetcode-cn.com/problems/letter-tile-possibilities) 
## 原题
你有一套活字字模 `tiles` ，其中每个字模上都刻有一个字母 `tiles[i]` 。返回你可以印出的非空字母序列的数目。

 **注意：** 本题中，每个活字字模只能使用一次。

 

 **示例 1：** 

```

输入："AAB"
输出：8
解释：可能的序列为 "A", "B", "AA", "AB", "BA", "AAB", "ABA", "BAA"。

```
 **示例 2：** 

```

输入："AAABBC"
输出：188

```
 **示例 3：** 

```

输入："V"
输出：1
```


 **提示：** 
-  `1 <= tiles.length <= 7` 
-  `tiles` 由大写英文字母组成

**标签**
`字符串` `回溯` 


##
```python

```
>
# 1087.花括号展开
[https://leetcode-cn.com/problems/brace-expansion](https://leetcode-cn.com/problems/brace-expansion) 
## 原题


**标签**
`广度优先搜索` `字符串` `回溯` 


##
```python

```
>
# 1088.易混淆数 II
[https://leetcode-cn.com/problems/confusing-number-ii](https://leetcode-cn.com/problems/confusing-number-ii) 
## 原题


**标签**
`数学` `回溯` 


##
```python

```
>
# 1096.花括号展开 II
[https://leetcode-cn.com/problems/brace-expansion-ii](https://leetcode-cn.com/problems/brace-expansion-ii) 
## 原题
如果你熟悉 Shell 编程，那么一定了解过花括号展开，它可以用来生成任意字符串。

花括号展开的表达式可以看作一个由 **花括号** 、 **逗号** 和 **小写英文字母** 组成的字符串，定义下面几条语法规则：
- 如果只给出单一的元素 `x` ，那么表达式表示的字符串就只有 `"x"` 。 `R(x) = {x}` 

- 例如，表达式 `"a"` 表示字符串 `"a"` 。
- 而表达式 `"w"` 就表示字符串 `"w"` 。
	
- 当两个或多个表达式并列，以逗号分隔，我们取这些表达式中元素的并集。 `R({e_1,e_2,...}) = R(e_1) ∪ R(e_2) ∪ ...` 
	
- 例如，表达式 `"{a,b,c}"` 表示字符串 `"a","b","c"` 。
- 而表达式 `"{{a,b},{b,c}}"` 也可以表示字符串 `"a","b","c"` 。
	
- 要是两个或多个表达式相接，中间没有隔开时，我们从这些表达式中各取一个元素依次连接形成字符串。 `R(e_1 + e_2) = {a + b for (a, b) in R(e_1) × R(e_2)}` 
	
- 例如，表达式 `"{a,b}{c,d}"` 表示字符串 `"ac","ad","bc","bd"` 。
	
- 表达式之间允许嵌套，单一元素与表达式的连接也是允许的。
	
- 例如，表达式 `"a{b,c,d}"` 表示字符串 `"ab","ac","ad"​​​​​​` 。
- 例如，表达式 `"a{b,c}{d,e}f{g,h}"` 可以表示字符串 `"abdfg", "abdfh", "abefg", "abefh", "acdfg", "acdfh", "acefg", "acefh"` 。
	
	

给出表示基于给定语法规则的表达式 `expression` ，返回它所表示的所有字符串组成的有序列表。

假如你希望以「集合」的概念了解此题，也可以通过点击 “ **显示英文描述** ” 获取详情。

 

 **示例 1：** 

```

输入：expression = "{a,b}{c,{d,e}}"
输出：["ac","ad","ae","bc","bd","be"]
```
 **示例 2：** 

```

输入：expression = "{{a,z},a{b,c},{ab,z}}"
输出：["a","ab","ac","z"]
解释：输出中 不应 出现重复的组合结果。

```


 **提示：** 
-  `1 <= expression.length <= 60` 
-  `expression[i]` 由 `'{'` ， `'}'` ， `','` 或小写英文字母组成
- 给出的表达式 `expression` 用以表示一组基于题目描述中语法构造的字符串

**标签**
`栈` `广度优先搜索` `字符串` `回溯` 


##
```python

```
>
# 1215.步进数
[https://leetcode-cn.com/problems/stepping-numbers](https://leetcode-cn.com/problems/stepping-numbers) 
## 原题


**标签**
`广度优先搜索` `回溯` 


##
```python

```
>
# 1219.黄金矿工
[https://leetcode-cn.com/problems/path-with-maximum-gold](https://leetcode-cn.com/problems/path-with-maximum-gold) 
## 原题
你要开发一座金矿，地质勘测学家已经探明了这座金矿中的资源分布，并用大小为 `m * n` 的网格 `grid` 进行了标注。每个单元格中的整数就表示这一单元格中的黄金数量；如果该单元格是空的，那么就是 `0` 。

为了使收益最大化，矿工需要按以下规则来开采黄金：
- 每当矿工进入一个单元，就会收集该单元格中的所有黄金。
- 矿工每次可以从当前位置向上下左右四个方向走。
- 每个单元格只能被开采（进入）一次。
-  **不得开采** （进入）黄金数目为 `0` 的单元格。
- 矿工可以从网格中 **任意一个** 有黄金的单元格出发或者是停止。


 **示例 1：** 

```
输入：grid = [[0,6,0],[5,8,7],[0,9,0]]
输出：24
解释：
[[0,6,0],
 [5,8,7],
 [0,9,0]]
一种收集最多黄金的路线是：9 -> 8 -> 7。

```
 **示例 2：** 

```
输入：grid = [[1,0,7],[2,0,6],[3,4,5],[0,3,0],[9,0,20]]
输出：28
解释：
[[1,0,7],
 [2,0,6],
 [3,4,5],
 [0,3,0],
 [9,0,20]]
一种收集最多黄金的路线是：1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7。

```


 **提示：** 
-  `1 <= grid.length, grid[i].length <= 15` 
-  `0 <= grid[i][j] <= 100` 
- 最多 **25** 个单元格中有黄金。

**标签**
`数组` `回溯` `矩阵` 


##
```python

```
>
# 1238.循环码排列
[https://leetcode-cn.com/problems/circular-permutation-in-binary-representation](https://leetcode-cn.com/problems/circular-permutation-in-binary-representation) 
## 原题
给你两个整数 `n` 和 `start` 。你的任务是返回任意 `(0,1,2,,...,2^n-1)` 的排列 `p` ，并且满足：
-  `p[0] = start` 
-  `p[i]` 和 `p[i+1]` 的二进制表示形式只有一位不同
-  `p[0]` 和 `p[2^n -1]` 的二进制表示形式也只有一位不同


 **示例 1：** 

```
输入：n = 2, start = 3
输出：[3,2,0,1]
解释：这个排列的二进制表示是 (11,10,00,01)
     所有的相邻元素都有一位是不同的，另一个有效的排列是 [3,1,0,2]

```
 **示例 2：** 

```
输出：n = 3, start = 2
输出：[2,6,7,5,4,0,1,3]
解释：这个排列的二进制表示是 (010,110,111,101,100,000,001,011)

```


 **提示：** 
-  `1 <= n <= 16` 
-  `0 <= start < 2^n` 

**标签**
`位运算` `数学` `回溯` 


##
```python

```
>
# 1239.串联字符串的最大长度
[https://leetcode-cn.com/problems/maximum-length-of-a-concatenated-string-with-unique-characters](https://leetcode-cn.com/problems/maximum-length-of-a-concatenated-string-with-unique-characters) 
## 原题
给定一个字符串数组 `arr` ，字符串 `s` 是将 `arr` 某一子序列字符串连接所得的字符串，如果 `s` 中的每一个字符都只出现过一次，那么它就是一个可行解。

请返回所有可行解 `s` 中最长长度。

 

 **示例 1：** 

```
输入：arr = ["un","iq","ue"]
输出：4
解释：所有可能的串联组合是 "","un","iq","ue","uniq" 和 "ique"，最大长度为 4。

```
 **示例 2：** 

```
输入：arr = ["cha","r","act","ers"]
输出：6
解释：可能的解答有 "chaers" 和 "acters"。

```
 **示例 3：** 

```
输入：arr = ["abcdefghijklmnopqrstuvwxyz"]
输出：26

```


 **提示：** 
-  `1 <= arr.length <= 16` 
-  `1 <= arr[i].length <= 26` 
-  `arr[i]` 中只含有小写英文字母

**标签**
`位运算` `数组` `字符串` `回溯` 


##
```python

```
>
# 1240.铺瓷砖
[https://leetcode-cn.com/problems/tiling-a-rectangle-with-the-fewest-squares](https://leetcode-cn.com/problems/tiling-a-rectangle-with-the-fewest-squares) 
## 原题
你是一位施工队的工长，根据设计师的要求准备为一套设计风格独特的房子进行室内装修。

房子的客厅大小为 `n` x `m` ，为保持极简的风格，需要使用尽可能少的 **正方形** 瓷砖来铺盖地面。

假设正方形瓷砖的规格不限，边长都是整数。

请你帮设计师计算一下，最少需要用到多少块方形瓷砖？

 

 **示例 1：** 

<img alt="" src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/10/25/sample_11_1592.png" style="height: 106px; width: 154px;">

```
输入：n = 2, m = 3
输出：3
解释：3 块地砖就可以铺满卧室。
     2 块 1x1 地砖
     1 块 2x2 地砖
```
 **示例 2：** 

<img alt="" src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/10/25/sample_22_1592.png" style="height: 126px; width: 224px;">

```
输入：n = 5, m = 8
输出：5

```
 **示例 3：** 

<img alt="" src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/10/25/sample_33_1592.png" style="height: 189px; width: 224px;">

```
输入：n = 11, m = 13
输出：6

```


 **提示：** 
-  `1 <= n <= 13` 
-  `1 <= m <= 13` 

**标签**
`动态规划` `回溯` 


##
```python

```
>
# 1255.得分最高的单词集合
[https://leetcode-cn.com/problems/maximum-score-words-formed-by-letters](https://leetcode-cn.com/problems/maximum-score-words-formed-by-letters) 
## 原题
你将会得到一份单词表 `words` ，一个字母表 `letters` （可能会有重复字母），以及每个字母对应的得分情况表 `score` 。

请你帮忙计算玩家在单词拼写游戏中所能获得的「最高得分」：能够由 `letters` 里的字母拼写出的 **任意** 属于 `words` 单词子集中，分数最高的单词集合的得分。

单词拼写游戏的规则概述如下：
- 玩家需要用字母表 `letters` 里的字母来拼写单词表 `words` 中的单词。
- 可以只使用字母表 `letters` 中的部分字母，但是每个字母最多被使用一次。
- 单词表 `words` 中每个单词只能计分（使用）一次。
- 根据字母得分情况表 `score` ，字母 `';a';` , `';b';` , `';c';` , ... , `';z';` 对应的得分分别为 `score[0]` , `score[1]` , ..., `score[25]` 。
- 本场游戏的「得分」是指：玩家所拼写出的单词集合里包含的所有字母的得分之和。


 **示例 1：** 

```
输入：words = ["dog","cat","dad","good"], letters = ["a","a","c","d","d","d","g","o","o"], score = [1,0,9,5,0,0,3,0,0,0,0,0,0,0,2,0,0,0,0,0,0,0,0,0,0,0]
输出：23
解释：
字母得分为  a=1, c=9, d=5, g=3, o=2
使用给定的字母表 letters，我们可以拼写单词 "dad" (5+1+5)和 "good" (3+2+2+5)，得分为 23 。
而单词 "dad" 和 "dog" 只能得到 21 分。
```
 **示例 2：** 

```
输入：words = ["xxxz","ax","bx","cx"], letters = ["z","a","b","c","x","x","x"], score = [4,4,4,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,5,0,10]
输出：27
解释：
字母得分为  a=4, b=4, c=4, x=5, z=10
使用给定的字母表 letters，我们可以组成单词 "ax" (4+5)， "bx" (4+5) 和 "cx" (4+5) ，总得分为 27 。
单词 "xxxz" 的得分仅为 25 。
```
 **示例 3：** 

```
输入：words = ["leetcode"], letters = ["l","e","t","c","o","d"], score = [0,0,1,1,1,0,0,0,0,0,0,1,0,0,1,0,0,0,0,1,0,0,0,0,0,0]
输出：0
解释：
字母 "e" 在字母表 letters 中只出现了一次，所以无法组成单词表 words 中的单词。
```


 **提示：** 
-  `1 <= words.length <= 14` 
-  `1 <= words[i].length <= 15` 
-  `1 <= letters.length <= 100` 
-  `letters[i].length == 1` 
-  `score.length == 26` 
-  `0 <= score[i] <= 10` 
-  `words[i]` 和 `letters[i]` 只包含小写的英文字母。

**标签**
`位运算` `数组` `字符串` `动态规划` `回溯` `状态压缩` 


##
```python

```
>
# 1258.近义词句子
[https://leetcode-cn.com/problems/synonymous-sentences](https://leetcode-cn.com/problems/synonymous-sentences) 
## 原题


**标签**
`并查集` `数组` `哈希表` `字符串` `回溯` 


##
```python

```
>
# 1286.字母组合迭代器
[https://leetcode-cn.com/problems/iterator-for-combination](https://leetcode-cn.com/problems/iterator-for-combination) 
## 原题
请你设计一个迭代器类，包括以下内容：
- 一个构造函数，输入参数包括：一个 **有序且字符唯一** 的字符串 `characters` （该字符串只包含小写英文字母）和一个数字 `combinationLength` 。
- 函数 *next()* ，按 **字典序** 返回长度为 `combinationLength` 的下一个字母组合。
- 函数 *hasNext()* ，只有存在长度为 `combinationLength` 的下一个字母组合时，才返回 `True` ；否则，返回 `False` 。


 **示例：** 

```
CombinationIterator iterator = new CombinationIterator("abc", 2); // 创建迭代器 iterator

iterator.next(); // 返回 "ab"
iterator.hasNext(); // 返回 true
iterator.next(); // 返回 "ac"
iterator.hasNext(); // 返回 true
iterator.next(); // 返回 "bc"
iterator.hasNext(); // 返回 false

```


 **提示：** 
-  `1 <= combinationLength <= characters.length <= 15` 
- 每组测试数据最多包含 `10^4` 次函数调用。
- 题目保证每次调用函数 `next` 时都存在下一个字母组合。

**标签**
`设计` `字符串` `回溯` `迭代器` 


##
```python

```
>
# 1307.口算难题
[https://leetcode-cn.com/problems/verbal-arithmetic-puzzle](https://leetcode-cn.com/problems/verbal-arithmetic-puzzle) 
## 原题
给你一个方程，左边用 `words` 表示，右边用 `result` 表示。

你需要根据以下规则检查方程是否可解：
- 每个字符都会被解码成一位数字（0 - 9）。
- 每对不同的字符必须映射到不同的数字。
- 每个 `words[i]` 和 `result` 都会被解码成一个没有前导零的数字。
- 左侧数字之和（ `words` ）等于右侧数字（ `result` ）。 
如果方程可解，返回 `True` ，否则返回 `False` 。

 

 **示例 1：** 

```
输入：words = ["SEND","MORE"], result = "MONEY"
输出：true
解释：映射 ';S';-> 9, ';E';->5, ';N';->6, ';D';->7, ';M';->1, ';O';->0, ';R';->8, ';Y';->';2';
所以 "SEND" + "MORE" = "MONEY" ,  9567 + 1085 = 10652
```
 **示例 2：** 

```
输入：words = ["SIX","SEVEN","SEVEN"], result = "TWENTY"
输出：true
解释：映射 ';S';-> 6, ';I';->5, ';X';->0, ';E';->8, ';V';->7, ';N';->2, ';T';->1, ';W';->';3';, ';Y';->4
所以 "SIX" + "SEVEN" + "SEVEN" = "TWENTY" ,  650 + 68782 + 68782 = 138214
```
 **示例 3：** 

```
输入：words = ["THIS","IS","TOO"], result = "FUNNY"
输出：true

```
 **示例 4：** 

```
输入：words = ["LEET","CODE"], result = "POINT"
输出：false

```


 **提示：** 
-  `2 <= words.length <= 5` 
-  `1 <= words[i].length, results.length <= 7` 
-  `words[i], result` 只含有大写英文字母
- 表达式中使用的不同字符数最大为 10

**标签**
`数组` `数学` `字符串` `回溯` 


##
```python

```
>
# 1415.长度为 n 的开心字符串中字典序第 k 小的字符串
[https://leetcode-cn.com/problems/the-k-th-lexicographical-string-of-all-happy-strings-of-length-n](https://leetcode-cn.com/problems/the-k-th-lexicographical-string-of-all-happy-strings-of-length-n) 
## 原题
一个 「开心字符串」定义为：
- 仅包含小写字母 `[';a';, ';b';, ';c';]` .
- 对所有在 `1` 到 `s.length - 1` 之间的 `i` ，满足 `s[i] != s[i + 1]` （字符串的下标从 1 开始）。
比方说，字符串 **"abc"** ， **"ac"，"b"** 和 **"abcbabcbcb"** 都是开心字符串，但是 **"aa"** ， **"baa"** 和 **"ababbc"** 都不是开心字符串。

给你两个整数 `n` 和 `k` ，你需要将长度为 `n` 的所有开心字符串按字典序排序。

请你返回排序后的第 k 个开心字符串，如果长度为 `n` 的开心字符串少于 `k` 个，那么请你返回 **空字符串** 。

 

 **示例 1：** 

```
输入：n = 1, k = 3
输出："c"
解释：列表 ["a", "b", "c"] 包含了所有长度为 1 的开心字符串。按照字典序排序后第三个字符串为 "c" 。

```
 **示例 2：** 

```
输入：n = 1, k = 4
输出：""
解释：长度为 1 的开心字符串只有 3 个。

```
 **示例 3：** 

```
输入：n = 3, k = 9
输出："cab"
解释：长度为 3 的开心字符串总共有 12 个 ["aba", "abc", "aca", "acb", "bab", "bac", "bca", "bcb", "cab", "cac", "cba", "cbc"] 。第 9 个字符串为 "cab"

```
 **示例 4：** 

```
输入：n = 2, k = 7
输出：""

```
 **示例 5：** 

```
输入：n = 10, k = 100
输出："abacbabacb"

```


 **提示：** 
-  `1 <= n <= 10` 
-  `1 <= k <= 100` 



**标签**
`字符串` `回溯` 


##
```python

```
>
# 1467.两个盒子中球的颜色数相同的概率
[https://leetcode-cn.com/problems/probability-of-a-two-boxes-having-the-same-number-of-distinct-balls](https://leetcode-cn.com/problems/probability-of-a-two-boxes-having-the-same-number-of-distinct-balls) 
## 原题
桌面上有 `2n` 个颜色不完全相同的球，球上的颜色共有 `k` 种。给你一个大小为 `k` 的整数数组 `balls` ，其中 `balls[i]` 是颜色为 `i` 的球的数量。

所有的球都已经 **随机打乱顺序** ，前 `n` 个球放入第一个盒子，后 `n` 个球放入另一个盒子（请认真阅读示例 2 的解释部分）。

 **注意：** 这两个盒子是不同的。例如，两个球颜色分别为 `a` 和 `b` ，盒子分别为 `[]` 和 `()` ，那么 `[a] (b)` 和 `[b] (a)` 这两种分配方式是不同的（请认真阅读示例 1 的解释部分）。

请计算「两个盒子中球的颜色数相同」的情况的概率。

 

 **示例 1：** 

```
输入：balls = [1,1]
输出：1.00000
解释：球平均分配的方式只有两种：
- 颜色为 1 的球放入第一个盒子，颜色为 2 的球放入第二个盒子
- 颜色为 2 的球放入第一个盒子，颜色为 1 的球放入第二个盒子
这两种分配，两个盒子中球的颜色数都相同。所以概率为 2/2 = 1 。

```
 **示例 2：** 

```
输入：balls = [2,1,1]
输出：0.66667
解释：球的列表为 [1, 1, 2, 3]
随机打乱，得到 12 种等概率的不同打乱方案，每种方案概率为 1/12 ：
[1,1 / 2,3], [1,1 / 3,2], [1,2 / 1,3], [1,2 / 3,1], [1,3 / 1,2], [1,3 / 2,1], [2,1 / 1,3], [2,1 / 3,1], [2,3 / 1,1], [3,1 / 1,2], [3,1 / 2,1], [3,2 / 1,1]
然后，我们将前两个球放入第一个盒子，后两个球放入第二个盒子。
这 12 种可能的随机打乱方式中的 8 种满足「两个盒子中球的颜色数相同」。
概率 = 8/12 = 0.66667

```
 **示例 3：** 

```
输入：balls = [1,2,1,2]
输出：0.60000
解释：球的列表为 [1, 2, 2, 3, 4, 4]。要想显示所有 180 种随机打乱方案是很难的，但只检查「两个盒子中球的颜色数相同」的 108 种情况是比较容易的。
概率 = 108 / 180 = 0.6 。

```
 **示例 4：** 

```
输入：balls = [3,2,1]
输出：0.30000
解释：球的列表为 [1, 1, 1, 2, 2, 3]。要想显示所有 60 种随机打乱方案是很难的，但只检查「两个盒子中球的颜色数相同」的 18 种情况是比较容易的。
概率 = 18 / 60 = 0.3 。

```
 **示例 5：** 

```
输入：balls = [6,6,6,6,6,6]
输出：0.90327

```


 **提示：** 
-  `1 <= balls.length <= 8` 
-  `1 <= balls[i] <= 6` 
-  `sum(balls)` 是偶数
- 答案与真实值误差在 `10^-5` 以内，则被视为正确答案

**标签**
`数学` `动态规划` `回溯` `组合数学` `概率与统计` 


##
```python

```
>
# 1593.拆分字符串使唯一子字符串的数目最大
[https://leetcode-cn.com/problems/split-a-string-into-the-max-number-of-unique-substrings](https://leetcode-cn.com/problems/split-a-string-into-the-max-number-of-unique-substrings) 
## 原题
给你一个字符串 `s` ，请你拆分该字符串，并返回拆分后唯一子字符串的最大数目。

字符串 `s` 拆分后可以得到若干 **非空子字符串** ，这些子字符串连接后应当能够还原为原字符串。但是拆分出来的每个子字符串都必须是 **唯一的** 。

注意： **子字符串** 是字符串中的一个连续字符序列。

 

 **示例 1：** 

```
输入：s = "ababccc"
输出：5
解释：一种最大拆分方法为 [';a';, ';b';, ';ab';, ';c';, ';cc';] 。像 [';a';, ';b';, ';a';, ';b';, ';c';, ';cc';] 这样拆分不满足题目要求，因为其中的 ';a'; 和 ';b'; 都出现了不止一次。

```
 **示例 2：** 

```
输入：s = "aba"
输出：2
解释：一种最大拆分方法为 [';a';, ';ba';] 。

```
 **示例 3：** 

```
输入：s = "aa"
输出：1
解释：无法进一步拆分字符串。

```


 **提示：** 
- 
	 `1 <= s.length <= 16` 
	
- 
	 `s` 仅包含小写英文字母
	

**标签**
`哈希表` `字符串` `回溯` 


##
```python

```
>
# 1601.最多可达成的换楼请求数目
[https://leetcode-cn.com/problems/maximum-number-of-achievable-transfer-requests](https://leetcode-cn.com/problems/maximum-number-of-achievable-transfer-requests) 
## 原题
我们有 `n` 栋楼，编号从 `0` 到 `n - 1` 。每栋楼有若干员工。由于现在是换楼的季节，部分员工想要换一栋楼居住。

给你一个数组 `requests` ，其中 `requests[i] = [from<sub>i</sub>, to<sub>i</sub>]` ，表示一个员工请求从编号为 `from<sub>i</sub>` 的楼搬到编号为 `to<sub>i</sub>` <sub> </sub>的楼。

一开始 **所有楼都是满的** ，所以从请求列表中选出的若干个请求是可行的需要满足 **每栋楼员工净变化为 0** 。意思是每栋楼 **离开** 的员工数目 **等于** 该楼 **搬入** 的员工数数目。比方说 `n = 3` 且两个员工要离开楼 `0` ，一个员工要离开楼 `1` ，一个员工要离开楼 `2` ，如果该请求列表可行，应该要有两个员工搬入楼 `0` ，一个员工搬入楼 `1` ，一个员工搬入楼 `2` 。

请你从原请求列表中选出若干个请求，使得它们是一个可行的请求列表，并返回所有可行列表中最大请求数目。

 

 **示例 1：** 

<img alt="" src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/09/26/move1.jpg" style="height: 406px; width: 600px;">

```
输入：n = 5, requests = [[0,1],[1,0],[0,1],[1,2],[2,0],[3,4]]
输出：5
解释：请求列表如下：
从楼 0 离开的员工为 x 和 y ，且他们都想要搬到楼 1 。
从楼 1 离开的员工为 a 和 b ，且他们分别想要搬到楼 2 和 0 。
从楼 2 离开的员工为 z ，且他想要搬到楼 0 。
从楼 3 离开的员工为 c ，且他想要搬到楼 4 。
没有员工从楼 4 离开。
我们可以让 x 和 b 交换他们的楼，以满足他们的请求。
我们可以让 y，a 和 z 三人在三栋楼间交换位置，满足他们的要求。
所以最多可以满足 5 个请求。
```
 **示例 2：** 

<img alt="" src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/09/26/move2.jpg" style="height: 327px; width: 450px;">

```
输入：n = 3, requests = [[0,0],[1,2],[2,1]]
输出：3
解释：请求列表如下：
从楼 0 离开的员工为 x ，且他想要回到原来的楼 0 。
从楼 1 离开的员工为 y ，且他想要搬到楼 2 。
从楼 2 离开的员工为 z ，且他想要搬到楼 1 。
我们可以满足所有的请求。
```
 **示例 3：** 

```
输入：n = 4, requests = [[0,3],[3,1],[1,2],[2,0]]
输出：4

```


 **提示：** 
-  `1 <= n <= 20` 
-  `1 <= requests.length <= 16` 
-  `requests[i].length == 2` 
-  `0 <= from<sub>i</sub>, to<sub>i</sub> < n` 

**标签**
`位运算` `数组` `回溯` `枚举` 


##
```python

```
>
# 1655.分配重复整数
[https://leetcode-cn.com/problems/distribute-repeating-integers](https://leetcode-cn.com/problems/distribute-repeating-integers) 
## 原题
给你一个长度为  `n`  的整数数组  `nums`  ，这个数组中至多有  `50`  个不同的值。同时你有 `m`  个顾客的订单 `quantity`  ，其中，整数  `quantity[i]`  是第  `i`  位顾客订单的数目。请你判断是否能将 `nums`  中的整数分配给这些顾客，且满足：
- 第  `i`  位顾客 **恰好 ** 有  `quantity[i]`  个整数。
- 第  `i`  位顾客拿到的整数都是 **相同的**  。
- 每位顾客都满足上述两个要求。
如果你可以分配 `nums`  中的整数满足上面的要求，那么请返回  `true`  ，否则返回 `false`  。

 

 **示例 1：** 

```
输入：nums = [1,2,3,4], quantity = [2]
输出：false
解释：第 0 位顾客没办法得到两个相同的整数。

```
 **示例 2：** 

```
输入：nums = [1,2,3,3], quantity = [2]
输出：true
解释：第 0 位顾客得到 [3,3] 。整数 [1,2] 都没有被使用。

```
 **示例 3：** 

```
输入：nums = [1,1,2,2], quantity = [2,2]
输出：true
解释：第 0 位顾客得到 [1,1] ，第 1 位顾客得到 [2,2] 。

```
 **示例 4：** 

```
输入：nums = [1,1,2,3], quantity = [2,2]
输出：false
解释：尽管第 0 位顾客可以得到 [1,1] ，第 1 位顾客没法得到 2 个一样的整数。
```
 **示例 5：** 

```
输入：nums = [1,1,1,1,1], quantity = [2,3]
输出：true
解释：第 0 位顾客得到 [1,1] ，第 1 位顾客得到 [1,1,1] 。

```


 **提示：** 
-  `n == nums.length` 
-  `1 <= n <= 10^5` 
-  `1 <= nums[i] <= 1000` 
-  `m == quantity.length` 
-  `1 <= m <= 10` 
-  `1 <= quantity[i] <= 10^5` 
-  `nums`  中至多有  `50`  个不同的数字。

**标签**
`位运算` `数组` `动态规划` `回溯` `状态压缩` 


##
```python

```
>
# 1718.构建字典序最大的可行序列
[https://leetcode-cn.com/problems/construct-the-lexicographically-largest-valid-sequence](https://leetcode-cn.com/problems/construct-the-lexicographically-largest-valid-sequence) 
## 原题
给你一个整数  `n`  ，请你找到满足下面条件的一个序列：
- 整数  `1`  在序列中只出现一次。
-  `2`  到  `n`  之间每个整数都恰好出现两次。
- 对于每个  `2`  到  `n`  之间的整数  `i`  ，两个  `i`  之间出现的距离恰好为  `i`  。
序列里面两个数 `a[i]`  和 `a[j]`  之间的 **距离**  ，我们定义为它们下标绝对值之差  `|j - i|`  。

请你返回满足上述条件中  **字典序最大**  的序列。题目保证在给定限制条件下，一定存在解。

一个序列  `a`  被认为比序列  `b`  （两者长度相同）字典序更大的条件是：  `a` 和  `b`  中第一个不一样的数字处， `a`  序列的数字比  `b`  序列的数字大。比方说， `[0,1,9,0]`  比  `[0,1,5,6]`  字典序更大，因为第一个不同的位置是第三个数字，且  `9`  比  `5`  大。

 

 **示例 1：** 

```
输入：n = 3
输出：[3,1,2,3,2]
解释：[2,3,2,1,3] 也是一个可行的序列，但是 [3,1,2,3,2] 是字典序最大的序列。

```
 **示例 2：** 

```
输入：n = 5
输出：[5,3,1,4,3,5,2,4,2]

```


 **提示：** 
-  `1 <= n <= 20` 

**标签**
`数组` `回溯` 


##
```python

```
>
# 1723.完成所有工作的最短时间
[https://leetcode-cn.com/problems/find-minimum-time-to-finish-all-jobs](https://leetcode-cn.com/problems/find-minimum-time-to-finish-all-jobs) 
## 原题
给你一个整数数组 `jobs` ，其中 `jobs[i]` 是完成第 `i` 项工作要花费的时间。

请你将这些工作分配给 `k` 位工人。所有工作都应该分配给工人，且每项工作只能分配给一位工人。工人的 **工作时间** 是完成分配给他们的所有工作花费时间的总和。请你设计一套最佳的工作分配方案，使工人的 **最大工作时间** 得以 **最小化** 。

返回分配方案中尽可能 **最小** 的 **最大工作时间** 。

 

 **示例 1：** 

```

输入：jobs = [3,2,3], k = 3
输出：3
解释：给每位工人分配一项工作，最大工作时间是 3 。

```
 **示例 2：** 

```

输入：jobs = [1,2,4,7,8], k = 2
输出：11
解释：按下述方式分配工作：
1 号工人：1、2、8（工作时间 = 1 + 2 + 8 = 11）
2 号工人：4、7（工作时间 = 4 + 7 = 11）
最大工作时间是 11 。
```


 **提示：** 
-  `1 <= k <= jobs.length <= 12` 
-  `1 <= jobs[i] <= 10^7` 

**标签**
`位运算` `数组` `动态规划` `回溯` `状态压缩` 


##
```python

```
>
# 1774.最接近目标价格的甜点成本
[https://leetcode-cn.com/problems/closest-dessert-cost](https://leetcode-cn.com/problems/closest-dessert-cost) 
## 原题
你打算做甜点，现在需要购买配料。目前共有 `n` 种冰激凌基料和 `m` 种配料可供选购。而制作甜点需要遵循以下几条规则：
- 必须选择 **一种** 冰激凌基料。
- 可以添加 **一种或多种** 配料，也可以不添加任何配料。
- 每种类型的配料 **最多两份** 。
给你以下三个输入：
-  `baseCosts` ，一个长度为 `n` 的整数数组，其中每个 `baseCosts[i]` 表示第 `i` 种冰激凌基料的价格。
-  `toppingCosts` ，一个长度为 `m` 的整数数组，其中每个 `toppingCosts[i]` 表示 **一份** 第 `i` 种冰激凌配料的价格。
-  `target` ，一个整数，表示你制作甜点的目标价格。
你希望自己做的甜点总成本尽可能接近目标价格 `target` 。

返回最接近 `target` 的甜点成本。如果有多种方案，返回  **成本相对较低** 的一种。

 

 **示例 1：** 

```

输入：baseCosts = [1,7], toppingCosts = [3,4], target = 10
输出：10
解释：考虑下面的方案组合（所有下标均从 0 开始）：
- 选择 1 号基料：成本 7
- 选择 1 份 0 号配料：成本 1 x 3 = 3
- 选择 0 份 1 号配料：成本 0 x 4 = 0
总成本：7 + 3 + 0 = 10 。

```
 **示例 2：** 

```

输入：baseCosts = [2,3], toppingCosts = [4,5,100], target = 18
输出：17
解释：考虑下面的方案组合（所有下标均从 0 开始）：
- 选择 1 号基料：成本 3
- 选择 1 份 0 号配料：成本 1 x 4 = 4
- 选择 2 份 1 号配料：成本 2 x 5 = 10
- 选择 0 份 2 号配料：成本 0 x 100 = 0
总成本：3 + 4 + 10 + 0 = 17 。不存在总成本为 18 的甜点制作方案。

```
 **示例 3：** 

```

输入：baseCosts = [3,10], toppingCosts = [2,5], target = 9
输出：8
解释：可以制作总成本为 8 和 10 的甜点。返回 8 ，因为这是成本更低的方案。

```
 **示例 4：** 

```

输入：baseCosts = [10], toppingCosts = [1], target = 1
输出：10
解释：注意，你可以选择不添加任何配料，但你必须选择一种基料。
```


 **提示：** 
-  `n == baseCosts.length` 
-  `m == toppingCosts.length` 
-  `1 <= n, m <= 10` 
-  `1 <= baseCosts[i], toppingCosts[i] <= 10^4` 
-  `1 <= target <= 10^4` 

**标签**
`数组` `动态规划` `回溯` 


##
```python

```
>
# 1799.N 次操作后的最大分数和
[https://leetcode-cn.com/problems/maximize-score-after-n-operations](https://leetcode-cn.com/problems/maximize-score-after-n-operations) 
## 原题
给你  `nums`  ，它是一个大小为  `2 * n`  的正整数数组。你必须对这个数组执行 `n`  次操作。

在第  `i`  次操作时（操作编号从 **1**  开始），你需要：
- 选择两个元素  `x` 和  `y`  。
- 获得分数  `i * gcd(x, y)`  。
- 将  `x`  和  `y` 从  `nums`  中删除。
请你返回 `n`  次操作后你能获得的分数和最大为多少。

函数  `gcd(x, y)`  是  `x` 和  `y`  的最大公约数。

 

 **示例 1：** 

```
输入：nums = [1,2]
输出：1
解释：最优操作是：
(1 * gcd(1, 2)) = 1

```
 **示例 2：** 

```
输入：nums = [3,4,6,8]
输出：11
解释：最优操作是：
(1 * gcd(3, 6)) + (2 * gcd(4, 8)) = 3 + 8 = 11

```
 **示例 3：** 

```
输入：nums = [1,2,3,4,5,6]
输出：14
解释：最优操作是：
(1 * gcd(1, 5)) + (2 * gcd(2, 4)) + (3 * gcd(3, 6)) = 1 + 4 + 9 = 14

```


 **提示：** 
-  `1 <= n <= 7` 
-  `nums.length == 2 * n` 
-  `1 <= nums[i] <= 10^6` 

**标签**
`位运算` `数组` `数学` `动态规划` `回溯` `状态压缩` `数论` 


##
```python

```
>
# 1820.最多邀请的个数
[https://leetcode-cn.com/problems/maximum-number-of-accepted-invitations](https://leetcode-cn.com/problems/maximum-number-of-accepted-invitations) 
## 原题


**标签**
`数组` `回溯` `矩阵` 


##
```python

```
>
# 1849.将字符串拆分为递减的连续值
[https://leetcode-cn.com/problems/splitting-a-string-into-descending-consecutive-values](https://leetcode-cn.com/problems/splitting-a-string-into-descending-consecutive-values) 
## 原题
给你一个仅由数字组成的字符串 `s` 。

请你判断能否将 `s` 拆分成两个或者多个 **非空子字符串** ，使子字符串的 **数值** 按 **降序** 排列，且每两个 **相邻子字符串** 的数值之 **差** 等于 `1` 。
- 例如，字符串 `s = "0090089"` 可以拆分成 `["0090", "089"]` ，数值为 `[90,89]` 。这些数值满足按降序排列，且相邻值相差 `1` ，这种拆分方法可行。
- 另一个例子中，字符串 `s = "001"` 可以拆分成 `["0", "01"]` 、 `["00", "1"]` 或 `["0", "0", "1"]` 。然而，所有这些拆分方法都不可行，因为对应数值分别是 `[0,1]` 、 `[0,1]` 和 `[0,0,1]` ，都不满足按降序排列的要求。
如果可以按要求拆分 `s` ，返回 `true` ；否则，返回 `false` 。

 **子字符串** 是字符串中的一个连续字符序列。

 

 **示例 1：** 

```

输入：s = "1234"
输出：false
解释：不存在拆分 s 的可行方法。

```
 **示例 2：** 

```

输入：s = "050043"
输出：true
解释：s 可以拆分为 ["05", "004", "3"] ，对应数值为 [5,4,3] 。
满足按降序排列，且相邻值相差 1 。

```
 **示例 3：** 

```

输入：s = "9080701"
输出：false
解释：不存在拆分 s 的可行方法。

```
 **示例 4：** 

```

输入：s = "10009998"
输出：true
解释：s 可以拆分为 ["100", "099", "98"] ，对应数值为 [100,99,98] 。
满足按降序排列，且相邻值相差 1 。
```


 **提示：** 
-  `1 <= s.length <= 20` 
-  `s` 仅由数字组成

**标签**
`字符串` `回溯` 


##
```python

```
>
# 1863.找出所有子集的异或总和再求和
[https://leetcode-cn.com/problems/sum-of-all-subset-xor-totals](https://leetcode-cn.com/problems/sum-of-all-subset-xor-totals) 
## 原题
一个数组的 **异或总和** 定义为数组中所有元素按位 `XOR` 的结果；如果数组为 **空** ，则异或总和为 `0` 。
- 例如，数组  `[2,5,6]` 的 **异或总和** 为 `2 XOR 5 XOR 6 = 1` 。
给你一个数组 `nums` ，请你求出 `nums` 中每个 **子集** 的 **异或总和** ，计算并返回这些值相加之 **和** 。

 **注意：** 在本题中，元素 **相同** 的不同子集应 **多次** 计数。

数组 `a` 是数组 `b` 的一个 **子集** 的前提条件是：从 `b` 删除几个（也可能不删除）元素能够得到 `a` 。

 

 **示例 1：** 

```
输入：nums = [1,3]
输出：6
解释：[1,3] 共有 4 个子集：
- 空子集的异或总和是 0 。
- [1] 的异或总和为 1 。
- [3] 的异或总和为 3 。
- [1,3] 的异或总和为 1 XOR 3 = 2 。
0 + 1 + 3 + 2 = 6

```
 **示例 2：** 

```
输入：nums = [5,1,6]
输出：28
解释：[5,1,6] 共有 8 个子集：
- 空子集的异或总和是 0 。
- [5] 的异或总和为 5 。
- [1] 的异或总和为 1 。
- [6] 的异或总和为 6 。
- [5,1] 的异或总和为 5 XOR 1 = 4 。
- [5,6] 的异或总和为 5 XOR 6 = 3 。
- [1,6] 的异或总和为 1 XOR 6 = 7 。
- [5,1,6] 的异或总和为 5 XOR 1 XOR 6 = 2 。
0 + 5 + 1 + 6 + 4 + 3 + 7 + 2 = 28

```
 **示例 3：** 

```
输入：nums = [3,4,5,6,7,8]
输出：480
解释：每个子集的全部异或总和值之和为 480 。

```


 **提示：** 
-  `1 <= nums.length <= 12` 
-  `1 <= nums[i] <= 20` 

**标签**
`位运算` `数组` `数学` `回溯` `组合数学` 


##
```python

```
>
# 1947.最大兼容性评分和
[https://leetcode-cn.com/problems/maximum-compatibility-score-sum](https://leetcode-cn.com/problems/maximum-compatibility-score-sum) 
## 原题
有一份由 `n` 个问题组成的调查问卷，每个问题的答案要么是 `0` （no，否），要么是 `1` （yes，是）。

这份调查问卷被分发给 `m` 名学生和 `m` 名导师，学生和导师的编号都是从 `0` 到 `m - 1` 。学生的答案用一个二维整数数组 `students` 表示，其中 `students[i]` 是一个整数数组，包含第 `i` 名学生对调查问卷给出的答案（ **下标从 0 开始** ）。导师的答案用一个二维整数数组 `mentors` 表示，其中 `mentors[j]` 是一个整数数组，包含第 `j` 名导师对调查问卷给出的答案（ **下标从 0 开始** ）。

每个学生都会被分配给 **一名** 导师，而每位导师也会分配到 **一名** 学生。配对的学生与导师之间的兼容性评分等于学生和导师答案相同的次数。
- 例如，学生答案为 `[1, ** *0* ** , ** *1* ** ]` 而导师答案为 `[0, ** *0* ** , ** *1* ** ]` ，那么他们的兼容性评分为 2 ，因为只有第二个和第三个答案相同。
请你找出最优的学生与导师的配对方案，以 **最大程度上** 提高 **兼容性评分和** 。

给你 `students` 和 `mentors` ，返回可以得到的 **最大兼容性评分和** 。

 

 **示例 1：** 

```
输入：students = [[1,1,0],[1,0,1],[0,0,1]], mentors = [[1,0,0],[0,0,1],[1,1,0]]
输出：8
解释：按下述方式分配学生和导师：
- 学生 0 分配给导师 2 ，兼容性评分为 3 。
- 学生 1 分配给导师 0 ，兼容性评分为 2 。
- 学生 2 分配给导师 1 ，兼容性评分为 3 。
最大兼容性评分和为 3 + 2 + 3 = 8 。
```
 **示例 2：** 

```
输入：students = [[0,0],[0,0],[0,0]], mentors = [[1,1],[1,1],[1,1]]
输出：0
解释：任意学生与导师配对的兼容性评分都是 0 。

```


 **提示：** 
-  `m == students.length == mentors.length` 
-  `n == students[i].length == mentors[j].length` 
-  `1 <= m, n <= 8` 
-  `students[i][k]` 为 `0` 或 `1` 
-  `mentors[j][k]` 为 `0` 或 `1` 

**标签**
`位运算` `数组` `动态规划` `回溯` `状态压缩` 


##
```python

```
>
# 1980.找出不同的二进制字符串
[https://leetcode-cn.com/problems/find-unique-binary-string](https://leetcode-cn.com/problems/find-unique-binary-string) 
## 原题
给你一个字符串数组 `nums` ，该数组由 `n` 个 **互不相同** 的二进制字符串组成，且每个字符串长度都是 `n` 。请你找出并返回一个长度为 `n` 且 **没有出现** 在 `nums` 中的二进制字符串 *。* 如果存在多种答案，只需返回 **任意一个** 即可。

 

 **示例 1：** 

```

输入：nums = ["01","10"]
输出："11"
解释："11" 没有出现在 nums 中。"00" 也是正确答案。

```
 **示例 2：** 

```

输入：nums = ["00","01"]
输出："11"
解释："11" 没有出现在 nums 中。"10" 也是正确答案。

```
 **示例 3：** 

```

输入：nums = ["111","011","001"]
输出："101"
解释："101" 没有出现在 nums 中。"000"、"010"、"100"、"110" 也是正确答案。
```


 **提示：** 
-  `n == nums.length` 
-  `1 <= n <= 16` 
-  `nums[i].length == n` 
-  `nums[i]` 为 `'0'` 或 `'1'` 
-  `nums` 中的所有字符串 **互不相同** 

**标签**
`数组` `字符串` `回溯` 


##
```python

```
>
# 1986.完成任务的最少工作时间段
[https://leetcode-cn.com/problems/minimum-number-of-work-sessions-to-finish-the-tasks](https://leetcode-cn.com/problems/minimum-number-of-work-sessions-to-finish-the-tasks) 
## 原题
你被安排了 `n` 个任务。任务需要花费的时间用长度为 `n` 的整数数组 `tasks` 表示，第 `i` 个任务需要花费 `tasks[i]` 小时完成。一个 **工作时间段** 中，你可以 **至多** 连续工作 `sessionTime` 个小时，然后休息一会儿。

你需要按照如下条件完成给定任务：
- 如果你在某一个时间段开始一个任务，你需要在 **同一个** 时间段完成它。
- 完成一个任务后，你可以 **立马** 开始一个新的任务。
- 你可以按 **任意顺序** 完成任务。
给你 `tasks` 和 `sessionTime` ，请你按照上述要求，返回完成所有任务所需要的 **最少** 数目的 **工作时间段** 。

测试数据保证 `sessionTime` **大于等于** `tasks[i]` 中的 **最大值** 。

 

 **示例 1：** 

```
输入：tasks = [1,2,3], sessionTime = 3
输出：2
解释：你可以在两个工作时间段内完成所有任务。
- 第一个工作时间段：完成第一和第二个任务，花费 1 + 2 = 3 小时。
- 第二个工作时间段：完成第三个任务，花费 3 小时。

```
 **示例 2：** 

```
输入：tasks = [3,1,3,1,1], sessionTime = 8
输出：2
解释：你可以在两个工作时间段内完成所有任务。
- 第一个工作时间段：完成除了最后一个任务以外的所有任务，花费 3 + 1 + 3 + 1 = 8 小时。
- 第二个工作时间段，完成最后一个任务，花费 1 小时。

```
 **示例 3：** 

```
输入：tasks = [1,2,3,4,5], sessionTime = 15
输出：1
解释：你可以在一个工作时间段以内完成所有任务。

```


 **提示：** 
-  `n == tasks.length` 
-  `1 <= n <= 14` 
-  `1 <= tasks[i] <= 10` 
-  `max(tasks[i]) <= sessionTime <= 15` 

**标签**
`位运算` `数组` `动态规划` `回溯` `状态压缩` 


##
```python

```
>
# 2002.两个回文子序列长度的最大乘积
[https://leetcode-cn.com/problems/maximum-product-of-the-length-of-two-palindromic-subsequences](https://leetcode-cn.com/problems/maximum-product-of-the-length-of-two-palindromic-subsequences) 
## 原题
给你一个字符串 `s` ，请你找到 `s` 中两个 **不相交回文子序列** ，使得它们长度的 **乘积最大** 。两个子序列在原字符串中如果没有任何相同下标的字符，则它们是 **不相交** 的。

请你返回两个回文子序列长度可以达到的 **最大乘积** 。

 **子序列** 指的是从原字符串中删除若干个字符（可以一个也不删除）后，剩余字符不改变顺序而得到的结果。如果一个字符串从前往后读和从后往前读一模一样，那么这个字符串是一个 **回文字符串** 。

 

 **示例 1：** 

<img alt="example-1" src="https://assets.leetcode.com/uploads/2021/08/24/two-palindromic-subsequences.png" style="width: 550px; height: 124px;">

```
输入：s = "leetcodecom"
输出：9
解释：最优方案是选择 "ete" 作为第一个子序列，"cdc" 作为第二个子序列。
它们的乘积为 3 * 3 = 9 。

```
 **示例 2：** 

```
输入：s = "bb"
输出：1
解释：最优方案为选择 "b" （第一个字符）作为第一个子序列，"b" （第二个字符）作为第二个子序列。
它们的乘积为 1 * 1 = 1 。

```
 **示例 3：** 

```
输入：s = "accbcaxxcxx"
输出：25
解释：最优方案为选择 "accca" 作为第一个子序列，"xxcxx" 作为第二个子序列。
它们的乘积为 5 * 5 = 25 。

```


 **提示：** 
-  `2 <= s.length <= 12` 
-  `s` 只含有小写英文字母。

**标签**
`位运算` `字符串` `动态规划` `回溯` `状态压缩` 


##
```python

```
>
# 2014.重复 K 次的最长子序列
[https://leetcode-cn.com/problems/longest-subsequence-repeated-k-times](https://leetcode-cn.com/problems/longest-subsequence-repeated-k-times) 
## 原题
给你一个长度为 `n` 的字符串 `s` ，和一个整数 `k` 。请你找出字符串 `s` 中 **重复** `k` 次的 **最长子序列** 。

 **子序列** 是由其他字符串删除某些（或不删除）字符派生而来的一个字符串。

如果 `seq * k` 是 `s` 的一个子序列，其中 `seq * k` 表示一个由 `seq` 串联 `k` 次构造的字符串，那么就称 `seq` **** 是字符串 `s` 中一个 **重复 `k` 次** 的子序列。
- 举个例子， `"bba"` 是字符串 `"bababcba"` 中的一个重复 `2` 次的子序列，因为字符串 `"bbabba"` 是由 `"bba"` 串联 `2` 次构造的，而 `"bbabba"` 是字符串 `" ***b*** a ***bab*** c ***ba*** "` 的一个子序列。
返回字符串 `s` 中 **重复 k 次的最长子序列** 。如果存在多个满足的子序列，则返回 **字典序最大** 的那个。如果不存在这样的子序列，返回一个 **空** 字符串。

 

 **示例 1：** 

<img alt="example 1" src="https://assets.leetcode.com/uploads/2021/08/30/longest-subsequence-repeat-k-times.png" style="width: 457px; height: 99px;" />

```

输入：s = "letsleetcode", k = 2
输出："let"
解释：存在两个最长子序列重复 2 次：let" 和 "ete" 。
"let" 是其中字典序最大的一个。

```
 **示例 2：** 

```

输入：s = "bb", k = 2
输出："b"
解释：重复 2 次的最长子序列是 "b" 。

```
 **示例 3：** 

```

输入：s = "ab", k = 2
输出：""
解释：不存在重复 2 次的最长子序列。返回空字符串。

```
 **示例 4：** 

```

输入：s = "bbabbabbbbabaababab", k = 3
输出："bbbb"
解释：在 "bbabbabbbbabaababab" 中重复 3 次的最长子序列是 "bbbb" 。

```


 **提示：** 
-  `n == s.length` 
-  `2 <= k <= 2000` 
-  `2 <= n < k * 8` 
-  `s` 由小写英文字母组成

**标签**
`贪心` `字符串` `回溯` `计数` `枚举` 


##
```python

```
>
# 2044.统计按位或能得到最大值的子集数目
[https://leetcode-cn.com/problems/count-number-of-maximum-bitwise-or-subsets](https://leetcode-cn.com/problems/count-number-of-maximum-bitwise-or-subsets) 
## 原题
给你一个整数数组 `nums` ，请你找出 `nums` 子集 **按位或** 可能得到的 **** **最大值** ，并返回按位或能得到最大值的 **不同非空子集的数目** 。

如果数组 `a` 可以由数组 `b` 删除一些元素（或不删除）得到，则认为数组 `a` 是数组 `b` 的一个 **子集** 。如果选中的元素下标位置不一样，则认为两个子集 **不同** 。

对数组 `a` 执行 **按位或** ，结果等于 `a[0] **OR** a[1] **OR** ... **OR** a[a.length - 1]` （下标从 **0** 开始）。

 

 **示例 1：** 

```

输入：nums = [3,1]
输出：2
解释：子集按位或能得到的最大值是 3 。有 2 个子集按位或可以得到 3 ：
- [3]
- [3,1]

```
 **示例 2：** 

```

输入：nums = [2,2,2]
输出：7
解释：[2,2,2] 的所有非空子集的按位或都可以得到 2 。总共有 2^3 - 1 = 7 个子集。

```
 **示例 3：** 

```

输入：nums = [3,2,1,5]
输出：6
解释：子集按位或可能的最大值是 7 。有 6 个子集按位或可以得到 7 ：
- [3,5]
- [3,1,5]
- [3,2,5]
- [3,2,1,5]
- [2,5]
- [2,1,5]
```


 **提示：** 
-  `1 <= nums.length <= 16` 
-  `1 <= nums[i] <= 10^5` 

**标签**
`位运算` `数组` `回溯` 


##
```python

```
>
# 2048.下一个更大的数值平衡数
[https://leetcode-cn.com/problems/next-greater-numerically-balanced-number](https://leetcode-cn.com/problems/next-greater-numerically-balanced-number) 
## 原题
如果整数 `x` 满足：对于每个数位 `d` ，这个数位 **恰好** 在 `x` 中出现 `d` 次。那么整数 `x` 就是一个 **数值平衡数** 。

给你一个整数 `n` ，请你返回 **严格大于** `n` 的 **最小数值平衡数** 。

 

 **示例 1：** 

```

输入：n = 1
输出：22
解释：
22 是一个数值平衡数，因为：
- 数字 2 出现 2 次 
这也是严格大于 1 的最小数值平衡数。

```
 **示例 2：** 

```

输入：n = 1000
输出：1333
解释：
1333 是一个数值平衡数，因为：
- 数字 1 出现 1 次。
- 数字 3 出现 3 次。 
这也是严格大于 1000 的最小数值平衡数。
注意，1022 不能作为本输入的答案，因为数字 0 的出现次数超过了 0 。
```
 **示例 3：** 

```

输入：n = 3000
输出：3133
解释：
3133 是一个数值平衡数，因为：
- 数字 1 出现 1 次。
- 数字 3 出现 3 次。 
这也是严格大于 3000 的最小数值平衡数。

```


 **提示：** 
-  `0 <= n <= 10^6` 

**标签**
`数学` `回溯` `枚举` 


##
```python

```
>
# 2056.棋盘上有效移动组合的数目
[https://leetcode-cn.com/problems/number-of-valid-move-combinations-on-chessboard](https://leetcode-cn.com/problems/number-of-valid-move-combinations-on-chessboard) 
## 原题
有一个 `8 x 8` 的棋盘，它包含 `n` 个棋子（棋子包括车，后和象三种）。给你一个长度为 `n` 的字符串数组 `pieces` ，其中 `pieces[i]` 表示第 `i` 个棋子的类型（车，后或象）。除此以外，还给你一个长度为 `n` 的二维整数数组 `positions` ，其中 `positions[i] = [r<sub>i</sub>, c<sub>i</sub>]` 表示第 `i` 个棋子现在在棋盘上的位置为 `(r<sub>i</sub>, c<sub>i</sub>)` ，棋盘下标从 **1** 开始。

棋盘上每个棋子都可以移动 <b>至多一次</b> 。每个棋子的移动中，首先选择移动的 **方向** ，然后选择 **移动的步数** ，同时你要确保移动过程中棋子不能移到棋盘以外的地方。棋子需按照以下规则移动：
- 车可以 **水平或者竖直** 从 `(r, c)` 沿着方向 `(r+1, c)` ， `(r-1, c)` ， `(r, c+1)` 或者 `(r, c-1)` 移动。
- 后可以 **水平竖直或者斜对角** 从 `(r, c)` 沿着方向 `(r+1, c)` ， `(r-1, c)` ， `(r, c+1)` ， `(r, c-1)` ， `(r+1, c+1)` ， `(r+1, c-1)` ， `(r-1, c+1)` ， `(r-1, c-1)` 移动。
- 象可以 **斜对角** 从 `(r, c)` 沿着方向 `(r+1, c+1)` ， `(r+1, c-1)` ， `(r-1, c+1)` ， `(r-1, c-1)` 移动。
 **移动组合** 包含所有棋子的 **移动** 。每一秒，每个棋子都沿着它们选择的方向往前移动 **一步** ，直到它们到达目标位置。所有棋子从时刻 `0` 开始移动。如果在某个时刻，两个或者更多棋子占据了同一个格子，那么这个移动组合 **不有效** 。

请你返回 **有效** 移动组合的数目。

 **注意：** 
- 初始时， **不会有两个棋子** 在 **同一个位置 。** 
- 有可能在一个移动组合中，有棋子不移动。
- 如果两个棋子 **直接相邻** 且两个棋子下一秒要互相占据对方的位置，可以将它们在同一秒内 **交换位置** 。


 **示例 1:** 

<img alt="" src="https://assets.leetcode.com/uploads/2021/09/23/a1.png" style="width: 215px; height: 215px;" />

```

输入：pieces = ["rook"], positions = [[1,1]]
输出：15
解释：上图展示了棋子所有可能的移动。

```
 **示例 2：** 

<img alt="" src="https://assets.leetcode.com/uploads/2021/09/23/a2.png" style="width: 215px; height: 215px;" />

```

输入：pieces = ["queen"], positions = [[1,1]]
输出：22
解释：上图展示了棋子所有可能的移动。

```
 **示例 3:** 

<img alt="" src="https://assets.leetcode.com/uploads/2021/09/23/a3.png" style="width: 214px; height: 215px;" />

```

输入：pieces = ["bishop"], positions = [[4,3]]
输出：12
解释：上图展示了棋子所有可能的移动。

```
 **示例 4:** 

<img alt="" src="https://assets.leetcode.com/uploads/2021/09/23/a4.png" style="width: 216px; height: 219px;" />

```

输入：pieces = ["rook","rook"], positions = [[1,1],[8,8]]
输出：223
解释：每个车有 15 种移动，所以总共有 15 * 15 = 225 种移动组合。
但是，有两个是不有效的移动组合：
- 将两个车都移动到 (8, 1) ，会导致它们在同一个格子相遇。
- 将两个车都移动到 (1, 8) ，会导致它们在同一个格子相遇。
所以，总共有 225 - 2 = 223 种有效移动组合。
注意，有两种有效的移动组合，分别是一个车在 (1, 8) ，另一个车在 (8, 1) 。
即使棋盘状态是相同的，这两个移动组合被视为不同的，因为每个棋子移动操作是不相同的。

```
 **示例 5：** 

<img alt="" src="https://assets.leetcode.com/uploads/2021/09/23/a5.png" style="width: 214px; height: 213px;" />

```

输入：pieces = ["queen","bishop"], positions = [[5,7],[3,4]]
输出：281
解释：总共有 12 * 24 = 288 种移动组合。
但是，有一些不有效的移动组合：
- 如果后停在 (6, 7) ，它会阻挡象到达 (6, 7) 或者 (7, 8) 。
- 如果后停在 (5, 6) ，它会阻挡象到达 (5, 6) ，(6, 7) 或者 (7, 8) 。
- 如果象停在 (5, 2) ，它会阻挡后到达 (5, 2) 或者 (5, 1) 。
在 288 个移动组合当中，281 个是有效的。

```


 **提示：** 
-  `n == pieces.length` 
-  `n == positions.length` 
-  `1 <= n <= 4` 
-  `pieces` 只包含字符串 `"rook"` ， `"queen"` 和 `"bishop"` 。
- 棋盘上总共最多只有一个后。
-  `1 <= x<sub>i</sub>, y<sub>i</sub> <= 8` 
- 每一个 `positions[i]` 互不相同。

**标签**
`数组` `字符串` `回溯` `模拟` 


##
```python

```
>
# 2065.最大化一张图中的路径价值
[https://leetcode-cn.com/problems/maximum-path-quality-of-a-graph](https://leetcode-cn.com/problems/maximum-path-quality-of-a-graph) 
## 原题
给你一张 **无向** 图，图中有 `n` 个节点，节点编号从 `0` 到 `n - 1` （ **都包括** ）。同时给你一个下标从 **0** 开始的整数数组 `values` ，其中 `values[i]` 是第 `i` 个节点的 **价值** 。同时给你一个下标从 **0** 开始的二维整数数组 `edges` ，其中 `edges[j] = [u<sub>j</sub>, v<sub>j</sub>, time<sub>j</sub>]` 表示节点 `u<sub>j</sub>` 和 `v<sub>j</sub>` 之间有一条需要 `time<sub>j</sub>` 秒才能通过的无向边。最后，给你一个整数 `maxTime` 。

 **合法路径** 指的是图中任意一条从节点 `0` 开始，最终回到节点 `0` ，且花费的总时间 **不超过** `maxTime` 秒的一条路径。你可以访问一个节点任意次。一条合法路径的 <b>价值</b> 定义为路径中 **不同节点** 的价值 **之和** （每个节点的价值 **至多** 算入价值总和中一次）。

请你返回一条合法路径的 **最大** 价值。

 **注意：** 每个节点 **至多** 有 **四条** 边与之相连。

 

 **示例 1：** 

<img alt="" src="https://assets.leetcode.com/uploads/2021/10/19/ex1drawio.png" style="width: 269px; height: 170px;" />

```

输入：values = [0,32,10,43], edges = [[0,1,10],[1,2,15],[0,3,10]], maxTime = 49
输出：75
解释：
一条可能的路径为：0 -> 1 -> 0 -> 3 -> 0 。总花费时间为 10 + 10 + 10 + 10 = 40 <= 49 。
访问过的节点为 0 ，1 和 3 ，最大路径价值为 0 + 32 + 43 = 75 。

```
 **示例 2：** 

<img alt="" src="https://assets.leetcode.com/uploads/2021/10/19/ex2drawio.png" style="width: 269px; height: 170px;" />

```

输入：values = [5,10,15,20], edges = [[0,1,10],[1,2,10],[0,3,10]], maxTime = 30
输出：25
解释：
一条可能的路径为：0 -> 3 -> 0 。总花费时间为 10 + 10 = 20 <= 30 。
访问过的节点为 0 和 3 ，最大路径价值为 5 + 20 = 25 。

```
 **示例 3：** 

<img alt="" src="https://assets.leetcode.com/uploads/2021/10/19/ex31drawio.png" style="width: 236px; height: 170px;" />

```

输入：values = [1,2,3,4], edges = [[0,1,10],[1,2,11],[2,3,12],[1,3,13]], maxTime = 50
输出：7
解释：
一条可能的路径为：0 -> 1 -> 3 -> 1 -> 0 。总花费时间为 10 + 13 + 13 + 10 = 46 <= 50 。
访问过的节点为 0 ，1 和 3 ，最大路径价值为 1 + 2 + 4 = 7 。
```
 **示例 4：** 

 **<img alt="" src="https://assets.leetcode.com/uploads/2021/10/21/ex4drawio.png" style="width: 270px; height: 71px;" />** 

```

输入：values = [0,1,2], edges = [[1,2,10]], maxTime = 10
输出：0
解释：
唯一一条路径为 0 。总花费时间为 0 。
唯一访问过的节点为 0 ，最大路径价值为 0 。

```


 **提示：** 
-  `n == values.length` 
-  `1 <= n <= 1000` 
-  `0 <= values[i] <= 10^8` 
-  `0 <= edges.length <= 2000` 
-  `edges[j].length == 3` 
-  `0 <= u<sub>j </sub>< v<sub>j</sub> <= n - 1` 
-  `10 <= time<sub>j</sub>, maxTime <= 100` 
-  `[u<sub>j</sub>, v<sub>j</sub>]` 所有节点对 **互不相同** 。
- 每个节点 **至多有四条** 边。
- 图可能不连通。

**标签**
`图` `数组` `回溯` 


##
```python

```
>
# 2151.基于陈述统计最多好人数
[https://leetcode-cn.com/problems/maximum-good-people-based-on-statements](https://leetcode-cn.com/problems/maximum-good-people-based-on-statements) 
## 原题
游戏中存在两种角色：
-  **好人** ：该角色只说真话。
-  **坏人** ：该角色可能说真话，也可能说假话。
给你一个下标从 **0** 开始的二维整数数组 `statements` ，大小为 `n x n` ，表示 `n` 个玩家对彼此角色的陈述。具体来说， `statements[i][j]` 可以是下述值之一：
-  `0` 表示 `i` 的陈述认为 `j` 是 **坏人** 。
-  `1` 表示 `i` 的陈述认为 `j` 是 **好人** 。
-  `2` 表示 `i` 没有对 `j` 作出陈述。
另外，玩家不会对自己进行陈述。形式上，对所有 `0 <= i < n` ，都有 `statements[i][i] = 2` 。

根据这 `n` 个玩家的陈述，返回可以认为是 **好人** 的 **最大** 数目。

 

 **示例 1：** 
<img alt="" src="https://assets.leetcode.com/uploads/2022/01/15/logic1.jpg" style="width: 600px; height: 262px;">
```
输入：statements = [[2,1,2],[1,2,2],[2,0,2]]
输出：2
解释：每个人都做一条陈述。
- 0 认为 1 是好人。
- 1 认为 0 是好人。
- 2 认为 1 是坏人。
以 2 为突破点。
- 假设 2 是一个好人：
    - 基于 2 的陈述，1 是坏人。
    - 那么可以确认 1 是坏人，2 是好人。
    - 基于 1 的陈述，由于 1 是坏人，那么他在陈述时可能：
        - 说真话。在这种情况下会出现矛盾，所以假设无效。
        - 说假话。在这种情况下，0 也是坏人并且在陈述时说假话。
    - 在认为 2 是好人的情况下，这组玩家中只有一个好人。
- 假设 2 是一个坏人：
    - 基于 2 的陈述，由于 2 是坏人，那么他在陈述时可能：
        - 说真话。在这种情况下，0 和 1 都是坏人。
            - 在认为 2 是坏人但说真话的情况下，这组玩家中没有一个好人。
        - 说假话。在这种情况下，1 是好人。
            - 由于 1 是好人，0 也是好人。
            - 在认为 2 是坏人且说假话的情况下，这组玩家中有两个好人。
在最佳情况下，至多有两个好人，所以返回 2 。
注意，能得到此结论的方法不止一种。

```
 **示例 2：** 
<img alt="" src="https://assets.leetcode.com/uploads/2022/01/15/logic2.jpg" style="width: 600px; height: 262px;">
```
输入：statements = [[2,0],[0,2]]
输出：1
解释：每个人都做一条陈述。
- 0 认为 1 是坏人。
- 1 认为 0 是坏人。
以 0 为突破点。
- 假设 0 是一个好人：
    - 基于与 0 的陈述，1 是坏人并说假话。
    - 在认为 0 是好人的情况下，这组玩家中只有一个好人。
- 假设 0 是一个坏人：
    - 基于 0 的陈述，由于 0 是坏人，那么他在陈述时可能：
        - 说真话。在这种情况下，0 和 1 都是坏人。
            - 在认为 0 是坏人但说真话的情况下，这组玩家中没有一个好人。
        - 说假话。在这种情况下，1 是好人。
            - 在认为 0 是坏人且说假话的情况下，这组玩家中只有一个好人。
在最佳情况下，至多有一个好人，所以返回 1 。 
注意，能得到此结论的方法不止一种。

```


 **提示：** 
-  `n == statements.length == statements[i].length` 
-  `2 <= n <= 15` 
-  `statements[i][j]` 的值为 `0` 、 `1` 或 `2` 
-  `statements[i][i] == 2` 

**标签**
`位运算` `数组` `回溯` `枚举` 


##
```python

```
>
# 2152
##
```python

```
>

