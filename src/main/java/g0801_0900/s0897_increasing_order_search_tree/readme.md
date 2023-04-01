[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 897\. Increasing Order Search Tree

Easy

Given the `root` of a binary search tree, rearrange the tree in **in-order** so that the leftmost node in the tree is now the root of the tree, and every node has no left child and only one right child.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/17/ex1.jpg)

**Input:** root = [5,3,6,2,4,null,8,1,null,null,null,7,9]

**Output:** [1,null,2,null,3,null,4,null,5,null,6,null,7,null,8,null,9] 

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/17/ex2.jpg)

**Input:** root = [5,1,7]

**Output:** [1,null,5,null,7] 

**Constraints:**

*   The number of nodes in the given tree will be in the range `[1, 100]`.
*   `0 <= Node.val <= 1000`

## Solution

```java
import com_github_leetcode.TreeNode;
import java.util.LinkedList;
import java.util.List;

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
    public TreeNode increasingBST(final TreeNode root) {
        final List<TreeNode> list = new LinkedList<>();
        traverse(root, list);
        for (int i = 1; i < list.size(); i++) {
            list.get(i - 1).right = list.get(i);
            list.get(i).left = null;
        }
        return list.get(0);
    }

    private void traverse(final TreeNode root, final List<TreeNode> list) {
        if (root != null) {
            traverse(root.left, list);
            list.add(root);
            traverse(root.right, list);
        }
    }
}
```