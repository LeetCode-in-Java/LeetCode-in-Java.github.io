[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 104\. Maximum Depth of Binary Tree

Easy

Given the `root` of a binary tree, return _its maximum depth_.

A binary tree's **maximum depth** is the number of nodes along the longest path from the root node down to the farthest leaf node.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/26/tmp-tree.jpg)

**Input:** root = [3,9,20,null,null,15,7]

**Output:** 3 

**Example 2:**

**Input:** root = [1,null,2]

**Output:** 2 

**Example 3:**

**Input:** root = []

**Output:** 0 

**Example 4:**

**Input:** root = [0]

**Output:** 1 

**Constraints:**

*   The number of nodes in the tree is in the range <code>[0, 10<sup>4</sup>]</code>.
*   `-100 <= Node.val <= 100`

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
    public int maxDepth(TreeNode root) {
        return findDepth(root, 0);
    }

    private int findDepth(TreeNode node, int currentDepth) {
        if (node == null) {
            return 0;
        }
        currentDepth++;
        return 1
                + Math.max(findDepth(node.left, currentDepth), findDepth(node.right, currentDepth));
    }
}
```

**Time Complexity (Big O Time):**

The program uses a recursive approach to traverse the binary tree and find its maximum depth. The time complexity can be analyzed as follows:

- In the worst case, when the binary tree is completely unbalanced (e.g., a skewed tree), the program needs to visit all N nodes in the tree.

- The program processes each node exactly once and performs constant-time operations (addition and comparison) for each node.

- Therefore, the time complexity of this program is O(N), where N is the number of nodes in the binary tree.

**Space Complexity (Big O Space):**

The space complexity of the program is determined by the space required for the recursive function call stack. Here's the analysis of space complexity:

- The program uses a recursive function `findDepth` to traverse the binary tree. In the worst case, when the binary tree is completely unbalanced (e.g., a skewed tree), the maximum depth of the call stack can be equal to the height of the tree.

- For a balanced binary tree, the maximum depth of the call stack is proportional to the logarithm of the number of nodes (log₂(N)), which is the height of a balanced binary tree.

- Therefore, the space complexity due to the call stack is O(H), where H is the height of the binary tree.

- Additional space is used for the function's local variables, but this space is constant and does not depend on the input size.

- Overall, the space complexity of the program is O(H), where H is the height of the binary tree, and it represents the maximum depth of the call stack during the recursive traversal.

In summary, the time complexity of the program is O(N) because it visits each node once, and the space complexity is O(H), where H is the height of the binary tree. The space complexity is primarily determined by the maximum depth of the call stack, which can vary depending on the shape of the tree.
