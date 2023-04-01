[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 687\. Longest Univalue Path

Medium

Given the `root` of a binary tree, return _the length of the longest path, where each node in the path has the same value_. This path may or may not pass through the root.

**The length of the path** between two nodes is represented by the number of edges between them.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/13/ex1.jpg)

**Input:** root = [5,4,5,1,1,5]

**Output:** 2

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/10/13/ex2.jpg)

**Input:** root = [1,4,5,4,4,5]

**Output:** 2

**Constraints:**

*   The number of nodes in the tree is in the range <code>[0, 10<sup>4</sup>]</code>.
*   `-1000 <= Node.val <= 1000`
*   The depth of the tree will not exceed `1000`.

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
    public int longestUnivaluePath(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int[] res = new int[1];
        preorderLongestSinglePathLen(root, res);
        return res[0];
    }

    private int preorderLongestSinglePathLen(TreeNode root, int[] res) {
        if (root == null) {
            return -1;
        }
        int left = preorderLongestSinglePathLen(root.left, res);
        int right = preorderLongestSinglePathLen(root.right, res);

        if (root.left == null || root.val == root.left.val) {
            left = left + 1;
        } else {
            left = 0;
        }

        if (root.right == null || root.val == root.right.val) {
            right = right + 1;
        } else {
            right = 0;
        }
        int longestPathLenPassingThroughRoot = left + right;
        res[0] = Math.max(res[0], longestPathLenPassingThroughRoot);
        return Math.max(left, right);
    }
}
```