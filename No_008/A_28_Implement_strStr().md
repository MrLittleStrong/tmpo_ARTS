## [28. Implement strStr()](https://leetcode-cn.com/problems/implement-strstr/)

###Introduction

Return the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.

**Example 1:**

```
Input: haystack = "hello", needle = "ll"
Output: 2
```

**Example 2:**

```
Input: haystack = "aaaaa", needle = "bba"
Output: -1
```

**Clarification:**

What should we return when needle is an empty string? This is a great question to ask during an interview.

For the purpose of this problem, we will return 0 when needle is an empty string. This is consistent to C's strstr() and Java's indexOf().

###Solution

KMP or violent search.

###Code

ViolentSearch below:

```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        if needle is None or len(needle)==0:
            return 0
        else:
            index = -1
            for i in range(0,len(haystack)-len(needle)+1):
                j=0
                while(j<len(needle)):
                    if(haystack[i+j]!=needle[j]):
                        break
                    j+=1
                if j == len(needle):
                    index = i
                    break
            return index
```

###Performance

1. Time Complexity: 

   O(m*n)

2. Space Complexity:

   O(1)

###Better Solution

KMP

```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        if (needle is None or haystack is None or len(needle)==0):
            return 0
        elif len(haystack)<len(needle):
            return -1
        def getNext(needle:str):
            j = 0
            next = [0]*len(needle)
            for i in range(1,len(needle)):
                while j>0 and needle[j]!=needle[i]:
                    j = next[j-1]
                if needle[j]== needle[i]:
                    j+=1
                next[i]=j
            return next
        next = getNext(needle)
        m = 0
        for n in range(len(haystack)):
            while m>0 and haystack[n]!= needle[m]:
                m=next[m-1]
            if haystack[n]==needle[m] :
                m+=1
            if m == len(needle):
                return n+1-m
        
        return -1
```

However kmp cost more time and memory in console.  Considering kmp is more efficient when strings are long enough. 

