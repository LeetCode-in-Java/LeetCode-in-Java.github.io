[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 404\. Sum of Left Leaves

Easy

Given the `root` of a binary tree, return the sum of all left leaves.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/04/08/leftsum-tree.jpg)

**Input:** root = [3,9,20,null,null,15,7]

**Output:** 24

**Explanation:** There are two left leaves in the binary tree, with values 9 and 15 respectively. 

**Example 2:**

**Input:** root = [1]

**Output:** 0 

**Constraints:**

*   The number of nodes in the tree is in the range `[1, 1000]`.
*   `-1000 <= Node.val <= 1000`

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
    public int sumOfLeftLeaves(TreeNode root) {
        List<Integer> arr = new ArrayList<>();
        traverse(root, arr);
        return getSum(arr);
    }

    private void traverse(TreeNode root, List<Integer> arr) {
        if (root == null) {
            return;
        }
        if (root.left != null && root.left.left == null && root.left.right == null) {
            arr.add(root.left.val);
        }
        traverse(root.left, arr);
        traverse(root.right, arr);
    }

    private int getSum(List<Integer> arr) {
        int sum = 0;
        for (Integer integer : arr) {
            sum += integer;
        }
        return sum;
    }
}
```