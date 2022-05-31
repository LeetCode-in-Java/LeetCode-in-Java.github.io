## 106\. Construct Binary Tree from Inorder and Postorder Traversal

Medium

Given two integer arrays `inorder` and `postorder` where `inorder` is the inorder traversal of a binary tree and `postorder` is the postorder traversal of the same tree, construct and return _the binary tree_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/tree.jpg)

**Input:** inorder = [9,3,15,20,7], postorder = [9,15,7,20,3]

**Output:** [3,9,20,null,null,15,7] 

**Example 2:**

**Input:** inorder = [-1], postorder = [-1]

**Output:** [-1] 

**Constraints:**

*   `1 <= inorder.length <= 3000`
*   `postorder.length == inorder.length`
*   `-3000 <= inorder[i], postorder[i] <= 3000`
*   `inorder` and `postorder` consist of **unique** values.
*   Each value of `postorder` also appears in `inorder`.
*   `inorder` is **guaranteed** to be the inorder traversal of the tree.
*   `postorder` is **guaranteed** to be the postorder traversal of the tree.

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
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        int[] inIndex = new int[] {inorder.length - 1};
        int[] postIndex = new int[] {postorder.length - 1};
        return helper(inorder, postorder, inIndex, postIndex, Integer.MAX_VALUE);
    }

    private TreeNode helper(int[] in, int[] post, int[] index, int[] pIndex, int target) {
        if (index[0] < 0 || in[index[0]] == target) {
            return null;
        }
        TreeNode root = new TreeNode(post[pIndex[0]--]);
        root.right = helper(in, post, index, pIndex, root.val);
        index[0]--;
        root.left = helper(in, post, index, pIndex, target);
        return root;
    }
}
```