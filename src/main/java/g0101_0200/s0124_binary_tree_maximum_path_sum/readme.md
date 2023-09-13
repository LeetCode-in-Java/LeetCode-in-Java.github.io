[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 124\. Binary Tree Maximum Path Sum

Hard

A **path** in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence **at most once**. Note that the path does not need to pass through the root.

The **path sum** of a path is the sum of the node's values in the path.

Given the `root` of a binary tree, return _the maximum **path sum** of any **non-empty** path_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/13/exx1.jpg)

**Input:** root = [1,2,3]

**Output:** 6

**Explanation:** The optimal path is 2 -> 1 -> 3 with a path sum of 2 + 1 + 3 = 6. 

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/10/13/exx2.jpg)

**Input:** root = [-10,9,20,null,null,15,7]

**Output:** 42

**Explanation:** The optimal path is 15 -> 20 -> 7 with a path sum of 15 + 20 + 7 = 42. 

**Constraints:**

*   The number of nodes in the tree is in the range <code>[1, 3 * 10<sup>4</sup>]</code>.
*   `-1000 <= Node.val <= 1000`

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
    private int max = Integer.MIN_VALUE;

    private int helper(TreeNode root) {
        if (root == null) {
            return 0;
        }
        // to avoid the -ve values in left side we will compare them with 0
        int left = Math.max(0, helper(root.left));
        int right = Math.max(0, helper(root.right));
        int current = root.val + left + right;
        if (current > max) {
            max = current;
        }
        return root.val + Math.max(left, right);
    }

    public int maxPathSum(TreeNode root) {
        helper(root);
        return max;
    }
}
```

**Time Complexity (Big O Time):**

The time complexity of the program is determined by the recursive traversal of the binary tree. Here's the breakdown:

- The program uses a recursive function `helper` to traverse the binary tree.

- In the worst case, the program visits each node of the binary tree exactly once. Therefore, the time complexity of this program is O(N), where N is the number of nodes in the binary tree.

- Within each function call, there are constant-time operations such as comparisons and arithmetic operations.

- Therefore, the dominant factor in the time complexity is the traversal of the tree, which is O(N).

**Space Complexity (Big O Space):**

The space complexity of the program is primarily determined by the call stack during the recursive traversal. Here's the analysis:

- In the worst case, the maximum depth of the call stack is equal to the height of the binary tree.

- In the case of a balanced binary tree, the height is approximately log₂(N), where N is the number of nodes. Therefore, the space complexity of the program can be approximated as O(log N) for balanced trees.

- In the worst case scenario of an unbalanced tree, the height can be as large as N, leading to a space complexity of O(N) for pathological cases.

- Additionally, the program uses a few integer variables (`max`, `left`, `right`, and `current`) that occupy constant space, regardless of the tree's size.

- Therefore, the overall space complexity of the program is O(N) in the worst case due to the call stack's space usage during recursion, and it can be considered O(log N) for balanced trees.

In summary, the time complexity of the program is O(N), where N is the number of nodes in the binary tree, and the space complexity is O(N) in the worst case due to the call stack space during recursion, but it can be more space-efficient (O(log N)) for balanced trees. The program efficiently finds the maximum path sum in the binary tree.
