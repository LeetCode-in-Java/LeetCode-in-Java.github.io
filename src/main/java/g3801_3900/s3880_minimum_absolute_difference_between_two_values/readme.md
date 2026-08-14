[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3880\. Minimum Absolute Difference Between Two Values

Easy

You are given an integer array `nums` consisting only of 0, 1, and 2.

A pair of indices `(i, j)` is called **valid** if `nums[i] == 1` and `nums[j] == 2`.

Return the **minimum** absolute difference between `i` and `j` among all valid pairs. If no valid pair exists, return -1.

The absolute difference between indices `i` and `j` is defined as `abs(i - j)`.

**Example 1:**

**Input:** nums = [1,0,0,2,0,1]

**Output:** 2

**Explanation:**

The valid pairs are:

*   (0, 3) which has absolute difference of `abs(0 - 3) = 3`.
*   (5, 3) which has absolute difference of `abs(5 - 3) = 2`.

Thus, the answer is 2.

**Example 2:**

**Input:** nums = [1,0,1,0]

**Output:** \-1

**Explanation:**

There are no valid pairs in the array, thus the answer is -1.

**Constraints:**

*   `1 <= nums.length <= 100`
*   `0 <= nums[i] <= 2`

## Solution

```java
public class Solution {
    public int minAbsoluteDifference(int[] nums) {
        int min = Integer.MAX_VALUE;
        int n = nums.length;
        int prev = -1;
        int last = -1;
        for (int i = 0; i < n; i++) {
            if (prev == -1) {
                if (nums[i] == 1) {
                    prev = 1;
                    last = i;
                } else if (nums[i] == 2) {
                    prev = 2;
                    last = i;
                }
            } else {
                if (nums[i] == 1) {
                    if (prev == 2) {
                        min = Math.min(min, i - last);
                        prev = 1;
                    }
                    last = i;
                } else if (nums[i] == 2) {
                    if (prev == 1) {
                        min = Math.min(min, i - last);
                        prev = 2;
                    }
                    last = i;
                }
            }
        }
        return min != Integer.MAX_VALUE ? min : -1;
    }
}
```