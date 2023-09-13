[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 206\. Reverse Linked List

Easy

Given the `head` of a singly linked list, reverse the list, and return _the reversed list_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg)

**Input:** head = [1,2,3,4,5]

**Output:** [5,4,3,2,1] 

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/02/19/rev1ex2.jpg)

**Input:** head = [1,2]

**Output:** [2,1] 

**Example 3:**

**Input:** head = []

**Output:** [] 

**Constraints:**

*   The number of nodes in the list is the range `[0, 5000]`.
*   `-5000 <= Node.val <= 5000`

**Follow up:** A linked list can be reversed either iteratively or recursively. Could you implement both?

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
    public ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode curr = head;
        while (curr != null) {
            ListNode next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }
        return prev;
    }
}
```

**Time Complexity (Big O Time):**

The time complexity of this program is O(N), where N is the number of nodes in the linked list. Here's why:

1. The program uses a `while` loop that iterates through the linked list once, visiting each node exactly once. Therefore, the number of iterations in the loop is proportional to the number of nodes in the list, which is N.

2. Inside the loop, there are constant-time operations. Assignments and pointer adjustments such as `curr.next = prev` take constant time regardless of the list's size.

3. Overall, the time complexity is linear, O(N), because the program performs a constant amount of work for each node in the list.

**Space Complexity (Big O Space):**

The space complexity of this program is O(1), which means it uses a constant amount of additional space regardless of the size of the input linked list. Here's why:

1. The program uses a constant number of variables (`prev`, `curr`, `next`), and the space required for these variables does not depend on the size of the linked list. Therefore, the space complexity is O(1) for auxiliary space.

2. The program does not create any data structures or arrays whose space requirements depend on the size of the input.

In summary, the time complexity is O(N) because the program iterates through the linked list once, and the space complexity is O(1) because it uses a constant amount of additional space for variables.
