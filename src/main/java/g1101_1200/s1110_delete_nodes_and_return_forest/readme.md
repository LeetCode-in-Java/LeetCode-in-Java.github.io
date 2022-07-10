[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1110\. Delete Nodes And Return Forest

Medium

Given the `root` of a binary tree, each node in the tree has a distinct value.

After deleting all nodes with a value in `to_delete`, we are left with a forest (a disjoint union of trees).

Return the roots of the trees in the remaining forest. You may return the result in any order.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/07/01/screen-shot-2019-07-01-at-53836-pm.png)

**Input:** root = [1,2,3,4,5,6,7], to\_delete = [3,5]

**Output:** [[1,2,null,4],[6],[7]]

**Example 2:**

**Input:** root = [1,2,4,null,3], to\_delete = [3]

**Output:** [[1,2,4]]

**Constraints:**

*   The number of nodes in the given tree is at most `1000`.
*   Each node has a distinct value between `1` and `1000`.
*   `to_delete.length <= 1000`
*   `to_delete` contains distinct values between `1` and `1000`.

## Solution

```java
import com_github_leetcode.TreeNode;
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;
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
    private Set<Integer> toDelete;
    private Queue<TreeNode> nodes = new LinkedList<>();

    public TreeNode deleteAndSplit(TreeNode root) {
        if (root == null) {
            return null;
        }
        if (toDelete.contains(root.val)) {
            if (root.left != null) {
                nodes.add(root.left);
            }
            if (root.right != null) {
                nodes.add(root.right);
            }
            return null;
        }
        root.left = deleteAndSplit(root.left);
        root.right = deleteAndSplit(root.right);
        return root;
    }

    private List<TreeNode> delNodes(TreeNode root, int[] localToDelete) {
        toDelete = new HashSet<>();
        for (int node : localToDelete) {
            toDelete.add(node);
        }
        nodes.add(root);
        List<TreeNode> forests = new ArrayList<>();
        while (nodes.size() > 0) {
            TreeNode node = nodes.poll();
            node = deleteAndSplit(node);
            if (node != null) {
                forests.add(node);
            }
        }
        return forests;
    }
}
```