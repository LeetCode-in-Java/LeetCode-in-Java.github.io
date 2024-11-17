[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 437\. Path Sum III

Medium

Given the `root` of a binary tree and an integer `targetSum`, return _the number of paths where the sum of the values along the path equals_ `targetSum`.

The path does not need to start or end at the root or a leaf, but it must go downwards (i.e., traveling only from parent nodes to child nodes).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/04/09/pathsum3-1-tree.jpg)

**Input:** root = [10,5,-3,3,2,null,11,3,-2,null,1], targetSum = 8

**Output:** 3

**Explanation:** The paths that sum to 8 are shown. 

**Example 2:**

**Input:** root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22

**Output:** 3 

**Constraints:**

*   The number of nodes in the tree is in the range `[0, 1000]`.
*   <code>-10<sup>9</sup> <= Node.val <= 10<sup>9</sup></code>
*   `-1000 <= targetSum <= 1000`

## Solution

```java
import com_github_leetcode.TreeNode;
import java.util.HashMap;

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
    public int pathSum(TreeNode root, int targetSum) {
        HashMap<Long, Integer> h = new HashMap<>();
        return dfs(root, targetSum, h, 0L);
    }

    int dfs(TreeNode root, int t, HashMap<Long, Integer> h, Long cs) {
        int s = 0;
        if (root == null) {
            return 0;
        }
        Long k = cs + root.val;
        if (k == t) {
            s += 1;
        }
        if (h.getOrDefault(k - t, 0) > 0) {
            s += h.get(k - t);
        }
        h.put(k, h.getOrDefault(k, 0) + 1);
        s += dfs(root.left, t, h, k);
        s += dfs(root.right, t, h, k);
        h.put(k, h.getOrDefault(k, 0) - 1);
        return s;
    }
}
```

**Time Complexity (Big O Time):**

The time complexity of the program primarily depends on two recursive functions:

1. `pathSum(TreeNode root, int targetSum)`: This function is the main driver of the calculation. It traverses the entire tree and calls the `helper` function for each node. In the worst case, it visits all nodes of the binary tree once. Therefore, its time complexity is O(n), where 'n' is the number of nodes in the tree.

2. `helper(TreeNode node, int targetSum, long currSum)`: This function recursively explores all possible paths from a given node downward to its descendants. In the worst case, it traverses all paths from the root to leaf nodes. Since each node is visited once, the time complexity of this function is also O(n).

Combining both functions, the overall time complexity of the program is O(n), where 'n' is the number of nodes in the binary tree.

**Space Complexity (Big O Space):**

The space complexity of the program is primarily determined by the function call stack during recursion and the constant space used for variables:

1. Function Call Stack: The maximum depth of the call stack is the height of the binary tree. In the worst case, when the tree is skewed (completely unbalanced), the height can be 'n,' making the space complexity due to the call stack O(n).

2. Various integer variables used for iteration and intermediate calculations: These variables require constant space, O(1).

Combining these factors, the dominant factor in the space complexity is the call stack, which can grow up to O(n) in the worst case.

In summary, the provided program has a time complexity of O(n) and a space complexity of O(n), where 'n' is the number of nodes in the binary tree.
