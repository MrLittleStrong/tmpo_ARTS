## [114. Flatten Binary Tree to Linked List](https://leetcode-cn.com/problems/flatten-binary-tree-to-linked-list/)

### Introduction

Given a binary tree, flatten it to a linked list in-place.

For example, given the following tree:

**Example 1:**

```
    1
   / \
  2   5
 / \   \
3   4   6
```

The flattened tree should look like:

```
1
 \
  2
   \
    3
     \
      4
       \
        5
         \
          6
```

### Solution

考察前序遍历的变种

### Code

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def flatten(self, root: TreeNode) -> None:
        if root is None:
            return
        stack = [root]
        tmp = None
        while len(stack)>0:
            node = stack.pop()
            if tmp == None :
                tmp = node
            else:
                tmp.right = node
                tmp.left = None
                tmp = node
            if node.right != None:
                stack.append(node.right)
            if node.left != None:
                stack.append(node.left)
            
```

### Performance

1. Time Complexity:

   O(n)

2. Space Complexity:

   O(n)

### Better solution

寻找前驱节点

对于当前节点，如果其左子节点不为空，则在其左子树中找到最右边的节点，作为前驱节点，将当前节点的右子节点赋给前驱节点的右子节点，然后将当前节点的左子节点赋给当前节点的右子节点，并将当前节点的左子节点设为空。对当前节点处理结束后，继续处理链表中的下一个节点，直到所有节点都处理结束

```python
class Solution:
    def flatten(self, root: TreeNode) -> None:
        curr = root
        while curr:
            if curr.left:
                predecessor = nxt = curr.left
                while predecessor.right:
                    predecessor = predecessor.right
                predecessor.right = curr.right
                curr.left = None
                curr.right = nxt
            curr = curr.right
```



时间复杂度是O(n)，在寻找前驱节点的过程中，每个节点最多会被额外访问一次。

空间复杂度是O(1)。

