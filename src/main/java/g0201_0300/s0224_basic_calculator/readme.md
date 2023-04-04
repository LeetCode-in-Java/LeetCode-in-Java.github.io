[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 224\. Basic Calculator

Hard

Given a string `s` representing a valid expression, implement a basic calculator to evaluate it, and return _the result of the evaluation_.

**Note:** You are **not** allowed to use any built-in function which evaluates strings as mathematical expressions, such as `eval()`.

**Example 1:**

**Input:** s = "1 + 1"

**Output:** 2 

**Example 2:**

**Input:** s = " 2-1 + 2 "

**Output:** 3 

**Example 3:**

**Input:** s = "(1+(4+5+2)-3)+(6+8)"

**Output:** 23 

**Constraints:**

*   <code>1 <= s.length <= 3 * 10<sup>5</sup></code>
*   `s` consists of digits, `'+'`, `'-'`, `'('`, `')'`, and `' '`.
*   `s` represents a valid expression.
*   `'+'` is **not** used as a unary operation (i.e., `"+1"` and `"+(2 + 3)"` is invalid).
*   `'-'` could be used as a unary operation (i.e., `"-1"` and `"-(2 + 3)"` is valid).
*   There will be no two consecutive operators in the input.
*   Every number and running calculation will fit in a signed 32-bit integer.

## Solution

```java
public class Solution {
    private int i = 0;

    public int calculate(String s) {
        char[] ca = s.toCharArray();
        return helper(ca);
    }

    private int helper(char[] ca) {
        int num = 0;
        int prenum = 0;
        boolean isPlus = true;
        for (; i < ca.length; i++) {
            char c = ca[i];
            if (c != ' ') {
                if (c >= '0' && c <= '9') {
                    if (num == 0) {
                        num = (c - '0');
                    } else {
                        num = num * 10 + c - '0';
                    }
                } else if (c == '+') {
                    prenum += num * (isPlus ? 1 : -1);
                    isPlus = true;
                    num = 0;
                } else if (c == '-') {
                    prenum += num * (isPlus ? 1 : -1);
                    num = 0;
                    isPlus = false;
                } else if (c == '(') {
                    i++;
                    prenum += helper(ca) * (isPlus ? 1 : -1);
                    isPlus = true;
                    num = 0;
                } else if (c == ')') {
                    return prenum + num * (isPlus ? 1 : -1);
                }
            }
        }
        return prenum + num * (isPlus ? 1 : -1);
    }
}
```