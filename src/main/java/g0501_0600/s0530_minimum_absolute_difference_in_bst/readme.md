[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 530\. Minimum Absolute Difference in BST

Easy

Given the `root` of a Binary Search Tree (BST), return _the minimum absolute difference between the values of any two different nodes in the tree_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/05/bst1.jpg)

**Input:** root = [4,2,6,1,3]

**Output:** 1

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/02/05/bst2.jpg)

**Input:** root = [1,0,48,null,null,12,49]

**Output:** 1

**Constraints:**

*   The number of nodes in the tree is in the range <code>[2, 10<sup>4</sup>]</code>.
*   <code>0 <= Node.val <= 10<sup>5</sup></code>

**Note:** This question is the same as 783: [https://leetcode.com/problems/minimum-distance-between-bst-nodes/](https://leetcode.com/problems/minimum-distance-between-bst-nodes/)

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
    private int ans = Integer.MAX_VALUE;
    private int prev = Integer.MAX_VALUE;

    public int getMinimumDifference(TreeNode root) {
        if (root == null) {
            return ans;
        }
        getMinimumDifference(root.left);
        ans = Math.min(ans, Math.abs(root.val - prev));
        prev = root.val;
        getMinimumDifference(root.right);
        return ans;
    }
}
```