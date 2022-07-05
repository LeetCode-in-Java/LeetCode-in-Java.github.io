[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 222\. Count Complete Tree Nodes

Medium

Given the `root` of a **complete** binary tree, return the number of the nodes in the tree.

According to **[Wikipedia](http://en.wikipedia.org/wiki/Binary_tree#Types_of_binary_trees)**, every level, except possibly the last, is completely filled in a complete binary tree, and all nodes in the last level are as far left as possible. It can have between `1` and <code>2<sup>h</sup></code> nodes inclusive at the last level `h`.

Design an algorithm that runs in less than `O(n)` time complexity.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/14/complete.jpg)

**Input:** root = [1,2,3,4,5,6]

**Output:** 6 

**Example 2:**

**Input:** root = []

**Output:** 0 

**Example 3:**

**Input:** root = [1]

**Output:** 1 

**Constraints:**

*   The number of nodes in the tree is in the range <code>[0, 5 * 10<sup>4</sup>]</code>.
*   <code>0 <= Node.val <= 5 * 10<sup>4</sup></code>
*   The tree is guaranteed to be **complete**.

## Solution

```java
import com_github_leetcode.TreeNode;

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
    public int countNodes(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int leftHeight = leftHeight(root);
        int rightHeight = rightHeight(root);
        // case 1: When Height(Left sub-tree) = Height(right sub-tree) 2^h - 1
        if (leftHeight == rightHeight) {
            return (1 << leftHeight) - 1;
        } else {
            return 1 + countNodes(root.left) + countNodes(root.right);
        }
    }

    private int leftHeight(TreeNode root) {
        if (root == null) {
            return 0;
        }
        return 1 + leftHeight(root.left);
    }

    private int rightHeight(TreeNode root) {
        if (root == null) {
            return 0;
        }
        return 1 + rightHeight(root.right);
    }
}
```