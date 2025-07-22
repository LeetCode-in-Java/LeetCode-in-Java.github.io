[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3621\. Number of Integers With Popcount-Depth Equal to K I

Hard

You are given two integers `n` and `k`.

For any positive integer `x`, define the following sequence:

*   <code>p<sub>0</sub> = x</code>
*   <code>p<sub>i+1</sub> = popcount(p<sub>i</sub>)</code> for all `i >= 0`, where `popcount(y)` is the number of set bits (1's) in the binary representation of `y`.

This sequence will eventually reach the value 1.

The **popcount-depth** of `x` is defined as the **smallest** integer `d >= 0` such that <code>p<sub>d</sub> = 1</code>.

For example, if `x = 7` (binary representation `"111"`). Then, the sequence is: `7 → 3 → 2 → 1`, so the popcount-depth of 7 is 3.

Your task is to determine the number of integers in the range `[1, n]` whose popcount-depth is **exactly** equal to `k`.

Return the number of such integers.

**Example 1:**

**Input:** n = 4, k = 1

**Output:** 2

**Explanation:**

The following integers in the range `[1, 4]` have popcount-depth exactly equal to 1:

| x | Binary | Sequence   |
|---|--------|------------|
| 2 | `"10"` | `2 → 1`    |
| 4 | `"100"`| `4 → 1`    |

Thus, the answer is 2.

**Example 2:**

**Input:** n = 7, k = 2

**Output:** 3

**Explanation:**

The following integers in the range `[1, 7]` have popcount-depth exactly equal to 2:

| x | Binary  | Sequence       |
|---|---------|----------------|
| 3 | `"11"`  | `3 → 2 → 1`    |
| 5 | `"101"` | `5 → 2 → 1`    |
| 6 | `"110"` | `6 → 2 → 1`    |

Thus, the answer is 3.

**Constraints:**

*   <code>1 <= n <= 10<sup>15</sup></code>
*   `0 <= k <= 5`

## Solution

```java
public class Solution {
    private static final int MX_LN = 61;
    private final long[][] slct = new long[MX_LN][MX_LN];
    private final int[] popHeight = new int[MX_LN];

    public Solution() {
        for (int i = 0; i < MX_LN; i++) {
            slct[i][0] = slct[i][i] = 1;
            for (int j = 1; j < i; j++) {
                slct[i][j] = slct[i - 1][j - 1] + slct[i - 1][j];
            }
        }
        popHeight[1] = 0;
        for (int v = 2; v < MX_LN; v++) {
            popHeight[v] = 1 + popHeight[Long.bitCount(v)];
        }
    }

    private long countNumbers(long upperLimit, int setBits) {
        if (setBits == 0) {
            return 1;
        }
        long count = 0;
        int used = 0;
        int len = 0;
        for (long x = upperLimit; x > 0; x >>= 1) {
            len++;
        }
        for (int pos = len - 1; pos >= 0; pos--) {
            if (((upperLimit >> pos) & 1) == 1) {
                if (setBits - used <= pos) {
                    count += slct[pos][setBits - used];
                }
                used++;
                if (used > setBits) {
                    break;
                }
            }
        }
        if (Long.bitCount(upperLimit) == setBits) {
            count++;
        }
        return count;
    }

    public long popcountDepth(long tillNumber, int depthQuery) {
        if (depthQuery == 0) {
            return tillNumber >= 1 ? 1 : 0;
        }
        long total = 0;
        for (int ones = 1; ones < MX_LN; ones++) {
            if (popHeight[ones] == depthQuery - 1) {
                total += countNumbers(tillNumber, ones);
            }
        }
        if (depthQuery == 1) {
            total -= 1;
        }
        return total;
    }
}
```