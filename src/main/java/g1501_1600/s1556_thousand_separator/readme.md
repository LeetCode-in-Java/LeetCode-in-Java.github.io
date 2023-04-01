[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1556\. Thousand Separator

Easy

Given an integer `n`, add a dot (".") as the thousands separator and return it in string format.

**Example 1:**

**Input:** n = 987

**Output:** "987"

**Example 2:**

**Input:** n = 1234

**Output:** "1.234"

**Constraints:**

*   <code>0 <= n <= 2<sup>31</sup> - 1</code>

## Solution

```java
public class Solution {
    public String thousandSeparator(int n) {
        String str = Integer.toString(n);
        StringBuilder sb = new StringBuilder();
        int i = str.length() - 1;
        int j = 1;
        while (i >= 0) {
            sb.append(str.charAt(i));
            j++;
            if (j % 3 == 0) {
                sb.append(".");
            }
            i--;
            j++;
        }
        String result = sb.reverse().toString();
        if (result.charAt(0) == '.') {
            result = result.substring(1);
        }
        return result;
    }
}
```