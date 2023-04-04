[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 671\. Second Minimum Node In a Binary Tree

Easy

Given a non-empty special binary tree consisting of nodes with the non-negative value, where each node in this tree has exactly `two` or `zero` sub-node. If the node has two sub-nodes, then this node's value is the smaller value among its two sub-nodes. More formally, the property `root.val = min(root.left.val, root.right.val)` always holds.

Given such a binary tree, you need to output the **second minimum** value in the set made of all the nodes' value in the whole tree.

If no such second minimum value exists, output -1 instead.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/15/smbt1.jpg)

**Input:** root = [2,2,5,null,null,5,7]

**Output:** 5

**Explanation:** The smallest value is 2, the second smallest value is 5.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/10/15/smbt2.jpg)

**Input:** root = [2,2,2]

**Output:** -1

**Explanation:** The smallest value is 2, but there isn't any second smallest value.

**Constraints:**

*   The number of nodes in the tree is in the range `[1, 25]`.
*   <code>1 <= Node.val <= 2<sup>31</sup> - 1</code>
*   `root.val == min(root.left.val, root.right.val)` for each internal node of the tree.

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
    int min = Integer.MAX_VALUE;
    int secMin = -1;
    int diff = Integer.MAX_VALUE;

    public int findSecondMinimumValue(TreeNode root) {
        if (root == null) {
            return -1;
        }
        if (root.val < min) {
            min = root.val;
        }
        if (root.val != min && Math.abs(root.val - min) < diff) {
            secMin = root.val;
            diff = Math.abs(root.val - min);
        }
        findSecondMinimumValue(root.left);
        findSecondMinimumValue(root.right);
        return secMin;
    }
}
```