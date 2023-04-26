[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2521\. Distinct Prime Factors of Product of Array

Medium

Given an array of positive integers `nums`, return _the number of **distinct prime factors** in the product of the elements of_ `nums`.

**Note** that:

*   A number greater than `1` is called **prime** if it is divisible by only `1` and itself.
*   An integer `val1` is a factor of another integer `val2` if `val2 / val1` is an integer.

**Example 1:**

**Input:** nums = [2,4,3,7,10,6]

**Output:** 4

**Explanation:** The product of all the elements in nums is: 2 \* 4 \* 3 \* 7 \* 10 \* 6 = 10080 = 2<sup>5</sup> \* 3<sup>2</sup> \* 5 \* 7. 

There are 4 distinct prime factors so we return 4.

**Example 2:**

**Input:** nums = [2,4,8,16]

**Output:** 1

**Explanation:** The product of all the elements in nums is: 2 \* 4 \* 8 \* 16 = 1024 = 2<sup>10</sup>. 

There is 1 distinct prime factor so we return 1.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>4</sup></code>
*   `2 <= nums[i] <= 1000`

## Solution

```java
@SuppressWarnings("java:S1119")
public class Solution {
    static final int[] PRIMES = new int[] {2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31};

    public int distinctPrimeFactors(int[] nums) {
        final boolean[] hasPrime = new boolean[PRIMES.length];
        final boolean[] nr = new boolean[1001];
        int r = 0;
        a:
        for (int n : nums) {
            if (nr[n]) {
                continue;
            }
            nr[n] = true;
            for (int i = 0; i < PRIMES.length && n > 1; i++) {
                final int prime = PRIMES[i];
                while (n % prime == 0) {
                    n /= prime;
                    hasPrime[i] = true;
                    if (nr[n]) {
                        continue a;
                    }
                    nr[n] = true;
                }
            }
            if (n > 1) {
                r++;
            }
        }
        for (boolean p : hasPrime) {
            if (p) {
                r++;
            }
        }
        return r;
    }
}
```