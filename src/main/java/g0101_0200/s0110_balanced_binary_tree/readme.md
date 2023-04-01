[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 110\. Balanced Binary Tree

Easy

Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as:

> a binary tree in which the left and right subtrees of _every_ node differ in height by no more than 1.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/06/balance_1.jpg)

**Input:** root = [3,9,20,null,null,15,7]

**Output:** true 

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/10/06/balance_2.jpg)

**Input:** root = [1,2,2,3,3,null,null,4,4]

**Output:** false 

**Example 3:**

**Input:** root = []

**Output:** true 

**Constraints:**

*   The number of nodes in the tree is in the range `[0, 5000]`.
*   <code>-10<sup>4</sup> <= Node.val <= 10<sup>4</sup></code>

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
    public boolean isBalanced(TreeNode root) {
        // Empty Tree is balanced
        if (root == null) {
            return true;
        }
        // Get max height of subtree child
        // Get max height of subtree child
        // compare height difference (cannot be more than 1)
        int leftHeight = 0;
        int rightHeight = 0;
        if (root.left != null) {
            leftHeight = getMaxHeight(root.left);
        }
        if (root.right != null) {
            rightHeight = getMaxHeight(root.right);
        }
        int heightDiff = Math.abs(leftHeight - rightHeight);
        // if passes height check
        //  - Check if left subtree is balanced and if the right subtree is balanced
        //  - If one of both are imbalanced, then the tree is imbalanced
        return heightDiff <= 1 && isBalanced(root.left) && isBalanced(root.right);
    }

    private int getMaxHeight(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int leftHeight = 0;
        int rightHeight = 0;
        // Left
        if (root.left != null) {
            leftHeight = getMaxHeight(root.left);
        }
        // Right
        if (root.right != null) {
            rightHeight = getMaxHeight(root.right);
        }
        if (leftHeight > rightHeight) {
            return 1 + leftHeight;
        } else {
            return 1 + rightHeight;
        }
    }
}
```