[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2807\. Insert Greatest Common Divisors in Linked List

Medium

Given the head of a linked list `head`, in which each node contains an integer value.

Between every pair of adjacent nodes, insert a new node with a value equal to the **greatest common divisor** of them.

Return _the linked list after insertion_.

The **greatest common divisor** of two numbers is the largest positive integer that evenly divides both numbers.

**Example 1:**

![](https://assets.leetcode.com/uploads/2023/07/18/ex1_copy.png)

**Input:** head = [18,6,10,3]

**Output:** [18,6,6,2,10,1,3]

**Explanation:** The 1<sup>st</sup> diagram denotes the initial linked list and the 2<sup>nd</sup> diagram denotes the linked list after inserting the new nodes (nodes in blue are the inserted nodes). 

- We insert the greatest common divisor of 18 and 6 = 6 between the 1<sup>st</sup> and the 2<sup>nd</sup> nodes. 
- We insert the greatest common divisor of 6 and 10 = 2 between the 2<sup>nd</sup> and the 3<sup>rd</sup> nodes. 
- We insert the greatest common divisor of 10 and 3 = 1 between the 3<sup>rd</sup> and the 4<sup>th</sup> nodes. 

There are no more adjacent nodes, so we return the linked list.

**Example 2:**

![](https://assets.leetcode.com/uploads/2023/07/18/ex2_copy1.png)

**Input:** head = [7]

**Output:** [7]

**Explanation:** The 1<sup>st</sup> diagram denotes the initial linked list and the 2<sup>nd</sup> diagram denotes the linked list after inserting the new nodes. There are no pairs of adjacent nodes, so we return the initial linked list.

**Constraints:**

*   The number of nodes in the list is in the range `[1, 5000]`.
*   `1 <= Node.val <= 1000`

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
    public ListNode insertGreatestCommonDivisors(ListNode head) {
        ListNode prevNode = null;
        ListNode currNode = head;
        while (currNode != null) {
            if (prevNode != null) {
                int gcd = greatestCommonDivisor(prevNode.val, currNode.val);
                prevNode.next = new ListNode(gcd, currNode);
            }
            prevNode = currNode;
            currNode = currNode.next;
        }
        return head;
    }

    private int greatestCommonDivisor(int val1, int val2) {
        if (val2 == 0) {
            return val1;
        }
        return greatestCommonDivisor(val2, val1 % val2);
    }
}
```