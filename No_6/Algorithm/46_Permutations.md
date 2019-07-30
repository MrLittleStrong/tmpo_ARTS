##[46. Permutations](https://leetcode-cn.com/problems/permutations/)

###Introduction

Given a collection of **distinct** integers, return all possible permutations.

Example:

```shell
Input: [1,2,3]
Output:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```

###Solution

[Backtracing algorithms]([https://baike.baidu.com/item/%E5%9B%9E%E6%BA%AF%E7%AE%97%E6%B3%95/9258495](https://baike.baidu.com/item/回溯算法/9258495))

###Code

```python
class Solution(object):
    def permute(self, nums):
        def backtrack(first = 0):
            # if all integers are used up
            if first == n:  
                output.append(nums[:])
            for i in range(first, n):
                # place i-th integer first 
                # in the current permutation
                nums[first], nums[i] = nums[i], nums[first]
                # use next integers to complete the permutations
                backtrack(first + 1)
                # backtrack
                nums[first], nums[i] = nums[i], nums[first]
        
        n = len(nums)
        output = []
        backtrack()
        return output
```

###Performance

1. Time Complexity: 

   I can't do this math. copy from hint. 

   O(∑*k*=1*N**P*(*N*,*k*))

2. Space Complexity:

   O(*N*!)


###Better Solution

```python
class Solution(object):
    def permute(self, nums):
        res = list()
        for x in itertools.permutations(nums):
            x = list(x)
            res.append(x)
        return res
```

Tricky solution .



