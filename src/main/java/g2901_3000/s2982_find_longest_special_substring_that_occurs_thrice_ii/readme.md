[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2982\. Find Longest Special Substring That Occurs Thrice II

Medium

You are given a string `s` that consists of lowercase English letters.

A string is called **special** if it is made up of only a single character. For example, the string `"abc"` is not special, whereas the strings `"ddd"`, `"zz"`, and `"f"` are special.

Return _the length of the **longest special substring** of_ `s` _which occurs **at least thrice**_, _or_ `-1` _if no special substring occurs at least thrice_.

A **substring** is a contiguous **non-empty** sequence of characters within a string.

**Example 1:**

**Input:** s = "aaaa"

**Output:** 2

**Explanation:** The longest special substring which occurs thrice is "aa": substrings "<ins>**aa**</ins>aa", "a<ins>**aa**</ins>a", and "aa<ins>**aa**</ins>".

It can be shown that the maximum length achievable is 2. 

**Example 2:**

**Input:** s = "abcdef"

**Output:** -1

**Explanation:** There exists no special substring which occurs at least thrice. Hence return -1. 

**Example 3:**

**Input:** s = "abcaba"

**Output:** 1

**Explanation:** The longest special substring which occurs thrice is "a": substrings "<ins>**a**</ins>bcaba", "abc<ins>**a**</ins>ba", and "abcab<ins>**a**</ins>".

It can be shown that the maximum length achievable is 1. 

**Constraints:**

*   <code>3 <= s.length <= 5 * 10<sup>5</sup></code>
*   `s` consists of only lowercase English letters.

## Solution

```java
public class Solution {
    public int maximumLength(String s) {
        int[][] arr = new int[26][4];
        char prev = s.charAt(0);
        int count = 1;
        int max = 0;
        for (int index = 1; index < s.length(); index++) {
            if (s.charAt(index) != prev) {
                int[] ints = arr[prev - 'a'];
                updateArr(count, ints);
                prev = s.charAt(index);
                count = 1;
            } else {
                count++;
            }
        }
        updateArr(count, arr[prev - 'a']);
        for (int[] values : arr) {
            if (values[0] != 0) {
                if (values[1] >= 3) {
                    max = Math.max(max, values[0]);
                } else if (values[1] == 2 || values[2] == values[0] - 1) {
                    max = Math.max(max, values[0] - 1);
                } else {
                    max = Math.max(max, values[0] - 2);
                }
            }
        }
        return max == 0 ? -1 : max;
    }

    private void updateArr(int count, int[] ints) {
        if (ints[0] == count) {
            ints[1]++;
        } else if (ints[0] < count) {
            ints[3] = ints[1];
            ints[2] = ints[0];
            ints[0] = count;
            ints[1] = 1;
        } else if (ints[2] < count) {
            ints[2] = count;
            ints[3] = 1;
        }
    }
}
```