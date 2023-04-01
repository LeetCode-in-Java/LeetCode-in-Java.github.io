[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2124\. Check if All A's Appears Before All B's

Easy

Given a string `s` consisting of **only** the characters `'a'` and `'b'`, return `true` _if **every**_ `'a'` _appears before **every**_ `'b'` _in the string_. Otherwise, return `false`.

**Example 1:**

**Input:** s = "aaabbb"

**Output:** true

**Explanation:** 

The 'a's are at indices 0, 1, and 2, while the 'b's are at indices 3, 4, and 5. 

Hence, every 'a' appears before every 'b' and we return true.

**Example 2:**

**Input:** s = "abab"

**Output:** false

**Explanation:** 

There is an 'a' at index 2 and a 'b' at index 1. 

Hence, not every 'a' appears before every 'b' and we return false.

**Example 3:**

**Input:** s = "bbb"

**Output:** true

**Explanation:** There are no 'a's, hence, every 'a' appears before every 'b' and we return true.

**Constraints:**

*   `1 <= s.length <= 100`
*   `s[i]` is either `'a'` or `'b'`.

## Solution

```java
public class Solution {
    public boolean checkString(String s) {
        int aEndIndex = -1;
        int bStartIndex = -1;
        if (s.length() == 1) {
            return true;
        }
        for (int i = s.length() - 1; i >= 0; i--) {
            if (s.charAt(i) == 'a') {
                aEndIndex = i;
                break;
            }
        }
        for (int i = 0; i <= s.length() - 1; i++) {
            if (s.charAt(i) == 'b') {
                bStartIndex = i;
                break;
            }
        }
        if (aEndIndex == -1 || bStartIndex == -1) {
            return true;
        }
        return bStartIndex > aEndIndex;
    }
}
```