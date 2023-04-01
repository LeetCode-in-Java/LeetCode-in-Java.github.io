[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 863\. All Nodes Distance K in Binary Tree

Medium

Given the `root` of a binary tree, the value of a target node `target`, and an integer `k`, return _an array of the values of all nodes that have a distance_ `k` _from the target node._

You can return the answer in **any order**.

**Example 1:**

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/06/28/sketch0.png)

**Input:** root = [3,5,1,6,2,0,8,null,null,7,4], target = 5, k = 2

**Output:** [7,4,1]

**Explanation:** The nodes that are a distance 2 from the target node (with value 5) have values 7, 4, and 1.

**Example 2:**

**Input:** root = [1], target = 1, k = 3

**Output:** []

**Constraints:**

*   The number of nodes in the tree is in the range `[1, 500]`.
*   `0 <= Node.val <= 500`
*   All the values `Node.val` are **unique**.
*   `target` is the value of one of the nodes in the tree.
*   `0 <= k <= 1000`

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
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    private void kFar(TreeNode root, int k, TreeNode visited, List<Integer> ls) {
        if (root == null || k < 0 || root == visited) {
            return;
        }
        if (k == 0) {
            ls.add(root.val);
            return;
        }
        kFar(root.left, k - 1, visited, ls);
        kFar(root.right, k - 1, visited, ls);
    }

    public List<Integer> distanceK(TreeNode root, TreeNode target, int k) {
        List<Integer> ls = new ArrayList<>();
        if (k == 0) {
            ls.add(target.val);
            return ls;
        }
        nodeToRoot(root, target, k, ls);
        return ls;
    }

    private int nodeToRoot(TreeNode root, TreeNode target, int k, List<Integer> ls) {
        if (root == null) {
            return -1;
        }
        if (root.val == target.val) {
            kFar(root, k, null, ls);
            return 1;
        }
        int ld = nodeToRoot(root.left, target, k, ls);
        if (ld != -1) {
            kFar(root, k - ld, root.left, ls);
            return ld + 1;
        }
        int rd = nodeToRoot(root.right, target, k, ls);
        if (rd != -1) {
            kFar(root, k - rd, root.right, ls);
            return rd + 1;
        }
        return -1;
    }
}
```