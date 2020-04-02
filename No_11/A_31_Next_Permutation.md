## [31. Next Permutation](https://leetcode-cn.com/problems/next-permutation/submissions/)

###Introduction

Implement next permutation, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).

The replacement must be in-place and use only constant extra memory.

Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.

**Example 1:**

```
1,2,3 → 1,3,2
3,2,1 → 1,2,3
1,1,5 → 1,5,1
```

###Solution

分情况考虑，一种情况是完全有大到小排好序的数字组成的数组，这种情况只需要倒序输出即可；另一种情况为由左向右有上坡的情况，这种情况需要从后往前来看，将最后一个上坡点，在上坡点后找到比他大的最小的数，两个位置进行互换调整之后，再将上坡点位置后面的数组按照由小到大排序。 

###Code

```python
class Solution:
    def nextPermutation(self, nums: List[int]) -> None:
        findTop = -1
        maxNumTail = nums[len(nums)-1]
        for i in range(len(nums)-2,-1,-1):
            if nums[i]>=maxNumTail:
                maxNumTail = nums[i]
            else:
                findTop = i
                break
        if findTop == -1:
            for i in range(0,int(len(nums)/2)):
                temp = nums[i]
                nums[i]=nums[len(nums)-1-i]
                nums[len(nums)-1-i]=temp
        else:
            findMinTopIndex = 0
            findMinTop = maxNumTail
            for i in range(findTop+1,len(nums)):
                if nums[i]>nums[findTop] and findMinTop>=nums[i]:
                    findMinTop = nums[i]
                    findMinTopIndex = i
            temp = nums[findTop]
            nums[findTop]=nums[findMinTopIndex]
            nums[findMinTopIndex]=temp
            for i in range(findTop+1,len(nums)):
                for j in range(findTop+1,len(nums)-1):
                    if(nums[j]>nums[j+1]):
                        temp = nums[j+1]
                        nums[j+1]=nums[j]
                        nums[j]=temp
```

###Performance

1. Time Complexity: 

   O(n^2)

2. Space Complexity:

   O(1)

### Better solution

由于我们是由后向前来看的，所以上坡点后面一定是爬坡的过程，那么在找互换位置的时候，只需要由前向后挨个进行比较，出现比上坡点小的数的前一位就正好是互换位置了。

同理，换了位置之后，上坡点后面一定是严格的爬坡过程，只需要将上坡点后面的数字逆向输出就行了。这样时间复杂度能降到O(n)

```python
class Solution:
    def nextPermutation(self, nums: List[int]) -> None:
        findTop = -1
        maxNumTail = nums[len(nums)-1]
        for i in range(len(nums)-2,-1,-1):
            if nums[i]>=maxNumTail:
                maxNumTail = nums[i]
            else:
                findTop = i
                break
        if findTop == -1:
            for i in range(0,int(len(nums)/2)):
                temp = nums[i]
                nums[i]=nums[len(nums)-1-i]
                nums[len(nums)-1-i]=temp
        else:
            findMinIndex = findTop +1
            for i in range(findTop+1,len(nums)):
                if nums[i]<=nums[findTop]:
                    break
                else:
                    findMinIndex = i
            temp = nums[findMinIndex]
            nums[findMinIndex]=nums[findTop]
            nums[findTop]=temp
            for i in range(0,int((len(nums)-findTop-1)/2)):
                temp = nums[findTop+1+i]
                nums[findTop+1+i]=nums[len(nums)-1-i]
                nums[len(nums)-1-i]=temp
```

