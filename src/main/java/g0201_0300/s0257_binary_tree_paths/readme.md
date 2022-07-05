[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 257\. Binary Tree Paths

Easy

Given the `root` of a binary tree, return _all root-to-leaf paths in **any order**_.

A **leaf** is a node with no children.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/12/paths-tree.jpg)

**Input:** root = [1,2,3,null,5]

**Output:** ["1->2->5","1->3"] 

**Example 2:**

**Input:** root = [1]

**Output:** ["1"] 

**Constraints:**

*   The number of nodes in the tree is in the range `[1, 100]`.
*   `-100 <= Node.val <= 100`

## Solution

```java
import com_github_leetcode.TreeNode;
import java.util.ArrayList;
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
    private List<String> result;
    private StringBuilder sb;

    public List<String> binaryTreePaths(TreeNode root) {
        result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        sb = new StringBuilder();
        walkThrough(root);
        return result;
    }

    private void walkThrough(TreeNode root) {
        assert root != null;
        int length = sb.length();
        sb.append(root.val);
        length = sb.length() - length;
        if (root.left == null && root.right == null) {
            // leaf node.
            result.add(sb.toString());
            sb.delete(sb.length() - length, sb.length());
            return;
        }
        sb.append("->");
        length += 2;
        if (root.left != null) {
            walkThrough(root.left);
        }
        if (root.right != null) {
            walkThrough(root.right);
        }
        sb.delete(sb.length() - length, sb.length());
    }
}
```