## [47. Permutations II](https://leetcode-cn.com/problems/permutations-ii/)

### Introduction

Given a collection of numbers that might contain duplicates, return all possible unique permutations.

**Example 1:**

```
Input: [1,1,2]
Output:
[
  [1,1,2],
  [1,2,1],
  [2,1,1]
]
```

### Solution

数组中可能包含重复的数字，这种情况需要先将数组进行排序，这样才方便剪枝。

通过使用递归来完成回溯

### Code

```python
class Solution:
    def permuteUnique(self, nums: List[int]) -> List[List[int]]:
        def dfs(nums,path, used, res):
            if len(path)== len(nums):
                res.append(path.copy())
                return
            for i in range(len(nums)):
                if not used[i]:
                    if i>0 and nums[i]==nums[i-1] and not used[i-1]:
                        continue
                    path.append(nums[i])
                    used[i]= True
                    dfs(nums, path, used, res)
                    path.pop()
                    used[i]= False

        if len(nums) == 0:
            return []    
        nums.sort()
        used = [False]*len(nums)
        res = []
        path = []
        dfs(nums, path, used, res)
        return res

```

### Performance

1. Time Complexity:

   O(N* N!)

2. Space Complexity:

   O(N * N!)