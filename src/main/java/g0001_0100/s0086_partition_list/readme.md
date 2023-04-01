[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 86\. Partition List

Medium

Given the `head` of a linked list and a value `x`, partition it such that all nodes **less than** `x` come before nodes **greater than or equal** to `x`.

You should **preserve** the original relative order of the nodes in each of the two partitions.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/04/partition.jpg)

**Input:** head = [1,4,3,2,5,2], x = 3

**Output:** [1,2,2,4,3,5] 

**Example 2:**

**Input:** head = [2,1], x = 2

**Output:** [1,2] 

**Constraints:**

*   The number of nodes in the list is in the range `[0, 200]`.
*   `-100 <= Node.val <= 100`
*   `-200 <= x <= 200`

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
    public ListNode partition(ListNode head, int x) {
        ListNode nHead = new ListNode(0);
        ListNode nTail = new ListNode(0);
        ListNode ptr = nTail;
        ListNode temp = nHead;
        while (head != null) {
            ListNode nNext = head.next;
            if (head.val < x) {
                nHead.next = head;
                nHead = nHead.next;
            } else {
                nTail.next = head;
                nTail = nTail.next;
            }
            head = nNext;
        }
        nTail.next = null;
        nHead.next = ptr.next;
        return temp.next;
    }
}
```