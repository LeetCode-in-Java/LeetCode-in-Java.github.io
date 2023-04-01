[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 988\. Smallest String Starting From Leaf

Medium

You are given the `root` of a binary tree where each node has a value in the range `[0, 25]` representing the letters `'a'` to `'z'`.

Return _the **lexicographically smallest** string that starts at a leaf of this tree and ends at the root_.

As a reminder, any shorter prefix of a string is **lexicographically smaller**.

*   For example, `"ab"` is lexicographically smaller than `"aba"`.

A leaf of a node is a node that has no children.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/01/30/tree1.png)

**Input:** root = [0,1,2,3,4,3,4]

**Output:** "dba"

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/01/30/tree2.png)

**Input:** root = [25,1,3,1,3,0,2]

**Output:** "adz"

**Example 3:**

![](https://assets.leetcode.com/uploads/2019/02/01/tree3.png)

**Input:** root = [2,2,1,null,1,0,null,0]

**Output:** "abc"

**Constraints:**

*   The number of nodes in the tree is in the range `[1, 8500]`.
*   `0 <= Node.val <= 25`

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
    private String res = "";

    public String smallestFromLeaf(TreeNode root) {
        dfs(root, new StringBuilder());
        return res;
    }

    private void dfs(TreeNode root, StringBuilder currStr) {
        if (root == null) {
            return;
        }
        currStr.insert(0, (char) (root.val + 97));
        if (root.left == null && root.right == null) {
            if (res.equals("")) {
                res = currStr.toString();
            } else {
                res = res.compareTo(currStr.toString()) > 0 ? currStr.toString() : res;
            }
        } else {
            dfs(root.left, currStr);
            dfs(root.right, currStr);
        }
        currStr.deleteCharAt(0);
    }
}
```