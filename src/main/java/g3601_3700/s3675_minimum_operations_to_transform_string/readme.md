[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3675\. Minimum Operations to Transform String

Medium

You are given a string `s` consisting only of lowercase English letters.

You can perform the following operation any number of times (including zero):

*   Choose any character `c` in the string and replace **every** occurrence of `c` with the **next** lowercase letter in the English alphabet.


Return the **minimum** number of operations required to transform `s` into a string consisting of **only** `'a'` characters.

**Note:** Consider the alphabet as circular, thus `'a'` comes after `'z'`.

**Example 1:**

**Input:** s = "yz"

**Output:** 2

**Explanation:**

*   Change `'y'` to `'z'` to get `"zz"`.
*   Change `'z'` to `'a'` to get `"aa"`.
*   Thus, the answer is 2.

**Example 2:**

**Input:** s = "a"

**Output:** 0

**Explanation:**

*   The string `"a"` only consists of `'a'` characters. Thus, the answer is 0.

**Constraints:**

*   <code>1 <= s.length <= 5 * 10<sup>5</sup></code>
*   `s` consists only of lowercase English letters.

## Solution

```java
public class Solution {
    public int minOperations(String s) {
        int n = s.length();
        int ans = 0;
        for (int i = 0; i < n; i++) {
            final char c = s.charAt(i);
            if (c != 'a') {
                int ops = 'z' - c + 1;
                if (ops > ans) {
                    ans = ops;
                }
                if (ops == 25) {
                    break;
                }
            }
        }
        return ans;
    }
}
```