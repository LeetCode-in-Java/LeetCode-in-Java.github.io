[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 508\. Most Frequent Subtree Sum

Medium

Given the `root` of a binary tree, return the most frequent **subtree sum**. If there is a tie, return all the values with the highest frequency in any order.

The **subtree sum** of a node is defined as the sum of all the node values formed by the subtree rooted at that node (including the node itself).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/04/24/freq1-tree.jpg)

**Input:** root = [5,2,-3]

**Output:** [2,-3,4]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/04/24/freq2-tree.jpg)

**Input:** root = [5,2,-5]

**Output:** [2]c

**Constraints:**

*   The number of nodes in the tree is in the range <code>[1, 10<sup>4</sup>]</code>.
*   <code>-10<sup>5</sup> <= Node.val <= 10<sup>5</sup></code>

## Solution

```java
import com_github_leetcode.TreeNode;
import java.util.ArrayList;
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
    public int[] findFrequentTreeSum(TreeNode root) {
        ArrayList<Integer> arr = new ArrayList<>();
        HashMap<Integer, Integer> hm = new HashMap<>();
        fun(root, hm);
        int maxvalue = Integer.MIN_VALUE;
        for (Map.Entry<Integer, Integer> map : hm.entrySet()) {
            if (map.getValue() > maxvalue) {
                maxvalue = map.getValue();
            }
        }
        for (Map.Entry<Integer, Integer> map : hm.entrySet()) {
            if (map.getValue() == maxvalue) {
                arr.add(map.getKey());
            }
        }
        int[] newArr = new int[arr.size()];
        for (int i = 0; i < arr.size(); i++) {
            newArr[i] = arr.get(i);
        }
        return newArr;
    }

    private int fun(TreeNode node, HashMap<Integer, Integer> hm) {
        if (node == null) {
            return 0;
        }
        int left = fun(node.left, hm);
        int right = fun(node.right, hm);
        int sum = node.val + left + right;
        if (hm.containsKey(sum)) {
            hm.put(sum, hm.get(sum) + 1);
        } else {
            hm.put(sum, 0);
        }
        return sum;
    }
}
```