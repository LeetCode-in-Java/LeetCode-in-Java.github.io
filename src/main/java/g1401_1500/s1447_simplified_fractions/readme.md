[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1447\. Simplified Fractions

Medium

Given an integer `n`, return _a list of all **simplified** fractions between_ `0` _and_ `1` _(exclusive) such that the denominator is less-than-or-equal-to_ `n`. You can return the answer in **any order**.

**Example 1:**

**Input:** n = 2

**Output:** ["1/2"]

**Explanation:** "1/2" is the only unique fraction with a denominator less-than-or-equal-to 2.

**Example 2:**

**Input:** n = 3

**Output:** ["1/2","1/3","2/3"]

**Example 3:**

**Input:** n = 4

**Output:** ["1/2","1/3","1/4","2/3","3/4"]

**Explanation:** "2/4" is not a simplified fraction because it can be simplified to "1/2".

**Constraints:**

*   `1 <= n <= 100`

## Solution

```java
import java.util.ArrayList;
import java.util.List;

@SuppressWarnings("java:S2234")
public class Solution {
    public List<String> simplifiedFractions(int n) {
        List<String> result = new ArrayList<>();
        if (n == 1) {
            return result;
        }
        StringBuilder str = new StringBuilder();
        for (int denom = 2; denom <= n; denom++) {
            for (int num = 1; num < denom; num++) {
                if (checkGCD(num, denom) == 1) {
                    result.add(str.append(num).append("/").append(denom).toString());
                }
                str.setLength(0);
            }
        }
        return result;
    }

    private int checkGCD(int a, int b) {
        if (a < b) {
            return checkGCD(b, a);
        }
        if (a == b || a % b == 0 || b == 1) {
            return b;
        }
        return checkGCD(a % b, b);
    }
}
```