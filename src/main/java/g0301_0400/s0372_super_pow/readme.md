## 372\. Super Pow

Medium

Your task is to calculate <code>a<sup>b</sup></code> mod `1337` where `a` is a positive integer and `b` is an extremely large positive integer given in the form of an array.

**Example 1:**

**Input:** a = 2, b = [3]

**Output:** 8

**Example 2:**

**Input:** a = 2, b = [1,0]

**Output:** 1024

**Example 3:**

**Input:** a = 1, b = [4,3,3,8,5,2]

**Output:** 1

**Example 4:**

**Input:** a = 2147483647, b = [2,0,0]

**Output:** 1198

**Constraints:**

*   <code>1 <= a <= 2<sup>31</sup> - 1</code>
*   `1 <= b.length <= 2000`
*   `0 <= b[i] <= 9`
*   `b` doesn't contain leading zeros.

## Solution

```java
public class Solution {
    private static final int MOD = 1337;

    public int superPow(int a, int[] b) {
        int phi = phi(MOD);
        int arrMod = arrMod(b, phi);
        if (isGreaterOrEqual(b, phi)) {
            // Cycle has started
            // cycle starts at phi with length phi
            return exp(a % MOD, phi + arrMod);
        }
        return exp(a % MOD, arrMod);
    }

    private int phi(int n) {
        double result = n;
        for (int p = 2; p * p <= n; p++) {
            if (n % p > 0) {
                continue;
            }
            while (n % p == 0) {
                n /= p;
            }
            result *= 1.0 - 1.0 / p;
        }
        if (n > 1) {
            // if starting n was also prime (so it was greater than sqrt(n))
            result *= (1.0 - (1.0 / n));
        }
        return (int) result;
    }

    // Returns true if number in array is greater than integer named phi
    private boolean isGreaterOrEqual(int[] b, int phi) {
        int cur = 0;
        for (int j : b) {
            cur = cur * 10 + j;
            if (cur >= phi) {
                return true;
            }
        }
        return false;
    }

    // Returns number in array mod phi
    private int arrMod(int[] b, int phi) {
        int res = 0;
        for (int j : b) {
            res = (res * 10 + j) % phi;
        }
        return res;
    }

    // Binary exponentiation
    private int exp(int a, int b) {
        int y = 1;
        while (b > 0) {
            if (b % 2 == 1) {
                y = (y * a) % MOD;
            }
            a = (a * a) % MOD;
            b /= 2;
        }
        return y;
    }
}
```