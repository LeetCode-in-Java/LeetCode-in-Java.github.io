[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1161\. Maximum Level Sum of a Binary Tree

Medium

Given the `root` of a binary tree, the level of its root is `1`, the level of its children is `2`, and so on.

Return the **smallest** level `x` such that the sum of all the values of nodes at level `x` is **maximal**.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/05/03/capture.JPG)

**Input:** root = [1,7,0,7,-8,null,null]

**Output:** 2

**Explanation:**  

Level 1 sum = 1. 

Level 2 sum = 7 + 0 = 7. 

Level 3 sum = 7 + -8 = -1. 

So we return the level with the maximum sum which is level 2.

**Example 2:**

**Input:** root = [989,null,10250,98693,-89388,null,null,null,-32127]

**Output:** 2

**Constraints:**

*   The number of nodes in the tree is in the range <code>[1, 10<sup>4</sup>]</code>.
*   <code>-10<sup>5</sup> <= Node.val <= 10<sup>5</sup></code>

## Solution

```java
import com_github_leetcode.TreeNode;
import java.util.ArrayList;
import java.util.List;

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
    private List<Integer> sums;

    public int maxLevelSum(TreeNode root) {
        sums = new ArrayList<>();
        find(root, 1);
        int ans = 1;
        int maxv = Integer.MIN_VALUE;
        for (int i = 0; i < sums.size(); i++) {
            if (sums.get(i) > maxv) {
                maxv = sums.get(i);
                ans = i + 1;
            }
        }
        return ans;
    }

    private void find(TreeNode root, int height) {
        if (root == null) {
            return;
        }
        if (sums.size() < height) {
            sums.add(root.val);
        } else {
            sums.set(height - 1, sums.get(height - 1) + root.val);
        }
        find(root.left, height + 1);
        find(root.right, height + 1);
    }
}
```