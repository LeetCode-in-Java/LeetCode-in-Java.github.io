[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 236\. Lowest Common Ancestor of a Binary Tree

Medium

Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the [definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor): “The lowest common ancestor is defined between two nodes `p` and `q` as the lowest node in `T` that has both `p` and `q` as descendants (where we allow **a node to be a descendant of itself**).”

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

**Input:** root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1

**Output:** 3

**Explanation:** The LCA of nodes 5 and 1 is 3. 

**Example 2:**

![](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

**Input:** root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4

**Output:** 5

**Explanation:** The LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself according to the LCA definition. 

**Example 3:**

**Input:** root = [1,2], p = 1, q = 2

**Output:** 1 

**Constraints:**

*   The number of nodes in the tree is in the range <code>[2, 10<sup>5</sup>]</code>.
*   <code>-10<sup>9</sup> <= Node.val <= 10<sup>9</sup></code>
*   All `Node.val` are **unique**.
*   `p != q`
*   `p` and `q` will exist in the tree.

## Solution

```java
import com_github_leetcode.TreeNode;

/*
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null) {
            return null;
        }
        if (root.val == p.val || root.val == q.val) {
            return root;
        }
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        if (left != null && right != null) {
            return root;
        }
        if (left != null) {
            return left;
        }
        return right;
    }
}
```

**Time Complexity (Big O Time):**

The time complexity of the program is determined by the number of nodes it visits in the binary tree.

- In the worst case, the program visits all nodes in the binary tree, making a recursive call for each node in a post-order traversal fashion.

- For each node, it performs a constant amount of work (comparing values and checking if `left` and `right` are not `null`).

- Therefore, the time complexity of the program is O(n), where "n" is the number of nodes in the binary tree.

**Space Complexity (Big O Space):**

The space complexity of the program is determined by the space used in the recursive call stack.

- In the worst case, the depth of the recursive call stack is equal to the height of the binary tree.

- If the binary tree is balanced (height "h" is O(log n)), the space complexity is O(log n) because that's the maximum depth of the recursive call stack.

- If the binary tree is skewed (height "h" is O(n)), the space complexity is O(n) because the depth of the recursive call stack can be as large as "n."

In summary, the space complexity can be O(log n) in the best-case scenario (balanced tree) and O(n) in the worst-case scenario (skewed tree).
