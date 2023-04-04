[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1302\. Deepest Leaves Sum

Medium

Given the `root` of a binary tree, return _the sum of values of its deepest leaves_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/07/31/1483_ex1.png)

**Input:** root = [1,2,3,4,5,null,6,7,null,null,null,null,8]

**Output:** 15

**Example 2:**

**Input:** root = [6,7,8,2,7,1,3,9,null,1,4,null,null,null,5]

**Output:** 19

**Constraints:**

*   The number of nodes in the tree is in the range <code>[1, 10<sup>4</sup>]</code>.
*   `1 <= Node.val <= 100`

## Solution

```java
import com_github_leetcode.TreeNode;
import java.util.LinkedList;
import java.util.Objects;
import java.util.Queue;

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
    public int deepestLeavesSum(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        int sum = 0;
        while (!queue.isEmpty()) {
            int size = queue.size();
            sum = 0;
            for (int i = 0; i < size; i++) {
                TreeNode curr = queue.poll();
                sum += Objects.requireNonNull(curr).val;
                if (curr.left != null) {
                    queue.offer(curr.left);
                }
                if (curr.right != null) {
                    queue.offer(curr.right);
                }
            }
        }
        return sum;
    }
}
```