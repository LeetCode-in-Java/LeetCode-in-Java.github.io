[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3352\. Count K-Reducible Numbers Less Than N

Hard

You are given a **binary** string `s` representing a number `n` in its binary form.

You are also given an integer `k`.

An integer `x` is called **k-reducible** if performing the following operation **at most** `k` times reduces it to 1:

*   Replace `x` with the **count** of set bits in its binary representation.

For example, the binary representation of 6 is `"110"`. Applying the operation once reduces it to 2 (since `"110"` has two set bits). Applying the operation again to 2 (binary `"10"`) reduces it to 1 (since `"10"` has one set bit).

Return an integer denoting the number of positive integers **less** than `n` that are **k-reducible**.

Since the answer may be too large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** s = "111", k = 1

**Output:** 3

**Explanation:**

`n = 7`. The 1-reducible integers less than 7 are 1, 2, and 4.

**Example 2:**

**Input:** s = "1000", k = 2

**Output:** 6

**Explanation:**

`n = 8`. The 2-reducible integers less than 8 are 1, 2, 3, 4, 5, and 6.

**Example 3:**

**Input:** s = "1", k = 3

**Output:** 0

**Explanation:**

There are no positive integers less than `n = 1`, so the answer is 0.

**Constraints:**

*   `1 <= s.length <= 800`
*   `s` has no leading zeros.
*   `s` consists only of the characters `'0'` and `'1'`.
*   `1 <= k <= 5`

## Solution

```java
public class Solution {
    private static final int MOD = (int) (1e9 + 7);

    public int countKReducibleNumbers(String s, int k) {
        int n = s.length();
        int[] reducible = new int[n + 1];
        for (int i = 2; i < reducible.length; i++) {
            reducible[i] = 1 + reducible[Integer.bitCount(i)];
        }
        long[] dp = new long[n + 1];
        int curr = 0;
        for (int i = 0; i < n; i++) {
            for (int j = i - 1; j >= 0; j--) {
                dp[j + 1] += dp[j];
                dp[j + 1] %= MOD;
            }
            if (s.charAt(i) == '1') {
                dp[curr]++;
                dp[curr] %= MOD;
                curr++;
            }
        }
        long result = 0;
        for (int i = 1; i <= s.length(); i++) {
            if (reducible[i] < k) {
                result += dp[i];
                result %= MOD;
            }
        }
        return (int) (result % MOD);
    }
}
```