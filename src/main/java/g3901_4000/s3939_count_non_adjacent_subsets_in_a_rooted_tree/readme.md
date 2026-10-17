[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3939\. Count Non Adjacent Subsets in a Rooted Tree

Hard

You are given a rooted tree with `n` nodes labeled from 0 to `n - 1`, represented by an integer array `parent` of length `n`, where:

*   `parent[0] = -1` (node 0 is the root).
*   For each `1 <= i < n`, `parent[i]` is the parent of node `i` (`0 <= parent[i] < i`).

You are also given an integer array nums of length `n`, where `nums[i]` is the value of node `i`, and an integer `k`.

A non-empty subset of nodes is called **valid** if:

*   The **sum** of the values of the selected nodes is **divisible** by `k`.
*   No **two** selected nodes are **adjacent** in the tree (no node and its direct parent are both included in the subset).

Return the number of valid subsets modulo <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** parent = [-1,0,1], nums = [1,2,3], k = 3

**Output:** 1

**Explanation:**

**![](https://assets.leetcode.com/uploads/2025/07/17/image1.png)**

The only valid subset is `{2}`. It contains node 2 with value 3, which is divisible by 3.

**Example 2:**

**Input:** parent = [-1,0,0,0], nums = [2,1,2,1], k = 3

**Output:** 2

**Explanation:**

**![](https://assets.leetcode.com/uploads/2025/07/17/image2.png)**

The valid subsets are:

*   `{1, 2}`: Nodes 1 and 2 are both children of node 0 and not directly connected to each other. Their values sum to `1 + 2 = 3`, which is divisible by 3.
*   `{2, 3}`: Nodes 2 and 3 are also non-adjacent. Their values sum to `2 + 1 = 3`, which is divisible by 3.

No other subset satisfies both conditions. Therefore, the answer is 2.

**Constraints:**

*   `n == parent.length == nums.length`
*   `1 <= n <= 1000`
*   `parent[0] == -1`
*   For all `1 <= i < n`:
    *   `0 <= parent[i] < i`
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   `1 <= k <= 100`
*   `parent` describes a valid rooted tree.

## Solution

```java
import java.util.Arrays;

public class Solution {
    private static final int MOD = 1000000007;

    public int countValidSubsets(int[] parent, int[] nums, int k) {
        int n = parent.length;
        int[] h = new int[n];
        int[] to = new int[n - 1];
        int[] nx = new int[n - 1];
        buildTree(parent, h, to, nx);
        long[][][] dp = new long[n][2][k];
        dfs(0, h, to, nx, nums, k, dp);
        return getResult(dp);
    }

    private void buildTree(int[] parent, int[] h, int[] to, int[] nx) {
        Arrays.fill(h, -1);
        int idx = 0;
        for (int i = 1; i < parent.length; i++) {
            to[idx] = i;
            nx[idx] = h[parent[i]];
            h[parent[i]] = idx++;
        }
    }

    private void dfs(int u, int[] h, int[] to, int[] nx, int[] nums, int k, long[][][] dp) {
        initializeDp(u, nums, k, dp);
        for (int e = h[u]; e != -1; e = nx[e]) {
            int v = to[e];
            dfs(v, h, to, nx, nums, k, dp);
            mergeChild(dp[u], dp[v], k);
        }
    }

    private void initializeDp(int u, int[] nums, int k, long[][][] dp) {
        dp[u][0][0] = 1;
        dp[u][1][nums[u] % k] = 1;
    }

    private void mergeChild(long[][] current, long[][] child, int k) {
        long[][] next = new long[2][k];
        mergeExcludedState(current[0], child, next[0], k);
        mergeSelectedState(current[1], child[0], next[1], k);
        copyState(next, current, k);
    }

    private void mergeExcludedState(long[] current, long[][] child, long[] next, int k) {
        for (int a = 0; a < k; a++) {
            if (current[a] != 0) {
                mergeWithChildStates(current[a], child[0], child[1], next, a, k);
            }
        }
    }

    private void mergeSelectedState(long[] current, long[] childExcluded, long[] next, int k) {
        for (int a = 0; a < k; a++) {
            if (current[a] != 0) {
                mergeWithSingleChildState(current[a], childExcluded, next, a, k);
            }
        }
    }

    private void mergeWithChildStates(
            long currentWays,
            long[] childExcluded,
            long[] childSelected,
            long[] next,
            int currentMod,
            int k) {
        for (int childMod = 0; childMod < k; childMod++) {
            long childWays = (childExcluded[childMod] + childSelected[childMod]) % MOD;
            addWays(next, currentMod, childMod, currentWays, childWays, k);
        }
    }

    private void mergeWithSingleChildState(
            long currentWays, long[] child, long[] next, int currentMod, int k) {
        for (int childMod = 0; childMod < k; childMod++) {
            addWays(next, currentMod, childMod, currentWays, child[childMod], k);
        }
    }

    private void addWays(
            long[] target, int currentMod, int childMod, long currentWays, long childWays, int k) {
        if (childWays == 0) {
            return;
        }
        int newMod = (currentMod + childMod) % k;
        target[newMod] = (target[newMod] + currentWays * childWays) % MOD;
    }

    private void copyState(long[][] source, long[][] target, int k) {
        for (int state = 0; state < 2; state++) {
            if (k >= 0) {
                System.arraycopy(source[state], 0, target[state], 0, k);
            }
        }
    }

    private int getResult(long[][][] dp) {
        long result = dp[0][0][0] + dp[0][1][0];
        result %= MOD;
        result = (result - 1 + MOD) % MOD;
        return (int) result;
    }
}
```