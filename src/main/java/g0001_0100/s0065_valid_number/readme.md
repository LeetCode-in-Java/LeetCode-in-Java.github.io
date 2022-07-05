[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 65\. Valid Number

Hard

A **valid number** can be split up into these components (in order):

1.  A **decimal number** or an **integer**.
2.  (Optional) An `'e'` or `'E'`, followed by an **integer**.

A **decimal number** can be split up into these components (in order):

1.  (Optional) A sign character (either `'+'` or `'-'`).
2.  One of the following formats:
    1.  One or more digits, followed by a dot `'.'`.
    2.  One or more digits, followed by a dot `'.'`, followed by one or more digits.
    3.  A dot `'.'`, followed by one or more digits.

An **integer** can be split up into these components (in order):

1.  (Optional) A sign character (either `'+'` or `'-'`).
2.  One or more digits.

For example, all the following are valid numbers: `["2", "0089", "-0.1", "+3.14", "4.", "-.9", "2e10", "-90E3", "3e+7", "+6e-1", "53.5e93", "-123.456e789"]`, while the following are not valid numbers: `["abc", "1a", "1e", "e3", "99e2.5", "--6", "-+3", "95a54e53"]`.

Given a string `s`, return `true` _if_ `s` _is a **valid number**_.

**Example 1:**

**Input:** s = "0"

**Output:** true 

**Example 2:**

**Input:** s = "e"

**Output:** false 

**Example 3:**

**Input:** s = "."

**Output:** false 

**Example 4:**

**Input:** s = ".1"

**Output:** true 

**Constraints:**

*   `1 <= s.length <= 20`
*   `s` consists of only English letters (both uppercase and lowercase), digits (`0-9`), plus `'+'`, minus `'-'`, or dot `'.'`.

## Solution

```java
public class Solution {
    public boolean isNumber(String s) {
        if (s == null || s.length() == 0) {
            return false;
        }
        boolean eSeen = false;
        boolean numberSeen = false;
        boolean decimalSeen = false;
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if (c >= 48 && c <= 57) {
                numberSeen = true;
            } else if (c == '+' || c == '-') {
                if (i == s.length() - 1
                        || (i != 0 && s.charAt(i - 1) != 'e' && s.charAt(i - 1) != 'E')) {
                    return false;
                }
            } else if (c == '.') {
                if (eSeen || decimalSeen) {
                    return false;
                }
                decimalSeen = true;
            } else if (c == 'e' || c == 'E') {
                if (i == s.length() - 1 || eSeen || !numberSeen) {
                    return false;
                }
                eSeen = true;
            } else {
                return false;
            }
        }
        return numberSeen;
    }
}
```