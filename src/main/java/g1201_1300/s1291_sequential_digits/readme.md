[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1291\. Sequential Digits

Medium

An integer has _sequential digits_ if and only if each digit in the number is one more than the previous digit.

Return a **sorted** list of all the integers in the range `[low, high]` inclusive that have sequential digits.

**Example 1:**

**Input:** low = 100, high = 300

**Output:** [123,234]

**Example 2:**

**Input:** low = 1000, high = 13000

**Output:** [1234,2345,3456,4567,5678,6789,12345]

**Constraints:**

*   `10 <= low <= high <= 10^9`

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<Integer> sequentialDigits(int low, int high) {
        int[] arr = {
            12, 23, 34, 45, 56, 67, 78, 89, 123, 234, 345, 456, 567, 678, 789, 1234, 2345, 3456,
            4567, 5678, 6789, 12345, 23456, 34567, 45678, 56789, 123456, 234567, 345678, 456789,
            1234567, 2345678, 3456789, 12345678, 23456789, 123456789
        };
        List<Integer> result = new ArrayList<>();
        for (int j : arr) {
            // 234    148         234        256
            if (j >= low && j <= high) {
                result.add(j);
            }
        }
        return result;
    }
}
```