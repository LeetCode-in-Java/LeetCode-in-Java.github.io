[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 793\. Preimage Size of Factorial Zeroes Function

Hard

Let `f(x)` be the number of zeroes at the end of `x!`. Recall that `x! = 1 * 2 * 3 * ... * x` and by convention, `0! = 1`.

*   For example, `f(3) = 0` because `3! = 6` has no zeroes at the end, while `f(11) = 2` because `11! = 39916800` has two zeroes at the end.

Given an integer `k`, return the number of non-negative integers `x` have the property that `f(x) = k`.

**Example 1:**

**Input:** k = 0

**Output:** 5

**Explanation:** 0!, 1!, 2!, 3!, and 4! end with k = 0 zeroes. 

**Example 2:**

**Input:** k = 5

**Output:** 0

**Explanation:** There is no x such that x! ends in k = 5 zeroes. 

**Example 3:**

**Input:** k = 3

**Output:** 5 

**Constraints:**

*   <code>0 <= k <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    public int preimageSizeFZF(int k) {
        long left = 0;
        long right = 5L * (k + 1);
        while (left < right - 1) {
            long mid = left + (right - left) / 2;
            int cnt = countZeros(mid);
            if (cnt == k) {
                return 5;
            } else if (cnt < k) {
                left = mid;
            } else {
                right = mid;
            }
        }
        return countZeros(left) == k || countZeros(right) == k ? 5 : 0;
    }

    private int countZeros(long n) {
        long rst = 0;
        while (n > 0) {
            rst += n / 5;
            n /= 5;
        }
        return (int) rst;
    }
}
```