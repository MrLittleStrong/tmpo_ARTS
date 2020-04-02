## [29. Divide Two Integers](https://leetcode-cn.com/problems/divide-two-integers/)

###Introduction

Given two integers dividend and divisor, divide two integers without using multiplication, division and mod operator.

Return the quotient after dividing dividend by divisor.

The integer division should truncate toward zero.

**Example 1:**

```
Input: dividend = 10, divisor = 3
Output: 3
```

**Example 2:**

```
Input: dividend = 7, divisor = -3
Output: -2
```

**Clarification:**

Both dividend and divisor will be 32-bit signed integers.
The divisor will never be 0.
Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−2^31,  2^31 − 1]. For the purpose of this problem, assume that your function returns 2^31 − 1 when the division result overflows.

###Solution

Dividor bit operation << can be close to divident by exponential increasing.

###Code

```python
class Solution:
    def divide(self, dividend: int, divisor: int) -> int:
        if dividend == -2147483648 and divisor == -1:
            return 2147483647
        tag = 0
        if (dividend > 0 and divisor < 0) or(dividend < 0 and divisor > 0):
            tag = 1
        result = 0
        if dividend < 0:
            dividend = - dividend
        if divisor < 0:
            divisor = - divisor
        while dividend >= divisor :
            tmp_result = 1
            tmp_divisor = divisor
            while dividend >= tmp_divisor<<1:
                tmp_divisor = tmp_divisor<<1
                tmp_result = tmp_result<<1
            dividend -= tmp_divisor
            result +=tmp_result
        if tag >0 :
            result = - result
        return result 
```

###Performance

1. Time Complexity: 

   O(logn)

2. Space Complexity:

   O(1)

