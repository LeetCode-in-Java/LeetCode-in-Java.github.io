[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2710\. Remove Trailing Zeros From a String

Easy

Given a **positive** integer `num` represented as a string, return _the integer_ `num` _without trailing zeros as a string_.

**Example 1:**

**Input:** num = "51230100"

**Output:** "512301"

**Explanation:** Integer "51230100" has 2 trailing zeros, we remove them and return integer "512301".

**Example 2:**

**Input:** num = "123"

**Output:** "123"

**Explanation:** Integer "123" has no trailing zeros, we return integer "123".

**Constraints:**

*   `1 <= num.length <= 1000`
*   `num` consists of only digits.
*   `num` doesn't have any leading zeros.

## Solution

```java
public class Solution {
    public String removeTrailingZeros(String num) {
        int endIndex = num.length() - 1;
        while (endIndex >= 0 && num.charAt(endIndex) == '0') {
            endIndex--;
        }
        return num.substring(0, endIndex + 1);
    }
}
```