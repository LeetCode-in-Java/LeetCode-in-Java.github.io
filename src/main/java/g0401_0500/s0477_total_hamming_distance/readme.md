[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 477\. Total Hamming Distance

Medium

The [Hamming distance](https://en.wikipedia.org/wiki/Hamming_distance) between two integers is the number of positions at which the corresponding bits are different.

Given an integer array `nums`, return _the sum of **Hamming distances** between all the pairs of the integers in_ `nums`.

**Example 1:**

**Input:** nums = [4,14,2]

**Output:** 6

**Explanation:** In binary representation, the 4 is 0100, 14 is 1110, and 2 is 0010 (just showing the four bits relevant in this case). The answer will be: HammingDistance(4, 14) + HammingDistance(4, 2) + HammingDistance(14, 2) = 2 + 2 + 2 = 6.

**Example 2:**

**Input:** nums = [4,14,4]

**Output:** 4

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>4</sup></code>
*   <code>0 <= nums[i] <= 10<sup>9</sup></code>
*   The answer for the given input will fit in a **32-bit** integer.

## Solution

```java
public class Solution {
    public int totalHammingDistance(int[] nums) {
        int ans = 0;
        int n = nums.length;
        for (int i = 0; i < 32; i++) {
            int ones = 0;
            for (int k : nums) {
                ones += ((k >> i) & 1);
            }
            ans = ans + (ones * (n - ones));
        }
        return ans;
    }
}
```