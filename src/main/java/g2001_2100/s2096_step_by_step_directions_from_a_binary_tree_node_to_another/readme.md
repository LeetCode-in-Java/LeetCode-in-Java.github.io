[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2096\. Step-By-Step Directions From a Binary Tree Node to Another

Medium

You are given the `root` of a **binary tree** with `n` nodes. Each node is uniquely assigned a value from `1` to `n`. You are also given an integer `startValue` representing the value of the start node `s`, and a different integer `destValue` representing the value of the destination node `t`.

Find the **shortest path** starting from node `s` and ending at node `t`. Generate step-by-step directions of such path as a string consisting of only the **uppercase** letters `'L'`, `'R'`, and `'U'`. Each letter indicates a specific direction:

*   `'L'` means to go from a node to its **left child** node.
*   `'R'` means to go from a node to its **right child** node.
*   `'U'` means to go from a node to its **parent** node.

Return _the step-by-step directions of the **shortest path** from node_ `s` _to node_ `t`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/11/15/eg1.png)

**Input:** root = [5,1,2,3,null,6,4], startValue = 3, destValue = 6

**Output:** "UURL"

**Explanation:** The shortest path is: 3 → 1 → 5 → 2 → 6. 

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/11/15/eg2.png)

**Input:** root = [2,1], startValue = 2, destValue = 1

**Output:** "L"

**Explanation:** The shortest path is: 2 → 1. 

**Constraints:**

*   The number of nodes in the tree is `n`.
*   <code>2 <= n <= 10<sup>5</sup></code>
*   `1 <= Node.val <= n`
*   All the values in the tree are **unique**.
*   `1 <= startValue, destValue <= n`
*   `startValue != destValue`

## Solution

```java
import com_github_leetcode.TreeNode;

public class Solution {
    private boolean find(TreeNode n, int val, StringBuilder sb) {
        if (n.val == val) {
            return true;
        }
        if (n.left != null && find(n.left, val, sb)) {
            sb.append("L");
        } else if (n.right != null && find(n.right, val, sb)) {
            sb.append("R");
        }
        return !sb.isEmpty();
    }

    public String getDirections(TreeNode root, int startValue, int destValue) {
        StringBuilder s = new StringBuilder();
        StringBuilder d = new StringBuilder();
        find(root, startValue, s);
        find(root, destValue, d);
        int i = 0;
        int maxI = Math.min(d.length(), s.length());
        while (i < maxI && s.charAt(s.length() - i - 1) == d.charAt(d.length() - i - 1)) {
            ++i;
        }
        return "U".repeat(Math.max(0, s.length() - i)) + d.reverse().substring(i);
    }
}
```