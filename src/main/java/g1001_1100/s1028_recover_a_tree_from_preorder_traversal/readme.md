[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1028\. Recover a Tree From Preorder Traversal

Hard

We run a preorder depth-first search (DFS) on the `root` of a binary tree.

At each node in this traversal, we output `D` dashes (where `D` is the depth of this node), then we output the value of this node. If the depth of a node is `D`, the depth of its immediate child is `D + 1`. The depth of the `root` node is `0`.

If a node has only one child, that child is guaranteed to be **the left child**.

Given the output `traversal` of this traversal, recover the tree and return _its_ `root`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/04/08/recover-a-tree-from-preorder-traversal.png)

**Input:** traversal = "1-2--3--4-5--6--7"

**Output:** [1,2,5,3,4,6,7]

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/04/11/screen-shot-2019-04-10-at-114101-pm.png)

**Input:** traversal = "1-2--3---4-5--6---7"

**Output:** [1,2,5,3,null,6,null,4,null,7]

**Example 3:**

![](https://assets.leetcode.com/uploads/2019/04/11/screen-shot-2019-04-10-at-114955-pm.png)

**Input:** traversal = "1-401--349---90--88"

**Output:** [1,401,null,349,88,90]

**Constraints:**

*   The number of nodes in the original tree is in the range `[1, 1000]`.
*   <code>1 <= Node.val <= 10<sup>9</sup></code>

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
    private int ptr = 0;

    public TreeNode recoverFromPreorder(String traversal) {
        return find(traversal, 0);
    }

    private TreeNode find(String traversal, int level) {
        if (ptr == traversal.length()) {
            return null;
        }
        int i = ptr;
        int count = 0;
        while (traversal.charAt(i) == '-') {
            count++;
            i++;
        }
        if (count == level) {
            int start = i;
            while (i < traversal.length() && traversal.charAt(i) != '-') {
                i++;
            }
            int val = Integer.parseInt(traversal.substring(start, i));
            ptr = i;
            TreeNode root = new TreeNode(val);
            root.left = find(traversal, level + 1);
            root.right = find(traversal, level + 1);
            return root;
        } else {
            return null;
        }
    }
}
```