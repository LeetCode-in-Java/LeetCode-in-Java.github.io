[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 105\. Construct Binary Tree from Preorder and Inorder Traversal

Medium

Given two integer arrays `preorder` and `inorder` where `preorder` is the preorder traversal of a binary tree and `inorder` is the inorder traversal of the same tree, construct and return _the binary tree_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/tree.jpg)

**Input:** preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]

**Output:** [3,9,20,null,null,15,7] 

**Example 2:**

**Input:** preorder = [-1], inorder = [-1]

**Output:** [-1] 

**Constraints:**

*   `1 <= preorder.length <= 3000`
*   `inorder.length == preorder.length`
*   `-3000 <= preorder[i], inorder[i] <= 3000`
*   `preorder` and `inorder` consist of **unique** values.
*   Each value of `inorder` also appears in `preorder`.
*   `preorder` is **guaranteed** to be the preorder traversal of the tree.
*   `inorder` is **guaranteed** to be the inorder traversal of the tree.

## Solution

```java
import com_github_leetcode.TreeNode;
import java.util.HashMap;
import java.util.Map;

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
    private int j;
    private Map<Integer, Integer> map = new HashMap<>();

    public int get(int key) {
        return map.get(key);
    }

    private TreeNode answer(int[] preorder, int[] inorder, int start, int end) {
        if (start > end || j > preorder.length) {
            return null;
        }
        int value = preorder[j++];
        int index = get(value);
        TreeNode node = new TreeNode(value);
        node.left = answer(preorder, inorder, start, index - 1);
        node.right = answer(preorder, inorder, index + 1, end);
        return node;
    }

    public TreeNode buildTree(int[] preorder, int[] inorder) {
        j = 0;
        for (int i = 0; i < preorder.length; i++) {
            map.put(inorder[i], i);
        }
        return answer(preorder, inorder, 0, preorder.length - 1);
    }
}
```