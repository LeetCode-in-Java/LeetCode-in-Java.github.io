## 313\. Super Ugly Number

Medium

A **super ugly number** is a positive integer whose prime factors are in the array `primes`.

Given an integer `n` and an array of integers `primes`, return _the_ <code>n<sup>th</sup></code> _**super ugly number**_.

The <code>n<sup>th</sup></code> **super ugly number** is **guaranteed** to fit in a **32-bit** signed integer.

**Example 1:**

**Input:** n = 12, primes = [2,7,13,19]

**Output:** 32

**Explanation:** [1,2,4,7,8,13,14,16,19,26,28,32] is the sequence of the first 12 super ugly numbers given primes = [2,7,13,19]. 

**Example 2:**

**Input:** n = 1, primes = [2,3,5]

**Output:** 1

**Explanation:** 1 has no prime factors, therefore all of its prime factors are in the array primes = [2,3,5]. 

**Constraints:**

*   <code>1 <= n <= 10<sup>6</sup></code>
*   `1 <= primes.length <= 100`
*   `2 <= primes[i] <= 1000`
*   `primes[i]` is **guaranteed** to be a prime number.
*   All the values of `primes` are **unique** and sorted in **ascending order**.

## Solution

```java
public class Solution {
    public int nthSuperUglyNumber(int n, int[] primes) {
        if (primes.length == 0) {
            return 1;
        }
        int[] uglies = new int[n];
        uglies[0] = 1;
        int[] uglyToMultiplyBy = new int[primes.length];

        for (int i = 1; i < n; i++) {
            int min = primes[0] * uglies[uglyToMultiplyBy[0]];
            int minIdx = 0;
            for (int j = 1; j < primes.length; j++) {
                int currNum = primes[j] * uglies[uglyToMultiplyBy[j]];
                if (currNum < min) {
                    min = currNum;
                    minIdx = j;
                } else if (currNum == min) {
                    uglyToMultiplyBy[j]++;
                }
            }
            uglyToMultiplyBy[minIdx]++;
            uglies[i] = min;
        }
        return uglies[n - 1];
    }
}
```