[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3233\. Find the Count of Numbers Which Are Not Special

Medium

You are given 2 **positive** integers `l` and `r`. For any number `x`, all positive divisors of `x` _except_ `x` are called the **proper divisors** of `x`.

A number is called **special** if it has exactly 2 **proper divisors**. For example:

*   The number 4 is _special_ because it has proper divisors 1 and 2.
*   The number 6 is _not special_ because it has proper divisors 1, 2, and 3.

Return the count of numbers in the range `[l, r]` that are **not** **special**.

**Example 1:**

**Input:** l = 5, r = 7

**Output:** 3

**Explanation:**

There are no special numbers in the range `[5, 7]`.

**Example 2:**

**Input:** l = 4, r = 16

**Output:** 11

**Explanation:**

The special numbers in the range `[4, 16]` are 4 and 9.

**Constraints:**

*   <code>1 <= l <= r <= 10<sup>9</sup></code>

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public int nonSpecialCount(int l, int r) {
        List<Integer> primes = sieveOfEratosthenes((int) Math.sqrt(r));
        int specialCount = 0;

        for (Integer prime : primes) {
            long primeSquare = (long) prime * prime;
            if (primeSquare >= l && primeSquare <= r) {
                specialCount++;
            }
        }

        int totalNumbersInRange = r - l + 1;
        return totalNumbersInRange - specialCount;
    }

    private List<Integer> sieveOfEratosthenes(int n) {
        boolean[] isPrime = new boolean[n + 1];
        for (int i = 2; i <= n; i++) {
            isPrime[i] = true;
        }

        for (int p = 2; p * p <= n; p++) {
            if (isPrime[p]) {
                for (int i = p * p; i <= n; i += p) {
                    isPrime[i] = false;
                }
            }
        }

        List<Integer> primes = new ArrayList<>();
        for (int i = 2; i <= n; i++) {
            if (isPrime[i]) {
                primes.add(i);
            }
        }
        return primes;
    }
}
```