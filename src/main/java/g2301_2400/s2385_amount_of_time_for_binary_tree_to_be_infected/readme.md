[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2385\. Amount of Time for Binary Tree to Be Infected

Medium

You are given the `root` of a binary tree with **unique** values, and an integer `start`. At minute `0`, an **infection** starts from the node with value `start`.

Each minute, a node becomes infected if:

*   The node is currently uninfected.
*   The node is adjacent to an infected node.

Return _the number of minutes needed for the entire tree to be infected._

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/06/25/image-20220625231744-1.png)

**Input:** root = [1,5,3,null,4,10,6,9,2], start = 3

**Output:** 4

**Explanation:** The following nodes are infected during:

- Minute 0: Node 3

- Minute 1: Nodes 1, 10 and 6

- Minute 2: Node 5

- Minute 3: Node 4

- Minute 4: Nodes 9 and 2

It takes 4 minutes for the whole tree to be infected so we return 4. 

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/06/25/image-20220625231812-2.png)

**Input:** root = [1], start = 1

**Output:** 0

**Explanation:** At minute 0, the only node in the tree is infected so we return 0. 

**Constraints:**

*   The number of nodes in the tree is in the range <code>[1, 10<sup>5</sup>]</code>.
*   <code>1 <= Node.val <= 10<sup>5</sup></code>
*   Each node has a **unique** value.
*   A node with a value of `start` exists in the tree.

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

    public int amountOfTime(TreeNode root, int start) {
        dfs(root, start, new Distance(-1));
        return max;
    }

    private int dfs(TreeNode root, int start, Distance l) {
        if (root == null) {
            return 0;
        }
        Distance ld = new Distance(-1);
        Distance rd = new Distance(-1);
        int left = dfs(root.left, start, ld);
        int right = dfs(root.right, start, rd);
        if (l.val == -1 && start == root.val) {
            max = Math.max(left, right);
            l.val = 1;
        }
        if (ld.val != -1) {
            max = Math.max(max, ld.val + right);
            l.val = ld.val + 1;
        } else if (rd.val != -1) {
            max = Math.max(max, rd.val + left);
            l.val = rd.val + 1;
        }
        return Math.max(left, right) + 1;
    }

    private static class Distance {
        int val;

        Distance(int v) {
            this.val = v;
        }
    }
}
```