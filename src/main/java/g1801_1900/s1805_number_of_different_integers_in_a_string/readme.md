[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1805\. Number of Different Integers in a String

Easy

You are given a string `word` that consists of digits and lowercase English letters.

You will replace every non-digit character with a space. For example, `"a123bc34d8ef34"` will become `" 123 34 8 34"`. Notice that you are left with some integers that are separated by at least one space: `"123"`, `"34"`, `"8"`, and `"34"`.

Return _the number of **different** integers after performing the replacement operations on_ `word`.

Two integers are considered different if their decimal representations **without any leading zeros** are different.

**Example 1:**

**Input:** word = "a123bc34d8ef34"

**Output:** 3

**Explanation:** The three different integers are "123", "34", and "8". Notice that "34" is only counted once.

**Example 2:**

**Input:** word = "leet1234code234"

**Output:** 2

**Example 3:**

**Input:** word = "a1b01c001"

**Output:** 1

**Explanation:** The three integers "1", "01", and "001" all represent the same integer because the leading zeros are ignored when comparing their decimal values.

**Constraints:**

*   `1 <= word.length <= 1000`
*   `word` consists of digits and lowercase English letters.

## Solution

```java
import java.util.HashSet;
import java.util.Set;

public class Solution {
    public int numDifferentIntegers(String word) {
        Set<String> ints = new HashSet<>();
        char[] chars = word.toCharArray();
        int start = -1;
        int stop = 0;
        for (int i = 0; i < chars.length; i++) {
            if (chars[i] >= '0' && chars[i] <= '9') {
                if (start == -1) {
                    start = i;
                }
                stop = i;
            } else if (start != -1) {
                ints.add(extractInt(chars, start, stop));
                start = -1;
            }
        }
        if (start != -1) {
            ints.add(extractInt(chars, start, stop));
        }
        return ints.size();
    }

    private String extractInt(char[] chrs, int start, int stop) {
        StringBuilder stb = new StringBuilder();
        while (start <= stop && chrs[start] == '0') {
            start++;
        }
        if (start >= stop) {
            stb.append(chrs[stop]);
        } else {
            while (start <= stop) {
                stb.append(chrs[start++]);
            }
        }
        return stb.toString();
    }
}
```