[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 686\. Repeated String Match

Medium

Given two strings `a` and `b`, return _the minimum number of times you should repeat string_ `a` _so that string_ `b` _is a substring of it_. If it is impossible for `b` to be a substring of `a` after repeating it, return `-1`.

**Notice:** string `"abc"` repeated 0 times is `""`, repeated 1 time is `"abc"` and repeated 2 times is `"abcabc"`.

**Example 1:**

**Input:** a = "abcd", b = "cdabcdab"

**Output:** 3

**Explanation:** We return 3 because by repeating a three times "ab**cdabcdab**cd", b is a substring of it.

**Example 2:**

**Input:** a = "a", b = "aa"

**Output:** 2

**Constraints:**

*   <code>1 <= a.length, b.length <= 10<sup>4</sup></code>
*   `a` and `b` consist of lowercase English letters.

## Solution

```java
public class Solution {
    public int repeatedStringMatch(String a, String b) {
        char[] existsChar = new char[127];
        for (char chA : a.toCharArray()) {
            existsChar[chA] = 1;
        }
        for (char chB : b.toCharArray()) {
            if (existsChar[chB] < 1) {
                return -1;
            }
        }

        int lenB = b.length() - 1;
        StringBuilder sb = new StringBuilder(a);
        int lenSbA = sb.length() - 1;
        int repeatCount = 1;
        while (lenSbA < lenB) {
            sb.append(a);
            repeatCount++;
            lenSbA = sb.length() - 1;
        }
        if (!isFound(sb, b)) {
            sb.append(a);
            repeatCount++;
            return !isFound(sb, b) ? -1 : repeatCount;
        }
        return repeatCount;
    }

    private boolean isFound(StringBuilder a, String b) {
        for (int i = 0; i < a.length(); i++) {
            int k = i;
            int m = 0;
            while (k < a.length() && m < b.length()) {
                if (a.charAt(k) != b.charAt(m)) {
                    break;
                } else {
                    k++;
                    m++;
                }
            }
            if (m == b.length()) {
                return true;
            }
        }
        return false;
    }
}
```