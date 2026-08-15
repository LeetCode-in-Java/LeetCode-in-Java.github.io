[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3702\. Longest Subsequence With Non-Zero Bitwise XOR

Medium

You are given an integer array `nums`.

Return the length of the **longest subsequence** in `nums` whose bitwise **XOR** is **non-zero**. If no such **subsequence** exists, return 0.

**Example 1:**

**Input:** nums = [1,2,3]

**Output:** 2

**Explanation:**

One longest subsequence is `[2, 3]`. The bitwise XOR is computed as `2 XOR 3 = 1`, which is non-zero.

**Example 2:**

**Input:** nums = [2,3,4]

**Output:** 3

**Explanation:**

The longest subsequence is `[2, 3, 4]`. The bitwise XOR is computed as `2 XOR 3 XOR 4 = 5`, which is non-zero.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>0 <= nums[i] <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    public int longestSubsequence(int[] nums) {
        int xorSum = 0;
        boolean allZero = true;
        for (int num : nums) {
            xorSum ^= num;
            if (num != 0) {
                allZero = false;
            }
        }
        if (allZero) {
            return 0;
        }
        return xorSum != 0 ? nums.length : nums.length - 1;
    }
}
```
### Complexity Analysis

- **Time Complexity:** **O(N)**, where N is the length of the `nums` array. We iterate through the array exactly once to calculate the total XOR sum and check for non-zero elements.
- **Space Complexity:** **O(1)**. The algorithm only uses two auxiliary variables (`xorSum` and `allZero`), which require constant extra space regardless of the input size.

### Intuition

- If all elements are `0`, no valid subsequence exists, so we return `0`.
- If the XOR sum of the entire array is non-zero, the longest subsequence is the entire array itself, returning `N`.
- If the total XOR sum is `0` (and the array has at least one non-zero element), removing exactly one non-zero element will flip the total XOR to a non-zero value. Thus, the maximum length becomes `N - 1`.
