[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 932\. Beautiful Array

Medium

An array `nums` of length `n` is **beautiful** if:

*   `nums` is a permutation of the integers in the range `[1, n]`.
*   For every `0 <= i < j < n`, there is no index `k` with `i < k < j` where `2 * nums[k] == nums[i] + nums[j]`.

Given the integer `n`, return _any **beautiful** array_ `nums` _of length_ `n`. There will be at least one valid answer for the given `n`.

**Example 1:**

**Input:** n = 4

**Output:** [2,1,4,3]

**Example 2:**

**Input:** n = 5

**Output:** [3,1,2,5,4]

**Constraints:**

*   `1 <= n <= 1000`

## Solution

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    private Map<Integer, int[]> memo;

    public int[] beautifulArray(int n) {
        memo = new HashMap<>();
        return helper(n);
    }

    private int[] helper(int n) {
        if (n == 1) {
            memo.put(1, new int[] {1});
            return new int[] {1};
        }
        if (memo.containsKey(n)) {
            return memo.get(n);
        }
        int mid = (n + 1) / 2;
        int[] left = helper(mid);
        int[] right = helper(n - mid);
        int[] rst = new int[n];
        for (int i = 0; i < mid; i++) {
            rst[i] = left[i] * 2 - 1;
        }
        for (int i = mid; i < n; i++) {
            rst[i] = right[i - mid] * 2;
        }
        memo.put(n, rst);
        return rst;
    }
}
```