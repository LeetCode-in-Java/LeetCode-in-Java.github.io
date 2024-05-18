[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3144\. Minimum Substring Partition of Equal Character Frequency

Medium

Given a string `s`, you need to partition it into one or more **balanced** substrings. For example, if `s == "ababcc"` then `("abab", "c", "c")`, `("ab", "abc", "c")`, and `("ababcc")` are all valid partitions, but <code>("a", **"bab"**, "cc")</code>, <code>(**"aba"**, "bc", "c")</code>, and <code>("ab", **"abcc"**)</code> are not. The unbalanced substrings are bolded.

Return the **minimum** number of substrings that you can partition `s` into.

**Note:** A **balanced** string is a string where each character in the string occurs the same number of times.

**Example 1:**

**Input:** s = "fabccddg"

**Output:** 3

**Explanation:**

We can partition the string `s` into 3 substrings in one of the following ways: `("fab, "ccdd", "g")`, or `("fabc", "cd", "dg")`.

**Example 2:**

**Input:** s = "abababaccddb"

**Output:** 2

**Explanation:**

We can partition the string `s` into 2 substrings like so: `("abab", "abaccddb")`.

**Constraints:**

*   `1 <= s.length <= 1000`
*   `s` consists only of English lowercase letters.

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int minimumSubstringsInPartition(String s) {
        char[] cs = s.toCharArray();
        int n = cs.length;
        int[] dp = new int[n + 1];
        Arrays.fill(dp, n);
        dp[0] = 0;
        for (int i = 1; i <= n; ++i) {
            int[] count = new int[26];
            int distinct = 0;
            int maxCount = 0;
            for (int j = i - 1; j >= 0; --j) {
                int index = cs[j] - 'a';
                if (++count[index] == 1) {
                    distinct++;
                }
                if (count[index] > maxCount) {
                    maxCount = count[index];
                }
                if (maxCount * distinct == i - j) {
                    dp[i] = Math.min(dp[i], dp[j] + 1);
                }
            }
        }
        return dp[n];
    }
}
```