[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 23\. Merge k Sorted Lists

Hard

You are given an array of `k` linked-lists `lists`, each linked-list is sorted in ascending order.

_Merge all the linked-lists into one sorted linked-list and return it._

**Example 1:**

**Input:** lists = \[\[1,4,5],[1,3,4],[2,6]]

**Output:** [1,1,2,3,4,4,5,6]

**Explanation:** The linked-lists are: [ 1->4->5, 1->3->4, 2->6 ] merging them into one sorted list: 1->1->2->3->4->4->5->6 

**Example 2:**

**Input:** lists = []

**Output:** [] 

**Example 3:**

**Input:** lists = \[\[]]

**Output:** [] 

**Constraints:**

*   `k == lists.length`
*   `0 <= k <= 10^4`
*   `0 <= lists[i].length <= 500`
*   `-10^4 <= lists[i][j] <= 10^4`
*   `lists[i]` is sorted in **ascending order**.
*   The sum of `lists[i].length` won't exceed `10^4`.

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
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists.length == 0) {
            return null;
        }
        return mergeKLists(lists, 0, lists.length);
    }

    private ListNode mergeKLists(ListNode[] lists, int leftIndex, int rightIndex) {
        if (rightIndex > leftIndex + 1) {
            int mid = (leftIndex + rightIndex) / 2;
            ListNode left = mergeKLists(lists, leftIndex, mid);
            ListNode right = mergeKLists(lists, mid, rightIndex);
            return mergeTwoLists(left, right);
        } else {
            return lists[leftIndex];
        }
    }

    private ListNode mergeTwoLists(ListNode left, ListNode right) {
        if (left == null) {
            return right;
        }
        if (right == null) {
            return left;
        }
        ListNode res;
        if (left.val <= right.val) {
            res = left;
            left = left.next;
        } else {
            res = right;
            right = right.next;
        }
        ListNode node = res;
        while (left != null || right != null) {
            if (left == null) {
                node.next = right;
                right = right.next;
            } else if (right == null) {
                node.next = left;
                left = left.next;
            } else {
                if (left.val <= right.val) {
                    node.next = left;
                    left = left.next;
                } else {
                    node.next = right;
                    right = right.next;
                }
            }
            node = node.next;
        }
        return res;
    }
}
```

**Time Complexity (Big O Time):**

1. The `mergeKLists` function is a recursive function that divides the problem into two subproblems of roughly half the size each time it's called. The recurrence relation for this function can be expressed as T(k) = 2T(k/2) + O(n), where k is the number of linked lists and n is the average number of nodes in each list. This recurrence relation represents the time complexity of the divide-and-conquer part of the algorithm.

2. The `mergeTwoLists` function, which merges two sorted linked lists of size m and n, takes O(m + n) time.

3. The `mergeKLists` function calls `mergeTwoLists` for merging two lists at each level of recursion.

Combining these factors, the overall time complexity of the program is O(k * n * log(k)), where k is the number of linked lists, and n is the average number of nodes in each list. The logarithmic term arises from the divide-and-conquer approach, and the linear term comes from merging two lists at each level of recursion.

**Space Complexity (Big O Space):**

The space complexity of this program is O(log(k)) due to the recursive calls to `mergeKLists`. In each recursive call, a new set of recursive function calls and local variables is created, but they are released when the recursion unwinds. Therefore, the space required for the call stack is proportional to the depth of the recursion, which is log(k) in this case.

Additionally, the program uses a constant amount of space for other variables, such as `ListNode` objects for merging the lists and temporary variables for traversal.

In summary, the time complexity of the provided program is O(k * n * log(k)), and the space complexity is O(log(k)), where k is the number of linked lists, and n is the average number of nodes in each list.
