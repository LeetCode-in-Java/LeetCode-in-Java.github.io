[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1397\. Find All Good Strings

Hard

Given the strings `s1` and `s2` of size `n` and the string `evil`, return _the number of **good** strings_.

A **good** string has size `n`, it is alphabetically greater than or equal to `s1`, it is alphabetically smaller than or equal to `s2`, and it does not contain the string `evil` as a substring. Since the answer can be a huge number, return this **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** n = 2, s1 = "aa", s2 = "da", evil = "b"

**Output:** 51

**Explanation:** There are 25 good strings starting with 'a': "aa","ac","ad",...,"az". Then there are 25 good strings starting with 'c': "ca","cc","cd",...,"cz" and finally there is one good string starting with 'd': "da".

**Example 2:**

**Input:** n = 8, s1 = "leetcode", s2 = "leetgoes", evil = "leet"

**Output:** 0

**Explanation:** All strings greater than or equal to s1 and smaller than or equal to s2 start with the prefix "leet", therefore, there is not any good string.

**Example 3:**

**Input:** n = 2, s1 = "gx", s2 = "gz", evil = "x"

**Output:** 2

**Constraints:**

*   `s1.length == n`
*   `s2.length == n`
*   `s1 <= s2`
*   `1 <= n <= 500`
*   `1 <= evil.length <= 50`
*   All strings consist of lowercase English letters.

## Solution

```java
@SuppressWarnings("java:S1172")
public class Solution {
    private int mod = 1_000_000_007;
    private int[] next;

    public int findGoodStrings(int n, String s1, String s2, String evil) {
        char[] s1arr = s1.toCharArray();
        for (int i = s1.length() - 1; i >= 0; i--) {
            if (s1arr[i] > 'a') {
                s1arr[i] = (char) (s1arr[i] - 1);
                break;
            } else {
                s1arr[i] = 'z';
            }
        }
        s1 = new String(s1arr);
        next = getNext(evil);
        if (s1.compareTo(s2) > 0) {
            return lessOrEqualThan(s2, evil);
        }
        return (lessOrEqualThan(s2, evil) - lessOrEqualThan(s1, evil) + mod) % mod;
    }

    private int lessOrEqualThan(String s, String e) {
        long[][][] dp = new long[s.length() + 1][e.length() + 1][2];
        dp[0][0][1] = 1;
        long res = 0;
        for (int i = 0; i < s.length(); i++) {
            for (int state = 0; state < e.length(); state++) {
                for (char c = 'a'; c <= 'z'; c++) {
                    int nextstate = getNextState(state, c, e);
                    dp[i + 1][nextstate][0] = (dp[i + 1][nextstate][0] + dp[i][state][0]) % mod;
                }
                for (char c = 'a'; c < s.charAt(i); c++) {
                    int nextstate = getNextState(state, c, e);
                    dp[i + 1][nextstate][0] = (dp[i + 1][nextstate][0] + dp[i][state][1]) % mod;
                }
                int nextstate = getNextState(state, s.charAt(i), e);
                dp[i + 1][nextstate][1] = (dp[i + 1][nextstate][1] + dp[i][state][1]) % mod;
            }
        }
        for (int i = 0; i < e.length(); i++) {
            res = (res + dp[s.length()][i][0]) % mod;
            res = (res + dp[s.length()][i][1]) % mod;
        }
        return (int) res;
    }

    private int getNextState(int prevState, char nextChar, String evil) {
        int idx = prevState;
        while (idx != -1 && evil.charAt(idx) != nextChar) {
            idx = next[idx];
        }
        return idx + 1;
    }

    private int[] getNext(String e) {
        int len = e.length();
        int[] localNext = new int[len];
        localNext[0] = -1;
        int last = -1;
        int i = 0;
        while (i < len - 1) {
            if (last == -1 || e.charAt(i) == e.charAt(last)) {
                i++;
                last++;
                localNext[i] = last;
            } else {
                last = localNext[last];
            }
        }
        return localNext;
    }
}
```