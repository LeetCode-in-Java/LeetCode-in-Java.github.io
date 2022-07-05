[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 61\. Rotate List

Medium

Given the `head` of a linked list, rotate the list to the right by `k` places.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/13/rotate1.jpg)

**Input:** head = [1,2,3,4,5], k = 2

**Output:** [4,5,1,2,3] 

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/13/roate2.jpg)

**Input:** head = [0,1,2], k = 4

**Output:** [2,0,1] 

**Constraints:**

*   The number of nodes in the list is in the range `[0, 500]`.
*   `-100 <= Node.val <= 100`
*   <code>0 <= k <= 2 * 10<sup>9</sup></code>

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
    public ListNode rotateRight(ListNode head, int k) {
        if (head == null || k == 0) {
            return head;
        }
        ListNode tail = head;
        // find the count and let tail points to last node
        int count = 1;
        while (tail != null && tail.next != null) {
            count++;
            tail = tail.next;
        }
        // calculate number of times to rotate by count modulas
        int times = k % count;
        if (times == 0) {
            return head;
        }
        ListNode temp = head;
        // iterate and go to the K+1 th node from the end or count - K - 1 node from
        // start
        for (int i = 1; i <= count - times - 1 && temp != null; i++) {
            temp = temp.next;
        }
        ListNode newHead = null;
        if (temp != null && tail != null) {
            newHead = temp.next;
            temp.next = null;
            tail.next = head;
        }
        return newHead;
    }
}
```