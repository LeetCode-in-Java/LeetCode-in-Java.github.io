[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 32\. Longest Valid Parentheses

Hard

Given a string containing just the characters `'('` and `')'`, find the length of the longest valid (well-formed) parentheses substring.

**Example 1:**

**Input:** s = "(()"

**Output:** 2

**Explanation:** The longest valid parentheses substring is "()". 

**Example 2:**

**Input:** s = ")()())"

**Output:** 4

**Explanation:** The longest valid parentheses substring is "()()". 

**Example 3:**

**Input:** s = ""

**Output:** 0 

**Constraints:**

*   <code>0 <= s.length <= 3 * 10<sup>4</sup></code>
*   `s[i]` is `'('`, or `')'`.

## Solution

```java
public class Solution {
    public int longestValidParentheses(String s) {
        int max = 0;
        int left = 0;
        int right = 0;
        int n = s.length();
        char ch;
        for (int i = 0; i < n; i++) {
            ch = s.charAt(i);
            if (ch == '(') {
                left++;
            } else {
                right++;
            }
            if (right > left) {
                left = 0;
                right = 0;
            }
            if (left == right) {
                max = Math.max(max, left + right);
            }
        }
        left = 0;
        right = 0;
        for (int i = n - 1; i >= 0; i--) {
            ch = s.charAt(i);
            if (ch == '(') {
                left++;
            } else {
                right++;
            }
            if (left > right) {
                left = 0;
                right = 0;
            }
            if (left == right) {
                max = Math.max(max, left + right);
            }
        }
        return max;
    }
}
```

﻿**Time Complexity (Big O Time):**

1. The program iterates through the input string `s` twice. In the first loop, it goes from left to right, and in the second loop, it goes from right to left. Each loop has a linear time complexity of O(n), where n is the length of the input string.

2. Within each loop, there are constant time operations like character comparisons and arithmetic calculations. These operations do not depend on the size of the input string and can be considered O(1).

Combining these factors, the overall time complexity of the program is O(n), where n is the length of the input string `s`. The two passes through the string do not make it O(2n) because constant factors are ignored in big O notation.

**Space Complexity (Big O Space):**

The space complexity of the program is O(1), which means it uses a constant amount of additional space regardless of the size of the input string `s`. The variables `max`, `left`, `right`, `n`, and `ch` all occupy constant space, and the program does not use any additional data structures or memory that scales with the input size.

In summary, the time complexity of the provided program is O(n), and the space complexity is O(1), where n is the length of the input string `s`.
