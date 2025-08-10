[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3628\. Maximum Number of Subsequences After One Inserting

Medium

You are given a string `s` consisting of uppercase English letters.

You are allowed to insert **at most one** uppercase English letter at **any** position (including the beginning or end) of the string.

Return the **maximum** number of `"LCT"` subsequences that can be formed in the resulting string after **at most one insertion**.

**Example 1:**

**Input:** s = "LMCT"

**Output:** 2

**Explanation:**

We can insert a `"L"` at the beginning of the string s to make `"LLMCT"`, which has 2 subsequences, at indices [0, 3, 4] and [1, 3, 4].

**Example 2:**

**Input:** s = "LCCT"

**Output:** 4

**Explanation:**

We can insert a `"L"` at the beginning of the string s to make `"LLCCT"`, which has 4 subsequences, at indices [0, 2, 4], [0, 3, 4], [1, 2, 4] and [1, 3, 4].

**Example 3:**

**Input:** s = "L"

**Output:** 0

**Explanation:**

Since it is not possible to obtain the subsequence `"LCT"` by inserting a single letter, the result is 0.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` consists of uppercase English letters.

## Solution

```java
public class Solution {
    public long numOfSubsequences(String s) {
        long tc = 0;
        char[] chs = s.toCharArray();
        for (char c : chs) {
            tc += (c == 'T') ? 1 : 0;
        }
        long ls = 0;
        long cs = 0;
        long lcf = 0;
        long ctf = 0;
        long lct = 0;
        long ocg = 0;
        long tp = 0;
        for (char curr : chs) {
            long rt = tc - tp;
            long cg = ls * rt;
            ocg = (cg > ocg) ? cg : ocg;
            if (curr == 'L') {
                ls++;
            } else {
                if (curr == 'C') {
                    cs++;
                    lcf += ls;
                } else {
                    if (curr == 'T') {
                        lct += lcf;
                        ctf += cs;
                        tp++;
                    }
                }
            }
        }
        long fcg = ls * (tc - tp);
        ocg = fcg > ocg ? fcg : ocg;
        long maxi = 0;
        long[] bo = {lcf, ctf, ocg};
        for (long op : bo) {
            maxi = op > maxi ? op : maxi;
        }
        return lct + maxi;
    }
}
```