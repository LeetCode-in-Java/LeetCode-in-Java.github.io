[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

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
@SuppressWarnings("java:S3012")
public class Solution {
    public int nthSuperUglyNumber(int n, int[] primes) {
        long[] primes1 = new long[primes.length];
        for (int i = 0; i < primes.length; i++) {
            primes1[i] = primes[i];
        }
        int[] index = new int[primes.length];
        long[] n1 = new long[n];
        n1[0] = 1L;
        for (int i = 1; i < n; i++) {
            long min = Long.MAX_VALUE;
            for (long l : primes1) {
                min = Math.min(min, l);
            }
            n1[i] = min;
            for (int j = 0; j < primes1.length; j++) {
                if (min == primes1[j]) {
                    primes1[j] = primes[j] * (n1[++index[j]]);
                }
            }
        }
        return (int) n1[n - 1];
    }
}
```