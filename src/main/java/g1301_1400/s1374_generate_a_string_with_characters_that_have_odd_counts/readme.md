[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1374\. Generate a String With Characters That Have Odd Counts

Easy

Given an integer `n`, _return a string with `n` characters such that each character in such string occurs **an odd number of times**_.

The returned string must contain only lowercase English letters. If there are multiples valid strings, return **any** of them.

**Example 1:**

**Input:** n = 4

**Output:** "pppz"

**Explanation:** "pppz" is a valid string since the character 'p' occurs three times and the character 'z' occurs once. Note that there are many other valid strings such as "ohhh" and "love".

**Example 2:**

**Input:** n = 2

**Output:** "xy"

**Explanation:** "xy" is a valid string since the characters 'x' and 'y' occur once. Note that there are many other valid strings such as "ag" and "ur".

**Example 3:**

**Input:** n = 7

**Output:** "holasss"

**Constraints:**

*   `1 <= n <= 500`

## Solution

```java
public class Solution {
    public String generateTheString(int n) {
        StringBuilder sb = new StringBuilder();
        if (n > 1 && n % 2 == 0) {
            while (n-- > 1) {
                sb.append("a");
            }
        } else if (n > 1) {
            while (n-- > 2) {
                sb.append("a");
            }
            sb.append("b");
        }
        sb.append("z");
        return sb.toString();
    }
}
```