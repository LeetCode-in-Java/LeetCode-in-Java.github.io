[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1717\. Maximum Score From Removing Substrings

Medium

You are given a string `s` and two integers `x` and `y`. You can perform two types of operations any number of times.

*   Remove substring `"ab"` and gain `x` points.
    *   For example, when removing `"ab"` from `"cabxbae"` it becomes `"cxbae"`.
*   Remove substring `"ba"` and gain `y` points.
    *   For example, when removing `"ba"` from `"cabxbae"` it becomes `"cabxe"`.

Return _the maximum points you can gain after applying the above operations on_ `s`.

**Example 1:**

**Input:** s = "cdbcbbaaabab", x = 4, y = 5

**Output:** 19

**Explanation:** 

- Remove the "ba" underlined in "cdbcbbaaabab". Now, s = "cdbcbbaaab" and 5 points are added to the score. 

- Remove the "ab" underlined in "cdbcbbaaab". Now, s = "cdbcbbaa" and 4 points are added to the score. 

- Remove the "ba" underlined in "cdbcbbaa". Now, s = "cdbcba" and 5 points are added to the score. - Remove the "ba" underlined in "cdbcba". Now, s = "cdbc" and 5 points are added to the score. Total score = 5 + 4 + 5 + 5 = 19.

**Example 2:**

**Input:** s = "aabbaaxybbaabb", x = 5, y = 4

**Output:** 20

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   <code>1 <= x, y <= 10<sup>4</sup></code>
*   `s` consists of lowercase English letters.

## Solution

```java
@SuppressWarnings("java:S135")
public class Solution {
    public int maximumGain(String s, int x, int y) {
        char[] v = s.toCharArray();
        if (x > y) {
            return helper(v, 'a', 'b', x) + helper(v, 'b', 'a', y);
        } else {
            return helper(v, 'b', 'a', y) + helper(v, 'a', 'b', x);
        }
    }

    private int helper(char[] v, char c1, char c2, int score) {
        int left = -1;
        int right = 0;
        int res = 0;
        while (right < v.length) {
            if (v[right] != c2) {
                left = right;
            } else {
                while (left >= 0) {
                    char cl = v[left];
                    if (cl == '#') {
                        left--;
                    } else if (cl == c1) {
                        res += score;
                        v[left] = '#';
                        v[right] = '#';
                        left--;
                        break;
                    } else {
                        break;
                    }
                }
            }
            right++;
        }
        return res;
    }
}
```