## 889\. Construct Binary Tree from Preorder and Postorder Traversal

Medium

Given two integer arrays, `preorder` and `postorder` where `preorder` is the preorder traversal of a binary tree of **distinct** values and `postorder` is the postorder traversal of the same tree, reconstruct and return _the binary tree_.

If there exist multiple answers, you can **return any** of them.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/07/24/lc-prepost.jpg)

**Input:** preorder = [1,2,4,5,3,6,7], postorder = [4,5,2,6,7,3,1]

**Output:** [1,2,3,4,5,6,7]

**Example 2:**

**Input:** preorder = [1], postorder = [1]

**Output:** [1]

**Constraints:**

*   `1 <= preorder.length <= 30`
*   `1 <= preorder[i] <= preorder.length`
*   All the values of `preorder` are **unique**.
*   `postorder.length == preorder.length`
*   `1 <= postorder[i] <= postorder.length`
*   All the values of `postorder` are **unique**.
*   It is guaranteed that `preorder` and `postorder` are the preorder traversal and postorder traversal of the same binary tree.

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
    public TreeNode constructFromPrePost(int[] preorder, int[] postorder) {
        if (preorder.length == 0 || preorder.length != postorder.length) {
            return null;
        }
        return buildTree(preorder, 0, preorder.length - 1, postorder, 0, postorder.length - 1);
    }

    private TreeNode buildTree(
            int[] preorder, int preStart, int preEnd, int[] postorder, int postStart, int postEnd) {
        if (preStart > preEnd || postStart > postEnd) {
            return null;
        }
        int data = preorder[preStart];
        TreeNode root = new TreeNode(data);
        if (preStart == preEnd) {
            return root;
        }
        int offset = postStart;
        for (; offset <= preEnd; offset++) {
            if (postorder[offset] == preorder[preStart + 1]) {
                break;
            }
        }
        root.left =
                buildTree(
                        preorder,
                        preStart + 1,
                        preStart + offset - postStart + 1,
                        postorder,
                        postStart,
                        offset);
        root.right =
                buildTree(
                        preorder,
                        preStart + offset - postStart + 2,
                        preEnd,
                        postorder,
                        offset + 1,
                        postEnd - 1);
        return root;
    }
}
```