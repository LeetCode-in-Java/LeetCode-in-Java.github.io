[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2598\. Smallest Missing Non-negative Integer After Operations

Medium

You are given a **0-indexed** integer array `nums` and an integer `value`.

In one operation, you can add or subtract `value` from any element of `nums`.

*   For example, if `nums = [1,2,3]` and `value = 2`, you can choose to subtract `value` from `nums[0]` to make `nums = [-1,2,3]`.

The MEX (minimum excluded) of an array is the smallest missing **non-negative** integer in it.

*   For example, the MEX of `[-1,2,3]` is `0` while the MEX of `[1,0,3]` is `2`.

Return _the maximum MEX of_ `nums` _after applying the mentioned operation **any number of times**_.

**Example 1:**

**Input:** nums = [1,-10,7,13,6,8], value = 5

**Output:** 4

**Explanation:**

One can achieve this result by applying the following operations:

- Add value to nums[1] twice to make nums = [1,**<ins>0</ins>**,7,13,6,8]

- Subtract value from nums[2] once to make nums = [1,0,**<ins>2</ins>**,13,6,8]

- Subtract value from nums[3] twice to make nums = [1,0,2,**<ins>3</ins>**,6,8]

The MEX of nums is 4. It can be shown that 4 is the maximum MEX we can achieve.

**Example 2:**

**Input:** nums = [1,-10,7,13,6,8], value = 7

**Output:** 2

**Explanation:**

One can achieve this result by applying the following operation:

- subtract value from nums[2] once to make nums = [1,-10,<ins>**0**</ins>,13,6,8]

The MEX of nums is 2. It can be shown that 2 is the maximum MEX we can achieve.

**Constraints:**

*   <code>1 <= nums.length, value <= 10<sup>5</sup></code>
*   <code>-10<sup>9</sup> <= nums[i] <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    public int findSmallestInteger(int[] nums, int value) {
        int n = nums.length;
        if (value == 1) {
            return n;
        }
        int[] a = new int[value];
        for (int num : nums) {
            int k = num % value;
            if (k < 0) {
                k = (value + k) % value;
            }
            a[k]++;
        }
        int[] minsResult = mins(a);
        int min = minsResult[0];
        int minIndex = minsResult[1];
        return min * value + minIndex;
    }

    private int[] mins(int[] a) {
        int n = a.length;
        int min = 100001;
        int minIndex = -1;
        for (int i = 0; i < n; i++) {
            if (a[i] < min) {
                min = a[i];
                minIndex = i;
            }
        }
        return new int[] {min, minIndex};
    }
}
```