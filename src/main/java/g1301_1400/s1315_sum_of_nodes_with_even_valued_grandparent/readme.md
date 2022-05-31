## 1315\. Sum of Nodes with Even-Valued Grandparent

Medium

Given the `root` of a binary tree, return _the sum of values of nodes with an **even-valued grandparent**_. If there are no nodes with an **even-valued grandparent**, return `0`.

A **grandparent** of a node is the parent of its parent if it exists.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/08/10/even1-tree.jpg)

**Input:** root = [6,7,8,2,7,1,3,9,null,1,4,null,null,null,5]

**Output:** 18

**Explanation:** The red nodes are the nodes with even-value grandparent while the blue nodes are the even-value grandparents.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/08/10/even2-tree.jpg)

**Input:** root = [1]

**Output:** 0

**Constraints:**

*   The number of nodes in the tree is in the range <code>[1, 10<sup>4</sup>]</code>.
*   `1 <= Node.val <= 100`

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
    public int sumEvenGrandparent(TreeNode root) {
        if (root == null) {
            return 0;
        }
        return dfs(root, root.left, 0) + dfs(root, root.right, 0);
    }

    private int dfs(TreeNode grandparent, TreeNode parent, int sum) {
        if (grandparent == null || parent == null) {
            return sum;
        }
        if (grandparent.val % 2 == 0 && parent.left != null) {
            sum += parent.left.val;
        }
        if (grandparent.val % 2 == 0 && parent.right != null) {
            sum += parent.right.val;
        }
        sum = dfs(parent, parent.left, sum);
        sum = dfs(parent, parent.right, sum);
        return sum;
    }
}
```