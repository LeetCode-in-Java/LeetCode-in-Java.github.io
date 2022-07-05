[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1802\. Maximum Value at a Given Index in a Bounded Array

Medium

You are given three positive integers: `n`, `index`, and `maxSum`. You want to construct an array `nums` (**0-indexed**) that satisfies the following conditions:

*   `nums.length == n`
*   `nums[i]` is a **positive** integer where `0 <= i < n`.
*   `abs(nums[i] - nums[i+1]) <= 1` where `0 <= i < n-1`.
*   The sum of all the elements of `nums` does not exceed `maxSum`.
*   `nums[index]` is **maximized**.

Return `nums[index]` _of the constructed array_.

Note that `abs(x)` equals `x` if `x >= 0`, and `-x` otherwise.

**Example 1:**

**Input:** n = 4, index = 2, maxSum = 6

**Output:** 2

**Explanation:** nums = [1,2,**2**,1] is one array that satisfies all the conditions. There are no arrays that satisfy all the conditions and have nums[2] == 3, so 2 is the maximum nums[2].

**Example 2:**

**Input:** n = 6, index = 1, maxSum = 10

**Output:** 3

**Constraints:**

*   <code>1 <= n <= maxSum <= 10<sup>9</sup></code>
*   `0 <= index < n`

## Solution

```java
public class Solution {
    private boolean isPossible(int n, int index, int maxSum, int value) {
        int leftValue = Math.max(value - index, 0);
        int rightValue = Math.max(value - ((n - 1) - index), 0);
        long sumBefore = (long) (value + leftValue) * (value - leftValue + 1) / 2;
        long sumAfter = (long) (value + rightValue) * (value - rightValue + 1) / 2;
        return sumBefore + sumAfter - value <= maxSum;
    }

    public int maxValue(int n, int index, int maxSum) {
        int left = 0;
        int right = maxSum - n;
        while (left < right) {
            int middle = (left + right + 1) / 2;
            if (isPossible(n, index, maxSum - n, middle)) {
                left = middle;
            } else {
                right = middle - 1;
            }
        }
        return left + 1;
    }
}
```