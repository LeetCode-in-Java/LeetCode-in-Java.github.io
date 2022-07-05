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
    public List<TreeNode> delNodes(TreeNode root, int[] toDelete) {
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        for (int d : toDelete) {
            delete(d, queue);
        }
        List<TreeNode> result = new ArrayList<>();
        while (!queue.isEmpty()) {
            result.add(queue.poll());
        }
        return result;
    }

    private void delete(int toDelete, Queue<TreeNode> queue) {
        int size = queue.size();
        for (int i = 0; i < size; i++) {
            TreeNode curr = queue.poll();
            if (delete(curr, toDelete, queue)) {
                if (toDelete != curr.val) {
                    queue.offer(curr);
                }
                break;
            } else {
                queue.offer(curr);
            }
        }
    }

    private boolean delete(TreeNode curr, int toDelete, Queue<TreeNode> queue) {
        if (curr == null) {
            return false;
        } else {
            if (curr.val == toDelete) {
                if (curr.left != null) {
                    queue.offer(curr.left);
                }
                if (curr.right != null) {
                    queue.offer(curr.right);
                }
                return true;
            } else if (curr.left != null && curr.left.val == toDelete) {
                if (curr.left.left != null) {
                    queue.offer(curr.left.left);
                }
                if (curr.left.right != null) {
                    queue.offer(curr.left.right);
                }
                curr.left = null;
                return true;
            } else if (curr.right != null && curr.right.val == toDelete) {
                if (curr.right.left != null) {
                    queue.offer(curr.right.left);
                }
                if (curr.right.right != null) {
                    queue.offer(curr.right.right);
                }
                curr.right = null;
                return true;
            }
            return delete(curr.left, toDelete, queue) || delete(curr.right, toDelete, queue);
        }
    }
}
```