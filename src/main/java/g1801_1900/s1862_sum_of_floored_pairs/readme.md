[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1862\. Sum of Floored Pairs

Hard

Given an integer array `nums`, return the sum of `floor(nums[i] / nums[j])` for all pairs of indices `0 <= i, j < nums.length` in the array. Since the answer may be too large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

The `floor()` function returns the integer part of the division.

**Example 1:**

**Input:** nums = [2,5,9]

**Output:** 10

**Explanation:** 

floor(2 / 5) = floor(2 / 9) = floor(5 / 9) = 0 

floor(2 / 2) = floor(5 / 5) = floor(9 / 9) = 1 

floor(5 / 2) = 2 

floor(9 / 2) = 4 

floor(9 / 5) = 1 

We calculate the floor of the division for every pair of indices in the array then sum them up.

**Example 2:**

**Input:** nums = [7,7,7,7,7,7,7]

**Output:** 49

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int sumOfFlooredPairs(int[] nums) {
        long mod = 1000000007;
        Arrays.sort(nums);
        int max = nums[nums.length - 1];
        int[] counts = new int[max + 1];
        long[] qnts = new long[max + 1];
        for (int k : nums) {
            counts[k]++;
        }
        for (int i = 1; i < max + 1; i++) {
            if (counts[i] == 0) {
                continue;
            }
            int j = i;
            while (j <= max) {
                qnts[j] += counts[i];
                j = j + i;
            }
        }
        for (int i = 1; i < max + 1; i++) {
            qnts[i] = (qnts[i] + qnts[i - 1]) % mod;
        }
        long sum = 0;
        for (int k : nums) {
            sum = (sum + qnts[k]) % mod;
        }
        return (int) sum;
    }
}
```