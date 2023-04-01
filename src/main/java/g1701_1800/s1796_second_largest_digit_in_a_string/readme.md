[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1796\. Second Largest Digit in a String

Easy

Given an alphanumeric string `s`, return _the **second largest** numerical digit that appears in_ `s`_, or_ `-1` _if it does not exist_.

An **alphanumeric** string is a string consisting of lowercase English letters and digits.

**Example 1:**

**Input:** s = "dfa12321afd"

**Output:** 2

**Explanation:** The digits that appear in s are [1, 2, 3]. The second largest digit is 2.

**Example 2:**

**Input:** s = "abc1111"

**Output:** -1

**Explanation:** The digits that appear in s are [1]. There is no second largest digit.

**Constraints:**

*   `1 <= s.length <= 500`
*   `s` consists of only lowercase English letters and/or digits.

## Solution

```java
public class Solution {
    public int secondHighest(String s) {
        int largest = -1;
        int sl = -1;
        for (char ch : s.toCharArray()) {
            if (Character.isDigit(ch)) {
                int n = ch - '0';
                if (n > largest) {
                    sl = largest;
                    largest = n;
                } else if (n > sl && n < largest) {
                    sl = n;
                }
            }
        }
        return sl;
    }
}
```