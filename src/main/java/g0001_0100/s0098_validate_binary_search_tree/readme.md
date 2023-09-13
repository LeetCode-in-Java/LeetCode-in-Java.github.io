[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 98\. Validate Binary Search Tree

Medium

Given the `root` of a binary tree, _determine if it is a valid binary search tree (BST)_.

A **valid BST** is defined as follows:

*   The left subtree of a node contains only nodes with keys **less than** the node's key.
*   The right subtree of a node contains only nodes with keys **greater than** the node's key.
*   Both the left and right subtrees must also be binary search trees.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/12/01/tree1.jpg)

**Input:** root = [2,1,3]

**Output:** true 

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/12/01/tree2.jpg)

**Input:** root = [5,1,4,null,null,3,6]

**Output:** false

**Explanation:** The root node's value is 5 but its right child's value is 4. 

**Constraints:**

*   The number of nodes in the tree is in the range <code>[1, 10<sup>4</sup>]</code>.
*   <code>-2<sup>31</sup> <= Node.val <= 2<sup>31</sup> - 1</code>

## Solution

```java
import com_github_leetcode.TreeNode;

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
    public boolean isValidBST(TreeNode root) {
        return solve(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }

    // we will send a valid range and check whether the root lies in the range
    // and update the range for the subtrees
    private boolean solve(TreeNode root, long left, long right) {
        if (root == null) {
            return true;
        }
        if (root.val <= left || root.val >= right) {
            return false;
        }
        return solve(root.left, left, root.val) && solve(root.right, root.val, right);
    }
}
```

**Time Complexity (Big O Time):**

The program uses a recursive approach to traverse the binary tree. In each recursive call, it checks whether the current node's value falls within a specified range (represented by `left` and `right`). Here's the analysis of time complexity:

- In the worst case, the program visits each node once. This happens when the tree is a valid BST, and the program checks every node.

- Therefore, the time complexity of this program is O(N), where N is the number of nodes in the binary tree.

**Space Complexity (Big O Space):**

The space complexity of the program is determined by the space required for the recursive call stack and the space needed for the function arguments and variables. Here's the analysis of space complexity:

- The main function `isValidBST` has a constant amount of space overhead. It doesn't use any additional data structures or arrays that depend on the input size.

- The recursive function `solve` is called recursively for each node in the binary tree. In the worst case, when the binary tree is a valid BST, the maximum depth of the call stack is equal to the height of the tree. In a balanced BST, this height is log(N), where N is the number of nodes.

- Therefore, the space complexity due to the call stack is O(log(N)) in the best-case scenario (balanced tree) and O(N) in the worst-case scenario (skewed tree).

- The space used for function arguments and variables (e.g., `left`, `right`, and `root`) is constant and does not depend on the input size.

- Overall, the space complexity of the program is O(log(N)) in the best-case scenario and O(N) in the worst-case scenario.

In summary, the time complexity of the program is O(N), where N is the number of nodes in the binary tree, and the space complexity is O(log(N)) in the best-case scenario and O(N) in the worst-case scenario. The space complexity is mainly determined by the depth of the call stack when checking a skewed tree.
