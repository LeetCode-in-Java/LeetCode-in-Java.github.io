[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 82\. Remove Duplicates from Sorted List II

Medium

Given the `head` of a sorted linked list, _delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list_. Return _the linked list **sorted** as well_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/04/linkedlist1.jpg)

**Input:** head = [1,2,3,3,4,4,5]

**Output:** [1,2,5] 

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/01/04/linkedlist2.jpg)

**Input:** head = [1,1,1,2,3]

**Output:** [2,3] 

**Constraints:**

*   The number of nodes in the list is in the range `[0, 300]`.
*   `-100 <= Node.val <= 100`
*   The list is guaranteed to be **sorted** in ascending order.

## Solution

```java
import com_github_leetcode.ListNode;

/*
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
public class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode dummy = new ListNode(0);
        ListNode prev = dummy;
        prev.next = head;
        ListNode curr = head.next;
        while (curr != null) {
            boolean flagFoundDuplicate = false;
            while (curr != null && prev.next.val == curr.val) {
                flagFoundDuplicate = true;
                curr = curr.next;
            }
            if (flagFoundDuplicate) {
                prev.next = curr;
                if (curr != null) {
                    curr = curr.next;
                }
            } else {
                prev = prev.next;
                prev.next = curr;
                curr = curr.next;
            }
        }
        return dummy.next;
    }
}
```