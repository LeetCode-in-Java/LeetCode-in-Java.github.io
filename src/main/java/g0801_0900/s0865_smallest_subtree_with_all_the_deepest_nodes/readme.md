[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 865\. Smallest Subtree with all the Deepest Nodes

Medium

Given the `root` of a binary tree, the depth of each node is **the shortest distance to the root**.

Return _the smallest subtree_ such that it contains **all the deepest nodes** in the original tree.

A node is called **the deepest** if it has the largest depth possible among any node in the entire tree.

The **subtree** of a node is a tree consisting of that node, plus the set of all descendants of that node.

**Example 1:**

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/01/sketch1.png)

**Input:** root = [3,5,1,6,2,0,8,null,null,7,4]

**Output:** [2,7,4]

**Explanation:**

We return the node with value 2, colored in yellow in the diagram.

The nodes coloured in blue are the deepest nodes of the tree.

Notice that nodes 5, 3 and 2 contain the deepest nodes in the tree but node 2 is the smallest subtree among them, so we return it.

**Example 2:**

**Input:** root = [1]

**Output:** [1]

**Explanation:** The root is the deepest node in the tree.

**Example 3:**

**Input:** root = [0,1,3,null,2]

**Output:** [2]

**Explanation:** The deepest node in the tree is 2, the valid subtrees are the subtrees of nodes 2, 1 and 0 but the subtree of node 2 is the smallest.

**Constraints:**

*   The number of nodes in the tree will be in the range `[1, 500]`.
*   `0 <= Node.val <= 500`
*   The values of the nodes in the tree are **unique**.

**Note:** This question is the same as 1123: [https://leetcode.com/problems/lowest-common-ancestor-of-deepest-leaves/](https://leetcode.com/problems/lowest-common-ancestor-of-deepest-leaves/)

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
    private int deepLevel = 0;
    private TreeNode left = null;
    private TreeNode right = null;

    public TreeNode subtreeWithAllDeepest(TreeNode root) {
        if (root == null || root.left == null && root.right == null) {
            return root;
        }
        deep(root, 0);
        if (right == null) {
            return left;
        } else {
            return lca(root, left.val, right.val);
        }
    }

    private TreeNode lca(TreeNode root, int left, int right) {
        if (root == null) {
            return null;
        }
        if (root.val == left || root.val == right) {
            return root;
        }
        TreeNode leftLca = lca(root.left, left, right);
        TreeNode rightLca = lca(root.right, left, right);

        if (leftLca != null && rightLca != null) {
            return root;
        } else if (leftLca != null) {
            return leftLca;
        } else {
            return rightLca;
        }
    }

    private void deep(TreeNode root, int level) {
        if (root == null) {
            return;
        }
        if (deepLevel < level) {
            deepLevel = level;
            left = root;
            right = null;
        } else if (deepLevel == level) {
            right = root;
        }
        deep(root.left, level + 1);
        deep(root.right, level + 1);
    }
}
```