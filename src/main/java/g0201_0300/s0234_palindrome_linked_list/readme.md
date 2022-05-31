## 234\. Palindrome Linked List

Easy

Given the `head` of a singly linked list, return `true` if it is a palindrome.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/03/pal1linked-list.jpg)

**Input:** head = [1,2,2,1]

**Output:** true 

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/03/pal2linked-list.jpg)

**Input:** head = [1,2]

**Output:** false 

**Constraints:**

*   The number of nodes in the list is in the range <code>[1, 10<sup>5</sup>]</code>.
*   `0 <= Node.val <= 9`

**Follow up:** Could you do it in `O(n)` time and `O(1)` space?

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
    public boolean isPalindrome(ListNode head) {
        int len = 0;
        ListNode right = head;
        // Culculate the length
        while (right != null) {
            right = right.next;
            len++;
        }
        // Reverse the right half of the list
        len = len / 2;
        right = head;
        for (int i = 0; i < len; i++) {
            right = right.next;
        }
        ListNode prev = null;
        while (right != null) {
            ListNode next = right.next;
            right.next = prev;
            prev = right;
            right = next;
        }
        // Compare left half and right half
        for (int i = 0; i < len; i++) {
            if (prev != null && head.val == prev.val) {
                head = head.next;
                prev = prev.next;
            } else {
                return false;
            }
        }
        return true;
    }
}
```