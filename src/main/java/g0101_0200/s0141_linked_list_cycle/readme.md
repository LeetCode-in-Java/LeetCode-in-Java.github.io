[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 141\. Linked List Cycle

Easy

Given `head`, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to. **Note that `pos` is not passed as a parameter**.

Return `true` _if there is a cycle in the linked list_. Otherwise, return `false`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

**Input:** head = [3,2,0,-4], pos = 1

**Output:** true

**Explanation:** There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed). 

**Example 2:**

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test2.png)

**Input:** head = [1,2], pos = 0

**Output:** true

**Explanation:** There is a cycle in the linked list, where the tail connects to the 0th node. 

**Example 3:**

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test3.png)

**Input:** head = [1], pos = -1

**Output:** false

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
    public boolean hasCycle(ListNode head) {
        if (head == null) {
            return false;
        }
        ListNode fast = head.next;
        ListNode slow = head;
        while (fast != null && fast.next != null) {
            if (fast == slow) {
                return true;
            }
            fast = fast.next.next;
            slow = slow.next;
        }
        return false;
    }
}
```

**Time Complexity (Big O Time):**

- The program uses two pointers, `fast` and `slow`, to traverse the linked list.
- The `fast` pointer moves twice as fast as the `slow` pointer in each iteration.

The key insight here is that if there is a cycle in the linked list, the `fast` and `slow` pointers will eventually meet (i.e., `fast == slow`). If there is no cycle, the `fast` pointer will reach the end of the list (i.e., `fast == null` or `fast.next == null`) before the `slow` pointer.

Therefore, the time complexity of this algorithm is O(N), where N is the number of nodes in the linked list. This is because in the worst case, when there is no cycle, both pointers will traverse the entire linked list once.

**Space Complexity (Big O Space):**

The space complexity of this algorithm is O(1), which means it uses a constant amount of extra space regardless of the size of the input linked list. This is because it only uses two additional pointers (`fast` and `slow`) to traverse the list and does not rely on additional data structures or recursion that would consume additional memory.

In summary, the time complexity of this program is O(N), and the space complexity is O(1), where N is the number of nodes in the linked list.
