[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3686\. Number of Stable Subsequences

Hard

You are given an integer array `nums`.

A **subsequence** is **stable** if it does not contain **three consecutive** elements with the **same** parity when the subsequence is read **in order** (i.e., consecutive **inside the subsequence**).

Return the number of stable subsequences.

Since the answer may be too large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** nums = [1,3,5]

**Output:** 6

**Explanation:**

*   Stable subsequences are `[1]`, `[3]`, `[5]`, `[1, 3]`, `[1, 5]`, and `[3, 5]`.
*   Subsequence `[1, 3, 5]` is not stable because it contains three consecutive odd numbers. Thus, the answer is 6.

**Example 2:**

**Input:** nums = [2,3,4,2]

**Output:** 14

**Explanation:**

*   The only subsequence that is not stable is `[2, 4, 2]`, which contains three consecutive even numbers.
*   All other subsequences are stable. Thus, the answer is 14.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    private static final long MOD = 1000000007L;

    public int countStableSubsequences(int[] nums) {
        long e1 = 0;
        long e2 = 0;
        long o1 = 0;
        long o2 = 0;
        for (int x : nums) {
            if ((x & 1) == 0) {
                long ne1 = (e1 + (o1 + o2 + 1)) % MOD;
                long ne2 = (e2 + e1) % MOD;
                e1 = ne1;
                e2 = ne2;
            } else {
                long no1 = (o1 + (e1 + e2 + 1)) % MOD;
                long no2 = (o2 + o1) % MOD;
                o1 = no1;
                o2 = no2;
            }
        }
        long ans = (e1 + e2 + o1 + o2) % MOD;
        return (int) ans;
    }
}
```