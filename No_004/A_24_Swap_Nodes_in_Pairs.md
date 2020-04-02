##[24. Swap Nodes in Pairs](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)

###Introduction

Given a linked list, swap every two adjacent nodes and return its head.

You may not modify the values in the list's nodes, only nodes itself may be changed.

Example:

```
Given 1->2->3->4, you should return the list as 2->1->4->3.
```



###Solution

Consider using three temp variables looping to solve.

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
    func swapPairs(_ head: ListNode?) -> ListNode? {
        if head==nil || head!.next==nil {
            return head
        }
        let res: ListNode = ListNode(0)
        res.next = head
        var rs1: ListNode? = res
        var rs2: ListNode? = res.next
        var rs3: ListNode? = rs2!.next
        while rs2 != nil && rs3 != nil{
            rs2!.next = rs3!.next
            rs3!.next = rs2
            rs1!.next = rs3
            rs1 = rs2
            rs2 = rs2!.next
            if rs2 != nil {
                rs3 = rs2!.next
            }
        }
        return res.next
    }
}
```

###Performance

1. Time Complexity: 

    O(n/2)

2. Space Complexity:

   O(1) 


###Better Solution

```swift
class Solution {
    func swapPairs(_ head: ListNode?) -> ListNode? {
        var node = head?.next != nil ? head!.next! : head
        var current = head
        var preCurrent: ListNode? = nil
        while  current != nil && current?.next != nil {
            var tmp = current?.next?.next
            current?.next?.next = current
            preCurrent?.next = current?.next
            current?.next = tmp
            preCurrent = current
            current = tmp         
        }
        return node
        
    }
}
```

This solution is much more clear. And my solution used more space.



