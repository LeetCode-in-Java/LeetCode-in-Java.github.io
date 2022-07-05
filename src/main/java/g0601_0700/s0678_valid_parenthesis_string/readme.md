[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 678\. Valid Parenthesis String

Medium

Given a string `s` containing only three types of characters: `'('`, `')'` and `'*'`, return `true` _if_ `s` _is **valid**_.

The following rules define a **valid** string:

*   Any left parenthesis `'('` must have a corresponding right parenthesis `')'`.
*   Any right parenthesis `')'` must have a corresponding left parenthesis `'('`.
*   Left parenthesis `'('` must go before the corresponding right parenthesis `')'`.
*   `'*'` could be treated as a single right parenthesis `')'` or a single left parenthesis `'('` or an empty string `""`.

**Example 1:**

**Input:** s = "()"

**Output:** true

**Example 2:**

**Input:** s = "(\*)"

**Output:** true

**Example 3:**

**Input:** s = "(\*))"

**Output:** true

**Constraints:**

*   `1 <= s.length <= 100`
*   `s[i]` is `'('`, `')'` or `'*'`.

## Solution

```java
public class Solution {
    public boolean checkValidString(String s) {
        int lo = 0;
        int hi = 0;
        for (int i = 0; i < s.length(); i++) {
            lo += s.charAt(i) == '(' ? 1 : -1;
            hi += s.charAt(i) != ')' ? 1 : -1;
            if (hi < 0) {
                break;
            }
            lo = Math.max(0, lo);
        }
        return lo == 0;
    }
}
```