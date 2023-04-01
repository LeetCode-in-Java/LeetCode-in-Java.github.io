[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 655\. Print Binary Tree

Medium

Given the `root` of a binary tree, construct a **0-indexed** `m x n` string matrix `res` that represents a **formatted layout** of the tree. The formatted layout matrix should be constructed using the following rules:

*   The **height** of the tree is `height` and the number of rows `m` should be equal to `height + 1`.
*   The number of columns `n` should be equal to <code>2<sup>height+1</sup> - 1</code>.
*   Place the **root node** in the **middle** of the **top row** (more formally, at location `res[0][(n-1)/2]`).
*   For each node that has been placed in the matrix at position `res[r][c]`, place its **left child** at <code>res[r+1][c-2<sup>height-r-1</sup>]</code> and its **right child** at <code>res[r+1][c+2<sup>height-r-1</sup>]</code>.
*   Continue this process until all the nodes in the tree have been placed.
*   Any empty cells should contain the empty string `""`.

Return _the constructed matrix_ `res`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/05/03/print1-tree.jpg)

**Input:** root = [1,2]

**Output:** [["","1",""], ["2","",""]]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/05/03/print2-tree.jpg)

**Input:** root = [1,2,3,null,4]

**Output:** [["","","","1","","",""], ["","2","","","","3",""], ["","","4","","","",""]]

**Constraints:**

*   The number of nodes in the tree is in the range <code>[1, 2<sup>10</sup>]</code>.
*   `-99 <= Node.val <= 99`
*   The depth of the tree will be in the range `[1, 10]`.

## Solution

```java
import com_github_leetcode.TreeNode;
import java.util.ArrayList;
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
    public List<List<String>> printTree(TreeNode root) {
        List<List<String>> result = new LinkedList<>();
        int height = root == null ? 1 : getHeight(root);
        int columns = (int) (Math.pow(2, height) - 1);
        List<String> row = new ArrayList<>();
        for (int i = 0; i < columns; i++) {
            row.add("");
        }
        for (int i = 0; i < height; i++) {
            result.add(new ArrayList<>(row));
        }
        populateResult(root, result, 0, height, 0, columns - 1);
        return result;
    }

    private void populateResult(
            TreeNode root, List<List<String>> result, int row, int totalRows, int i, int j) {
        if (row == totalRows || root == null) {
            return;
        }
        result.get(row).set((i + j) / 2, Integer.toString(root.val));
        populateResult(root.left, result, row + 1, totalRows, i, (i + j) / 2 - 1);
        populateResult(root.right, result, row + 1, totalRows, (i + j) / 2 + 1, j);
    }

    private int getHeight(TreeNode root) {
        if (root == null) {
            return 0;
        }
        return 1 + Math.max(getHeight(root.left), getHeight(root.right));
    }
}
```