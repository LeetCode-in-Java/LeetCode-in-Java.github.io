[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 24\. Swap Nodes in Pairs

Medium

Given a linked list, swap every two adjacent nodes and return its head. You must solve the problem without modifying the values in the list's nodes (i.e., only nodes themselves may be changed.)

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/03/swap_ex1.jpg)

**Input:** head = [1,2,3,4]

**Output:** [2,1,4,3] 

**Example 2:**

**Input:** head = []

**Output:** [] 

**Example 3:**

**Input:** head = [1]

**Output:** [1] 

**Constraints:**

*   The number of nodes in the list is in the range `[0, 100]`.
*   `0 <= Node.val <= 100`

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
    public ListNode swapPairs(ListNode head) {
        if (head == null) {
            return null;
        }
        int len = getLength(head);
        return reverse(head, len);
    }

    private int getLength(ListNode curr) {
        int cnt = 0;
        while (curr != null) {
            cnt++;
            curr = curr.next;
        }
        return cnt;
    }

    // Recursive function to reverse in groups
    private ListNode reverse(ListNode head, int len) {
        // base case
        if (len < 2) {
            return head;
        }

        ListNode curr = head;
        ListNode prev = null;
        ListNode next;
        for (int i = 0; i < 2; i++) {
            // reverse linked list code
            next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }

        head.next = reverse(curr, len - 2);
        return prev;
    }
}
```