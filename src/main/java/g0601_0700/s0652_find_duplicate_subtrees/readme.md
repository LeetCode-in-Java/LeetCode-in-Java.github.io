[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 652\. Find Duplicate Subtrees

Medium

Given the `root` of a binary tree, return all **duplicate subtrees**.

For each kind of duplicate subtrees, you only need to return the root node of any **one** of them.

Two trees are **duplicate** if they have the **same structure** with the **same node values**.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/08/16/e1.jpg)

**Input:** root = [1,2,3,4,null,2,4,null,null,4]

**Output:** [[2,4],[4]]

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/08/16/e2.jpg)

**Input:** root = [2,1,1]

**Output:** [[1]]

**Example 3:**

![](https://assets.leetcode.com/uploads/2020/08/16/e33.jpg)

**Input:** root = [2,2,2,3,null,3,null]

**Output:** [[2,3],[3]]

**Constraints:**

*   The number of the nodes in the tree will be in the range `[1, 10^4]`
*   `-200 <= Node.val <= 200`

## Solution

```java
import com_github_leetcode.TreeNode;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

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
    public List<TreeNode> findDuplicateSubtrees(TreeNode root) {
        Map<String, Integer> map = new HashMap<>();
        List<TreeNode> list = new ArrayList<>();
        helper(root, map, list);
        return list;
    }

    private String helper(TreeNode root, Map<String, Integer> map, List<TreeNode> list) {
        if (root == null) {
            return "#";
        }
        String key =
                helper(root.left, map, list) + "#" + helper(root.right, map, list) + "#" + root.val;
        map.put(key, map.getOrDefault(key, 0) + 1);
        if (map.get(key) == 2) {
            list.add(root);
        }
        return key;
    }
}
```