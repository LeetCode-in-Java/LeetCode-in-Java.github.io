## 501\. Find Mode in Binary Search Tree

Easy

Given the `root` of a binary search tree (BST) with duplicates, return _all the [mode(s)](https://en.wikipedia.org/wiki/Mode_(statistics)) (i.e., the most frequently occurred element) in it_.

If the tree has more than one mode, return them in **any order**.

Assume a BST is defined as follows:

*   The left subtree of a node contains only nodes with keys **less than or equal to** the node's key.
*   The right subtree of a node contains only nodes with keys **greater than or equal to** the node's key.
*   Both the left and right subtrees must also be binary search trees.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/11/mode-tree.jpg)

**Input:** root = [1,null,2,2]

**Output:** [2]

**Example 2:**

**Input:** root = [0]

**Output:** [0]

**Constraints:**

*   The number of nodes in the tree is in the range <code>[1, 10<sup>4</sup>]</code>.
*   <code>-10<sup>5</sup> <= Node.val <= 10<sup>5</sup></code>

**Follow up:** Could you do that without using any extra space? (Assume that the implicit stack space incurred due to recursion does not count).

## Solution

```java
import com_github_leetcode.TreeNode;
import java.util.ArrayList;
import java.util.List;

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
    private Integer prev = null;
    private int max = 0;
    private int cnt = 1;

    public int[] findMode(TreeNode root) {
        ArrayList<Integer> modes = new ArrayList<>();
        traverse(root, modes);
        int[] res = new int[modes.size()];
        for (int i = 0; i < modes.size(); i++) {
            res[i] = modes.get(i);
        }
        return res;
    }

    private void traverse(TreeNode root, List<Integer> modes) {
        if (root == null) {
            return;
        }
        traverse(root.left, modes);
        if (prev != null) {
            if (prev == root.val) {
                cnt++;
            } else {
                cnt = 1;
            }
        }
        if (cnt > max) {
            max = cnt;
            modes.clear();
            modes.add(root.val);
        } else if (cnt == max) {
            modes.add(root.val);
        }
        prev = root.val;
        traverse(root.right, modes);
    }
}
```