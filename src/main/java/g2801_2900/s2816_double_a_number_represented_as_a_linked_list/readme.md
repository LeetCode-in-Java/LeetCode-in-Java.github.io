[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2816\. Double a Number Represented as a Linked List

Medium

You are given the `head` of a **non-empty** linked list representing a non-negative integer without leading zeroes.

Return _the_ `head` _of the linked list after **doubling** it_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2023/05/28/example.png)

**Input:** head = [1,8,9]

**Output:** [3,7,8]

**Explanation:** The figure above corresponds to the given linked list which represents the number 189. Hence, the returned linked list represents the number 189 \* 2 = 378. 

**Example 2:**

![](https://assets.leetcode.com/uploads/2023/05/28/example2.png)

**Input:** head = [9,9,9]

**Output:** [1,9,9,8]

**Explanation:** The figure above corresponds to the given linked list which represents the number 999. Hence, the returned linked list reprersents the number 999 \* 2 = 1998. 

**Constraints:**

*   The number of nodes in the list is in the range <code>[1, 10<sup>4</sup>]</code>
*   `0 <= Node.val <= 9`
*   The input is generated such that the list represents a number that does not have leading zeros, except the number `0` itself.

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
    public ListNode doubleIt(ListNode head) {
        ListNode temp1 = revList(head);
        ListNode list1 = null;
        ListNode current = list1;
        int carry = 0;
        while (temp1 != null) {
            int val = temp1.val * 2;
            if (list1 == null) {
                list1 = new ListNode(val % 10 + carry);
                current = list1;
            } else {
                current.next = new ListNode(val % 10 + carry);
                current = current.next;
            }
            carry = val / 10;
            temp1 = temp1.next;
        }
        if (carry == 1) {
            current.next = new ListNode(carry);
        }
        return revList(list1);
    }

    private ListNode revList(ListNode head) {
        ListNode prev = null;
        ListNode nxt = null;
        ListNode current = head;
        while (current != null) {
            nxt = current.next;
            current.next = prev;
            prev = current;
            current = nxt;
        }
        return prev;
    }
}
```