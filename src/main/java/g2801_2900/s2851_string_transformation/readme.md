[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2851\. String Transformation

Hard

You are given two strings `s` and `t` of equal length `n`. You can perform the following operation on the string `s`:

*   Remove a **suffix** of `s` of length `l` where `0 < l < n` and append it at the start of `s`.   
    For example, let `s = 'abcd'` then in one operation you can remove the suffix `'cd'` and append it in front of `s` making `s = 'cdab'`.

You are also given an integer `k`. Return _the number of ways in which_ `s` _can be transformed into_ `t` _in **exactly**_ `k` _operations._

Since the answer can be large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** s = "abcd", t = "cdab", k = 2

**Output:** 2

**Explanation:** 

First way: 

In first operation, choose suffix from index = 3, so resulting s = "dabc". 

In second operation, choose suffix from index = 3, so resulting s = "cdab". 

Second way: 

In first operation, choose suffix from index = 1, so resulting s = "bcda".

In second operation, choose suffix from index = 1, so resulting s = "cdab".

**Example 2:**

**Input:** s = "ababab", t = "ababab", k = 1

**Output:** 2

**Explanation:** 

First way: 

Choose suffix from index = 2, so resulting s = "ababab". 

Second way: 

Choose suffix from index = 4, so resulting s = "ababab".

**Constraints:**

*   <code>2 <= s.length <= 5 * 10<sup>5</sup></code>
*   <code>1 <= k <= 10<sup>15</sup></code>
*   `s.length == t.length`
*   `s` and `t` consist of only lowercase English alphabets.

## Solution

```java
public class Solution {
    private long[][] g;

    public int numberOfWays(String s, String t, long k) {
        int n = s.length();
        int v = kmp(s + s, t);
        g = new long[][] { {v - 1, v}, {n - v, n - 1 - v}};
        long[][] f = qmi(k);
        return s.equals(t) ? (int) f[0][0] : (int) f[0][1];
    }

    private int kmp(String s, String p) {
        int n = p.length();
        int m = s.length();
        s = "#" + s;
        p = "#" + p;
        int[] ne = new int[n + 1];
        int j = 0;
        for (int i = 2; i <= n; i++) {
            while (j > 0 && p.charAt(i) != p.charAt(j + 1)) {
                j = ne[j];
            }
            if (p.charAt(i) == p.charAt(j + 1)) {
                j++;
            }
            ne[i] = j;
        }

        int cnt = 0;
        j = 0;
        for (int i = 1; i <= m; i++) {
            while (j > 0 && s.charAt(i) != p.charAt(j + 1)) {
                j = ne[j];
            }
            if (s.charAt(i) == p.charAt(j + 1)) {
                j++;
            }
            if (j == n) {
                if (i - n + 1 <= n) {
                    cnt++;
                }
                j = ne[j];
            }
        }
        return cnt;
    }

    private void mul(long[][] c, long[][] a, long[][] b) {
        long[][] t = new long[2][2];
        for (int i = 0; i < 2; i++) {
            for (int j = 0; j < 2; j++) {
                for (int k = 0; k < 2; k++) {
                    int mod = (int) 1e9 + 7;
                    t[i][j] = (t[i][j] + a[i][k] * b[k][j]) % mod;
                }
            }
        }
        for (int i = 0; i < 2; i++) {
            System.arraycopy(t[i], 0, c[i], 0, 2);
        }
    }

    private long[][] qmi(long k) {
        long[][] f = new long[2][2];
        f[0][0] = 1;
        while (k > 0) {
            if ((k & 1) == 1) {
                mul(f, f, g);
            }
            mul(g, g, g);
            k >>= 1;
        }
        return f;
    }
}
```