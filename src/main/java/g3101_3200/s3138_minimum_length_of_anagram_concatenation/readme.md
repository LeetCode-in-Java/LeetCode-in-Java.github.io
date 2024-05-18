[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3138\. Minimum Length of Anagram Concatenation

Medium

You are given a string `s`, which is known to be a concatenation of **anagrams** of some string `t`.

Return the **minimum** possible length of the string `t`.

An **anagram** is formed by rearranging the letters of a string. For example, "aab", "aba", and, "baa" are anagrams of "aab".

**Example 1:**

**Input:** s = "abba"

**Output:** 2

**Explanation:**

One possible string `t` could be `"ba"`.

**Example 2:**

**Input:** s = "cdef"

**Output:** 4

**Explanation:**

One possible string `t` could be `"cdef"`, notice that `t` can be equal to `s`.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` consist only of lowercase English letters.

## Solution

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Solution {
    public int minAnagramLength(String s) {
        int n = s.length();
        int[] sq = new int[n];
        for (int i = 0; i < s.length(); i++) {
            int ch = s.charAt(i);
            if (i == 0) {
                sq[i] = ch * ch;
            } else {
                sq[i] = sq[i - 1] + ch * ch;
            }
        }
        List<Integer> factors = getAllFactorsVer2(n);
        Collections.sort(factors);
        for (int j = 0; j < factors.size(); j++) {
            int factor = factors.get(j);
            if (factor == 1) {
                if (sq[0] * n == sq[n - 1]) {
                    return 1;
                }
            } else {
                int sum = sq[factor - 1];
                int start = 0;
                for (int i = factor - 1; i < n; i += factor) {
                    if (start + sum != sq[i]) {
                        break;
                    }
                    start += sum;
                    if (i == n - 1) {
                        return factor;
                    }
                }
            }
        }
        return n - 1;
    }

    private List<Integer> getAllFactorsVer2(int n) {
        List<Integer> factors = new ArrayList<>();
        for (int i = 1; i <= Math.sqrt(n); i++) {
            if (n % i == 0) {
                factors.add(i);
                factors.add(n / i);
            }
        }
        return factors;
    }
}
```