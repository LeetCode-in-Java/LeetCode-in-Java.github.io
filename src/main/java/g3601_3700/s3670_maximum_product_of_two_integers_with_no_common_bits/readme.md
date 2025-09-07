[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3670\. Maximum Product of Two Integers With No Common Bits

Medium

You are given an integer array `nums`.

Your task is to find two **distinct** indices `i` and `j` such that the product `nums[i] * nums[j]` is **maximized,** and the binary representations of `nums[i]` and `nums[j]` do not share any common set bits.

Return the **maximum** possible product of such a pair. If no such pair exists, return 0.

**Example 1:**

**Input:** nums = [1,2,3,4,5,6,7]

**Output:** 12

**Explanation:**

The best pair is 3 (011) and 4 (100). They share no set bits and `3 * 4 = 12`.

**Example 2:**

**Input:** nums = [5,6,4]

**Output:** 0

**Explanation:**

Every pair of numbers has at least one common set bit. Hence, the answer is 0.

**Example 3:**

**Input:** nums = [64,8,32]

**Output:** 2048

**Explanation:**

No pair of numbers share a common bit, so the answer is the product of the two maximum elements, 64 and 32 (`64 * 32 = 2048`).

**Constraints:**

*   <code>2 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>6</sup></code>

## Solution

```java
public class Solution {
    public long maxProduct(int[] nums) {
        // Find highest value to limit DP size
        int maxVal = 0;
        for (int v : nums) {
            if (v > maxVal) {
                maxVal = v;
            }
        }
        // If all numbers are >=1, maxVal > 0; compute needed bit-width
        // in [1..20]
        int maxBits = 32 - Integer.numberOfLeadingZeros(maxVal);
        int size = 1 << maxBits;
        // ---- store input midway, as required ----
        // dp[mask] = largest number present whose bitmask == mask (later becomes: max over all
        // submasks)
        int[] dp = new int[size];
        for (int x : nums) {
            // numbers themselves are their masks
            if (dp[x] < x) {
                dp[x] = x;
            }
        }
        // SOS DP: for each bit b, propagate lower-half block maxima to upper-half block
        // (branch-light)
        for (int b = 0; b < maxBits; b++) {
            int half = 1 << b;
            int step = half << 1;
            for (int base = 0; base < size; base += step) {
                int upper = base + half;
                for (int m = 0; m < half; m++) {
                    int u = upper + m;
                    int l = base + m;
                    if (dp[u] < dp[l]) {
                        dp[u] = dp[l];
                    }
                }
            }
        }
        // Now dp[mask] = max value among all submasks of 'mask'
        long ans = 0;
        int full = size - 1;
        for (int x : nums) {
            // masks with no bits in common with x
            int complement = (~x) & full;
            // best partner disjoint with x
            int y = dp[complement];
            if (y > 0) {
                long prod = (long) x * y;
                if (prod > ans) {
                    ans = prod;
                }
            }
        }
        // 0 if no valid pair
        return ans;
    }
}
```