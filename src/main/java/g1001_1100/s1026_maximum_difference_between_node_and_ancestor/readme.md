## 1026\. Maximum Difference Between Node and Ancestor

Medium

Given the `root` of a binary tree, find the maximum value `v` for which there exist **different** nodes `a` and `b` where `v = |a.val - b.val|` and `a` is an ancestor of `b`.

A node `a` is an ancestor of `b` if either: any child of `a` is equal to `b` or any child of `a` is an ancestor of `b`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/09/tmp-tree.jpg)

**Input:** root = [8,3,10,1,6,null,14,null,null,4,7,13]

**Output:** 7

**Explanation:** We have various ancestor-node differences, some of which are given below : 
    
    |8 - 3| = 5 
    |3 - 7| = 4 
    |8 - 1| = 7 
    |10 - 13| = 3 

Among all possible differences, the maximum value of 7 is obtained by \|8 - 1\| = 7.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/09/tmp-tree-1.jpg)

**Input:** root = [1,null,2,null,0,3]

**Output:** 3

**Constraints:**

*   The number of nodes in the tree is in the range `[2, 5000]`.
*   <code>0 <= Node.val <= 10<sup>5</sup></code>

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
    private int max = 0;

    public int maxAncestorDiff(TreeNode root) {
        traverse(root, -1, -1);
        return max;
    }

    private void traverse(TreeNode root, int maxAncestor, int minAncestor) {
        if (root == null) {
            return;
        }
        if (maxAncestor == -1) {
            traverse(root.left, root.val, root.val);
            traverse(root.right, root.val, root.val);
        }
        if (maxAncestor != -1) {
            max = Math.max(max, Math.abs(maxAncestor - root.val));
            max = Math.max(max, Math.abs(minAncestor - root.val));

            traverse(root.left, Math.max(root.val, maxAncestor), Math.min(root.val, minAncestor));
            traverse(root.right, Math.max(root.val, maxAncestor), Math.min(root.val, minAncestor));
        }
    }
}
```