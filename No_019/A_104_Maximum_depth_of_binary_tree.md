## [104. Maximum Depth of Binary Tree](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)

### Introduction

Given a binary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

Note: A leaf is a node with no children.

**Example 1:**

Given binary tree `[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

return its depth = 3.

### Solution

有两种做法，一种是深度优先遍历，用递归来做，另一种是广度优先遍历，用队列来做。

这里选用递归的深度优先遍历做，比较简单。

广度优先遍历需要把一整层的叶子都放在一起，随着层数变深，队列需要的空间会越来越大。

### Code

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def maxDepth(self, root: TreeNode) -> int:
        if root == None:
            return 0
        return max(self.maxDepth(root.left),self.maxDepth(root.right))+1
                
```

### Performance

1. Time Complexity:

   O(n^3)

2. Space Complexity:

   O(n)