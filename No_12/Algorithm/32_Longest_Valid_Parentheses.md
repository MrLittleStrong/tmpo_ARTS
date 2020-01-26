## [32. Longest Valid Parentheses](https://leetcode-cn.com/problems/longest-valid-parentheses/)

###Introduction

Given a string containing just the characters '(' and ')', find the length of the longest valid (well-formed) parentheses substring.

**Example 1:**

```
Input: "(()"
Output: 2
Explanation: The longest valid parentheses substring is "()"
```

###Solution

考虑使用动态规划来做。

如果一个")"左边是一个"("，那么 dp[*i*]=dp[*i*−2]+2

如果左边是")"，那需要看他左边是不是一个有效字符串，而且需要找到子串的左边是否是"("，如果是的话，还需要加上"("左边的dp的值。

###Code

```python
class Solution:
    def longestValidParentheses(self, s: str) -> int:
        maxLength = 0
        dp = [0]*len(s)
        for i in range(1,len(s)):
            if s[i] == ')':
                if s[i-1] == '(':
                    if i>=2:
                        dp[i]=dp[i-2]+2
                    else:
                        dp[i]=2
                elif i - dp[i - 1] > 0 and s[i - dp[i - 1] - 1] == '(' :
                    if i - dp[i - 1] >= 2:
                        dp[i] = dp[i - 1] +  dp[i - dp[i - 1] - 2] + 2
                    else:
                        dp[i] = dp[i - 1] +  2
                maxLength = max(maxLength, dp[i])
        return maxLengt
```

###Performance

1. Time Complexity: 

   O(n)

2. Space Complexity:

   O(n)
