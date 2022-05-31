## 783\. Minimum Distance Between BST Nodes

Easy

Given the `root` of a Binary Search Tree (BST), return _the minimum difference between the values of any two different nodes in the tree_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/05/bst1.jpg)

**Input:** root = [4,2,6,1,3]

**Output:** 1 

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/02/05/bst2.jpg)

**Input:** root = [1,0,48,null,null,12,49]

**Output:** 1 

**Constraints:**

*   The number of nodes in the tree is in the range `[2, 100]`.
*   <code>0 <= Node.val <= 10<sup>5</sup></code>

**Note:** This question is the same as 530: [https://leetcode.com/problems/minimum-absolute-difference-in-bst/](https://leetcode.com/problems/minimum-absolute-difference-in-bst/)

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
    private int prev = -1;
    private int min = Integer.MAX_VALUE;

    public int minDiffInBST(TreeNode root) {
        if (root == null) {
            return min;
        }
        minDiffInBST(root.left);
        if (prev != -1) {
            min = Math.min(min, Math.abs(root.val - prev));
        }
        prev = root.val;
        minDiffInBST(root.right);
        return min;
    }
}
```