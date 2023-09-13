[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 102\. Binary Tree Level Order Traversal

Medium

Given the `root` of a binary tree, return _the level order traversal of its nodes' values_. (i.e., from left to right, level by level).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg)

**Input:** root = [3,9,20,null,null,15,7]

**Output:** [[3],[9,20],[15,7]] 

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
import java.util.LinkedList;
import java.util.List;
import java.util.Queue;

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
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        queue.add(null);
        List<Integer> level = new ArrayList<>();
        while (!queue.isEmpty()) {
            root = queue.remove();
            while (!queue.isEmpty() && root != null) {
                level.add(root.val);
                if (root.left != null) {
                    queue.add(root.left);
                }
                if (root.right != null) {
                    queue.add(root.right);
                }
                root = queue.remove();
            }
            result.add(level);
            level = new ArrayList<>();
            if (!queue.isEmpty()) {
                queue.add(null);
            }
        }
        return result;
    }
}
```

**Time Complexity (Big O Time):**

The program uses a level order traversal approach, which visits each node once, and each edge is traversed exactly twice (once from parent to child and once from child to parent). Here's the analysis of time complexity:

- In the worst case, when the binary tree is completely unbalanced (e.g., a skewed tree), the program needs to visit all N nodes in the tree.

- During the traversal, the program processes each node once and performs constant-time operations (addition and removal from the queue) for each node.

- Therefore, the time complexity of this program is O(N), where N is the number of nodes in the binary tree.

**Space Complexity (Big O Space):**

The space complexity of the program is determined by the space required for data structures, specifically the queue used for the level order traversal, and the space needed for the output list of lists. Here's the analysis of space complexity:

- The program uses a queue to perform level order traversal. In the worst case, when the binary tree is completely unbalanced (e.g., a skewed tree), the queue can contain all N nodes in the tree. Therefore, the space complexity due to the queue is O(N).

- The program constructs a list of lists (`result`) to store the level order traversal result. In the worst case, the result list can contain N/2 lists (for a completely unbalanced tree), each with an average of 2 elements (at the lowest level of the tree).

- Therefore, the space complexity due to the `result` list is also O(N).

- Additional space is used for temporary variables such as `level` and loop control variables, but this space is constant and does not depend on the input size.

- Overall, the space complexity of the program is O(N) due to the space required for the queue and the level order traversal result.

In summary, the time complexity of the program is O(N) because it visits each node once, and the space complexity is also O(N) due to the space required for the queue and the level order traversal result. The space complexity is primarily determined by the size of the queue when traversing a completely unbalanced tree.
