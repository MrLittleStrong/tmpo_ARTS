## [40. Combination Sum II](https://leetcode-cn.com/problems/combination-sum-ii/)

### Introduction

Given a collection of candidate numbers (candidates) and a target number (target), find all unique combinations in candidates where the candidate numbers sums to target.

Each number in `candidates` may only be used **once** in the combination.

**Note**:

- All numbers (including target) will be positive integers.
- The solution set must not contain duplicate combinations.

**Example 1:**

```
Input: candidates = [10,1,2,7,6,1,5], target = 8,
A solution set is:
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]
```

**Example2:**

```
Input: candidates = [2,5,2,1,2], target = 5,
A solution set is:
[
  [1,2,2],
  [5]
]
```

### Solution

回溯法。思路为做减法的递归。与39类似，额外的部分就是如果选过这个数，则不能继续选择这个数字了。除此之外，如果当前的数和上一个数相同，也需要将其剪除掉。

### Code

```python
class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        size = len(candidates)
        if size == 0:
            return []

        # 剪枝是为了提速，在本题非必需
        candidates.sort()
        # 在遍历的过程中记录路径，它是一个栈
        path = []
        res = []
        # 注意要传入 size ，在 range 中， size 取不到
        self.__dfs(candidates, 0, size, path, res, target)
        return res

    def __dfs(self, candidates, begin, size, path, res, target):
        # 先写递归终止的情况
        if target == 0:
            # Python 中可变对象是引用传递，因此需要将当前 path 里的值拷贝出来
            # 或者使用 path.copy()
            res.append(path[:])
            return

        for index in range(begin, size):
            residue = target - candidates[index]
            # “剪枝”操作，不必递归到下一层，并且后面的分支也不必执行
            if residue < 0:
                break
            if index>begin and candidates[index-1]==candidates[index]:
                continue
            path.append(candidates[index])
            # 因为下一层不能比上一层还小，起始索引还从 index 开始
            self.__dfs(candidates, index+1, size, path, res, residue)
            path.pop()
```

### Performance

1. Time Complexity:

   这个会比较难分析，需要结合target和数组中数字的具体情况

2. Space Complexity:

   同上