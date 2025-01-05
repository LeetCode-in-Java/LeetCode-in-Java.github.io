[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 869\. Reordered Power of 2

Medium

You are given an integer `n`. We reorder the digits in any order (including the original order) such that the leading digit is not zero.

Return `true` _if and only if we can do this so that the resulting number is a power of two_.

**Example 1:**

**Input:** n = 1

**Output:** true

**Example 2:**

**Input:** n = 10

**Output:** false

**Constraints:**

*   <code>1 <= n <= 10<sup>9</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    public boolean reorderedPowerOf2(int n) {
        int[] originalDigits = countDigits(n);
        int num = 1;
        for (int i = 0; i < 31; i++) {
            if (Arrays.equals(originalDigits, countDigits(num))) {
                return true;
            }
            num <<= 1;
        }
        return false;
    }

    private int[] countDigits(int num) {
        int[] digitCount = new int[10];
        while (num > 0) {
            digitCount[num % 10]++;
            num /= 10;
        }
        return digitCount;
    }
}
```