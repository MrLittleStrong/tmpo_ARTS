##[22. Generate Parentheses](https://leetcode-cn.com/problems/generate-parentheses/)

###Introduction

Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

For example:

```
Given n = 3.

A solution set is:
[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]
```



###Solution

Consider using stack to generate the legal string. And use a binary tree to help remember push or pop move to avoid repeated solutions.

###Code

```swift
class Solution {
    func generateParenthesis(_ n: Int) -> [String] {
        var result = [String]()
        result.append("(")
        var queueRemain = [n-1]
        var queueStack = [1]
        for _ in 0..<2*n-1 {
            var newArray = [String]()
            var newQueueRemain = [Int]()
            var newQueueStack = [Int]()
            for j in 0..<result.count {
                let str = result[j], remain = queueRemain[j], stack = queueStack[j]
                if remain > 0{
                    let str1 = str + "("
                    let remain1 = remain - 1
                    let stack1 = stack + 1
                    newArray.append(str1)
                    newQueueRemain.append(remain1)
                    newQueueStack.append(stack1)
                }
                if stack > 0 {
                    let str2 = str + ")"
                    let remain2 = remain
                    let stack2 = stack - 1
                    newArray.append(str2)
                    newQueueRemain.append(remain2)
                    newQueueStack.append(stack2)
                }
            }
            result = newArray
            queueRemain = newQueueRemain
            queueStack = newQueueStack
        }
        return result;
    }
}
```

###Performance

1. Time Complexity: 

   I think is O(n*4^n), However there is an answer—O((4^n)/sqr(n))  

2. Space Complexity:

   I think is O(4^n), However there is an answer—O((4^n)/sqr(n))  


###Better Solution

```swift
class Solution {
    func generateParenthesis(_ n: Int) -> [String] {
        var result = [String]()
        func helper(l: Int, r: Int, s: String) {
            var r_count = r
            var str = s
            if l == n {
                while r_count < n {
                    r_count += 1
                    str += ")"
                }
                print(str)
                result += [str]
                return
            }
            if l == r_count {
                helper(l: l + 1, r: r_count, s: str + "(")
            }
            if l > r_count {
                helper(l: l + 1, r: r_count, s: str + "(")
                helper(l: l, r: r_count + 1, s: str + ")")
            }
        }
        helper(l: 0, r: 0, s: "")
        return result
    }
}
```

This solution is much more clear. And my solution used too much more space.



