[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1305\. All Elements in Two Binary Search Trees

Medium

Given two binary search trees `root1` and `root2`, return _a list containing all the integers from both trees sorted in **ascending** order_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/12/18/q2-e1.png)

**Input:** root1 = [2,1,4], root2 = [1,0,3]

**Output:** [0,1,1,2,3,4]

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/12/18/q2-e5-.png)

**Input:** root1 = [1,null,8], root2 = [8,1]

**Output:** [1,1,8,8]

**Constraints:**

*   The number of nodes in each tree is in the range `[0, 5000]`.
*   <code>-10<sup>5</sup> <= Node.val <= 10<sup>5</sup></code>

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
    public List<Integer> getAllElements(TreeNode root1, TreeNode root2) {
        List<Integer> list1 = getAllNodes(root1);
        List<Integer> list2 = getAllNodes(root2);
        List<Integer> merged = new ArrayList<>();
        merged.addAll(list1);
        merged.addAll(list2);
        Collections.sort(merged);
        return merged;
    }

    private List<Integer> getAllNodes(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        return inorder(root, list);
    }

    private List<Integer> inorder(TreeNode root, List<Integer> result) {
        if (root == null) {
            return result;
        }
        inorder(root.left, result);
        result.add(root.val);
        return inorder(root.right, result);
    }
}
```