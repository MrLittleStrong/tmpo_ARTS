##[18. 4Sum](https://leetcode-cn.com/problems/4sum/)

###Introduction

Given an array nums of n integers and an integer target, are there elements a, b, c, and d in nums such that a + b + c + d = target? Find all unique quadruplets in the array which gives the sum of target.

Note:

The solution set must not contain duplicate quadruplets.

Example:

```
Given array nums = [1, 0, -1, 0, -2, 2], and target = 0.

A solution set is:
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]
```



###Solution

Be similar to 3sum solution. Make one more loop outside of 3sum solution. Two loops make sure two number, as for  the other two number, we use two pointer to detect. To prevent same answer, we add a condition that is when a pointer moves to a new position, it needs to compare to the last value,  and if they are the same, this pointer continues to move.

###Code

```swift
class Solution {
    func fourSum(_ nums: [Int], _ target: Int) -> [[Int]] {
       let len = nums.count
        if len<4{ return []}
        var resultArray = [[Int]]()
        let sortedNums =  nums.sorted()
        print(sortedNums)
        for i in 0 ..< len-3 {
            
            if i > 0 && sortedNums[i] == sortedNums[i-1]{
                
                continue
            }
            if sortedNums[i]+sortedNums[i+1]+sortedNums[i+2]+sortedNums[i+3]>target{
               
                continue
            }
            if sortedNums[i] + sortedNums[len-1] + sortedNums[len-2] + sortedNums[len-3]<target{
                
                continue
            }
            for j in i+1 ..< len-2 {
                
                if j - i>1 && sortedNums[j]==sortedNums[j-1]{
                    
                    continue
                }
                if sortedNums[i]+sortedNums[j]+sortedNums[j+1]+sortedNums[j+2]>target{
                   
                    continue
                }
                if sortedNums[i]+sortedNums[j]+sortedNums[len-1]+sortedNums[len-2]<target{
                   
                    continue
                }
                var left = j+1, right = len-1
                while left < right{
                    if left>j + 1 && sortedNums[left]==sortedNums[left-1]{
                        left+=1
                        continue
                    }
                    if right<len-1 && sortedNums[right]==sortedNums[right+1]{
                        right-=1
                        continue
                    }
                   let temp = sortedNums[i]+sortedNums[j]+sortedNums[left]+sortedNums[right]
                    if temp == target{
                        resultArray.append([sortedNums[i],sortedNums[j],sortedNums[left],sortedNums[right]])
                        left+=1
                        right-=1
                    } else if temp < target {
                        left+=1
                    } else {
                        right-=1
                    }
                    
                }
            }
        }
        return resultArray
    }
}
```

###Performance

1. Time Complexity: 

   O(n^3)   

2. Space Complexity:

   O(1)

   

### Summary

It's my first time to use swift, a lot of compiling errors. The most difficult part is to prevent 