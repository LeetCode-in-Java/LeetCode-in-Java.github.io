[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 214\. Shortest Palindrome

Hard

You are given a string `s`. You can convert `s` to a palindrome by adding characters in front of it.

Return _the shortest palindrome you can find by performing this transformation_.

**Example 1:**

**Input:** s = "aacecaaa"

**Output:** "aaacecaaa" 

**Example 2:**

**Input:** s = "abcd"

**Output:** "dcbabcd" 

**Constraints:**

*   <code>0 <= s.length <= 5 * 10<sup>4</sup></code>
*   `s` consists of lowercase English letters only.

## Solution

```java
public class Solution {
    public String shortestPalindrome(String s) {
        int i;
        int x;
        int diff;
        int n = s.length();
        int m = (n << 1) + 1;
        char[] letters = new char[m];
        for (i = 0; i < n; i++) {
            letters[i] = letters[m - 1 - i] = s.charAt(i);
        }
        letters[i] = '#';
        int[] lps = new int[m];
        lps[0] = 0;
        for (i = 1; i < m; i++) {
            x = lps[i - 1];
            while (letters[i] != letters[x]) {
                if (x == 0) {
                    x = -1;
                    break;
                }
                x = lps[x - 1];
            }
            lps[i] = x + 1;
        }
        diff = n - lps[m - 1];
        if (diff == 0) {
            return s;
        } else {
            StringBuilder builder = new StringBuilder();
            for (i = n - 1; i >= n - diff; i--) {
                builder.append(s.charAt(i));
            }
            builder.append(s);
            return builder.toString();
        }
    }
}
```