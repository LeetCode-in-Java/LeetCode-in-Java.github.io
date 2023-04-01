[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 993\. Cousins in Binary Tree

Easy

Given the `root` of a binary tree with unique values and the values of two different nodes of the tree `x` and `y`, return `true` _if the nodes corresponding to the values_ `x` _and_ `y` _in the tree are **cousins**, or_ `false` _otherwise._

Two nodes of a binary tree are **cousins** if they have the same depth with different parents.

Note that in a binary tree, the root node is at the depth `0`, and children of each depth `k` node are at the depth `k + 1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/02/12/q1248-01.png)

**Input:** root = [1,2,3,4], x = 4, y = 3

**Output:** false

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/02/12/q1248-02.png)

**Input:** root = [1,2,3,null,4,null,5], x = 5, y = 4

**Output:** true

**Example 3:**

![](https://assets.leetcode.com/uploads/2019/02/13/q1248-03.png)

**Input:** root = [1,2,3,null,4], x = 2, y = 3

**Output:** false

**Constraints:**

*   The number of nodes in the tree is in the range `[2, 100]`.
*   `1 <= Node.val <= 100`
*   Each node has a **unique** value.
*   `x != y`
*   `x` and `y` are exist in the tree.

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
    public boolean isCousins(TreeNode root, int x, int y) {
        return !isSiblings(root, x, y) && isSameLevels(root, x, y);
    }

    private boolean isSameLevels(TreeNode root, int x, int y) {
        return findLevel(root, x, 0) == findLevel(root, y, 0);
    }

    private int findLevel(TreeNode root, int x, int level) {
        if (root == null) {
            return -1;
        }
        if (root.val == x) {
            return level;
        }
        int leftLevel = findLevel(root.left, x, level + 1);
        if (leftLevel == -1) {
            return findLevel(root.right, x, level + 1);
        } else {
            return leftLevel;
        }
    }

    private boolean isSiblings(TreeNode root, int x, int y) {
        if (root == null) {
            return false;
        }
        // Check children first
        boolean leftSubTreeContainsCousins = isSiblings(root.left, x, y);
        boolean rightSubTreeContainsCousins = isSiblings(root.right, x, y);
        if (leftSubTreeContainsCousins || rightSubTreeContainsCousins) {
            return true;
        }
        if (root.left == null || root.right == null) {
            return false;
        }
        // Check for siblings at parent
        return root.left.val == x && root.right.val == y
                || root.right.val == x && root.left.val == y;
    }
}
```