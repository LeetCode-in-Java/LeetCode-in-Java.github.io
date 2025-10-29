[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3725\. Count Ways to Choose Coprime Integers from Rows

Hard

You are given a `m x n` matrix `mat` of positive integers.

Return an integer denoting the number of ways to choose **exactly one** integer from each row of `mat` such that the **greatest common divisor** of all chosen integers is 1.

Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** mat = \[\[1,2],[3,4]]

**Output:** 3

**Explanation:**

| Chosen integer in the first row | Chosen integer in the second row | Greatest common divisor of chosen integers |
|---------------------------------|----------------------------------|--------------------------------------------|
| 1 | 3 | 1 |
| 1 | 4 | 1 |
| 2 | 3 | 1 |
| 2 | 4 | 2 |

3 of these combinations have a greatest common divisor of 1. Therefore, the answer is 3.

**Example 2:**

**Input:** mat = \[\[2,2],[2,2]]

**Output:** 0

**Explanation:**

Every combination has a greatest common divisor of 2. Therefore, the answer is 0.

**Constraints:**

*   `1 <= m == mat.length <= 150`
*   `1 <= n == mat[i].length <= 150`
*   `1 <= mat[i][j] <= 150`

## Solution

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    private static final int MOD = 1_000_000_007;

    public int countCoprime(int[][] mat) {
        int m = mat.length;
        int n = mat[0].length;
        int maxVal = 0;
        for (int[] ints : mat) {
            for (int j = 0; j < n; j++) {
                maxVal = Math.max(maxVal, ints[j]);
            }
        }
        Map<Integer, Long> gcdWays = new HashMap<>();
        for (int g = maxVal; g >= 1; g--) {
            long ways = countWaysWithDivisor(mat, g, m, n);
            if (ways > 0) {
                for (int multiple = 2 * g; multiple <= maxVal; multiple += g) {
                    if (gcdWays.containsKey(multiple)) {
                        ways = (ways - gcdWays.get(multiple) + MOD) % MOD;
                    }
                }
                gcdWays.put(g, ways);
            }
        }
        return gcdWays.getOrDefault(1, 0L).intValue();
    }

    private long countWaysWithDivisor(int[][] matrix, int divisor, int rows, int cols) {
        long totalWays = 1;
        for (int row = 0; row < rows; row++) {
            int validChoices = 0;
            for (int col = 0; col < cols; col++) {
                if (matrix[row][col] % divisor == 0) {
                    validChoices++;
                }
            }
            if (validChoices == 0) {
                return 0;
            }
            totalWays = (totalWays * validChoices) % MOD;
        }
        return totalWays;
    }
}
```