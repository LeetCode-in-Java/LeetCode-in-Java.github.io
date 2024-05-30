[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3152\. Special Array II

Medium

An array is considered **special** if every pair of its adjacent elements contains two numbers with different parity.

You are given an array of integer `nums` and a 2D integer matrix `queries`, where for <code>queries[i] = [from<sub>i</sub>, to<sub>i</sub>]</code> your task is to check that subarray <code>nums[from<sub>i</sub>..to<sub>i</sub>]</code> is **special** or not.

Return an array of booleans `answer` such that `answer[i]` is `true` if <code>nums[from<sub>i</sub>..to<sub>i</sub>]</code> is special.

**Example 1:**

**Input:** nums = [3,4,1,2,6], queries = \[\[0,4]]

**Output:** [false]

**Explanation:**

The subarray is `[3,4,1,2,6]`. 2 and 6 are both even.

**Example 2:**

**Input:** nums = [4,3,1,6], queries = \[\[0,2],[2,3]]

**Output:** [false,true]

**Explanation:**

1.  The subarray is `[4,3,1]`. 3 and 1 are both odd. So the answer to this query is `false`.
2.  The subarray is `[1,6]`. There is only one pair: `(1,6)` and it contains numbers with different parity. So the answer to this query is `true`.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>
*   <code>1 <= queries.length <= 10<sup>5</sup></code>
*   `queries[i].length == 2`
*   `0 <= queries[i][0] <= queries[i][1] <= nums.length - 1`

## Solution

```java
public class Solution {
    public boolean[] isArraySpecial(int[] nums, int[][] queries) {
        int n = nums.length;
        int[] bad = new int[n];
        for (int i = 1; i < n; i++) {
            bad[i] = bad[i - 1] + (((nums[i - 1] ^ nums[i]) & 1) ^ 1);
        }
        int nq = queries.length;
        boolean[] res = new boolean[nq];
        for (int i = 0; i < nq; i++) {
            int[] q = queries[i];
            res[i] = calc(bad, q[0], q[1]) == 0;
        }
        return res;
    }

    private int calc(int[] arr, int st, int end) {
        return arr[end] - arr[st];
    }
}
```