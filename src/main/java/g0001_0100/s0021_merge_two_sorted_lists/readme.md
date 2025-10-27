[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 21\. Merge Two Sorted Lists

Easy

You are given the heads of two sorted linked lists `list1` and `list2`.

Merge the two lists into one **sorted** list. The list should be made by splicing together the nodes of the first two lists.

Return _the head of the merged linked list_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/03/merge_ex1.jpg)

**Input:** list1 = [1,2,4], list2 = [1,3,4]

**Output:** [1,1,2,3,4,4] 

**Example 2:**

**Input:** list1 = [], list2 = []

**Output:** [] 

**Example 3:**

**Input:** list1 = [], list2 = [0]

**Output:** [0] 

**Constraints:**

*   The number of nodes in both lists is in the range `[0, 50]`.
*   `-100 <= Node.val <= 100`
*   Both `list1` and `list2` are sorted in **non-decreasing** order.

To solve the Merge Two Sorted Lists problem in Java with a `Solution` class, we'll implement a recursive approach. Here are the steps:

1. Define a `ListNode` class to represent a node in the linked list.
2. Define a `Solution` class with a method named `mergeTwoLists` that takes two linked lists `l1` and `l2` as input and returns a merged sorted list.
3. The base case for the recursion is when either `l1` or `l2` is null. In this case, return the non-null list because it's already sorted.
4. Compare the values of the heads of `l1` and `l2`. Let `head` be the smaller value of the two heads.
5. Recursively call `mergeTwoLists` with the next node of the smaller head and the other list that remained unchanged.
6. Update the `next` pointer of the smaller head to point to the result of the recursive call.
7. Return the smaller head, which is the merged sorted list.

Here's the implementation:

```java
public class Solution {
    static class ListNode {
        int val;
        ListNode next;

        ListNode(int val) {
            this.val = val;
        }
    }

    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if (l1 == null) {
            return l2;
        }
        if (l2 == null) {
            return l1;
        }

        ListNode head;
        if (l1.val < l2.val) {
            head = l1;
            head.next = mergeTwoLists(l1.next, l2);
        } else {
            head = l2;
            head.next = mergeTwoLists(l1, l2.next);
        }

        return head;
    }

    public static void main(String[] args) {
        Solution solution = new Solution();

        // Test cases
        ListNode l1 = new ListNode(1);
        l1.next = new ListNode(2);
        l1.next.next = new ListNode(4);

        ListNode l2 = new ListNode(1);
        l2.next = new ListNode(3);
        l2.next.next = new ListNode(4);

        ListNode mergedList = solution.mergeTwoLists(l1, l2);
        while (mergedList != null) {
            System.out.print(mergedList.val + " ");
            mergedList = mergedList.next;
        }
        System.out.println(); // newline
    }
}
```

This implementation provides a solution to the Merge Two Sorted Lists problem in Java using a recursive approach.