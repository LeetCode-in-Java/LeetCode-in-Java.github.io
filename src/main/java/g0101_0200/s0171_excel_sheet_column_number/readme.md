[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 171\. Excel Sheet Column Number

Easy

Given a string `columnTitle` that represents the column title as appear in an Excel sheet, return _its corresponding column number_.

For example:

    A -> 1
    B -> 2
    C -> 3
    ...
    Z -> 26
    AA -> 27
    AB -> 28
    ... 

**Example 1:**

**Input:** columnTitle = "A"

**Output:** 1 

**Example 2:**

**Input:** columnTitle = "AB"

**Output:** 28 

**Example 3:**

**Input:** columnTitle = "ZY"

**Output:** 701 

**Example 4:**

**Input:** columnTitle = "FXSHRXW"

**Output:** 2147483647 

**Constraints:**

*   `1 <= columnTitle.length <= 7`
*   `columnTitle` consists only of uppercase English letters.
*   `columnTitle` is in the range `["A", "FXSHRXW"]`.

## Solution

```java
public class Solution {
    public int titleToNumber(String s) {
        int num = 0;
        int pow = 0;
        for (int i = s.length() - 1; i >= 0; i--) {
            num += (int) Math.pow(26, pow++) * (s.charAt(i) - 'A' + 1);
        }
        return num;
    }
}
```