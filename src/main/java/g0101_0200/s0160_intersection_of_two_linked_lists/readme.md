[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 160\. Intersection of Two Linked Lists

Easy

Given the heads of two singly linked-lists `headA` and `headB`, return _the node at which the two lists intersect_. If the two linked lists have no intersection at all, return `null`.

For example, the following two linked lists begin to intersect at node `c1`:

![](https://assets.leetcode.com/uploads/2021/03/05/160_statement.png)

The test cases are generated such that there are no cycles anywhere in the entire linked structure.

**Note** that the linked lists must **retain their original structure** after the function returns.

**Custom Judge:**

The inputs to the **judge** are given as follows (your program is **not** given these inputs):

*   `intersectVal` - The value of the node where the intersection occurs. This is `0` if there is no intersected node.
*   `listA` - The first linked list.
*   `listB` - The second linked list.
*   `skipA` - The number of nodes to skip ahead in `listA` (starting from the head) to get to the intersected node.
*   `skipB` - The number of nodes to skip ahead in `listB` (starting from the head) to get to the intersected node.

The judge will then create the linked structure based on these inputs and pass the two heads, `headA` and `headB` to your program. If you correctly return the intersected node, then your solution will be **accepted**.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/05/160_example_1_1.png)

**Input:** intersectVal = 8, listA = [4,1,8,4,5], listB = [5,6,1,8,4,5], skipA = 2, skipB = 3

**Output:** Intersected at '8'

**Explanation:** The intersected node's value is 8 (note that this must not be 0 if the two lists intersect).

From the head of A, it reads as [4,1,8,4,5]. From the head of B, it reads as [5,6,1,8,4,5]. There are 2 nodes before the intersected node in A; There are 3 nodes before the intersected node in B.

- Note that the intersected node's value is not 1 because the nodes with value 1 in A and B (2<sup>nd</sup> node in A and 3<sup>rd</sup> node in B) are different node references. In other words, they point to two different locations in memory, while the nodes with value 8 in A and B (3<sup>rd</sup> node in A and 4<sup>th</sup> node in B) point to the same location in memory. 

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/05/160_example_2.png)

**Input:** intersectVal = 2, listA = [1,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1

**Output:** Intersected at '2'

**Explanation:** The intersected node's value is 2 (note that this must not be 0 if the two lists intersect). From the head of A, it reads as [1,9,1,2,4]. From the head of B, it reads as [3,2,4]. There are 3 nodes before the intersected node in A; There are 1 node before the intersected node in B. 

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/03/05/160_example_3.png)

**Input:** intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2

**Output:** No intersection

**Explanation:** From the head of A, it reads as [2,6,4]. From the head of B, it reads as [1,5]. Since the two lists do not intersect, intersectVal must be 0, while skipA and skipB can be arbitrary values. Explanation: The two lists do not intersect, so return null. 

**Constraints:**

*   The number of nodes of `listA` is in the `m`.
*   The number of nodes of `listB` is in the `n`.
*   <code>1 <= m, n <= 3 * 10<sup>4</sup></code>
*   <code>1 <= Node.val <= 10<sup>5</sup></code>
*   `0 <= skipA <= m`
*   `0 <= skipB <= n`
*   `intersectVal` is `0` if `listA` and `listB` do not intersect.
*   `intersectVal == listA[skipA] == listB[skipB]` if `listA` and `listB` intersect.

**Follow up:** Could you write a solution that runs in `O(m + n)` time and use only `O(1)` memory?

## Solution

```java
import com_github_leetcode.ListNode;

/*
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
@SuppressWarnings("java:S2583")
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode node1 = headA;
        ListNode node2 = headB;
        while (node1 != node2) {
            node1 = node1 == null ? headB : node1.next;
            node2 = node2 == null ? headA : node2.next;
        }
        return node1;
    }
}
```

**Time Complexity (Big O Time):**

The time complexity of this program is O(M + N), where M is the length of the first linked list (headA) and N is the length of the second linked list (headB). Here's why:

- The two pointers `node1` and `node2` traverse their respective linked lists until they either meet at the intersection or reach the end of their lists.
- In the worst case, where there is no intersection, both `node1` and `node2` will traverse both linked lists completely.
- Therefore, each of the two pointers will perform a total of M + N operations (M for the first list and N for the second list).

The program terminates as soon as `node1` and `node2` meet or both become null.

**Space Complexity (Big O Space):**

The space complexity of this program is O(1), which means it uses a constant amount of extra space, regardless of the size of the linked lists. The program only uses a fixed number of pointers (`node1`, `node2`) and variables to keep track of the intersection point.

In summary, the time complexity is O(M + N) where M and N are the lengths of the two linked lists, and the space complexity is O(1), indicating a constant amount of additional memory usage.
