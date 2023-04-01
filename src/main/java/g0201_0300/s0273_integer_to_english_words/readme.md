[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 273\. Integer to English Words

Hard

Convert a non-negative integer `num` to its English words representation.

**Example 1:**

**Input:** num = 123

**Output:** "One Hundred Twenty Three" 

**Example 2:**

**Input:** num = 12345

**Output:** "Twelve Thousand Three Hundred Forty Five" 

**Example 3:**

**Input:** num = 1234567

**Output:** "One Million Two Hundred Thirty Four Thousand Five Hundred Sixty Seven" 

**Example 4:**

**Input:** num = 1234567891

**Output:** "One Billion Two Hundred Thirty Four Million Five Hundred Sixty Seven Thousand Eight Hundred Ninety One" 

**Constraints:**

*   <code>0 <= num <= 2<sup>31</sup> - 1</code>

## Solution

```java
import java.util.StringJoiner;

public class Solution {
    private String[] ones = {
        "One ", "Two ", "Three ", "Four ", "Five ", "Six ", "Seven ", "Eight ", "Nine "
    };
    private String[] teens = {
        "Ten ",
        "Eleven ",
        "Twelve ",
        "Thirteen ",
        "Fourteen ",
        "Fifteen ",
        "Sixteen ",
        "Seventeen ",
        "Eighteen ",
        "Nineteen "
    };
    private String[] twenties = {
        "Twenty ", "Thirty ", "Forty ", "Fifty ", "Sixty ", "Seventy ", "Eighty ", "Ninety "
    };

    public String numberToWords(int num) {
        if (num == 0) {
            return "Zero";
        }
        StringJoiner joiner = new StringJoiner("");
        processThreeDigits(joiner, num / 1_000_000_000, "Billion ");
        processThreeDigits(joiner, num / 1_000_000, "Million ");
        processThreeDigits(joiner, num / 1_000, "Thousand ");
        processThreeDigits(joiner, num, null);
        return joiner.toString().trim();
    }

    private void processThreeDigits(StringJoiner joiner, int input, String name) {
        int threeDigit = input % 1000;
        if (threeDigit > 0) {
            if (threeDigit / 100 > 0) {
                joiner.add(ones[threeDigit / 100 - 1]);
                String hundred = "Hundred ";
                joiner.add(hundred);
            }
            if (threeDigit % 100 >= 20) {
                joiner.add(twenties[(threeDigit % 100) / 10 - 2]);
                if (threeDigit % 10 > 0) {
                    joiner.add(ones[threeDigit % 10 - 1]);
                }
            } else if (threeDigit % 100 >= 10 && threeDigit % 100 < 20) {
                joiner.add(teens[threeDigit % 10]);
            } else if (threeDigit % 100 > 0 && threeDigit % 100 < 10) {
                joiner.add(ones[threeDigit % 10 - 1]);
            }
            if (name != null) {
                joiner.add(name);
            }
        }
    }
}
```