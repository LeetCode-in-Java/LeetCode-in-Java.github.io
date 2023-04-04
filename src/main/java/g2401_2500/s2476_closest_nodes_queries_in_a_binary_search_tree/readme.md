[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2476\. Closest Nodes Queries in a Binary Search Tree

Medium

You are given the `root` of a **binary search tree** and an array `queries` of size `n` consisting of positive integers.

Find a **2D** array `answer` of size `n` where <code>answer[i] = [min<sub>i</sub>, max<sub>i</sub>]</code>:

*   <code>min<sub>i</sub></code> is the **largest** value in the tree that is smaller than or equal to `queries[i]`. If a such value does not exist, add `-1` instead.
*   <code>max<sub>i</sub></code> is the **smallest** value in the tree that is greater than or equal to `queries[i]`. If a such value does not exist, add `-1` instead.

Return _the array_ `answer`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/09/28/bstreeedrawioo.png)

**Input:** root = [6,2,13,1,4,9,15,null,null,null,null,null,null,14], queries = [2,5,16]

**Output:** [[2,2],[4,6],[15,-1]]

**Explanation:** We answer the queries in the following way:
- The largest number that is smaller or equal than 2 in the tree is 2, and the smallest number that is greater or equal than 2 is still 2. So the answer for the first query is [2,2]. 
- The largest number that is smaller or equal than 5 in the tree is 4, and the smallest number that is greater or equal than 5 is 6. So the answer for the second query is [4,6]. 
- The largest number that is smaller or equal than 16 in the tree is 15, and the smallest number that is greater or equal than 16 does not exist. So the answer for the third query is [15,-1].

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/09/28/bstttreee.png)

**Input:** root = [4,null,9], queries = [3]

**Output:** [[-1,4]]

**Explanation:** The largest number that is smaller or equal to 3 in the tree does not exist, and the smallest number that is greater or equal to 3 is 4. So the answer for the query is [-1,4].

**Constraints:**

*   The number of nodes in the tree is in the range <code>[2, 10<sup>5</sup>]</code>.
*   <code>1 <= Node.val <= 10<sup>6</sup></code>
*   `n == queries.length`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   <code>1 <= queries[i] <= 10<sup>6</sup></code>

## Solution

```java
import com_github_leetcode.TreeNode;
import java.util.ArrayList;
import java.util.List;
import java.util.TreeSet;

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
    private final TreeSet<Integer> set = new TreeSet<>();

    private void traverse(TreeNode root) {
        if (root == null) {
            return;
        }
        traverse(root.left);
        set.add(root.val);
        traverse(root.right);
    }

    public List<List<Integer>> closestNodes(TreeNode root, List<Integer> queries) {
        traverse(root);
        List<List<Integer>> ans = new ArrayList<>();
        for (int q : queries) {
            List<Integer> list = new ArrayList<>();
            Integer min = set.floor(q);
            if (min != null) {
                list.add(min);
            } else {
                list.add(-1);
            }
            Integer max = set.ceiling(q);
            if (max != null) {
                list.add(max);
            } else {
                list.add(-1);
            }
            ans.add(list);
        }
        return ans;
    }
}
```