## 662\. Maximum Width of Binary Tree

Medium

Given the `root` of a binary tree, return _the **maximum width** of the given tree_.

The **maximum width** of a tree is the maximum **width** among all levels.

The **width** of one level is defined as the length between the end-nodes (the leftmost and rightmost non-null nodes), where the null nodes between the end-nodes are also counted into the length calculation.

It is **guaranteed** that the answer will in the range of **32-bit** signed integer.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/05/03/width1-tree.jpg)

**Input:** root = [1,3,2,5,3,null,9]

**Output:** 4

**Explanation:** The maximum width existing in the third level with the length 4 (5,3,null,9).

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/05/03/width2-tree.jpg)

**Input:** root = [1,3,null,5,3]

**Output:** 2

**Explanation:** The maximum width existing in the third level with the length 2 (5,3).

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/05/03/width3-tree.jpg)

**Input:** root = [1,3,2,5]

**Output:** 2

**Explanation:** The maximum width existing in the second level with the length 2 (3,2).

**Constraints:**

*   The number of nodes in the tree is in the range `[1, 3000]`.
*   `-100 <= Node.val <= 100`

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
    static class Pair {
        TreeNode node;
        int idx;

        public Pair(TreeNode node, int idx) {
            this.node = node;
            this.idx = idx;
        }
    }

    public int widthOfBinaryTree(TreeNode root) {
        Queue<Pair> q = new LinkedList<>();
        q.add(new Pair(root, 0));
        int res = 1;
        while (!q.isEmpty()) {
            int qSize = q.size();
            int lastIdx = 0;
            int firstIdx = 0;
            for (int i = 0; i < qSize; i++) {
                Pair temp = q.poll();
                if (i == 0) {
                    firstIdx = temp.idx;
                }
                if (i == qSize - 1) {
                    lastIdx = Objects.requireNonNull(temp).idx;
                }
                if (Objects.requireNonNull(temp).node.left != null) {
                    q.add(new Pair(temp.node.left, 2 * (temp.idx) + 1));
                }
                if (temp.node.right != null) {
                    q.add(new Pair(temp.node.right, 2 * (temp.idx) + 2));
                }
            }
            res = Math.max((lastIdx - firstIdx) + 1, res);
        }
        return res;
    }
}
```