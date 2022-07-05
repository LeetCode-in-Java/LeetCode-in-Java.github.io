[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 145\. Binary Tree Postorder Traversal

Easy

Given the `root` of a binary tree, return _the postorder traversal of its nodes' values_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/08/28/pre1.jpg)

**Input:** root = [1,null,2,3]

**Output:** [3,2,1] 

**Example 2:**

**Input:** root = []

**Output:** [] 

**Example 3:**

**Input:** root = [1]

**Output:** [1] 

**Example 4:**

![](https://assets.leetcode.com/uploads/2020/08/28/pre3.jpg)

**Input:** root = [1,2]

**Output:** [2,1] 

**Example 5:**

![](https://assets.leetcode.com/uploads/2020/08/28/pre2.jpg)

**Input:** root = [1,null,2]

**Output:** [2,1] 

**Constraints:**

*   The number of the nodes in the tree is in the range `[0, 100]`.
*   `-100 <= Node.val <= 100`

**Follow up:** Recursive solution is trivial, could you do it iteratively?

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
    public List<Integer> postorderTraversal(TreeNode root) {
        if (root == null) {
            return new ArrayList<>();
        }
        List<Integer> res = postorderTraversal(root.left);
        res.addAll(postorderTraversal(root.right));
        res.add(root.val);
        return res;
    }
}
```