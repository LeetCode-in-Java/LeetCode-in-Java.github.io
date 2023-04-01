[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 166\. Fraction to Recurring Decimal

Medium

Given two integers representing the `numerator` and `denominator` of a fraction, return _the fraction in string format_.

If the fractional part is repeating, enclose the repeating part in parentheses.

If multiple answers are possible, return **any of them**.

It is **guaranteed** that the length of the answer string is less than <code>10<sup>4</sup></code> for all the given inputs.

**Example 1:**

**Input:** numerator = 1, denominator = 2

**Output:** "0.5" 

**Example 2:**

**Input:** numerator = 2, denominator = 1

**Output:** "2" 

**Example 3:**

**Input:** numerator = 2, denominator = 3

**Output:** "0.(6)" 

**Example 4:**

**Input:** numerator = 4, denominator = 333

**Output:** "0.(012)" 

**Example 5:**

**Input:** numerator = 1, denominator = 5

**Output:** "0.2" 

**Constraints:**

*   <code>-2<sup>31</sup> <= numerator, denominator <= 2<sup>31</sup> - 1</code>
*   `denominator != 0`

## Solution

```java
import java.util.HashMap;
import java.util.Map;

@SuppressWarnings("java:S2153")
public class Solution {
    public String fractionToDecimal(int numerator, int denominator) {
        if (numerator == 0) {
            return "0";
        }
        StringBuilder sb = new StringBuilder();
        // negative case
        if (numerator > 0 && denominator < 0 || numerator < 0 && denominator > 0) {
            sb.append("-");
        }
        long x = Math.abs(Long.valueOf(numerator));
        long y = Math.abs(Long.valueOf(denominator));
        sb.append(x / y);
        long remainder = x % y;
        if (remainder == 0) {
            return sb.toString();
        }
        // decimal case
        sb.append(".");
        // store the remainder in a Hashmap because in the case of recurring decimal, the remainder
        // repeats as dividend.
        Map<Long, Integer> map = new HashMap<>();
        while (remainder != 0) {
            if (map.containsKey(remainder)) {
                sb.insert(map.get(remainder), "(");
                sb.append(")");
                break;
            }
            // store the remainder and the index of it's occurence in the String
            map.put(remainder, sb.length());
            remainder *= 10;
            sb.append(remainder / y);
            remainder %= y;
        }
        return sb.toString();
    }
}
```