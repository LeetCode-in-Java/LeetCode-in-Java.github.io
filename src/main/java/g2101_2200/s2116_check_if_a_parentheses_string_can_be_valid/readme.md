[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2116\. Check if a Parentheses String Can Be Valid

Medium

A parentheses string is a **non-empty** string consisting only of `'('` and `')'`. It is valid if **any** of the following conditions is **true**:

*   It is `()`.
*   It can be written as `AB` (`A` concatenated with `B`), where `A` and `B` are valid parentheses strings.
*   It can be written as `(A)`, where `A` is a valid parentheses string.

You are given a parentheses string `s` and a string `locked`, both of length `n`. `locked` is a binary string consisting only of `'0'`s and `'1'`s. For **each** index `i` of `locked`,

*   If `locked[i]` is `'1'`, you **cannot** change `s[i]`.
*   But if `locked[i]` is `'0'`, you **can** change `s[i]` to either `'('` or `')'`.

Return `true` _if you can make `s` a valid parentheses string_. Otherwise, return `false`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/11/06/eg1.png)

**Input:** s = "))()))", locked = "010100"

**Output:** true

**Explanation:** locked[1] == '1' and locked[3] == '1', so we cannot change s[1] or s[3]. We change s[0] and s[4] to '(' while leaving s[2] and s[5] unchanged to make s valid.

**Example 2:**

**Input:** s = "()()", locked = "0000"

**Output:** true

**Explanation:** We do not need to make any changes because s is already valid.

**Example 3:**

**Input:** s = ")", locked = "0"

**Output:** false

**Explanation:** locked permits us to change s[0]. Changing s[0] to either '(' or ')' will not make s valid.

**Constraints:**

*   `n == s.length == locked.length`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   `s[i]` is either `'('` or `')'`.
*   `locked[i]` is either `'0'` or `'1'`.

## Solution

```java
public class Solution {
    public boolean canBeValid(String s, String locked) {
        if (s == null || s.isEmpty()) {
            return true;
        }
        if ((s.length() & 1) > 0) {
            return false;
        }
        if (locked == null || locked.isEmpty()) {
            return true;
        }
        int numOfLockedClose = 0;
        int numOfLockedOpen = 0;
        for (int i = 0; i < s.length(); i++) {
            int countOfChars = i + 1;
            if (s.charAt(i) == ')' && locked.charAt(i) == '1') {
                numOfLockedClose++;
                if (numOfLockedClose * 2 > countOfChars) {
                    return false;
                }
            }
            int j = s.length() - 1 - i;
            if (s.charAt(j) == '(' && locked.charAt(j) == '1') {
                numOfLockedOpen++;

                if (numOfLockedOpen * 2 > countOfChars) {
                    return false;
                }
            }
        }
        return true;
    }
}
```