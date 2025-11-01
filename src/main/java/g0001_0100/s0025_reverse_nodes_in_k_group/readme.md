[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 25\. Reverse Nodes in k-Group

Hard

Given the `head` of a linked list, reverse the nodes of the list `k` at a time, and return _the modified list_.

`k` is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of `k` then left-out nodes, in the end, should remain as it is.

You may not alter the values in the list's nodes, only nodes themselves may be changed.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex1.jpg)

**Input:** head = [1,2,3,4,5], k = 2

**Output:** [2,1,4,3,5] 

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex2.jpg)

**Input:** head = [1,2,3,4,5], k = 3

**Output:** [3,2,1,4,5] 

**Constraints:**

*   The number of nodes in the list is `n`.
*   `1 <= k <= n <= 5000`
*   `0 <= Node.val <= 1000`

**Follow-up:** Can you solve the problem in `O(1)` extra memory space?

To solve the "Reverse Nodes in k-Group" problem in Java with a `Solution` class, we can reverse the nodes in groups of k using a recursive approach. Here are the steps:

1. Define a `Solution` class.
2. Define a method named `reverseKGroup` that takes the head of a linked list and an integer k as input and returns the head of the modified list.
3. Define a helper method named `reverse` that takes the head and tail of a sublist as input and reverses the sublist in place. This method returns the new head of the sublist.
4. Create a dummy ListNode object and set its `next` pointer to the head of the input list. This dummy node will serve as the new head of the modified list.
5. Initialize pointers `prev`, `curr`, `next`, and `tail`. Set `prev` and `tail` to the dummy node, and `curr` to the head of the input list.
6. Iterate through the list:
   - Move `curr` k steps forward. If it's not possible (i.e., there are less than k nodes left), break the loop.
   - Set `next` to the `next` pointer of `curr`.
   - Reverse the sublist from `curr` to `next` using the `reverse` method. Update `prev` and `tail` accordingly.
   - Move `prev` and `tail` k steps forward to the last node of the reversed sublist.
   - Move `curr` to `next`.
7. Return the `next` pointer of the dummy node, which points to the head of the modified list.

Here's the implementation:

```java
public class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        // Create a dummy node and point its next to the head
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        
        // Initialize pointers
        ListNode prev = dummy, curr = head, next, tail;
        
        // Iterate through the list and reverse in groups of k
        while (true) {
            // Move curr k steps forward
            tail = prev;
            for (int i = 0; i < k; i++) {
                tail = tail.next;
                if (tail == null) return dummy.next; // Less than k nodes left
            }
            
            next = tail.next; // Save the next pointer of the sublist
            tail.next = null; // Disconnect the sublist from the rest of the list
            
            // Reverse the sublist and update prev and tail pointers
            prev.next = reverse(curr, tail);
            tail.next = next; // Connect the reversed sublist back to the rest of the list
            
            // Move prev, tail, and curr to the next group
            prev = curr;
            curr = next;
        }
    }
    
    // Helper method to reverse a sublist from head to tail
    private ListNode reverse(ListNode head, ListNode tail) {
        ListNode prev = null, curr = head, next;
        while (curr != null) {
            next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
            if (prev == tail) break;
        }
        return prev; // Return the new head of the reversed sublist
    }
}
```

This implementation provides a solution to the "Reverse Nodes in k-Group" problem in Java without modifying the values in the list's nodes. It recursively reverses the nodes in groups of k.