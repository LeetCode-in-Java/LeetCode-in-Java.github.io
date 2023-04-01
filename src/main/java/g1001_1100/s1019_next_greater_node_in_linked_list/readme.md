[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1019\. Next Greater Node In Linked List

Medium

You are given the `head` of a linked list with `n` nodes.

For each node in the list, find the value of the **next greater node**. That is, for each node, find the value of the first node that is next to it and has a **strictly larger** value than it.

Return an integer array `answer` where `answer[i]` is the value of the next greater node of the <code>i<sup>th</sup></code> node (**1-indexed**). If the <code>i<sup>th</sup></code> node does not have a next greater node, set `answer[i] = 0`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/08/05/linkedlistnext1.jpg)

**Input:** head = [2,1,5]

**Output:** [5,5,0]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/08/05/linkedlistnext2.jpg)

**Input:** head = [2,7,4,3,5]

**Output:** [7,0,5,5,0]

**Constraints:**

*   The number of nodes in the list is `n`.
*   <code>1 <= n <= 10<sup>4</sup></code>
*   <code>1 <= Node.val <= 10<sup>9</sup></code>

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
    public int[] nextLargerNodes(ListNode head) {
        int len = length(head);
        int i = 0;
        int[] arr = new int[len];
        int[] idx = new int[len];
        while (head != null) {
            arr[i] = head.val;
            head = head.next;
            i++;
        }
        hlp(arr, idx, 0);
        i = 0;
        while (i < idx.length) {
            int j = idx[i];
            if (j != -1) {
                arr[i] = arr[j];
            } else {
                arr[i] = 0;
            }
            i++;
        }
        arr[i - 1] = 0;
        return arr;
    }

    private void hlp(int[] arr, int[] idx, int i) {
        if (i == arr.length - 1) {
            idx[i] = -1;
            return;
        }
        hlp(arr, idx, i + 1);
        int j = i + 1;
        while (j != -1 && arr[i] >= arr[j]) {
            j = idx[j];
        }
        if ((j != -1) && arr[i] >= arr[j]) {
            idx[i] = -1;
        } else {
            idx[i] = j;
        }
    }

    private int length(ListNode head) {
        int len = 0;
        while (head != null) {
            head = head.next;
            len++;
        }
        return len;
    }
}
```