[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

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

**Time Complexity (Big O Time):**

1. **Calculating the Length of the List:** The program first iterates through the entire linked list to calculate its length. This traversal takes O(n) time, where "n" is the number of nodes in the linked list.

2. **Reversing the Right Half:** The program reverses the right half of the linked list, which also takes O(n/2) time in the worst case (as we reverse only the second half). In Big O notation, this is still O(n) time complexity.

3. **Comparing Left and Right Halves:** After reversing the right half, the program compares the left half and right half of the linked list by iterating through both halves, which takes O(n/2) time in the worst case.

Therefore, the overall time complexity of the program is O(n).

**Space Complexity (Big O Space):**

The space complexity of the program is determined by the additional memory used for reversing the right half of the linked list. Here's how space is used:

- Integer Variables: The program uses integer variables (`len`, `i`) to store indices and lengths. These variables occupy constant space, so their contribution to space complexity is O(1).

- Pointers: The program uses three pointers (`right`, `prev`, `next`) for reversing the right half. These pointers occupy constant space, so their contribution to space complexity is O(1).

In summary, the overall space complexity of the program is O(1) because it uses a constant amount of extra space, regardless of the size of the linked list. The space used for reversing the right half does not depend on the size of the input linked list.
