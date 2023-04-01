[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 552\. Student Attendance Record II

Hard

An attendance record for a student can be represented as a string where each character signifies whether the student was absent, late, or present on that day. The record only contains the following three characters:

*   `'A'`: Absent.
*   `'L'`: Late.
*   `'P'`: Present.

Any student is eligible for an attendance award if they meet **both** of the following criteria:

*   The student was absent (`'A'`) for **strictly** fewer than 2 days **total**.
*   The student was **never** late (`'L'`) for 3 or more **consecutive** days.

Given an integer `n`, return _the **number** of possible attendance records of length_ `n` _that make a student eligible for an attendance award. The answer may be very large, so return it **modulo**_ <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** n = 2

**Output:** 8

**Explanation:**

    There are 8 records with length 2 that are eligible for an award:
    "PP", "AP", "PA", "LP", "PL", "AL", "LA", "LL"
    Only "AA" is not eligible because there are 2 absences (there need to be fewer than 2). 

**Example 2:**

**Input:** n = 1

**Output:** 3 

**Example 3:**

**Input:** n = 10101

**Output:** 183236316 

**Constraints:**

*   <code>1 <= n <= 10<sup>5</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int checkRecord(int n) {
        if (n == 0 || n == 1 || n == 2) {
            return n * 3 + n * (n - 1);
        }
        long mod = 1000000007;
        long[][] matrix = {
            {1, 1, 0, 1, 0, 0},
            {1, 0, 1, 1, 0, 0},
            {1, 0, 0, 1, 0, 0},
            {0, 0, 0, 1, 1, 0},
            {0, 0, 0, 1, 0, 1},
            {0, 0, 0, 1, 0, 0},
        };
        long[][] e = quickPower(matrix, n - 1);
        return (int)
                ((Arrays.stream(e[0]).sum() + Arrays.stream(e[1]).sum() + Arrays.stream(e[3]).sum())
                        % mod);
    }

    private long[][] quickPower(long[][] a, int times) {
        int n = a.length;
        long[][] e = new long[n][n];
        for (int i = 0; i < n; i++) {
            e[i][i] = 1;
        }
        while (times != 0) {
            if (times % 2 == 1) {
                e = multiple(e, a, n);
            }
            times >>= 1;
            a = multiple(a, a, n);
        }
        return e;
    }

    private long[][] multiple(long[][] a, long[][] b, int n) {
        long[][] target = new long[n][n];
        for (int j = 0; j < n; j++) {
            for (int k = 0; k < n; k++) {
                for (int l = 0; l < n; l++) {
                    target[j][k] += a[j][l] * b[l][k];
                    target[j][k] %= 1000000007;
                }
            }
        }
        return target;
    }
}
```