[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 4010\. Maximize Pair Strength Using GCD

Easy

You are given an integer array `nums`.

Choose exactly one pair of distinct indices `i` and `j`. The strength of the pair is defined as <code>(nums[i] * nums[j]) / gcd(nums[i], nums[j])<sup>2</sup></code>.

Return the maximum strength over all possible pairs.

**Example 1:**

**Input:** nums = [2,3,5]

**Output:** 15

**Explanation:**

Choosing `i = 1` and `j = 2` gives strength <code>(3 * 5) / gcd(3, 5)<sup>2</sup> = 15 / 1 = 15</code>, which is the maximum over all pairs.

**Example 2:**

**Input:** nums = [4,6,8]

**Output:** 12

**Explanation:**

Choosing `i = 1` and `j = 2` gives strength <code>(6 * 8) / gcd(6, 8)<sup>2</sup> = 48 / 4 = 12</code>, which is the maximum over all pairs.

**Example 3:**

**Input:** nums = [3,3]

**Output:** 1

**Explanation:**

Choosing `i = 0` and `j = 1` gives strength <code>(3 * 3) / gcd(3, 3)<sup>2</sup> = 9 / 9 = 1</code>, the maximum over all pairs.

**Constraints:**

*   `2 <= nums.length <= 2000`
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```java
import java.util.Arrays;

@SuppressWarnings("java:S135")
public class Solution {
    private int gcd(int n, int m) {
        while (m != 0) {
            int remainder = n % m;
            n = m;
            m = remainder;
        }
        return n;
    }

    public long maxPairStrength(int[] nums) {
        long maxStrength = 0;
        int end = nums.length - 1;
        Arrays.sort(nums);
        for (int i = end; i > 0; i--) {
            for (int j = i - 1; j >= 0; j--) {
                long product = (long) nums[i] * nums[j];
                if (product <= maxStrength) {
                    break;
                }
                int hcf = gcd(nums[i], nums[j]);
                if (hcf == 0) {
                    continue;
                }
                long currentStrength = (long) (nums[i] / hcf) * (nums[j] / hcf);
                if (currentStrength > maxStrength) {
                    maxStrength = currentStrength;
                }
            }
        }
        return maxStrength;
    }
}
```