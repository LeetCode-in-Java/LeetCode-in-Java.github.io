[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2048\. Next Greater Numerically Balanced Number

Medium

An integer `x` is **numerically balanced** if for every digit `d` in the number `x`, there are **exactly** `d` occurrences of that digit in `x`.

Given an integer `n`, return _the **smallest numerically balanced** number **strictly greater** than_ `n`_._

**Example 1:**

**Input:** n = 1

**Output:** 22

**Explanation:** 

22 is numerically balanced since: 

- The digit 2 occurs 2 times. 
  
It is also the smallest numerically balanced number strictly greater than 1.

**Example 2:**

**Input:** n = 1000

**Output:** 1333

**Explanation:** 

1333 is numerically balanced since:

- The digit 1 occurs 1 time. 

- The digit 3 occurs 3 times. 
  
It is also the smallest numerically balanced number strictly greater than 1000. Note that 1022 cannot be the answer because 0 appeared more than 0 times.

**Example 3:**

**Input:** n = 3000

**Output:** 3133

**Explanation:** 

3133 is numerically balanced since: 

- The digit 1 occurs 1 time. 

- The digit 3 occurs 3 times. 
  
It is also the smallest numerically balanced number strictly greater than 3000.

**Constraints:**

*   <code>0 <= n <= 10<sup>6</sup></code>

## Solution

```java
public class Solution {
    public int nextBeautifulNumber(int n) {
        int[] arr = new int[] {0, 1, 2, 3, 4, 5, 6};
        boolean[] select = new boolean[7];
        int d = n == 0 ? 1 : (int) Math.log10(n) + 1;
        return solve(1, n, d, 0, select, arr);
    }

    private int solve(int i, int n, int d, int sz, boolean[] select, int[] arr) {
        if (sz > d + 1) {
            return Integer.MAX_VALUE;
        }
        if (i == select.length) {
            return sz >= d ? make(0, n, sz, select, arr) : Integer.MAX_VALUE;
        }
        int ans = solve(i + 1, n, d, sz, select, arr);
        select[i] = true;
        ans = Math.min(ans, solve(i + 1, n, d, sz + i, select, arr));
        select[i] = false;
        return ans;
    }

    private int make(int cur, int n, int end, boolean[] select, int[] arr) {
        if (end == 0) {
            return cur > n ? cur : Integer.MAX_VALUE;
        }

        int ans = Integer.MAX_VALUE;
        for (int j = 1; j < arr.length; j++) {
            if (!select[j] || arr[j] == 0) {
                continue;
            }
            --arr[j];
            ans = Math.min(make(10 * cur + j, n, end - 1, select, arr), ans);
            ++arr[j];
        }
        return ans;
    }
}
```