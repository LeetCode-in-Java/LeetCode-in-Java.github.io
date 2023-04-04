[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 337\. House Robber III

Medium

The thief has found himself a new place for his thievery again. There is only one entrance to this area, called `root`.

Besides the `root`, each house has one and only one parent house. After a tour, the smart thief realized that all houses in this place form a binary tree. It will automatically contact the police if **two directly-linked houses were broken into on the same night**.

Given the `root` of the binary tree, return _the maximum amount of money the thief can rob **without alerting the police**_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/10/rob1-tree.jpg)

**Input:** root = [3,2,3,null,3,null,1]

**Output:** 7

**Explanation:** Maximum amount of money the thief can rob = 3 + 3 + 1 = 7. 

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/10/rob2-tree.jpg)

**Input:** root = [3,4,5,1,3,null,1]

**Output:** 9

**Explanation:** Maximum amount of money the thief can rob = 4 + 5 = 9. 

**Constraints:**

*   The number of nodes in the tree is in the range <code>[1, 10<sup>4</sup>]</code>.
*   <code>0 <= Node.val <= 10<sup>4</sup></code>

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
    public int rob(TreeNode root) {
        int[] out = robRec(root);
        return Math.max(out[0], out[1]);
    }

    private int[] robRec(TreeNode curr) {
        if (curr == null) {
            return new int[] {0, 0};
        }
        int[] left = robRec(curr.left);
        int[] right = robRec(curr.right);
        int[] out = new int[2];
        // 1. If choosing to take the house
        out[0] = curr.val + left[1] + right[1];
        // 2. If choosing to skip the house
        out[1] = left[0] + right[0];
        // 3. Best Solution at house
        out[0] = Math.max(out[0], out[1]);
        return out;
    }
}
```