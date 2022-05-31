## 1721\. Swapping Nodes in a Linked List

Medium

You are given the `head` of a linked list, and an integer `k`.

Return _the head of the linked list after **swapping** the values of the_ <code>k<sup>th</sup></code> _node from the beginning and the_ <code>k<sup>th</sup></code> _node from the end (the list is **1-indexed**)._

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/21/linked1.jpg)

**Input:** head = [1,2,3,4,5], k = 2

**Output:** [1,4,3,2,5]

**Example 2:**

**Input:** head = [7,9,6,6,7,8,3,0,9,5], k = 5

**Output:** [7,9,6,6,8,7,3,0,9,5]

**Constraints:**

*   The number of nodes in the list is `n`.
*   <code>1 <= k <= n <= 10<sup>5</sup></code>
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
    public ListNode swapNodes(ListNode head, int k) {
        ListNode beg = null;
        ListNode end = null;
        ListNode node = head;
        while (node != null) {
            k--;
            if (k == 0) {
                beg = node;
                end = head;
            } else if (end != null) {
                end = end.next;
            }
            node = node.next;
        }
        if (beg != null) {
            int tem = beg.val;
            beg.val = end.val;
            end.val = tem;
        }
        return head;
    }
}
```