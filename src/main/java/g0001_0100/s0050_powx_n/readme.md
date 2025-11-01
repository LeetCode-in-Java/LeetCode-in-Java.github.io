[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 50\. Pow(x, n)

Medium

Implement [pow(x, n)](http://www.cplusplus.com/reference/valarray/pow/), which calculates `x` raised to the power `n` (i.e., <code>x<sup>n</sup></code>).

**Example 1:**

**Input:** x = 2.00000, n = 10

**Output:** 1024.00000 

**Example 2:**

**Input:** x = 2.10000, n = 3

**Output:** 9.26100 

**Example 3:**

**Input:** x = 2.00000, n = -2

**Output:** 0.25000

**Explanation:** 2<sup>\-2</sup> = 1/2<sup>2</sup> = 1/4 = 0.25 

**Constraints:**

*   `-100.0 < x < 100.0`
*   <code>-2<sup>31</sup> <= n <= 2<sup>31</sup>-1</code>
*   `n` is an integer.
*   Either `x` is not zero or `n > 0`.
*   <code>-10<sup>4</sup> <= x<sup>n</sup> <= 10<sup>4</sup></code>

## Solution

```java
public class Solution {
    public double myPow(double x, int n) {
        long nn = n;
        double res = 1;
        if (n < 0) {
            nn *= -1;
        }
        while (nn > 0) {
            if (nn % 2 == 1) {
                nn--;
                res *= x;
            } else {
                x *= x;
                nn /= 2;
            }
        }
        if (n < 0) {
            return 1.0 / res;
        }
        return res;
    }
}
```