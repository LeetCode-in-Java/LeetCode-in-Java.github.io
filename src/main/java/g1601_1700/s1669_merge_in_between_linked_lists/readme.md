[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1669\. Merge In Between Linked Lists

Medium

You are given two linked lists: `list1` and `list2` of sizes `n` and `m` respectively.

Remove `list1`'s nodes from the <code>a<sup>th</sup></code> node to the <code>b<sup>th</sup></code> node, and put `list2` in their place.

The blue edges and nodes in the following figure indicate the result:

![](https://assets.leetcode.com/uploads/2020/11/05/fig1.png)

_Build the result list and return its head._

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/05/merge_linked_list_ex1.png)

**Input:** list1 = [0,1,2,3,4,5], a = 3, b = 4, list2 = [1000000,1000001,1000002]

**Output:** [0,1,2,1000000,1000001,1000002,5]

**Explanation:** We remove the nodes 3 and 4 and put the entire list2 in their place.

The blue edges and nodes in the above figure indicate the result.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/05/merge_linked_list_ex2.png)

**Input:** list1 = [0,1,2,3,4,5,6], a = 2, b = 5, list2 = [1000000,1000001,1000002,1000003,1000004]

**Output:** [0,1,1000000,1000001,1000002,1000003,1000004,6]

**Explanation:** The blue edges and nodes in the above figure indicate the result.

**Constraints:**

*   <code>3 <= list1.length <= 10<sup>4</sup></code>
*   `1 <= a <= b < list1.length - 1`
*   <code>1 <= list2.length <= 10<sup>4</sup></code>

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
    public ListNode mergeInBetween(ListNode list1, int a, int b, ListNode list2) {
        ListNode start = list1;
        for (int i = 1; i < a; i++) {
            start = start.next;
        }
        ListNode end = start;
        for (int i = a; i <= b; i++) {
            end = end.next;
        }
        start.next = list2;
        while (list2.next != null) {
            list2 = list2.next;
        }
        list2.next = end.next;
        return list1;
    }
}
```