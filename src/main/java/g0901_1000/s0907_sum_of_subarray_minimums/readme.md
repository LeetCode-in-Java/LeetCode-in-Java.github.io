[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 907\. Sum of Subarray Minimums

Medium

Given an array of integers arr, find the sum of `min(b)`, where `b` ranges over every (contiguous) subarray of `arr`. Since the answer may be large, return the answer **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** arr = [3,1,2,4]

**Output:** 17

**Explanation:** 

Subarrays are [3], [1], [2], [4], [3,1], [1,2], [2,4], [3,1,2], [1,2,4], [3,1,2,4]. 

Minimums are 3, 1, 2, 4, 1, 1, 2, 1, 1, 1. Sum is 17.

**Example 2:**

**Input:** arr = [11,81,94,43,3]

**Output:** 444

**Constraints:**

*   <code>1 <= arr.length <= 3 * 10<sup>4</sup></code>
*   <code>1 <= arr[i] <= 3 * 10<sup>4</sup></code>

## Solution

```java
public class Solution {
    private static final int MOD = 1_000_000_007;

    private int calculateRight(int i, int start, int[] right, int[] arr, int len) {
        if (start >= len) {
            return 0;
        }
        if (arr[start] < arr[i]) {
            return 0;
        }
        return (1 + right[start] + calculateRight(i, start + right[start] + 1, right, arr, len))
                % MOD;
    }

    private int calculateLeft(int i, int start, int[] left, int[] arr, int len) {
        if (start < 0) {
            return 0;
        }
        if (arr[start] <= arr[i]) {
            return 0;
        }
        return (1 + left[start] + calculateLeft(i, start - left[start] - 1, left, arr, len)) % MOD;
    }

    public int sumSubarrayMins(int[] arr) {
        int len = arr.length;
        int[] right = new int[len];
        int[] left = new int[len];

        right[len - 1] = 0;
        for (int i = len - 2; i >= 0; --i) {
            right[i] = calculateRight(i, i + 1, right, arr, len);
        }
        left[0] = 0;
        for (int i = 1; i < len; ++i) {
            left[i] = calculateLeft(i, i - 1, left, arr, len);
        }
        int answer = 0;
        for (int i = 0; i < len; ++i) {
            long modl = 1_000_000_007;
            answer += (int) (((((1 + left[i]) * (long) (1 + right[i])) % modl) * arr[i]) % modl);
            answer %= MOD;
        }
        return answer;
    }
}
```