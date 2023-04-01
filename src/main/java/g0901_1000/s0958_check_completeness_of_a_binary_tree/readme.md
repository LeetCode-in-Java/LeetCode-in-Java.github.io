[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 958\. Check Completeness of a Binary Tree

Medium

Given the `root` of a binary tree, determine if it is a _complete binary tree_.

In a **[complete binary tree](http://en.wikipedia.org/wiki/Binary_tree#Types_of_binary_trees)**, every level, except possibly the last, is completely filled, and all nodes in the last level are as far left as possible. It can have between `1` and <code>2<sup>h</sup></code> nodes inclusive at the last level `h`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/12/15/complete-binary-tree-1.png)

**Input:** root = [1,2,3,4,5,6]

**Output:** true

**Explanation:** Every level before the last is full (ie. levels with node-values {1} and {2, 3}), and all nodes in the last level ({4, 5, 6}) are as far left as possible.

**Example 2:**

![](https://assets.leetcode.com/uploads/2018/12/15/complete-binary-tree-2.png)

**Input:** root = [1,2,3,4,5,null,7]

**Output:** false

**Explanation:** The node with value 7 isn't as far left as possible.

**Constraints:**

*   The number of nodes in the tree is in the range `[1, 100]`.
*   `1 <= Node.val <= 1000`

## Solution

```java
import com_github_leetcode.TreeNode;
import java.util.LinkedList;

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
    public boolean isCompleteTree(TreeNode root) {
        /* 1. Tree no exist? complete! */
        if (root == null) {
            return true;
        }
        /* 2. Create a queue for purposeful sequence of inserting children nodes
        //   (Left to right, level by level)  */
        LinkedList<TreeNode> queue = new LinkedList<>();
        queue.add(root.left);
        queue.add(root.right);
        /* 3. Essential for our loop. Checks if we seen a null. */
        boolean seenNull = false;
        /* 4. While the queue isn't empty.
        //   Catch that null, and if there is a # after, it's not complete */
        while (!queue.isEmpty()) {
            TreeNode node = queue.poll();
            if (node == null) {
                seenNull = true;
            } else {
                if (seenNull) {
                    return false;
                }
                queue.add(node.left);
                queue.add(node.right);
            }
        }
        /*5. If we don't catch null with # after: it's complete */
        return true;
    }
}
```