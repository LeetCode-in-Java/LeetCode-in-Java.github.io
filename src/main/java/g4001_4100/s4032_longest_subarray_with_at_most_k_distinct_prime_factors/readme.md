[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 4032\. Longest Subarray With at Most K Distinct Prime Factors

Medium

You are given an integer array `nums` consisting of positive integers and an integer `k`.

The **prime factor set** of a non-empty subarrays is the **union** of the distinct **prime** factors of all its elements.

Return the length of the **longest** subarray whose prime factor set contains **at most** `k` distinct prime factors. If no such subarray exists, return 0.

**Example 1:**

**Input:** nums = [7,6,10,12,11], k = 3

**Output:** 3

**Explanation:**

Consider the subarray `[6, 10, 12]`:

*   The distinct prime factors of 6 are `{2, 3}`.
*   The distinct prime factors of 10 are `{2, 5}`.
*   The distinct prime factors of 12 are `{2, 3}`.
*   The union of these sets is `{2, 3, 5}`, which contains 3 distinct prime factors.

No longer subarray satisfies the condition. Therefore, the answer is 3.

**Example 2:**

**Input:** nums = [4,6,9,18], k = 4

**Output:** 4

**Explanation:**

Consider the entire array `[4, 6, 9, 18]`:

*   The distinct prime factors of 4 are `{2}`.
*   The distinct prime factors of 6 are `{2, 3}`.
*   The distinct prime factors of 9 are `{3}`.
*   The distinct prime factors of 18 are `{2, 3}`.
*   The union of these sets is `{2, 3}`, which contains 2 distinct prime factors.

Since `2 <= 4`, the entire array is valid. Therefore, the answer is 4.

**Example 3:**

**Input:** nums = [6,10,15], k = 2

**Output:** 1

**Explanation:**

Every subarray of length at least 2 has prime factor set `{2, 3, 5}`, which contains 3 distinct prime factors.

Since `3 > 2`, only subarrays of length 1 are valid. Therefore, the answer is 1.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>2 <= nums[i] <= 10<sup>5</sup></code>
*   <code>1 <= k <= 10<sup>4</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int longestSubarray(int[] nums, int k) {
        int n = nums.length;
        int maxVal = 0;
        for (int x : nums) {
            if (x > maxVal) {
                maxVal = x;
            }
        }
        // 1. Sieve of Eratosthenes to compute Smallest Prime Factor (SPF)
        int[] spf = new int[maxVal + 1];
        for (int i = 2; i <= maxVal; i++) {
            spf[i] = i;
        }
        for (int i = 2; i * i <= maxVal; i++) {
            if (spf[i] == i) {
                for (int j = i * i; j <= maxVal; j += i) {
                    if (spf[j] == j) {
                        spf[j] = i;
                    }
                }
            }
        }
        // 2. Precompute distinct prime factors for each unique element present in nums
        int[][] primeFactors = new int[maxVal + 1][];
        for (int x : nums) {
            if (primeFactors[x] == null) {
                primeFactors[x] = getDistinctPrimeFactors(x, spf);
            }
        }
        // 3. Sliding Window (Two Pointers)
        int[] primeCount = new int[maxVal + 1];
        int distinctPrimes = 0;
        int maxLen = 0;
        int left = 0;
        for (int right = 0; right < n; right++) {
            // Add all prime factors of nums[right] to the window
            for (int prime : primeFactors[nums[right]]) {
                if (primeCount[prime] == 0) {
                    distinctPrimes++;
                }
                primeCount[prime]++;
            }
            // Shrink the window from the left if distinct prime factor count exceeds k
            while (distinctPrimes > k) {
                for (int prime : primeFactors[nums[left]]) {
                    primeCount[prime]--;
                    if (primeCount[prime] == 0) {
                        distinctPrimes--;
                    }
                }
                left++;
            }
            // If the valid window has length >= 1, update maxLen
            if (left <= right) {
                maxLen = Math.max(maxLen, right - left + 1);
            }
        }
        return maxLen;
    }

    private int[] getDistinctPrimeFactors(int n, int[] spf) {
        int[] temp = new int[7];
        int size = 0;
        while (n > 1) {
            int p = spf[n];
            temp[size++] = p;
            while (n % p == 0) {
                n /= p;
            }
        }
        return Arrays.copyOf(temp, size);
    }
}
```