[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3333\. Find the Original Typed String II

Hard

Alice is attempting to type a specific string on her computer. However, she tends to be clumsy and **may** press a key for too long, resulting in a character being typed **multiple** times.

You are given a string `word`, which represents the **final** output displayed on Alice's screen. You are also given a **positive** integer `k`.

Return the total number of _possible_ original strings that Alice _might_ have intended to type, if she was trying to type a string of size **at least** `k`.

Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** word = "aabbccdd", k = 7

**Output:** 5

**Explanation:**

The possible strings are: `"aabbccdd"`, `"aabbccd"`, `"aabbcdd"`, `"aabccdd"`, and `"abbccdd"`.

**Example 2:**

**Input:** word = "aabbccdd", k = 8

**Output:** 1

**Explanation:**

The only possible string is `"aabbccdd"`.

**Example 3:**

**Input:** word = "aaabbb", k = 3

**Output:** 8

**Constraints:**

*   <code>1 <= word.length <= 5 * 10<sup>5</sup></code>
*   `word` consists only of lowercase English letters.
*   `1 <= k <= 2000`

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    private static final long MOD = (long) 1e9 + 7;

    public int possibleStringCount(String word, int k) {
        List<Integer> list = new ArrayList<>();
        int n = word.length();
        int i = 0;
        while (i < n) {
            int j = i + 1;
            while (j < n && word.charAt(j) == word.charAt(j - 1)) {
                j++;
            }
            list.add(j - i);
            i = j;
        }
        int m = list.size();
        long[] power = new long[m];
        power[m - 1] = list.get(m - 1);
        for (i = m - 2; i >= 0; i--) {
            power[i] = (power[i + 1] * list.get(i)) % MOD;
        }
        if (m >= k) {
            return (int) power[0];
        }
        long[][] dp = new long[m][k - m + 1];
        for (i = 0; i < k - m + 1; i++) {
            if (list.get(m - 1) + i + m > k) {
                dp[m - 1][i] = list.get(m - 1) - (long) (k - m - i);
            }
        }
        for (i = m - 2; i >= 0; i--) {
            long sum = (dp[i + 1][k - m] * list.get(i)) % MOD;
            for (int j = k - m; j >= 0; j--) {
                sum += dp[i + 1][j];
                if (j + list.get(i) > k - m) {
                    sum = (sum - dp[i + 1][k - m] + MOD) % MOD;
                } else {
                    sum = (sum - dp[i + 1][j + list.get(i)] + MOD) % MOD;
                }
                dp[i][j] = sum;
            }
        }
        return (int) dp[0][0];
    }
}
```