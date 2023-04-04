[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 204\. Count Primes

Medium

Given an integer `n`, return _the number of prime numbers that are strictly less than_ `n`.

**Example 1:**

**Input:** n = 10

**Output:** 4

**Explanation:** There are 4 prime numbers less than 10, they are 2, 3, 5, 7. 

**Example 2:**

**Input:** n = 0

**Output:** 0 

**Example 3:**

**Input:** n = 1

**Output:** 0 

**Constraints:**

*   <code>0 <= n <= 5 * 10<sup>6</sup></code>

## Solution

```java
public class Solution {
    public int countPrimes(int n) {
        boolean[] isprime = new boolean[n];
        int count = 0;
        for (int i = 2; i * i <= n; i++) {
            if (!isprime[i]) {
                for (int j = i * 2; j < n; j += i) {
                    isprime[j] = true;
                }
            }
        }
        for (int i = 2; i < isprime.length; i++) {
            if (!isprime[i]) {
                count++;
            }
        }
        return count;
    }
}
```