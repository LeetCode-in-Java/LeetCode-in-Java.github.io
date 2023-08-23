[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2583\. Kth Largest Sum in a Binary Tree

Medium

You are given the `root` of a binary tree and a positive integer `k`.

The **level sum** in the tree is the sum of the values of the nodes that are on the **same** level.

Return _the_ <code>k<sup>th</sup></code> _**largest** level sum in the tree (not necessarily distinct)_. If there are fewer than `k` levels in the tree, return `-1`.

**Note** that two nodes are on the same level if they have the same distance from the root.

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/12/14/binaryytreeedrawio-2.png)

**Input:** root = [5,8,9,2,1,3,7,4,6], k = 2

**Output:** 13

**Explanation:** The level sums are the following: 

- Level 1: 5. - Level 2: 8 + 9 = 17. 

- Level 3: 2 + 1 + 3 + 7 = 13. 

- Level 4: 4 + 6 = 10. The 2<sup>nd</sup> largest level sum is 13.

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/12/14/treedrawio-3.png)

**Input:** root = [1,2,null,3], k = 1

**Output:** 3

**Explanation:** The largest level sum is 3.

**Constraints:**

*   The number of nodes in the tree is `n`.
*   <code>2 <= n <= 10<sup>5</sup></code>
*   <code>1 <= Node.val <= 10<sup>6</sup></code>
*   `1 <= k <= n`

## Solution

```java
import com_github_leetcode.TreeNode;
import java.util.ArrayList;
import java.util.Collections;
import java.util.LinkedList;
import java.util.List;
import java.util.Queue;
import java.util.Random;

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
@SuppressWarnings("java:S2245")
public class Solution {
    private final Random random = new Random();

    public long kthLargestLevelSum(TreeNode root, int k) {
        List<Long> ans = new ArrayList<>();
        Queue<TreeNode> q = new LinkedList<>();
        q.add(root);
        while (!q.isEmpty()) {
            long sum = 0;
            int size = q.size();
            for (int i = 0; i < size; i++) {
                TreeNode curr = q.remove();
                sum += curr.val;
                if (curr.left != null) {
                    q.add(curr.left);
                }
                if (curr.right != null) {
                    q.add(curr.right);
                }
            }
            ans.add(sum);
        }
        if (k > ans.size()) {
            return -1;
        }
        k = ans.size() - k;
        int start = 0;
        int idx;
        int end = ans.size() - 1;
        while (true) {
            idx = random.nextInt(end - start + 1) + start;
            Long piv = ans.get(idx);
            Collections.swap(ans, idx, end);
            idx = start;
            for (int i = start; i <= end; i++) {
                if (ans.get(i) <= piv) {
                    Collections.swap(ans, i, idx++);
                }
            }
            idx--;
            if (idx < k) {
                start = idx + 1;
            } else if (idx > k) {
                end = idx - 1;
            } else {
                break;
            }
        }
        return ans.get(idx);
    }
}
```