[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3757\. Number of Effective Subsequences

Hard

You are given an integer array `nums`.

The **strength** of the array is defined as the **bitwise OR** of all its elements.

A **subsequence** is considered **effective** if removing that subsequence **strictly decreases** the strength of the remaining elements.

Return the number of **effective subsequences** in `nums`. Since the answer may be large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

The bitwise OR of an empty array is 0.

**Example 1:**

**Input:** nums = [1,2,3]

**Output:** 3

**Explanation:**

*   The Bitwise OR of the array is `1 OR 2 OR 3 = 3`.
*   Subsequences that are effective are:
    *   `[1, 3]`: The remaining element `[2]` has a Bitwise OR of 2.
    *   `[2, 3]`: The remaining element `[1]` has a Bitwise OR of 1.
    *   `[1, 2, 3]`: The remaining elements `[]` have a Bitwise OR of 0.
*   Thus, the total number of effective subsequences is 3.

**Example 2:**

**Input:** nums = [7,4,6]

**Output:** 4

**Explanation:**

*   The Bitwise OR of the array is `7 OR 4 OR 6 = 7`.
*   Subsequences that are effective are:
    *   `[7]`: The remaining elements `[4, 6]` have a Bitwise OR of 6.
    *   `[7, 4]`: The remaining element `[6]` has a Bitwise OR of 6.
    *   `[7, 6]`: The remaining element `[4]` has a Bitwise OR of 4.
    *   `[7, 4, 6]`: The remaining elements `[]` have a Bitwise OR of 0.
*   Thus, the total number of effective subsequences is 4.

**Example 3:**

**Input:** nums = [8,8]

**Output:** 1

**Explanation:**

*   The Bitwise OR of the array is `8 OR 8 = 8`.
*   Only the subsequence `[8, 8]` is effective since removing it leaves `[]` which has a Bitwise OR of 0.
*   Thus, the total number of effective subsequences is 1.

**Example 4:**

**Input:** nums = [2,2,1]

**Output:** 5

**Explanation:**

*   The Bitwise OR of the array is `2 OR 2 OR 1 = 3`.
*   Subsequences that are effective are:
    *   `[1]`: The remaining elements `[2, 2]` have a Bitwise OR of 2.
    *   `[2, 1]` (using `nums[0]`, `nums[2]`): The remaining element `[2]` has a Bitwise OR of 2.
    *   `[2, 1]` (using `nums[1]`, `nums[2]`): The remaining element `[2]` has a Bitwise OR of 2.
    *   `[2, 2]`: The remaining element `[1]` has a Bitwise OR of 1.
    *   `[2, 2, 1]`: The remaining elements `[]` have a Bitwise OR of 0.
*   Thus, the total number of effective subsequences is 5.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>6</sup></code>

## Solution

```java
public class Solution {
    private static final int MOD = 1_000_000_007;

    public int countEffective(int[] nums) {
        int n = nums.length;
        int t = 0;
        for (int v : nums) {
            t |= v;
        }
        if (t == 0) {
            return 0;
        }
        int[] bits = new int[20];
        int m = 0;
        for (int b = 0; b < 20; ++b) {
            if (((t >> b) & 1) != 0) {
                bits[m++] = b;
            }
        }
        int s = 1 << m;
        int[] freq = new int[s];
        for (int v : nums) {
            int m1 = 0;
            for (int j = 0; j < m; ++j) {
                if (((v >> bits[j]) & 1) != 0) {
                    m1 |= 1 << j;
                }
            }
            freq[m1]++;
        }
        int[] f = new int[s];
        System.arraycopy(freq, 0, f, 0, s);
        for (int i = 0; i < m; ++i) {
            for (int mask = 0; mask < s; ++mask) {
                if ((mask & (1 << i)) != 0) {
                    f[mask] += f[mask ^ (1 << i)];
                }
            }
        }
        long[] p2 = new long[n + 1];
        p2[0] = 1;
        for (int i = 1; i <= n; ++i) {
            p2[i] = (p2[i - 1] << 1) % MOD;
        }
        long ans = 0;
        int all = s - 1;
        for (int bmask = 1; bmask < s; ++bmask) {
            int comp = all ^ bmask;
            int cnt = f[comp];
            long add = p2[cnt];
            if (Integer.bitCount(bmask) % 2 == 1) {
                ans = (ans + add) % MOD;
            } else {
                ans = (ans - add) % MOD;
            }
        }
        ans = (ans % MOD + MOD) % MOD;
        return (int) ans;
    }
}
```