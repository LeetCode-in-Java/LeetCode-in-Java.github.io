[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 668\. Kth Smallest Number in Multiplication Table

Hard

Nearly everyone has used the [Multiplication Table](https://en.wikipedia.org/wiki/Multiplication_table). The multiplication table of size `m x n` is an integer matrix `mat` where `mat[i][j] == i * j` (**1-indexed**).

Given three integers `m`, `n`, and `k`, return _the_ <code>k<sup>th</sup></code> _smallest element in the_ `m x n` _multiplication table_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/05/02/multtable1-grid.jpg)

**Input:** m = 3, n = 3, k = 5

**Output:** 3

**Explanation:** The 5<sup>th</sup> smallest number is 3.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/05/02/multtable2-grid.jpg)

**Input:** m = 2, n = 3, k = 6

**Output:** 6

**Explanation:** The 6<sup>th</sup> smallest number is 6.

**Constraints:**

*   <code>1 <= m, n <= 3 * 10<sup>4</sup></code>
*   `1 <= k <= m * n`

## Solution

```java
public class Solution {
    public int findKthNumber(int m, int n, int k) {
        int lo = 1;
        int hi = m * n;
        while (lo < hi) {
            int mid = lo + (hi - lo) / 2;
            int col = n;
            int row = 1;
            int count = 0;
            while (row <= m && col >= 1) {
                int val = row * col;
                if (val > mid) {
                    col--;
                } else {
                    count += col;
                    row++;
                }
            }
            if (count < k) {
                lo = mid + 1;
            } else {
                hi = mid;
            }
        }
        return lo;
    }
}
```