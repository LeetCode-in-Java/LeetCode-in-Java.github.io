[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1190\. Reverse Substrings Between Each Pair of Parentheses

Medium

You are given a string `s` that consists of lower case English letters and brackets.

Reverse the strings in each pair of matching parentheses, starting from the innermost one.

Your result should **not** contain any brackets.

**Example 1:**

**Input:** s = "(abcd)"

**Output:** "dcba"

**Example 2:**

**Input:** s = "(u(love)i)"

**Output:** "iloveu"

**Explanation:** The substring "love" is reversed first, then the whole string is reversed.

**Example 3:**

**Input:** s = "(ed(et(oc))el)"

**Output:** "leetcode"

**Explanation:** First, we reverse the substring "oc", then "etco", and finally, the whole string.

**Constraints:**

*   `1 <= s.length <= 2000`
*   `s` only contains lower case English characters and parentheses.
*   It is guaranteed that all parentheses are balanced.

## Solution

```java
public class Solution {
    public String reverseParentheses(String s) {
        int n = s.length();
        StringBuilder sb = new StringBuilder();
        int i = 0;
        while (i < n) {
            if (s.charAt(i) == '(') {
                int l = 1;
                int r = 0;
                int idx = i;
                while (l != r) {
                    i++;
                    if (s.charAt(i) == '(') {
                        l++;
                    } else if (s.charAt(i) == ')') {
                        r++;
                    }
                }
                String reversed = reverseParentheses(s.substring(idx + 1, i));
                StringBuilder temp = new StringBuilder().append(reversed);
                sb.append(temp.reverse());
            } else {
                sb.append(s.charAt(i));
            }
            i++;
        }
        return sb.toString();
    }
}
```