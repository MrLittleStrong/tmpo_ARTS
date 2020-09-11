## [49. Group Anagrams](https://leetcode-cn.com/problems/group-anagrams/)

### Introduction

Given an array of strings strs, group the anagrams together. You can return the answer in any order.

An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

**Example 1:**

```
Input: strs = ["eat","tea","tan","ate","nat","bat"]
Output: [["bat"],["nat","tan"],["ate","eat","tea"]]
```

**Example2:**

```
Input: strs = [""]
Output: [[""]]
```

### Solution

将字符串排序后作为key存起来，

### Code

```python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        dic = {}
        res = []
        for s in strs:
            keys=''.join(sorted(s))
            if keys not in dic:
                dic[keys] = [s]
            else:
                dic[keys].append(s)
        return list(dic.values())
```

### Performance

1. Time Complexity:

   O(NK)

2. Space Complexity:

   O(NK)