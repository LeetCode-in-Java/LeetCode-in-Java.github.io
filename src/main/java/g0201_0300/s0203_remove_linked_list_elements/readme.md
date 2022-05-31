## 203\. Remove Linked List Elements

Easy

Given the `head` of a linked list and an integer `val`, remove all the nodes of the linked list that has `Node.val == val`, and return _the new head_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/06/removelinked-list.jpg)

**Input:** head = [1,2,6,3,4,5,6], val = 6

**Output:** [1,2,3,4,5] 

**Example 2:**

**Input:** head = [], val = 1

**Output:** [] 

**Example 3:**

**Input:** head = [7,7,7,7], val = 7

**Output:** [] 

**Constraints:**

*   The number of nodes in the list is in the range <code>[0, 10<sup>4</sup>]</code>.
*   `1 <= Node.val <= 50`
*   `0 <= val <= 50`

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
    public ListNode removeElements(ListNode head, int val) {
        if (head == null) {
            return null;
        }
        while (head != null && head.val == val) {
            head = head.next;
        }
        ListNode r = head;
        ListNode t = head;
        while (r != null) {
            if (r.val == val) {
                t.next = r.next;
            } else {
                t = r;
            }
            r = r.next;
        }
        return head;
    }
}
```