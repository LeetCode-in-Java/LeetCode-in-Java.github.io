[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1994\. The Number of Good Subsets

Hard

You are given an integer array `nums`. We call a subset of `nums` **good** if its product can be represented as a product of one or more **distinct prime** numbers.

*   For example, if `nums = [1, 2, 3, 4]`:
    *   `[2, 3]`, `[1, 2, 3]`, and `[1, 3]` are **good** subsets with products `6 = 2*3`, `6 = 2*3`, and `3 = 3` respectively.
    *   `[1, 4]` and `[4]` are not **good** subsets with products `4 = 2*2` and `4 = 2*2` respectively.

Return _the number of different **good** subsets in_ `nums` _**modulo**_ <code>10<sup>9</sup> + 7</code>.

A **subset** of `nums` is any array that can be obtained by deleting some (possibly none or all) elements from `nums`. Two subsets are different if and only if the chosen indices to delete are different.

**Example 1:**

**Input:** nums = [1,2,3,4]

**Output:** 6

**Explanation:** The good subsets are:

- \[1,2]: product is 2, which is the product of distinct prime 2.

- \[1,2,3]: product is 6, which is the product of distinct primes 2 and 3.

- \[1,3]: product is 3, which is the product of distinct prime 3.

- \[2]: product is 2, which is the product of distinct prime 2.

- \[2,3]: product is 6, which is the product of distinct primes 2 and 3.

- \[3]: product is 3, which is the product of distinct prime 3. 

**Example 2:**

**Input:** nums = [4,2,3,15]

**Output:** 5

**Explanation:** The good subsets are:

- \[2]: product is 2, which is the product of distinct prime 2.

- \[2,3]: product is 6, which is the product of distinct primes 2 and 3.

- \[2,15]: product is 30, which is the product of distinct primes 2, 3, and 5.

- \[3]: product is 3, which is the product of distinct prime 3.

- \[15]: product is 15, which is the product of distinct primes 3 and 5. 

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   `1 <= nums[i] <= 30`

## Solution

```java
public class Solution {
    private static final long MOD = (long) (1e9 + 7);

    private long add(long a, long b) {
        a += b;
        return a < MOD ? a : a - MOD;
    }

    private long mul(long a, long b) {
        a *= b;
        return a < MOD ? a : a % MOD;
    }

    private long pow(long a, long b) {
        // a %= MOD;
        // b%=(MOD-1);//if MOD is prime
        long res = 1;
        while (b > 0) {
            if ((b & 1) == 1) {
                res = mul(res, a);
            }
            a = mul(a, a);
            b >>= 1;
        }
        return add(res, 0);
    }

    public int numberOfGoodSubsets(int[] nums) {
        int[] primes = {2, 3, 5, 7, 11, 13, 17, 19, 23, 29};
        int[] mask = new int[31];
        int[] freq = new int[31];
        for (int x : nums) {
            freq[x]++;
        }
        for (int i = 1; i <= 30; i++) {
            for (int j = 0; j < primes.length; j++) {
                if (i % primes[j] == 0) {
                    if ((i / primes[j]) % primes[j] == 0) {
                        mask[i] = 0;
                        break;
                    }
                    mask[i] |= (int) pow(2, j);
                }
            }
        }
        long[] dp = new long[1024];
        dp[0] = 1;
        for (int i = 1; i <= 30; i++) {
            if (mask[i] != 0) {
                for (int j = 0; j < 1024; j++) {
                    if ((mask[i] & j) == 0 && dp[j] > 0) {
                        dp[(mask[i] | j)] = add(dp[(mask[i] | j)], mul(dp[j], freq[i]));
                    }
                }
            }
        }
        long ans = 0;
        for (int i = 1; i < 1024; i++) {
            ans = add(ans, dp[i]);
        }
        ans = mul(ans, pow(2, freq[1]));
        ans = add(ans, 0);
        return (int) ans;
    }
}
```