[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1367\. Linked List in Binary Tree

Medium

Given a binary tree `root` and a linked list with `head` as the first node.

Return True if all the elements in the linked list starting from the `head` correspond to some _downward path_ connected in the binary tree otherwise return False.

In this context downward path means a path that starts at some node and goes downwards.

**Example 1:**

**![](https://assets.leetcode.com/uploads/2020/02/12/sample_1_1720.png)**

**Input:** head = [4,2,8], root = [1,4,4,null,2,2,null,1,null,6,8,null,null,null,null,1,3]

**Output:** true

**Explanation:** Nodes in blue form a subpath in the binary Tree.

**Example 2:**

**![](https://assets.leetcode.com/uploads/2020/02/12/sample_2_1720.png)**

**Input:** head = [1,4,2,6], root = [1,4,4,null,2,2,null,1,null,6,8,null,null,null,null,1,3]

**Output:** true

**Example 3:**

**Input:** head = [1,4,2,6,8], root = [1,4,4,null,2,2,null,1,null,6,8,null,null,null,null,1,3]

**Output:** false

**Explanation:** There is no path in the binary tree that contains all the elements of the linked list from `head`.

**Constraints:**

*   The number of nodes in the tree will be in the range `[1, 2500]`.
*   The number of nodes in the list will be in the range `[1, 100]`.
*   `1 <= Node.val <= 100` for each node in the linked list and binary tree.

## Solution

```java
import com_github_leetcode.ListNode;
import com_github_leetcode.TreeNode;

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
/*
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
public class Solution {
    public boolean isSubPath(ListNode head, TreeNode root) {
        if (root == null) {
            return false;
        }
        return doesRootHaveList(head, root)
                || isSubPath(head, root.left)
                || isSubPath(head, root.right);
    }

    private boolean doesRootHaveList(ListNode head, TreeNode root) {
        if (head == null) {
            return true;
        }
        if (root == null) {
            return false;
        }

        return head.val == root.val
                && (doesRootHaveList(head.next, root.left)
                        || doesRootHaveList(head.next, root.right));
    }
}
```