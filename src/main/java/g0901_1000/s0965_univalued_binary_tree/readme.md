## 965\. Univalued Binary Tree

Easy

A binary tree is **uni-valued** if every node in the tree has the same value.

Given the `root` of a binary tree, return `true` _if the given tree is **uni-valued**, or_ `false` _otherwise._

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/12/28/unival_bst_1.png)

**Input:** root = [1,1,1,1,1,null,1]

**Output:** true

**Example 2:**

![](https://assets.leetcode.com/uploads/2018/12/28/unival_bst_2.png)

**Input:** root = [2,2,2,5,2]

**Output:** false

**Constraints:**

*   The number of nodes in the tree is in the range `[1, 100]`.
*   `0 <= Node.val < 100`

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
    public boolean isUnivalTree(TreeNode root) {
        int val = root.val;
        LinkedList<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        while (!queue.isEmpty()) {
            TreeNode node = queue.poll();
            if (node.val != val) {
                return false;
            }
            if (node.left != null) {
                queue.add(node.left);
            }
            if (node.right != null) {
                queue.add(node.right);
            }
        }
        return true;
    }
}
```