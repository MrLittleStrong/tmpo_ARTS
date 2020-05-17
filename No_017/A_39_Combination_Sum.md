## [39. Combination Sum](https://leetcode-cn.com/problems/combination-sum/)

### Introduction

Given a set of candidate numbers (candidates) (without duplicates) and a target number (target), find all unique combinations in candidates where the candidate numbers sums to target.

The same repeated number may be chosen from candidates unlimited number of times.

**Note**:

- All numbers (including target) will be positive integers.
- The solution set must not contain duplicate combinations.

**Example 1:**

```
Input: candidates = [2,3,6,7], target = 7,
A solution set is:
[
  [7],
  [2,2,3]
]
```

**Example2:**

```
Input: candidates = [2,3,5], target = 8,
A solution set is:
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]
```

### Solution

回溯法。思路为做减法的递归。如果为0就将这个路径加入到结果集里，如果小于0，则跳过。核心是需要剪除掉重复的选项。这里就首先要把给到的数字先做个排序，然后减法的时候需要在遍历的时候只允许一个方向，例如路径里有[3,4],则不允许再遍历3形成[3,4,3]的情况

### Code

```python
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        candidates.sort()
        res = []
        def _ss(candidates:List[int], index: int, path: List[int], target: int):
            if target == 0:
                res.append(path[:])
                return
            elif target < 0:
                return
            for i in range(index, len(candidates)):
                newPath = path[:]
                newTarget = target
                newTarget -= candidates[i]
                newPath.append(candidates[i])
                _ss(candidates, i, newPath[:], newTarget)
        _ss(candidates, 0, [], target)
        return res
        
```

### Performance

1. Time Complexity:

   这个会比较难分析，需要结合target和数组中数字的具体情况

2. Space Complexity:

   同上