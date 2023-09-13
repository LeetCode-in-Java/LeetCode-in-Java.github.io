[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 25\. Reverse Nodes in k-Group

Hard

Given a linked list, reverse the nodes of a linked list _k_ at a time and return its modified list.

_k_ is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of _k_ then left-out nodes, in the end, should remain as it is.

You may not alter the values in the list's nodes, only nodes themselves may be changed.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex1.jpg)

**Input:** head = [1,2,3,4,5], k = 2

**Output:** [2,1,4,3,5] 

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex2.jpg)

**Input:** head = [1,2,3,4,5], k = 3

**Output:** [3,2,1,4,5] 

**Example 3:**

**Input:** head = [1,2,3,4,5], k = 1

**Output:** [1,2,3,4,5] 

**Example 4:**

**Input:** head = [1], k = 1

**Output:** [1] 

**Constraints:**

*   The number of nodes in the list is in the range `sz`.
*   `1 <= sz <= 5000`
*   `0 <= Node.val <= 1000`
*   `1 <= k <= sz`

**Follow-up:** Can you solve the problem in O(1) extra memory space?

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
    public ListNode reverseKGroup(ListNode head, int k) {
        if (head == null || head.next == null || k == 1) {
            return head;
        }
        int j = 0;
        ListNode len = head;
        // loop for checking the length of the linklist, if the linklist is less than k, then return
        // as it is.
        while (j < k) {
            if (len == null) {
                return head;
            }
            len = len.next;
            j++;
        }
        // Reverse linked list logic applied here.
        ListNode c = head;
        ListNode n = null;
        ListNode prev = null;
        int i = 0;
        // Traverse the while loop for K times to reverse the node in K groups.
        while (i != k) {
            n = c.next;
            c.next = prev;
            prev = c;
            c = n;
            i++;
        }
        // C1 is pointing to 1st node of K group, which is now going to point to the next K group
        // linklist.
        // recursion, for futher remaining linked list.
        head.next = reverseKGroup(n, k);
        return prev;
    }
}
```

**Time Complexity (Big O Time):**

1. The program first checks if the length of the linked list is less than k by iterating through the list. This operation takes O(k) time.

2. The main logic of reversing the linked list is contained in a loop that iterates k times (for each group of k nodes). Inside this loop, the reversal of the k nodes takes constant time, O(k). This is because regardless of the size of the input linked list, you are reversing a fixed number of nodes (k) in each iteration.

3. The program uses recursion to reverse the remaining part of the linked list, which will also be done in groups of k nodes. The depth of the recursion will depend on the length of the input linked list, but each level of recursion deals with a sublist of the original list, which is a fraction of the size. Therefore, the overall time complexity of the recursion can be expressed as O(n), where n is the number of nodes in the input linked list.

Combining these factors, the overall time complexity of the program is O(n), where n is the number of nodes in the input linked list. The O(k) part is overshadowed by the O(n) complexity.

**Space Complexity (Big O Space):**

1. The space complexity of the program is O(k), where k is the group size. This is because in each iteration of the reversal loop, a constant amount of additional space is used for variables like `c`, `n`, `prev`, and `i`. The recursion also consumes space on the call stack, but the maximum depth of the recursion is bounded by the length of the list divided by k, so the space used for the call stack can be considered O(n/k), which simplifies to O(n) when analyzing in big O notation.

In summary, the time complexity of the provided program is O(n), and the space complexity is O(k) for the reversal logic within each group of k nodes, and O(n) for the space used on the call stack during recursion, where n is the number of nodes in the input linked list and k is the group size.
