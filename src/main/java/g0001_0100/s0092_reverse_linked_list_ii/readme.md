[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 92\. Reverse Linked List II

Medium

Given the `head` of a singly linked list and two integers `left` and `right` where `left <= right`, reverse the nodes of the list from position `left` to position `right`, and return _the reversed list_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/rev2ex2.jpg)

**Input:** head = [1,2,3,4,5], left = 2, right = 4

**Output:** [1,4,3,2,5] 

**Example 2:**

**Input:** head = [5], left = 1, right = 1

**Output:** [5] 

**Constraints:**

*   The number of nodes in the list is `n`.
*   `1 <= n <= 500`
*   `-500 <= Node.val <= 500`
*   `1 <= left <= right <= n`

**Follow up:** Could you do it in one pass?

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
    public ListNode reverseBetween(ListNode head, int left, int right) {
        if (left == right) {
            return head;
        }
        ListNode prev = null;
        ListNode temp = head;
        ListNode start;
        int k = left;
        while (temp != null && k > 1) {
            prev = temp;
            temp = temp.next;
            k--;
        }
        if (left > 1 && prev != null) {
            prev.next = null;
        }
        ListNode prev1 = null;
        start = temp;
        while (temp != null && right - left >= 0) {
            prev1 = temp;
            temp = temp.next;
            right--;
        }
        if (prev1 != null) {
            prev1.next = null;
        }
        if (left > 1 && prev != null) {
            prev.next = reverse(start);
        } else {
            head = reverse(start);
            prev = head;
        }
        while (prev.next != null) {
            prev = prev.next;
        }
        prev.next = temp;
        return head;
    }

    public ListNode reverse(ListNode head) {
        ListNode p;
        ListNode q;
        ListNode r = null;
        p = head;
        while (p != null) {
            q = p.next;
            p.next = r;
            r = p;
            p = q;
        }
        return r;
    }
}
```