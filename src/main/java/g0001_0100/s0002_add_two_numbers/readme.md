[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2\. Add Two Numbers

Medium

You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order**, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/02/addtwonumber1.jpg)

**Input:** l1 = [2,4,3], l2 = [5,6,4]

**Output:** [7,0,8]

**Explanation:** 342 + 465 = 807. 

**Example 2:**

**Input:** l1 = [0], l2 = [0]

**Output:** [0] 

**Example 3:**

**Input:** l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]

**Output:** [8,9,9,9,0,0,0,1] 

**Constraints:**

*   The number of nodes in each linked list is in the range `[1, 100]`.
*   `0 <= Node.val <= 9`
*   It is guaranteed that the list represents a number that does not have leading zeros.

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
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode dummyHead = new ListNode(0);
        ListNode p = l1;
        ListNode q = l2;
        ListNode curr = dummyHead;
        int carry = 0;
        while (p != null || q != null) {
            int x = (p != null) ? p.val : 0;
            int y = (q != null) ? q.val : 0;
            int sum = carry + x + y;
            carry = sum / 10;
            curr.next = new ListNode(sum % 10);
            curr = curr.next;
            if (p != null) {
                p = p.next;
            }
            if (q != null) {
                q = q.next;
            }
        }
        if (carry > 0) {
            curr.next = new ListNode(carry);
        }
        return dummyHead.next;
    }
}
```

**Time Complexity (Big O Time):**

The time complexity of this program is O(max(N, M)), where "N" and "M" represent the lengths of the input linked lists `l1` and `l2`, respectively. Here's the breakdown:

1. The program iterates through both linked lists `l1` and `l2` simultaneously using a `while` loop. The loop runs as long as either of the two lists has elements to process.
2. Inside the loop, it performs constant-time operations for each node:
   - It calculates the sum of three integer values (constant time).
   - It creates a new node (constant time).
   - It updates pointers and variables (constant time).

Since the loop runs until the end of the longer of the two input linked lists, the overall time complexity is O(max(N, M)), where "N" and "M" are the lengths of `l1` and `l2`, respectively.

**Space Complexity (Big O Space):**

The space complexity of this program is O(max(N, M)), which is primarily determined by the space used for the output linked list. Here's why:

1. The program creates a new linked list to store the result, and the length of this linked list can be at most max(N, M) + 1 (where +1 is for the potential carry at the end).
2. Other than the output linked list, the program uses a constant amount of space for variables like `dummyHead`, `p`, `q`, `curr`, and `carry`.

Therefore, the space complexity is O(max(N, M)).
