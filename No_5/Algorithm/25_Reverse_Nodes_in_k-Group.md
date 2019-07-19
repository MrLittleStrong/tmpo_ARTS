##[25. Reverse Nodes in k-Group](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/)

###Introduction

Given a linked list, reverse the nodes of a linked list k at a time and return its modified list.

k is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of k then left-out nodes in the end should remain as it is.

Example:

```
Given this linked list: 1->2->3->4->5

For k = 2, you should return: 2->1->4->3->5

For k = 3, you should return: 3->2->1->4->5

```

#####Note:

- Only constant extra memory is allowed.
- You may not alter the values in the list's nodes, only nodes itself may be changed.

###Solution

Based on [24. Swap Nodes in Pairs](https://leetcode-cn.com/problems/swap-nodes-in-pairs/).

###Code

```swift
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     public var val: Int
 *     public var next: ListNode?
 *     public init(_ val: Int) {
 *         self.val = val
 *         self.next = nil
 *     }
 * }
 */
class Solution {
    func reverseKGroup(_ head: ListNode?, _ k: Int) -> ListNode? {
        var rs: ListNode? = head
        var res: ListNode? = head
        var shortHead: ListNode? = head
        var shortLast: ListNode? = head
        var lastTail: ListNode? = nil
        var isHeadSet = false
        while(res != nil) {
            shortHead = res
            shortLast = res
            for i in 0 ..< k-1 {
                res = res?.next
                if res == nil {
                    return rs
                }
            }
            if isHeadSet == false {
                rs = res
                isHeadSet = true
            }
            res = res?.next
            for i in 0 ..< k-1 {
                var tmp: ListNode? = shortLast?.next
                shortLast?.next = tmp?.next
                tmp?.next = shortHead
                shortHead = tmp
            }
            if lastTail != nil {
                lastTail?.next = shortHead
            }
            lastTail = shortLast
        }
        return rs
    }
}
```

###Performance

1. Time Complexity: 

   O(n)

2. Space Complexity:

   O(1)


###Better Solution

```swift
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     public var val: Int
 *     public var next: ListNode?
 *     public init(_ val: Int) {
 *         self.val = val
 *         self.next = nil
 *     }
 * }
 */
class Solution {
    func reverseKGroup(_ head: ListNode?, _ k: Int) -> ListNode? {
        var head = head
        var current = head
        var count = 0
        
        while current != nil && count != k {
            current = current?.next
            count += 1
        }
        
        if count == k {
            current = reverseKGroup(current, k)
            while count > 0 {
                let temp = head?.next
                head?.next = current
                current = head
                head = temp
                count -= 1
            }
            head = current
        }
        return head
    }
}
```

This solution used Stack and Recursive methods.  For stack, use an integer to indicate the stack's situation.



