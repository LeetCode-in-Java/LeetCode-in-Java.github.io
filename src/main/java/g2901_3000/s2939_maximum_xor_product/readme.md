[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2939\. Maximum Xor Product

Medium

Given three integers `a`, `b`, and `n`, return _the **maximum value** of_ `(a XOR x) * (b XOR x)` _where_ <code>0 <= x < 2<sup>n</sup></code>.

Since the answer may be too large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Note** that `XOR` is the bitwise XOR operation.

**Example 1:**

**Input:** a = 12, b = 5, n = 4

**Output:** 98

**Explanation:** For x = 2, (a XOR x) = 14 and (b XOR x) = 7. Hence, (a XOR x) \* (b XOR x) = 98. It can be shown that 98 is the maximum value of (a XOR x) \* (b XOR x) for all 0 <= x < 2<sup>n</sup>.

**Example 2:**

**Input:** a = 6, b = 7 , n = 5

**Output:** 930

**Explanation:** For x = 25, (a XOR x) = 31 and (b XOR x) = 30. Hence, (a XOR x) \* (b XOR x) = 930. It can be shown that 930 is the maximum value of (a XOR x) \* (b XOR x) for all 0 <= x < 2<sup>n</sup>.

**Example 3:**

**Input:** a = 1, b = 6, n = 3

**Output:** 12

**Explanation:** For x = 5, (a XOR x) = 4 and (b XOR x) = 3. Hence, (a XOR x) \* (b XOR x) = 12. It can be shown that 12 is the maximum value of (a XOR x) \* (b XOR x) for all 0 <= x < 2<sup>n</sup>.

**Constraints:**

*   <code>0 <= a, b < 2<sup>50</sup></code>
*   `0 <= n <= 50`

## Solution

```java
public class Solution {
    public int maximumXorProduct(long a, long b, int n) {
        for (int i = n - 1; i >= 0; i--) {
            long mask = (1L << i);
            boolean bitA = (a & mask) != 0;
            boolean bitB = (b & mask) != 0;
            if (bitA) {
                if (a > b) {
                    a ^= mask;
                    b |= mask;
                }
            } else if (bitB) {
                if (b > a) {
                    b ^= mask;
                    a |= mask;
                }
            } else {
                b |= mask;
                a |= mask;
            }
        }
        int mod = 1000000007;
        a = a % mod;
        b = b % mod;
        return (int) ((a * b) % mod);
    }
}
```