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
    private int count(TreeNode root, int targetSum, Map<Integer, Integer> hashMap, int currentSum) {
        if (root == null) {
            return 0;
        }
        int count = 0;
        if (hashMap.containsKey(currentSum + root.val - targetSum)) {
            count = count + hashMap.get(currentSum + root.val - targetSum);
        }
        hashMap.put(currentSum + root.val, hashMap.getOrDefault(currentSum + root.val, 0) + 1);
        int l1 = count(root.left, targetSum, hashMap, currentSum + root.val);
        int l2 = count(root.right, targetSum, hashMap, currentSum + root.val);
        hashMap.put(currentSum + root.val, hashMap.getOrDefault(currentSum + root.val, 0) - 1);
        return l1 + l2 + count;
    }

    public int pathSum(TreeNode root, int targetSum) {
        Map<Integer, Integer> map = new HashMap<>();
        map.put(0, 1);
        return count(root, targetSum, map, 0);
    }
}
```