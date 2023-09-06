[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2641\. Cousins in Binary Tree II

Medium

Given the `root` of a binary tree, replace the value of each node in the tree with the **sum of all its cousins' values**.

Two nodes of a binary tree are **cousins** if they have the same depth with different parents.

Return _the_ `root` _of the modified tree_.

**Note** that the depth of a node is the number of edges in the path from the root node to it.

**Example 1:**

![](https://assets.leetcode.com/uploads/2023/01/11/example11.png)

**Input:** root = [5,4,9,1,10,null,7]

**Output:** [0,0,0,7,7,null,11]

**Explanation:** The diagram above shows the initial binary tree and the binary tree after changing the value of each node. 
- Node with value 5 does not have any cousins so its sum is 0. 
- Node with value 4 does not have any cousins so its sum is 0. 
- Node with value 9 does not have any cousins so its sum is 0. 
- Node with value 1 has a cousin with value 7 so its sum is 7. 
- Node with value 10 has a cousin with value 7 so its sum is 7. 
- Node with value 7 has cousins with values 1 and 10 so its sum is 11.

**Example 2:**

![](https://assets.leetcode.com/uploads/2023/01/11/diagram33.png)

**Input:** root = [3,1,2]

**Output:** [0,0,0]

**Explanation:** The diagram above shows the initial binary tree and the binary tree after changing the value of each node.
- Node with value 3 does not have any cousins so its sum is 0. 
- Node with value 1 does not have any cousins so its sum is 0. 
- Node with value 2 does not have any cousins so its sum is 0.

**Constraints:**

*   The number of nodes in the tree is in the range <code>[1, 10<sup>5</sup>]</code>.
*   <code>1 <= Node.val <= 10<sup>4</sup></code>

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
    private List<Integer> horizontalSum;

    private void traverse(TreeNode root, int depth) {
        if (root == null) {
            return;
        }
        if (depth < horizontalSum.size()) {
            horizontalSum.set(depth, horizontalSum.get(depth) + root.val);
        } else {
            horizontalSum.add(root.val);
        }
        traverse(root.left, depth + 1);
        traverse(root.right, depth + 1);
    }

    private void traverse1(TreeNode root, int depth) {
        if (root == null) {
            return;
        }
        if (depth > 0) {
            int sum = 0;
            if (root.left != null) {
                sum += root.left.val;
            }
            if (root.right != null) {
                sum += root.right.val;
            }
            if (root.left != null) {
                root.left.val = horizontalSum.get(depth + 1) - sum;
            }
            if (root.right != null) {
                root.right.val = horizontalSum.get(depth + 1) - sum;
            }
        }
        traverse1(root.left, depth + 1);
        traverse1(root.right, depth + 1);
    }

    public TreeNode replaceValueInTree(TreeNode root) {
        horizontalSum = new ArrayList<>();
        root.val = 0;
        if (root.left != null) {
            root.left.val = 0;
        }
        if (root.right != null) {
            root.right.val = 0;
        }
        traverse(root, 0);
        traverse1(root, 0);
        return root;
    }
}
```