[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3426\. Manhattan Distances of All Arrangements of Pieces

Hard

You are given three integers `m`, `n`, and `k`.

There is a rectangular grid of size `m × n` containing `k` identical pieces. Return the sum of Manhattan distances between every pair of pieces over all **valid arrangements** of pieces.

A **valid arrangement** is a placement of all `k` pieces on the grid with **at most** one piece per cell.

Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

The Manhattan Distance between two cells <code>(x<sub>i</sub>, y<sub>i</sub>)</code> and <code>(x<sub>j</sub>, y<sub>j</sub>)</code> is <code>|x<sub>i</sub> - x<sub>j</sub>| + |y<sub>i</sub> - y<sub>j</sub>|</code>.

**Example 1:**

**Input:** m = 2, n = 2, k = 2

**Output:** 8

**Explanation:**

The valid arrangements of pieces on the board are:

![](https://leetcode-images.github.io/g3401_3500/s3426_manhattan_distances_of_all_arrangements_of_pieces/untitled-diagramdrawio.png)

*   In the first 4 arrangements, the Manhattan distance between the two pieces is 1.
*   In the last 2 arrangements, the Manhattan distance between the two pieces is 2.

Thus, the total Manhattan distance across all valid arrangements is `1 + 1 + 1 + 1 + 2 + 2 = 8`.

**Example 2:**

**Input:** m = 1, n = 4, k = 3

**Output:** 20

**Explanation:**

The valid arrangements of pieces on the board are:

![](https://leetcode-images.github.io/g3401_3500/s3426_manhattan_distances_of_all_arrangements_of_pieces/4040example2drawio.png)

*   The first and last arrangements have a total Manhattan distance of `1 + 1 + 2 = 4`.
*   The middle two arrangements have a total Manhattan distance of `1 + 2 + 3 = 6`.

The total Manhattan distance between all pairs of pieces across all arrangements is `4 + 6 + 6 + 4 = 20`.

**Constraints:**

*   <code>1 <= m, n <= 10<sup>5</sup></code>
*   <code>2 <= m * n <= 10<sup>5</sup></code>
*   `2 <= k <= m * n`

## Solution

```java
public class Solution {
    private long comb(long a, long b, long mod) {
        if (b > a) {
            return 0;
        }
        long numer = 1;
        long denom = 1;
        for (long i = 0; i < b; ++i) {
            numer = numer * (a - i) % mod;
            denom = denom * (i + 1) % mod;
        }
        long denomInv = 1;
        long exp = mod - 2;
        while (exp > 0) {
            if (exp % 2 > 0) {
                denomInv = denomInv * denom % mod;
            }
            denom = denom * denom % mod;
            exp /= 2;
        }
        return numer * denomInv % mod;
    }

    public int distanceSum(int m, int n, int k) {
        long res = 0;
        long mod = 1000000007;
        long base = comb((long) m * n - 2, k - 2L, mod);
        for (int d = 1; d < n; ++d) {
            res = (res + (long) d * (n - d) % mod * m % mod * m % mod) % mod;
        }
        for (int d = 1; d < m; ++d) {
            res = (res + (long) d * (m - d) % mod * n % mod * n % mod) % mod;
        }
        return (int) (res * base % mod);
    }
}
```