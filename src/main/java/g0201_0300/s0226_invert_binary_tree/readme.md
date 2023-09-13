[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 226\. Invert Binary Tree

Easy

Given the `root` of a binary tree, invert the tree, and return _its root_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/14/invert1-tree.jpg)

**Input:** root = [4,2,7,1,3,6,9]

**Output:** [4,7,2,9,6,3,1] 

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/14/invert2-tree.jpg)

**Input:** root = [2,1,3]

**Output:** [2,3,1] 

**Example 3:**

**Input:** root = []

**Output:** [] 

**Constraints:**

*   The number of nodes in the tree is in the range `[0, 100]`.
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
    public TreeNode invertTree(TreeNode root) {
        if (root == null) {
            return null;
        }
        TreeNode temp = root.left;
        root.left = invertTree(root.right);
        root.right = invertTree(temp);
        return root;
    }
}
```

**Time Complexity (Big O Time):**

The time complexity of this program can be analyzed based on the number of nodes in the binary tree.

1. **Traversal and Swapping:**
   - The program recursively traverses each node of the binary tree exactly once.
   - At each node, it swaps the left and right subtrees.
   - Therefore, the time complexity is O(n), where "n" is the number of nodes in the binary tree.

**Space Complexity (Big O Space):**

The space complexity of this program is determined by the recursion stack and the memory required for the TreeNode objects.

1. **Recursion Stack:**
   - The program uses recursion to traverse the binary tree.
   - The maximum depth of the recursion stack is equal to the height of the binary tree.
   - In the worst case, if the binary tree is completely unbalanced (skewed), the height could be "n" (the number of nodes), resulting in O(n) space complexity.
   - In the best case, if the binary tree is balanced, the height would be approximately log₂(n), resulting in O(log n) space complexity.

2. **TreeNode Objects:**
   - The program doesn't use additional data structures or collections that grow with the input size.
   - The TreeNode objects exist in memory for each node in the binary tree.
   - The space required for TreeNode objects is O(n) in the worst case, where "n" is the number of nodes in the binary tree.

In summary, the space complexity is O(n) due to the recursion stack and the TreeNode objects, and the time complexity is O(n) as it visits each node once during the traversal and swapping process.
