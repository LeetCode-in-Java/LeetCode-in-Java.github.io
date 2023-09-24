[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2741\. Special Permutations

Medium

You are given a **0-indexed** integer array `nums` containing `n` **distinct** positive integers. A permutation of `nums` is called special if:

*   For all indexes `0 <= i < n - 1`, either `nums[i] % nums[i+1] == 0` or `nums[i+1] % nums[i] == 0`.

Return _the total number of special permutations. _As the answer could be large, return it **modulo **<code>10<sup>9 </sup>+ 7</code>.

**Example 1:**

**Input:** nums = [2,3,6]

**Output:** 2

**Explanation:** [3,6,2] and [2,6,3] are the two special permutations of nums.

**Example 2:**

**Input:** nums = [1,4,3]

**Output:** 2

**Explanation:** [3,1,4] and [4,1,3] are the two special permutations of nums.

**Constraints:**

*   `2 <= nums.length <= 14`
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    private int n;
    private Integer[][] memo;
    private int[] nums;

    public int specialPerm(int[] nums) {
        this.n = nums.length;
        this.memo = new Integer[n][1 << n];
        this.nums = nums;
        return backtrack(0, 0);
    }

    private int backtrack(int preIndex, int mask) {
        if (mask == (1 << n) - 1) {
            return 1;
        }
        if (memo[preIndex][mask] != null) {
            return memo[preIndex][mask];
        }
        int count = 0;
        int mod = (int) 1e9 + 7;
        for (int i = 0; i < n; i++) {
            if ((mask & (1 << i)) == 0
                    && (mask == 0
                            || nums[i] % nums[preIndex] == 0
                            || nums[preIndex] % nums[i] == 0)) {
                count = (count + backtrack(i, mask | (1 << i))) % mod;
            }
        }
        memo[preIndex][mask] = count;
        return memo[preIndex][mask];
    }
}
```