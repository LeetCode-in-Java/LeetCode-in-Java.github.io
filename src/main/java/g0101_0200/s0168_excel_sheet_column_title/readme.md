[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 168\. Excel Sheet Column Title

Easy

Given an integer `columnNumber`, return _its corresponding column title as it appears in an Excel sheet_.

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

**Input:** columnNumber = 1

**Output:** "A" 

**Example 2:**

**Input:** columnNumber = 28

**Output:** "AB" 

**Example 3:**

**Input:** columnNumber = 701

**Output:** "ZY" 

**Example 4:**

**Input:** columnNumber = 2147483647

**Output:** "FXSHRXW" 

**Constraints:**

*   <code>1 <= columnNumber <= 2<sup>31</sup> - 1</code>

## Solution

```java
public class Solution {
    public String convertToTitle(int n) {
        StringBuilder sb = new StringBuilder();
        while (n != 0) {
            int remainder = n % 26;
            if (remainder == 0) {
                remainder += 26;
            }
            if (n >= remainder) {
                n -= remainder;
                sb.append((char) (remainder + 64));
            }
            n /= 26;
        }
        return sb.reverse().toString();
    }
}
```