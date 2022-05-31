## 543\. Diameter of Binary Tree

Easy

Given the `root` of a binary tree, return _the length of the **diameter** of the tree_.

The **diameter** of a binary tree is the **length** of the longest path between any two nodes in a tree. This path may or may not pass through the `root`.

The **length** of a path between two nodes is represented by the number of edges between them.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/06/diamtree.jpg)

**Input:** root = [1,2,3,4,5]

**Output:** 3

**Explanation:** 3 is the length of the path [4,2,1,3] or [5,2,1,3]. 

**Example 2:**

**Input:** root = [1,2]

**Output:** 1 

**Constraints:**

*   The number of nodes in the tree is in the range <code>[1, 10<sup>4</sup>]</code>.
*   `-100 <= Node.val <= 100`

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
    private int diameter;

    public int diameterOfBinaryTree(TreeNode root) {
        diameter = 0;
        diameterOfBinaryTreeUtil(root);
        return diameter;
    }

    private int diameterOfBinaryTreeUtil(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int leftLength = root.left != null ? 1 + diameterOfBinaryTreeUtil(root.left) : 0;
        int rightLength = root.right != null ? 1 + diameterOfBinaryTreeUtil(root.right) : 0;
        diameter = Math.max(diameter, leftLength + rightLength);
        return Math.max(leftLength, rightLength);
    }
}
```