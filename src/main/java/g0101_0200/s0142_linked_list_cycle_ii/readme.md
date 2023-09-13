[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 142\. Linked List Cycle II

Medium

Given the `head` of a linked list, return _the node where the cycle begins. If there is no cycle, return_ `null`.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to (**0-indexed**). It is `-1` if there is no cycle. **Note that** `pos` **is not passed as a parameter**.

**Do not modify** the linked list.

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

**Input:** head = [3,2,0,-4], pos = 1

**Output:** tail connects to node index 1

**Explanation:** There is a cycle in the linked list, where tail connects to the second node. 

**Example 2:**

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test2.png)

**Input:** head = [1,2], pos = 0

**Output:** tail connects to node index 0

**Explanation:** There is a cycle in the linked list, where tail connects to the first node. 

**Example 3:**

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test3.png)

**Input:** head = [1], pos = -1

**Output:** no cycle

**Explanation:** There is no cycle in the linked list. 

**Constraints:**

*   The number of the nodes in the list is in the range <code>[0, 10<sup>4</sup>]</code>.
*   <code>-10<sup>5</sup> <= Node.val <= 10<sup>5</sup></code>
*   `pos` is `-1` or a **valid index** in the linked-list.

**Follow up:** Can you solve it using `O(1)` (i.e. constant) memory?

## Solution

```java
import com_github_leetcode.ListNode;

/*
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode detectCycle(ListNode head) {
        if (head == null || head.next == null) {
            return null;
        }
        ListNode slow = head;
        ListNode fast = head;
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
            // intersected inside the loop.
            if (slow == fast) {
                break;
            }
        }
        if (fast == null || fast.next == null) {
            return null;
        }
        slow = head;
        while (slow != fast) {
            slow = slow.next;
            fast = fast.next;
        }
        return slow;
    }
}
```

**Time Complexity (Big O Time):**

- The program uses two pointers, `fast` and `slow`, to traverse the linked list.
- The first while loop is used to determine if there is a cycle and find the meeting point of `fast` and `slow`. This loop runs until `fast` and `slow` meet inside the loop or reach the end of the list.

If there is no cycle, both pointers will reach the end of the list in O(N) time, where N is the number of nodes in the linked list.

If there is a cycle, the `fast` and `slow` pointers will meet inside the loop. The maximum number of iterations needed to detect the cycle and find the meeting point is less than or equal to N. Therefore, the time complexity remains O(N).

The second while loop is used to find the starting node of the cycle. It starts from the head of the list and moves `slow` and `fast` pointers at the same speed until they meet at the starting node of the cycle. This loop runs in O(N) time.

Overall, the time complexity of this algorithm is O(N).

**Space Complexity (Big O Space):**

The space complexity of this algorithm is O(1), which means it uses a constant amount of extra space regardless of the size of the input linked list. It only uses a few additional pointers (`fast` and `slow`) to traverse the list and does not rely on additional data structures or recursion that would consume additional memory.

In summary, the time complexity of this program is O(N), and the space complexity is O(1), where N is the number of nodes in the linked list.
