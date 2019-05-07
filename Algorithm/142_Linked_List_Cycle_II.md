[142. Linked List Cycle II](https://leetcode-cn.com/problems/linked-list-cycle-ii/submissions/)
Given a linked list, return the node where the cycle begins. If there is no cycle, return null.

To represent a cycle in the given linked list, we use an integer pos which represents the position (0-indexed) in the linked list where tail connects to. If pos is -1, then there is no cycle in the linked list.

Note: Do not modify the linked list.

Example 1:
```
Input: head = [3,2,0,-4], pos = 1
Output: tail connects to node index 1
Explanation: There is a cycle in the linked list, where tail connects to the second node.
```

Example 2:
```
Input: head = [1,2], pos = 0
Output: tail connects to node index 0
Explanation: There is a cycle in the linked list, where tail connects to the first node.
```

Example 3:
```
Input: head = [1], pos = -1
Output: no cycle
Explanation: There is no cycle in the linked list.
```


Solution
```
Consider two steps:
1. Make sure there is cycle or not
   Solution: Two pointer ,one fast ,one slow. If they meet at somewhere, we can say there is cycle.
2. if has cycle, then find cycle postion
   Solution: Two pointer, one from starter, one from step1.meet position, both move 1 step one time. The place they meet is the entrance of cycle.
```


Code
```
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
def detectCycle(self, head):
    fast = head
    slow = head
    if head==None:
        return None
    hasCycle = False
    while True:
        if fast.next:
            fast = fast.next
            if fast.next:
                fast = fast.next
            else:
                break
        else:
            break
        slow = slow.next

        if fast == slow:
            hasCycle = True
            break
    if hasCycle==False:
        return None
    else:
        detection = head
        while detection!=fast:
            detection = detection.next
            fast = fast.next
        return detection

```

