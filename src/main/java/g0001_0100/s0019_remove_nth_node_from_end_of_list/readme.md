[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 19\. Remove Nth Node From End of List

Medium

Given the `head` of a linked list, remove the `nth` node from the end of the list and return its head.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/03/remove_ex1.jpg)

**Input:** head = [1,2,3,4,5], n = 2

**Output:** [1,2,3,5] 

**Example 2:**

**Input:** head = [1], n = 1

**Output:** [] 

**Example 3:**

**Input:** head = [1,2], n = 1

**Output:** [1] 

**Constraints:**

*   The number of nodes in the list is `sz`.
*   `1 <= sz <= 30`
*   `0 <= Node.val <= 100`
*   `1 <= n <= sz`

**Follow up:** Could you do this in one pass?

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
    private int n;

    public ListNode removeNthFromEnd(ListNode head, int n) {
        this.n = n;
        ListNode node = new ListNode(0, head);
        removeNth(node);
        return node.next;
    }

    private void removeNth(ListNode node) {
        if (node.next == null) {
            return;
        }
        removeNth(node.next);
        this.n--;

        if (this.n == 0) {
            node.next = node.next.next;
        }
    }
}
```

**Time Complexity (Big O Time):**

The time complexity of this program is O(L), where "L" is the length of the input linked list `head`. Here's the breakdown:

1. The program uses a recursive approach to remove the nth node from the end of the linked list.

2. The `removeNthFromEnd` method is called recursively for each node in the linked list. The recursive calls traverse the entire list once, visiting each node exactly once.

3. Since the recursive calls traverse the entire list, the total number of operations performed is proportional to the length of the linked list `head`, which is "L."

Therefore, the overall time complexity is O(L), where "L" is the length of the linked list.

**Space Complexity (Big O Space):**

The space complexity of this program is O(L), where "L" is the length of the input linked list `head`. Here's why:

1. The program uses a recursive approach, and for each recursive call, it adds a new frame to the call stack. In the worst case, when the linked list is traversed from beginning to end, there can be a maximum of "L" recursive calls on the stack at once.

2. Each recursive call stores local variables and a reference to the next recursive call. The space used by each call frame is constant, but the number of frames on the stack is proportional to the length of the linked list.

3. Additionally, the program uses a few integer variables and references to linked list nodes, but these use constant space relative to the input size.

Therefore, the overall space complexity is O(L), where "L" is the length of the linked list.
