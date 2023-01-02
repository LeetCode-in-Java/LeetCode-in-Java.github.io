[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2458\. Height of Binary Tree After Subtree Removal Queries

Hard

You are given the `root` of a **binary tree** with `n` nodes. Each node is assigned a unique value from `1` to `n`. You are also given an array `queries` of size `m`.

You have to perform `m` **independent** queries on the tree where in the <code>i<sup>th</sup></code> query you do the following:

*   **Remove** the subtree rooted at the node with the value `queries[i]` from the tree. It is **guaranteed** that `queries[i]` will **not** be equal to the value of the root.

Return _an array_ `answer` _of size_ `m` _where_ `answer[i]` _is the height of the tree after performing the_ <code>i<sup>th</sup></code> _query_.

**Note**:

*   The queries are independent, so the tree returns to its **initial** state after each query.
*   The height of a tree is the **number of edges in the longest simple path** from the root to some node in the tree.

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/09/07/binaryytreeedrawio-1.png)

**Input:** root = [1,3,4,2,null,6,5,null,null,null,null,null,7], queries = [4]

**Output:** [2]

**Explanation:** The diagram above shows the tree after removing the subtree rooted at node with value 4. The height of the tree is 2 (The path 1 -> 3 -> 2).

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/09/07/binaryytreeedrawio-2.png)

**Input:** root = [5,8,9,2,1,3,7,4,6], queries = [3,2,4,8]

**Output:** [3,2,3,2]

**Explanation:** We have the following queries: 

- Removing the subtree rooted at node with value 3. The height of the tree becomes 3 (The path 5 -> 8 -> 2 -> 4). 
 
- Removing the subtree rooted at node with value 2. The height of the tree becomes 2 (The path 5 -> 8 -> 1). 

- Removing the subtree rooted at node with value 4. The height of the tree becomes 3 (The path 5 -> 8 -> 2 -> 6). 

- Removing the subtree rooted at node with value 8. The height of the tree becomes 2 (The path 5 -> 9 -> 3).

**Constraints:**

*   The number of nodes in the tree is `n`.
*   <code>2 <= n <= 10<sup>5</sup></code>
*   `1 <= Node.val <= n`
*   All the values in the tree are **unique**.
*   `m == queries.length`
*   <code>1 <= m <= min(n, 10<sup>4</sup>)</code>
*   `1 <= queries[i] <= n`
*   `queries[i] != root.val`

## Solution

```java
import com_github_leetcode.TreeNode;
import java.util.HashMap;
import java.util.Map;

public class Solution {
    public int[] treeQueries(TreeNode root, int[] queries) {
        Map<Integer, int[]> levels = new HashMap<>();
        Map<Integer, int[]> map = new HashMap<>();
        int max = dfs(root, 0, map, levels) - 1;
        int n = queries.length;
        for (int i = 0; i < n; i++) {
            int q = queries[i];
            int[] node = map.get(q);
            int height = node[0];
            int level = node[1];
            int[] lev = levels.get(level);
            if (lev[0] == height) {
                if (lev[1] != -1) {
                    queries[i] = max - Math.abs(lev[0] - lev[1]);
                } else {
                    queries[i] = max - height - 1;
                }
            } else {
                queries[i] = max;
            }
        }
        return queries;
    }

    private int dfs(TreeNode root, int level, Map<Integer, int[]> map, Map<Integer, int[]> levels) {
        if (root == null) {
            return 0;
        }
        int left = dfs(root.left, level + 1, map, levels);
        int right = dfs(root.right, level + 1, map, levels);
        int height = Math.max(left, right);
        int[] lev = levels.getOrDefault(level, new int[] {-1, -1});
        if (height >= lev[0]) {
            lev[1] = lev[0];
            lev[0] = height;
        } else {
            lev[1] = Math.max(lev[1], height);
        }
        levels.put(level, lev);
        map.put(root.val, new int[] {height, level});
        return Math.max(left, right) + 1;
    }
}
```