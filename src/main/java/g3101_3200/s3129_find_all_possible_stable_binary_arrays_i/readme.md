[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3129\. Find All Possible Stable Binary Arrays I

Medium

You are given 3 positive integers `zero`, `one`, and `limit`.

A binary array `arr` is called **stable** if:

*   The number of occurrences of 0 in `arr` is **exactly** `zero`.
*   The number of occurrences of 1 in `arr` is **exactly** `one`.
*   Each subarray of `arr` with a size greater than `limit` must contain **both** 0 and 1.

Return the _total_ number of **stable** binary arrays.

Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** zero = 1, one = 1, limit = 2

**Output:** 2

**Explanation:**

The two possible stable binary arrays are `[1,0]` and `[0,1]`, as both arrays have a single 0 and a single 1, and no subarray has a length greater than 2.

**Example 2:**

**Input:** zero = 1, one = 2, limit = 1

**Output:** 1

**Explanation:**

The only possible stable binary array is `[1,0,1]`.

Note that the binary arrays `[1,1,0]` and `[0,1,1]` have subarrays of length 2 with identical elements, hence, they are not stable.

**Example 3:**

**Input:** zero = 3, one = 3, limit = 2

**Output:** 14

**Explanation:**

All the possible stable binary arrays are `[0,0,1,0,1,1]`, `[0,0,1,1,0,1]`, `[0,1,0,0,1,1]`, `[0,1,0,1,0,1]`, `[0,1,0,1,1,0]`, `[0,1,1,0,0,1]`, `[0,1,1,0,1,0]`, `[1,0,0,1,0,1]`, `[1,0,0,1,1,0]`, `[1,0,1,0,0,1]`, `[1,0,1,0,1,0]`, `[1,0,1,1,0,0]`, `[1,1,0,0,1,0]`, and `[1,1,0,1,0,0]`.

**Constraints:**

*   `1 <= zero, one, limit <= 200`

## Solution

```java
public class Solution {
    private static final int MODULUS = (int) 1e9 + 7;

    private int add(int x, int y) {
        return (x + y) % MODULUS;
    }

    private int subtract(int x, int y) {
        return (x + MODULUS - y) % MODULUS;
    }

    private int multiply(int x, int y) {
        return (int) ((long) x * y % MODULUS);
    }

    public int numberOfStableArrays(int zero, int one, int limit) {
        if (limit == 1) {
            return Math.max(2 - Math.abs(zero - one), 0);
        }
        int max = Math.max(zero, one);
        int min = Math.min(zero, one);
        int[][] lcn = new int[max + 1][max + 1];
        int[] row0 = lcn[0];
        int[] row1;
        int[] row2;
        row0[0] = 1;
        for (int s = 1, sLim = s - limit; s <= max; s++, sLim++) {
            row2 = sLim > 0 ? lcn[sLim - 1] : new int[] {};
            row1 = row0;
            row0 = lcn[s];
            int c;
            for (c = (s - 1) / limit + 1; c <= sLim; c++) {
                row0[c] = subtract(add(row1[c], row1[c - 1]), row2[c - 1]);
            }
            for (; c <= s; c++) {
                row0[c] = add(row1[c], row1[c - 1]);
            }
        }
        row1 = lcn[min];
        int result = 0;
        int s0 = add(min < max ? row0[min + 1] : 0, row0[min]);
        for (int c = min; c > 0; c--) {
            int s1 = s0;
            s0 = add(row0[c], row0[c - 1]);
            result = add(result, multiply(row1[c], add(s0, s1)));
        }
        return result;
    }
}
```