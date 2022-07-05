[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 971\. Flip Binary Tree To Match Preorder Traversal

Medium

You are given the `root` of a binary tree with `n` nodes, where each node is uniquely assigned a value from `1` to `n`. You are also given a sequence of `n` values `voyage`, which is the **desired** [**pre-order traversal**](https://en.wikipedia.org/wiki/Tree_traversal#Pre-order) of the binary tree.

Any node in the binary tree can be **flipped** by swapping its left and right subtrees. For example, flipping node 1 will have the following effect:

![](https://assets.leetcode.com/uploads/2021/02/15/fliptree.jpg)

Flip the **smallest** number of nodes so that the **pre-order traversal** of the tree **matches** `voyage`.

Return _a list of the values of all **flipped** nodes. You may return the answer in **any order**. If it is **impossible** to flip the nodes in the tree to make the pre-order traversal match_ `voyage`_, return the list_ `[-1]`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/01/02/1219-01.png)

**Input:** root = [1,2], voyage = [2,1]

**Output:** [-1]

**Explanation:** It is impossible to flip the nodes such that the pre-order traversal matches voyage.

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/01/02/1219-02.png)

**Input:** root = [1,2,3], voyage = [1,3,2]

**Output:** [1]

**Explanation:** Flipping node 1 swaps nodes 2 and 3, so the pre-order traversal matches voyage.

**Example 3:**

![](https://assets.leetcode.com/uploads/2019/01/02/1219-02.png)

**Input:** root = [1,2,3], voyage = [1,2,3]

**Output:** []

**Explanation:** The tree's pre-order traversal already matches voyage, so no nodes need to be flipped.

**Constraints:**

*   The number of nodes in the tree is `n`.
*   `n == voyage.length`
*   `1 <= n <= 100`
*   `1 <= Node.val, voyage[i] <= n`
*   All the values in the tree are **unique**.
*   All the values in `voyage` are **unique**.

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
    private List<Integer> list = new ArrayList<>();
    private int preIndex = 0;
    private boolean isFlipPossible = true;

    public List<Integer> flipMatchVoyage(TreeNode root, int[] voyage) {
        list.clear();
        preIndex = 0;
        isFlipPossible = true;
        traverse(root, voyage);
        if (!isFlipPossible) {
            list.clear();
            list.add(-1);
        }
        return list;
    }

    private void traverse(TreeNode root, int[] voyage) {
        if (root == null) {
            return;
        }
        if (root.val != voyage[preIndex]) {
            isFlipPossible = false;
        } else {
            if (preIndex + 1 < voyage.length
                    && root.left != null
                    && root.left.val != voyage[preIndex + 1]) {
                // swap
                list.add(root.val);
                TreeNode temp = root.right;
                root.right = root.left;
                root.left = temp;
            }
            preIndex++;
            traverse(root.left, voyage);
            traverse(root.right, voyage);
        }
    }
}
```