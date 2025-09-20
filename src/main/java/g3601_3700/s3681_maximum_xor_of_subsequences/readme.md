[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3681\. Maximum XOR of Subsequences

Hard

You are given an integer array `nums` of length `n` where each element is a non-negative integer.

Select **two** **subsequences** of `nums` (they may be empty and are **allowed** to **overlap**), each preserving the original order of elements, and let:

*   `X` be the bitwise XOR of all elements in the first subsequence.
*   `Y` be the bitwise XOR of all elements in the second subsequence.

Return the **maximum** possible value of `X XOR Y`.

**Note:** The XOR of an **empty** subsequence is 0.

**Example 1:**

**Input:** nums = [1,2,3]

**Output:** 3

**Explanation:**

Choose subsequences:

*   First subsequence `[2]`, whose XOR is 2.
*   Second subsequence `[2,3]`, whose XOR is 1.

Then, XOR of both subsequences = `2 XOR 1 = 3`.

This is the maximum XOR value achievable from any two subsequences.

**Example 2:**

**Input:** nums = [5,2]

**Output:** 7

**Explanation:**

Choose subsequences:

*   First subsequence `[5]`, whose XOR is 5.
*   Second subsequence `[2]`, whose XOR is 2.

Then, XOR of both subsequences = `5 XOR 2 = 7`.

This is the maximum XOR value achievable from any two subsequences.

**Constraints:**

*   <code>2 <= nums.length <= 10<sup>5</sup></code>
*   <code>0 <= nums[i] <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    public int maxXorSubsequences(int[] nums) {
        int n = nums.length;
        if (n == 0) {
            return 0;
        }
        int x = 0;
        while (true) {
            int y = 0;
            for (int v : nums) {
                if (v > y) {
                    y = v;
                }
            }
            if (y == 0) {
                return x;
            }
            x = Math.max(x, x ^ y);
            for (int i = 0; i < n; i++) {
                int v = nums[i];
                nums[i] = Math.min(v, v ^ y);
            }
        }
    }
}
```