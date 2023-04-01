[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2130\. Maximum Twin Sum of a Linked List

Medium

In a linked list of size `n`, where `n` is **even**, the <code>i<sup>th</sup></code> node (**0-indexed**) of the linked list is known as the **twin** of the <code>(n-1-i)<sup>th</sup></code> node, if `0 <= i <= (n / 2) - 1`.

*   For example, if `n = 4`, then node `0` is the twin of node `3`, and node `1` is the twin of node `2`. These are the only nodes with twins for `n = 4`.

The **twin sum** is defined as the sum of a node and its twin.

Given the `head` of a linked list with even length, return _the **maximum twin sum** of the linked list_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/12/03/eg1drawio.png)

**Input:** head = [5,4,2,1]

**Output:** 6

**Explanation:** 

Nodes 0 and 1 are the twins of nodes 3 and 2, respectively. All have twin sum = 6. 

There are no other nodes with twins in the linked list. 

Thus, the maximum twin sum of the linked list is 6.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/12/03/eg2drawio.png)

**Input:** head = [4,2,2,3]

**Output:** 7

**Explanation:** 

The nodes with twins present in this linked list are: 

- Node 0 is the twin of node 3 having a twin sum of 4 + 3 = 7. 

- Node 1 is the twin of node 2 having a twin sum of 2 + 2 = 4. 
  
Thus, the maximum twin sum of the linked list is max(7, 4) = 7.

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/12/03/eg3drawio.png)

**Input:** head = [1,100000]

**Output:** 100001

**Explanation:** 

There is only one node with a twin in the linked list having twin sum of 1 + 100000 = 100001.

**Constraints:**

*   The number of nodes in the list is an **even** integer in the range <code>[2, 10<sup>5</sup>]</code>.
*   <code>1 <= Node.val <= 10<sup>5</sup></code>

## Solution

```java
import com_github_leetcode.ListNode;

public class Solution {
    public int pairSum(ListNode head) {
        if (head == null) {
            return 0;
        }
        int maxSum = Integer.MIN_VALUE;
        ListNode slow = head;
        ListNode fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        if (slow.next == null) {
            return head.val + slow.val;
        }
        ListNode tail = head;
        ListNode pivot = reverse(slow);
        while (pivot != null) {
            maxSum = Math.max(maxSum, (tail.val + pivot.val));
            tail = tail.next;
            pivot = pivot.next;
        }
        return maxSum;
    }

    private ListNode reverse(ListNode head) {
        ListNode tail = head;
        ListNode prev = null;
        while (tail != null) {
            ListNode temp = tail.next;
            tail.next = prev;
            prev = tail;
            tail = temp;
        }
        return prev;
    }
}
```