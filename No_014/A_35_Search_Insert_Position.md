## [35. Search Insert Position](https://leetcode-cn.com/problems/search-insert-position/)

###Introduction

Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You may assume no duplicates in the array.

**Example 1:**

```
Input: [1,3,5,6], 5
Output: 2
```

**Example 2:**

```
Input: [1,3,5,6], 2
Output: 1
```

**Example 3:**

```
Input: [1,3,5,6], 7
Output: 4
```

**Example 4:**

```
Input: [1,3,5,6], 0
Output: 0
```



###Solution

二分查找的变种来做，主要是边界值考虑需要花费点心思。

###Code

```python
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        left = 0
        right = len(nums)-1
        mid = 0
        while(left<=right):
            mid = int((left+right)/2)
            if(target == nums[mid]):
                return mid
            elif(target<nums[mid]):
                if target<nums[left]:
                    return left
                else :
                    right = mid-1
            elif(target>nums[mid]):
                if target>nums[right]:
                    return right +1
                else:
                    left = mid+1
        return -1
```

###Performance

1. Time Complexity: 

   O(logn)

2. Space Complexity:

   O(1)