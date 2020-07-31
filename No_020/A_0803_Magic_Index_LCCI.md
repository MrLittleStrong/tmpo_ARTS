## [面试题 08.03. Magic Index LCCI](https://leetcode-cn.com/problems/magic-index-lcci/)

### Introduction

A magic index in an array A[0...n-1] is defined to be an index such that A[i] = i. Given a sorted array of integers, write a method to find a magic index, if one exists, in array A. If not, return -1. If there are more than one magic index, return the smallest one.

**Example 1:**

```
Input: nums = [0, 2, 3, 4, 5]
Output: 0
```

**Example2:**

```
 Input: nums = [1, 1, 1]
 Output: 1
```

### Solution

很简单地题目，一次遍历就结束

### Code

```python
class Solution:
    def findMagicIndex(self, nums: List[int]) -> int:
        for i in range(0,len(nums)):
            if nums[i]== i:
                return i
        return -1

```

### Performance

1. Time Complexity:

   O(n)

2. Space Complexity:

   O(1)

### Better solution

利用**二分法**，如果没有重复的情况下，将所有的数字都减去自己的下标，符合条件的那个数左边都是负数，自身是0，右边都是正数，很好解。

然而此题是存在重复数的，则原来的方法无法使用了，但是仍然可以通过二分法进行一定程度的剪枝。具体策略如下：

- 从中间找，如果中间满足条件，则右边的都不考虑了。
- 如果没满足条件，先去左边找，左边没有再去右边找。

最坏的情况下也是时间复杂度是O(n)，空间复杂度最坏是O(n)。