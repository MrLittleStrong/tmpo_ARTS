## [27. Remove Element](https://leetcode-cn.com/problems/remove-element/)

###Introduction

Given an array nums and a value val, remove all instances of that value in-place and return the new length.

Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.

The order of elements can be changed. It doesn't matter what you leave beyond the new length.

**Example 1:**

```
Given nums = [3,2,2,3], val = 3,

Your function should return length = 2, with the first two elements of nums being 2.

It doesn't matter what you leave beyond the returned length.
```

**Example 2:**

```
Given nums = [0,1,2,2,3,0,4,2], val = 2,

Your function should return length = 5, with the first five elements of nums containing 0, 1, 3, 0, and 4.

Note that the order of those five elements can be arbitrary.

It doesn't matter what values are set beyond the returned length.
```

**Clarification:**

Confused why the returned value is an integer but your answer is an array?

Note that the input array is passed in by reference, which means modification to the input array will be known to the caller as well.

Internally you can think of this:

###Solution

Loop once.

###Code

```python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        i = 0
        while(i<len(nums)):
            if nums[i]==val:
                nums.pop(i)
            else:
                i+=1
        return len(nums)
```

###Performance

1. Time Complexity: 

   O(n)

2. Space Complexity:

   O(1)


###Better Solution

```python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        i = 0
        for j in range(0, len(nums)):
          if nums[j]!=val:
            nums[i]=nums[j]
            i+=1
        return i
```

I used python function. But this solution is more 'algorithm' which used two pointers. 



