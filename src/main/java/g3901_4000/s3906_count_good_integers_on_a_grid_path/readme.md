[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3906\. Count Good Integers on a Grid Path

Hard

You are given two integers `l` and `r`, and a string `directions` consisting of **exactly** three `'D'` characters and three `'R'` characters.

For each integer `x` in the range `[l, r]` (inclusive), perform the following steps:

1.  If `x` has fewer than 16 digits, pad it on the left with **leading zeros** to obtain a 16-digit string.
2.  Place the 16 digits into a `4 × 4` grid in **row-major** order (the first 4 digits form the first row from left to right, the next 4 digits form the second row, and so on).
3.  Starting at the **top-left** cell (`row = 0`, `column = 0`), apply the 6 characters of `directions` in order:
    *   `'D'` increments the row by 1.
    *   `'R'` increments the column by 1.
4.  Record the sequence of digits visited along the path (including the starting cell), producing a sequence of length 7.

The integer `x` is considered **good** if the recorded sequence is **non-decreasing**.

Return an integer representing the number of good integers in the range `[l, r]`.

**Example 1:**

**Input:** l = 8, r = 10, directions = "DDDRRR"

**Output:** 2

**Explanation:**

The grid for `x = 8`:

|  |  |  |  |
|---|---|---|---|
| 0 | 0 | 0 | 0 |
| 0 | 0 | 0 | 0 |
| 0 | 0 | 0 | 0 |
| 0 | 0 | 0 | 8 |

*   Path: `(0,0) → (1,0) → (2,0) → (3,0) → (3,1) → (3,2) → (3,3)`
*   The sequence of digits visited is `[0, 0, 0, 0, 0, 0, 8]`.
*   As the sequence of digits visited is non-decreasing, 8 is a good integer.

The grid for `x = 9`:

|  |  |  |  |
|---|---|---|---|
| 0 | 0 | 0 | 0 |
| 0 | 0 | 0 | 0 |
| 0 | 0 | 0 | 0 |
| 0 | 0 | 0 | 9 |

*   The sequence of digits visited is `[0, 0, 0, 0, 0, 0, 9]`.
*   As the sequence of digits visited is non-decreasing, 9 is a good integer.

The grid for `x = 10`:

|  |  |  |  |
|---|---|---|---|
| 0 | 0 | 0 | 0 |
| 0 | 0 | 0 | 0 |
| 0 | 0 | 0 | 0 |
| 0 | 0 | 1 | 0 |

*   The sequence of digits visited is `[0, 0, 0, 0, 0, 1, 0]`.
*   As the sequence of digits visited is not non-decreasing, 10 is not a good integer.
*   Hence, only 8 and 9 are good, giving a total of 2 good integers in the range.

**Example 2:**

**Input:** l = 123456789, r = 123456790, directions = "DDRRDR"

**Output:** 1

**Explanation:**

The grid for `x = 123456789`:

|  |  |  |  |
|---|---|---|---|
| 0 | 0 | 0 | 0 |
| 0 | 0 | 0 | 1 |
| 2 | 3 | 4 | 5 |
| 6 | 7 | 8 | 9 |

*   Path: `(0,0) → (1,0) → (2,0) → (2,1) → (2,2) → (3,2) → (3,3)`
*   The sequence of digits visited is `[0, 0, 2, 3, 4, 8, 9]`.
*   As the sequence of digits visited is non-decreasing, 123456789 is a good integer.

The grid for `x = 123456790`:

|  |  |  |  |
|---|---|---|---|
| 0 | 0 | 0 | 0 |
| 0 | 0 | 0 | 1 |
| 2 | 3 | 4 | 5 |
| 6 | 7 | 9 | 0 |

*   The sequence of digits visited is `[0, 0, 2, 3, 4, 9, 0]`.
*   As the sequence of digits visited is not non-decreasing, 123456790 is not a good integer.
*   Hence, only 123456789 is good, giving a total of 1 good integer in the range.

**Example 3:**

**Input:** l = 1288561398769758, r = 1288561398769758, directions = "RRRDDD"

**Output:** 0

**Explanation:**

The grid for `x = 1288561398769758`:

|  |  |  |  |
|---|---|---|---|
| 1 | 2 | 8 | 8 |
| 5 | 6 | 1 | 3 |
| 9 | 8 | 7 | 6 |
| 9 | 7 | 5 | 8 |

*   Path: `(0,0) → (0,1) → (0,2) → (0,3) → (1,3) → (2,3) → (3,3)`
*   The sequence of digits visited is `[1, 2, 8, 8, 3, 6, 8]`.
*   As the sequence of digits visited is not non-decreasing, 1288561398769758 is not a good integer.
*   No numbers are good, giving a total of 0 good integers in the range.

**Constraints:**

*   <code>1 <= l <= r <= 9 × 10<sup>15</sup></code>
*   `directions.length == 6`
*   `directions` consists of **exactly** three `'D'` characters and three `'R'` characters.

## Solution

```java
import java.util.Arrays;

public class Solution {
    private StringBuilder a;
    private StringBuilder b;
    private boolean[] arr;
    private final long[][] dp = new long[16][11];

    private long rec(int idx, int tl, int tu, int prev) {
        int n = 16;
        if (idx == n) {
            return 1;
        }
        if (tl == 0 && tu == 0 && dp[idx][prev] != -1) {
            return dp[idx][prev];
        }
        long res = 0;
        int lb = (tl == 1) ? a.charAt(idx) - '0' : 0;
        int ub = (tu == 1) ? b.charAt(idx) - '0' : 9;
        for (int digit = lb; digit <= ub; digit += 1) {
            int ntl = (tl == 1 && digit == lb) ? 1 : 0;
            int ntu = (tu == 1 && digit == ub) ? 1 : 0;
            if (arr[idx] || prev == 10) {
                if (prev == 10 || digit >= prev) {
                    res += rec(idx + 1, ntl, ntu, digit);
                }
            } else {
                res += rec(idx + 1, ntl, ntu, prev);
            }
        }
        if (tl == 0 && tu == 0) {
            dp[idx][prev] = res;
        }
        return res;
    }

    public long countGoodIntegersOnPath(long l, long r, String s) {
        a = new StringBuilder(String.valueOf(l));
        b = new StringBuilder(String.valueOf(r));
        while (b.length() < 16) {
            b.insert(0, '0');
        }
        while (a.length() < 16) {
            a.insert(0, '0');
        }
        arr = new boolean[16];
        arr[0] = true;
        int i = 0;
        int j = 0;
        for (int k = 0; k < 6; k++) {
            char c = s.charAt(k);
            if (c == 'D') {
                i += 1;
            } else {
                j += 1;
            }
            arr[(i * 4) + j] = true;
        }
        for (long[] x : dp) {
            Arrays.fill(x, -1);
        }
        return rec(0, 1, 1, 10);
    }
}
```