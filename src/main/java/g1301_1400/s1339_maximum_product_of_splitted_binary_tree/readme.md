[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1339\. Maximum Product of Splitted Binary Tree

Medium

Given the `root` of a binary tree, split the binary tree into two subtrees by removing one edge such that the product of the sums of the subtrees is maximized.

Return _the maximum product of the sums of the two subtrees_. Since the answer may be too large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Note** that you need to maximize the answer before taking the mod and not after taking it.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/01/21/sample_1_1699.png)

**Input:** root = [1,2,3,4,5,6]

**Output:** 110

**Explanation:** Remove the red edge and get 2 binary trees with sum 11 and 10. Their product is 110 (11\*10)

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/01/21/sample_2_1699.png)

**Input:** root = [1,null,2,3,4,null,null,5,6]

**Output:** 90

**Explanation:** Remove the red edge and get 2 binary trees with sum 15 and 6.Their product is 90 (15\*6)

**Constraints:**

*   The number of nodes in the tree is in the range <code>[2, 5 * 10<sup>4</sup>]</code>.
*   <code>1 <= Node.val <= 10<sup>4</sup></code>

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
    private long maxProduct = 0;
    private long total = 0;

    public int sumTree(TreeNode node) {
        if (node == null) {
            return 0;
        }
        node.val += sumTree(node.left) + sumTree(node.right);
        return node.val;
    }

    private void helper(TreeNode root) {
        if (root == null) {
            return;
        }
        helper(root.left);
        helper(root.right);
        long leftSubtreeVal = root.left != null ? root.left.val : 0L;
        long leftProduct = leftSubtreeVal * (total - leftSubtreeVal);
        long rightSubtreeVal = root.right != null ? root.right.val : 0L;
        long rightProduct = rightSubtreeVal * (total - rightSubtreeVal);
        maxProduct = Math.max(maxProduct, Math.max(leftProduct, rightProduct));
    }

    public int maxProduct(TreeNode root) {
        if (root == null) {
            return 0;
        }
        total = sumTree(root);
        helper(root);
        return (int) (maxProduct % 1000000007L);
    }
}
```