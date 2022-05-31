## 1175\. Prime Arrangements

Easy

Return the number of permutations of 1 to `n` so that prime numbers are at prime indices (1-indexed.)

_(Recall that an integer is prime if and only if it is greater than 1, and cannot be written as a product of two positive integers both smaller than it.)_

Since the answer may be large, return the answer **modulo `10^9 + 7`**.

**Example 1:**

**Input:** n = 5

**Output:** 12

**Explanation:** For example [1,2,5,4,3] is a valid permutation, but [5,2,3,4,1] is not because the prime number 5 is at index 1.

**Example 2:**

**Input:** n = 100

**Output:** 682289015

**Constraints:**

*   `1 <= n <= 100`

## Solution

```java
import java.math.BigInteger;
import java.util.Arrays;

public class Solution {
    private static int mod = 1000000007;

    public int numPrimeArrangements(int n) {
        int numberOfPrimes = generatePrimes(n);
        BigInteger x = factorial(numberOfPrimes);
        BigInteger y = factorial(n - numberOfPrimes);
        return x.multiply(y).mod(BigInteger.valueOf(mod)).intValue();
    }

    private BigInteger factorial(int n) {
        BigInteger fac = BigInteger.ONE;
        for (int i = 2; i <= n; i++) {
            fac = fac.multiply(BigInteger.valueOf(i));
        }
        return fac.mod(BigInteger.valueOf(mod));
    }

    private int generatePrimes(int n) {
        boolean[] prime = new boolean[n + 1];
        Arrays.fill(prime, 2, n + 1, true);
        for (int i = 2; i * i <= n; i++) {
            if (prime[i]) {
                for (int j = i * i; j <= n; j += i) {
                    prime[j] = false;
                }
            }
        }
        int count = 0;
        for (boolean b : prime) {
            if (b) {
                count++;
            }
        }
        return count;
    }
}
```