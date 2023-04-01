[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2265\. Count Nodes Equal to Average of Subtree

Medium

Given the `root` of a binary tree, return _the number of nodes where the value of the node is equal to the **average** of the values in its **subtree**_.

**Note:**

*   The **average** of `n` elements is the **sum** of the `n` elements divided by `n` and **rounded down** to the nearest integer.
*   A **subtree** of `root` is a tree consisting of `root` and all of its descendants.

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/03/15/image-20220315203925-1.png)

**Input:** root = [4,8,5,0,1,null,6]

**Output:** 5

**Explanation:**

For the node with value 4: The average of its subtree is (4 + 8 + 5 + 0 + 1 + 6) / 6 = 24 / 6 = 4.

For the node with value 5: The average of its subtree is (5 + 6) / 2 = 11 / 2 = 5.

For the node with value 0: The average of its subtree is 0 / 1 = 0.

For the node with value 1: The average of its subtree is 1 / 1 = 1.

For the node with value 6: The average of its subtree is 6 / 1 = 6.

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/03/26/image-20220326133920-1.png)

**Input:** root = [1]

**Output:** 1

**Explanation:** For the node with value 1: The average of its subtree is 1 / 1 = 1.

**Constraints:**

*   The number of nodes in the tree is in the range `[1, 1000]`.
*   `0 <= Node.val <= 1000`

## Solution

```java
import com_github_leetcode.TreeNode;

public class Solution {
    private int ans = 0;

    public int averageOfSubtree(TreeNode root) {
        dfs(root);
        return ans;
    }

    private int[] dfs(TreeNode node) {
        if (node == null) {
            return new int[] {0, 0};
        }
        int[] left = dfs(node.left);
        int[] right = dfs(node.right);
        int currsum = left[0] + right[0] + node.val;
        int currcount = left[1] + right[1] + 1;
        if (currsum / currcount == node.val) {
            ++ans;
        }
        return new int[] {currsum, currcount};
    }
}
```