[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 237\. Delete Node in a Linked List

Easy

Write a function to **delete a node** in a singly-linked list. You will **not** be given access to the `head` of the list, instead you will be given access to **the node to be deleted** directly.

It is **guaranteed** that the node to be deleted is **not a tail node** in the list.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/01/node1.jpg)

**Input:** head = [4,5,1,9], node = 5

**Output:** [4,1,9]

**Explanation:** You are given the second node with value 5, the linked list should become 4 -> 1 -> 9 after calling your function. 

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/09/01/node2.jpg)

**Input:** head = [4,5,1,9], node = 1

**Output:** [4,5,9]

**Explanation:** You are given the third node with value 1, the linked list should become 4 -> 5 -> 9 after calling your function. 

**Example 3:**

**Input:** head = [1,2,3,4], node = 3

**Output:** [1,2,4] 

**Example 4:**

**Input:** head = [0,1], node = 0

**Output:** [1] 

**Example 5:**

**Input:** head = [-3,5,-99], node = -3

**Output:** [5,-99] 

**Constraints:**

*   The number of the nodes in the given list is in the range `[2, 1000]`.
*   `-1000 <= Node.val <= 1000`
*   The value of each node in the list is **unique**.
*   The `node` to be deleted is **in the list** and is **not a tail** node

## Solution

```java
import com_github_leetcode.ListNode;

/*
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    public void deleteNode(ListNode node) {
        while (node.next.next != null) {
            node.val = node.next.val;
            node = node.next;
        }
        node.val = node.next.val;
        node.next = null;
    }
}
```