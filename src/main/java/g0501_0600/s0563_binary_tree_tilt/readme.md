[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 563\. Binary Tree Tilt

Easy

Given the `root` of a binary tree, return _the sum of every tree node's **tilt**._

The **tilt** of a tree node is the **absolute difference** between the sum of all left subtree node **values** and all right subtree node **values**. If a node does not have a left child, then the sum of the left subtree node **values** is treated as `0`. The rule is similar if the node does not have a right child.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/20/tilt1.jpg)

**Input:** root = [1,2,3]

**Output:** 1

**Explanation:** 

    Tilt of node 2 : |0-0| = 0 (no children)
    Tilt of node 3 : |0-0| = 0 (no children)
    Tilt of node 1 : |2-3| = 1 (left subtree is just left child, so sum is 2; right subtree is just right child, so sum is 3) 
    Sum of every tilt : 0 + 0 + 1 = 1

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/10/20/tilt2.jpg)

**Input:** root = [4,2,9,3,5,null,7]

**Output:** 15

**Explanation:** 

    Tilt of node 3 : |0-0| = 0 (no children) 
    Tilt of node 5 : |0-0| = 0 (no children) 
    Tilt of node 7 : |0-0| = 0 (no children) 
    Tilt of node 2 : |3-5| = 2 (left subtree is just left child, so sum is 3; right subtree is just right child, so sum is 5) 
    Tilt of node 9 : |0-7| = 7 (no left child, so sum is 0; right subtree is just right child, so sum is 7) 
    Tilt of node 4 : |(3+5+2)-(9+7)| = |10-16| = 6 (left subtree values are 3, 5, and 2, which sums to 10; right subtree values are 9 and 7, which sums to 16) 
    Sum of every tilt : 0 + 0 + 0 + 2 + 7 + 6 = 15

**Example 3:**

![](https://assets.leetcode.com/uploads/2020/10/20/tilt3.jpg)

**Input:** root = [21,7,14,1,1,2,2,3,3]

**Output:** 9

**Constraints:**

*   The number of nodes in the tree is in the range <code>[0, 10<sup>4</sup>]</code>.
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
    private int sum = 0;

    private int sumTilt(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int ls = sumTilt(root.left);
        int rs = sumTilt(root.right);
        sum += Math.abs(ls - rs);
        return ls + rs + root.val;
    }

    public int findTilt(TreeNode root) {
        sum = 0;
        sumTilt(root);
        return sum;
    }
}
```