## 878\. Nth Magical Number

Hard

A positive integer is _magical_ if it is divisible by either `a` or `b`.

Given the three integers `n`, `a`, and `b`, return the <code>n<sup>th</sup></code> magical number. Since the answer may be very large, **return it modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** n = 1, a = 2, b = 3

**Output:** 2

**Example 2:**

**Input:** n = 4, a = 2, b = 3

**Output:** 6

**Constraints:**

*   <code>1 <= n <= 10<sup>9</sup></code>
*   <code>2 <= a, b <= 4 * 10<sup>4</sup></code>

## Solution

```java
@SuppressWarnings("java:S3518")
public class Solution {
    private static final int MOD = 1_000_000_007;

    public int nthMagicalNumber(int n, int a, int b) {
        long c = lcm(a, b);
        long l = 2;
        long r = n * c;
        long ans = r;
        while (l <= r) {
            long mid = l + (r - l) / 2;
            long cnt = mid / a + mid / b - mid / c;
            if (cnt < n) {
                l = mid + 1;
            } else {
                ans = mid;
                r = mid - 1;
            }
        }
        return (int) (ans % MOD);
    }

    private long lcm(long a, long b) {
        return a * b / gcd(a, b);
    }

    private long gcd(long a, long b) {
        long t;
        while (b != 0) {
            t = b;
            b = a % b;
            a = t;
        }
        return a;
    }
}
```