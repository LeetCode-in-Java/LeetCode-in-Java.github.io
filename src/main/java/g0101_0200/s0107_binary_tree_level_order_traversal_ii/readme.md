## 107\. Binary Tree Level Order Traversal II

Medium

Given the `root` of a binary tree, return _the bottom-up level order traversal of its nodes' values_. (i.e., from left to right, level by level from leaf to root).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg)

**Input:** root = [3,9,20,null,null,15,7]

**Output:** [[15,7],[9,20],[3]] 

**Example 2:**

**Input:** root = [1]

**Output:** [[1]] 

**Example 3:**

**Input:** root = []

**Output:** [] 

**Constraints:**

*   The number of nodes in the tree is in the range `[0, 2000]`.
*   `-1000 <= Node.val <= 1000`

## Solution

```java
import com_github_leetcode.TreeNode;
import java.util.ArrayList;
import java.util.Collections;
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
    private List<List<Integer>> order = new ArrayList<>();

    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        getOrder(root, 0);
        Collections.reverse(order);
        return order;
    }

    public void getOrder(TreeNode root, int level) {
        if (root != null) {
            if (level + 1 > order.size()) {
                order.add(new ArrayList<>());
            }
            order.get(level).add(root.val);
            getOrder(root.left, level + 1);
            getOrder(root.right, level + 1);
        }
    }
}
```