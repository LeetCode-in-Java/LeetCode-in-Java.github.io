[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1903\. Largest Odd Number in String

Easy

You are given a string `num`, representing a large integer. Return _the **largest-valued odd** integer (as a string) that is a **non-empty substring** of_ `num`_, or an empty string_ `""` _if no odd integer exists_.

A **substring** is a contiguous sequence of characters within a string.

**Example 1:**

**Input:** num = "52"

**Output:** "5"

**Explanation:** The only non-empty substrings are "5", "2", and "52". "5" is the only odd number.

**Example 2:**

**Input:** num = "4206"

**Output:** ""

**Explanation:** There are no odd numbers in "4206".

**Example 3:**

**Input:** num = "35427"

**Output:** "35427"

**Explanation:** "35427" is already an odd number.

**Constraints:**

*   <code>1 <= num.length <= 10<sup>5</sup></code>
*   `num` only consists of digits and does not contain any leading zeros.

## Solution

```java
public class Solution {
    public String largestOddNumber(String num) {
        String str = "";
        for (int i = num.length() - 1; i >= 0; i--) {
            char c = num.charAt(i);
            if (c % 2 == 1) {
                str = num.substring(0, i + 1);
                break;
            }
        }
        return str;
    }
}
```