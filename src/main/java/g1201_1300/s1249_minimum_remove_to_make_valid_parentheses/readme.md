[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1249\. Minimum Remove to Make Valid Parentheses

Medium

Given a string s of `'('` , `')'` and lowercase English characters.

Your task is to remove the minimum number of parentheses ( `'('` or `')'`, in any positions ) so that the resulting _parentheses string_ is valid and return **any** valid string.

Formally, a _parentheses string_ is valid if and only if:

*   It is the empty string, contains only lowercase characters, or
*   It can be written as `AB` (`A` concatenated with `B`), where `A` and `B` are valid strings, or
*   It can be written as `(A)`, where `A` is a valid string.

**Example 1:**

**Input:** s = "lee(t(c)o)de)"

**Output:** "lee(t(c)o)de"

**Explanation:** "lee(t(co)de)" , "lee(t(c)ode)" would also be accepted.

**Example 2:**

**Input:** s = "a)b(c)d"

**Output:** "ab(c)d"

**Example 3:**

**Input:** s = "))(("

**Output:** ""

**Explanation:** An empty string is also valid.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s[i]` is either`'('` , `')'`, or lowercase English letter`.`

## Solution

```java
public class Solution {
    public String minRemoveToMakeValid(String s) {
        int closingParantheis = 0;
        for (char ch : s.toCharArray()) {
            if (ch == ')') {
                closingParantheis++;
            }
        }
        StringBuilder result = new StringBuilder();
        int openingParanthesis = 0;
        for (char ch : s.toCharArray()) {
            if (ch == ')' && openingParanthesis == 0) {
                closingParantheis--;
            } else {
                if (ch == ')') {
                    openingParanthesis--;
                }
                if (ch == '(' && closingParantheis == 0) {
                    continue;
                }
                if (ch == '(') {
                    openingParanthesis++;
                    closingParantheis--;
                }
                result.append(ch);
            }
        }
        return result.toString();
    }
}
```