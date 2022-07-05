[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 513\. Find Bottom Left Tree Value

Medium

Given the `root` of a binary tree, return the leftmost value in the last row of the tree.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/12/14/tree1.jpg)

**Input:** root = [2,1,3]

**Output:** 1

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/12/14/tree2.jpg)

**Input:** root = [1,2,3,4,null,5,6,null,null,7]

**Output:** 7

**Constraints:**

*   The number of nodes in the tree is in the range <code>[1, 10<sup>4</sup>]</code>.
*   <code>-2<sup>31</sup> <= Node.val <= 2<sup>31</sup> - 1</code>

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
    private int[] func(TreeNode root, int level) {
        if (root.left == null && root.right == null) {
            int[] a = new int[2];
            a[0] = root.val;
            a[1] = level;
            return a;
        }
        int[] a = null;
        int[] b = null;
        if (root.left != null) {
            a = func(root.left, level + 1);
        }
        if (root.right != null) {
            b = func(root.right, level + 1);
        }
        if (a == null) {
            return b;
        } else if (b == null) {
            return a;
        } else {
            if (a[1] >= b[1]) {
                return a;
            } else {
                return b;
            }
        }
    }

    public int findBottomLeftValue(TreeNode root) {
        int[] a = func(root, 0);
        if (a != null && a.length > 0) {
            return a[0];
        }
        return -1;
    }
}
```