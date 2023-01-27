[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2487\. Remove Nodes From Linked List

Medium

You are given the `head` of a linked list.

Remove every node which has a node with a **strictly greater** value anywhere to the right side of it.

Return _the_ `head` _of the modified linked list._

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/10/02/drawio.png)

**Input:** head = [5,2,13,3,8]

**Output:** [13,8]

**Explanation:** The nodes that should be removed are 5, 2 and 3. 

- Node 13 is to the right of node 5. 
- Node 13 is to the right of node 2. 
- Node 8 is to the right of node 3.

**Example 2:**

**Input:** head = [1,1,1,1]

**Output:** [1,1,1,1]

**Explanation:** Every node has value 1, so no nodes are removed.

**Constraints:**

*   The number of the nodes in the given list is in the range <code>[1, 10<sup>5</sup>]</code>.
*   <code>1 <= Node.val <= 10<sup>5</sup></code>

## Solution

```java
import com_github_leetcode.ListNode;

public class Solution {
    public ListNode removeNodes(ListNode head) {
        head = reverse(head);
        if (head == null) {
            return null;
        }
        int max = head.val;
        ListNode temp = head;
        ListNode temp1 = head.next;
        while (temp1 != null) {
            if (temp1.val >= max) {
                max = temp1.val;
                temp.next = temp1;
                temp = temp.next;
            }
            temp1 = temp1.next;
        }
        temp.next = null;
        return reverse(head);
    }

    private ListNode reverse(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode prev = null;
        ListNode curr = head;
        ListNode next;
        while (curr != null) {
            next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }
        return prev;
    }
}
```