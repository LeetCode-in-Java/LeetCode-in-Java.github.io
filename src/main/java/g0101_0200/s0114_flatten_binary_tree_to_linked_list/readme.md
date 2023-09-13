[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 114\. Flatten Binary Tree to Linked List

Medium

Given the `root` of a binary tree, flatten the tree into a "linked list":

*   The "linked list" should use the same `TreeNode` class where the `right` child pointer points to the next node in the list and the `left` child pointer is always `null`.
*   The "linked list" should be in the same order as a [**pre-order** **traversal**](https://en.wikipedia.org/wiki/Tree_traversal#Pre-order,_NLR) of the binary tree.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/14/flaten.jpg)

**Input:** root = [1,2,5,3,4,null,6]

**Output:** [1,null,2,null,3,null,4,null,5,null,6] 

**Example 2:**

**Input:** root = []

**Output:** [] 

**Example 3:**

**Input:** root = [0]

**Output:** [0] 

**Constraints:**

*   The number of nodes in the tree is in the range `[0, 2000]`.
*   `-100 <= Node.val <= 100`

**Follow up:** Can you flatten the tree in-place (with `O(1)` extra space)?

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
    public void flatten(TreeNode root) {
        if (root != null) {
            findTail(root);
        }
    }

    private TreeNode findTail(TreeNode root) {
        TreeNode left = root.left;
        TreeNode right = root.right;
        TreeNode tail;
        // find the tail of left subtree, tail means the most left leaf
        if (left != null) {
            tail = findTail(left);
            // stitch the right subtree below the tail
            root.left = null;
            root.right = left;
            tail.right = right;
        } else {
            tail = root;
        }
        // find tail of the right subtree
        if (tail.right == null) {
            return tail;
        } else {
            return findTail(tail.right);
        }
    }
}
```

**Time Complexity (Big O Time):**

The time complexity of the program can be analyzed as follows:

- The `flatten` function is called once with the root of the binary tree, and it, in turn, calls the `findTail` function recursively.

- The `findTail` function is a recursive function that traverses the left subtree of the current node and connects the right subtree to the left subtree to flatten the tree.

- In the worst case, the `findTail` function is called for each node in the binary tree exactly once. Each recursive call of `findTail` takes constant time O(1) for node manipulations.

- Therefore, the overall time complexity of the program is O(N), where N is the number of nodes in the binary tree.

**Space Complexity (Big O Space):**

The space complexity of the program is determined by the space required for the recursive call stack and the space for temporary variables. Here's the analysis of space complexity:

- The primary factor contributing to space complexity is the recursive call stack. In the worst case, when the binary tree is completely unbalanced (e.g., a skewed tree), the maximum depth of the call stack can be equal to the height of the tree, which is N in the worst case.

- Additionally, the program uses a few temporary variables like `left`, `right`, and `tail`, which occupy constant space.

- Therefore, the space complexity due to the call stack is O(N) in the worst case, and the space complexity due to temporary variables is constant O(1).

- Overall, the space complexity of the program is O(N) due to the call stack, which can grow linearly with the number of nodes in the binary tree.

In summary, the time complexity of the program is O(N), and the space complexity is O(N) in the worst case. The space complexity is primarily determined by the call stack depth during the recursive flattening of the binary tree.
