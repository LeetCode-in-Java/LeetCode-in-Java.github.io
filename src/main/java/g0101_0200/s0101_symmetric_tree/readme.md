[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 101\. Symmetric Tree

Easy

Given the `root` of a binary tree, _check whether it is a mirror of itself_ (i.e., symmetric around its center).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/symtree1.jpg)

**Input:** root = [1,2,2,3,4,4,3]

**Output:** true 

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/02/19/symtree2.jpg)

**Input:** root = [1,2,2,null,3,null,3]

**Output:** false 

**Constraints:**

*   The number of nodes in the tree is in the range `[1, 1000]`.
*   `-100 <= Node.val <= 100`

**Follow up:** Could you solve it both recursively and iteratively?

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
    public boolean isSymmetric(TreeNode root) {
        if (root == null) {
            return true;
        }
        return helper(root.left, root.right);
    }

    private boolean helper(TreeNode leftNode, TreeNode rightNode) {
        if (leftNode == null || rightNode == null) {
            return leftNode == null && rightNode == null;
        }
        if (leftNode.val != rightNode.val) {
            return false;
        }
        return helper(leftNode.left, rightNode.right) && helper(leftNode.right, rightNode.left);
    }
}
```

**Time Complexity (Big O Time):**

The program uses a recursive approach to check if the binary tree is symmetric. It recursively compares the left subtree of the root with the right subtree of the root. Here's the analysis of time complexity:

- In the worst case, the program visits each node once. This happens when the tree is perfectly symmetric (a mirror image of itself), and the program checks every node.

- Therefore, the time complexity of this program is O(N), where N is the number of nodes in the binary tree.

**Space Complexity (Big O Space):**

The space complexity of the program is determined by the space required for the recursive call stack and the space needed for the function arguments and variables. Here's the analysis of space complexity:

- The main function `isSymmetric` has a constant amount of space overhead. It doesn't use any additional data structures or arrays that depend on the input size.

- The recursive function `helper` is called recursively for each pair of nodes in the tree. In the worst case, when the binary tree is perfectly symmetric, the maximum depth of the call stack is equal to the height of the tree. In a balanced tree, this height is log(N), where N is the number of nodes.

- Therefore, the space complexity due to the call stack is O(log(N)) in the best-case scenario (balanced tree) and O(N) in the worst-case scenario (skewed tree).

- The space used for function arguments and variables (e.g., `leftNode`, `rightNode`) is constant and does not depend on the input size.

- Overall, the space complexity of the program is O(log(N)) in the best-case scenario and O(N) in the worst-case scenario. The space complexity is mainly determined by the depth of the call stack when checking a perfectly symmetric tree.

In summary, the time complexity of the program is O(N), where N is the number of nodes in the binary tree, and the space complexity is O(log(N)) in the best-case scenario (perfectly symmetric tree) and O(N) in the worst-case scenario (skewed tree). The space complexity is primarily determined by the depth of the call stack when checking a symmetric tree.
