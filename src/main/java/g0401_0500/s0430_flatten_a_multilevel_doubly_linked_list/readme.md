[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 430\. Flatten a Multilevel Doubly Linked List

Medium

You are given a doubly linked list, which contains nodes that have a next pointer, a previous pointer, and an additional **child pointer**. This child pointer may or may not point to a separate doubly linked list, also containing these special nodes. These child lists may have one or more children of their own, and so on, to produce a **multilevel data structure** as shown in the example below.

Given the `head` of the first level of the list, **flatten** the list so that all the nodes appear in a single-level, doubly linked list. Let `curr` be a node with a child list. The nodes in the child list should appear **after** `curr` and **before** `curr.next` in the flattened list.

Return _the_ `head` _of the flattened list. The nodes in the list must have **all** of their child pointers set to_ `null`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/11/09/flatten11.jpg)

**Input:** head = [1,2,3,4,5,6,null,null,null,7,8,9,10,null,null,11,12]

**Output:** [1,2,3,7,8,11,12,9,10,4,5,6]

**Explanation:** The multilevel linked list in the input is shown. After flattening the multilevel linked list it becomes: ![](https://assets.leetcode.com/uploads/2021/11/09/flatten12.jpg) 

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/11/09/flatten2.1jpg)

**Input:** head = [1,2,null,3]

**Output:** [1,3,2]

**Explanation:**

    The multilevel linked list in the input is shown.
    After flattening the multilevel linked list it becomes:

![](https://assets.leetcode.com/uploads/2021/11/24/list.jpg) 

**Example 3:**

**Input:** head = []

**Output:** []

**Explanation:** There could be empty list in the input. 

**Constraints:**

*   The number of Nodes will not exceed `1000`.
*   <code>1 <= Node.val <= 10<sup>5</sup></code>

**How the multilevel linked list is represented in test cases:**

We use the multilevel linked list from **Example 1** above:

    1---2---3---4---5---6--NULL
            |
            7---8---9---10--NULL
                |
                11--12--NULL

The serialization of each level is as follows:

    [1,2,3,4,5,6,null]
    [7,8,9,10,null]
    [11,12,null] 

To serialize all levels together, we will add nulls in each level to signify no node connects to the upper node of the previous level. The serialization becomes:

    [1,    2,    3, 4, 5, 6, null]
                 |
    [null, null, 7,    8, 9, 10, null]
                       |
    [            null, 11, 12, null] 

Merging the serialization of each level and removing trailing nulls we obtain:

    [1,2,3,4,5,6,null,null,null,7,8,9,10,null,null,11,12]

## Solution

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node prev;
    public Node next;
    public Node child;
};
*/
public class Solution {
    // is true ONLY for the first element of the list
    private boolean first = true;
    // Holds the head node of the newly constructed doubly linked list
    private Node root;
    // Holds the current node of the newly constructed doubly linked list
    private Node current;

    public Node flatten(Node head) {
        if (head == null) {
            return root;
        } else {
            // Construct our doubly linked list
            if (first) {
                first = !first;
                root = new Node(head.val);
                current = root;
            } else {
                // Map all values to the newly constructed list.
                // temp value to hold our prev element
                Node temp = current;
                current.next = new Node(head.val);
                current = current.next;
                current.prev = temp;
            }
        }
        // iterate over child nodes.
        if (head.child != null) {
            flatten(head.child);
        }
        if (head.next != null) {
            // iterate next
            flatten(head.next);
        }
        return root;
    }
}
```