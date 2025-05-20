[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3550\. Smallest Index With Digit Sum Equal to Index

Easy

You are given an integer array `nums`.

Return the **smallest** index `i` such that the sum of the digits of `nums[i]` is equal to `i`.

If no such index exists, return `-1`.

**Example 1:**

**Input:** nums = [1,3,2]

**Output:** 2

**Explanation:**

*   For `nums[2] = 2`, the sum of digits is 2, which is equal to index `i = 2`. Thus, the output is 2.

**Example 2:**

**Input:** nums = [1,10,11]

**Output:** 1

**Explanation:**

*   For `nums[1] = 10`, the sum of digits is `1 + 0 = 1`, which is equal to index `i = 1`.
*   For `nums[2] = 11`, the sum of digits is `1 + 1 = 2`, which is equal to index `i = 2`.
*   Since index 1 is the smallest, the output is 1.

**Example 3:**

**Input:** nums = [1,2,3]

**Output:** \-1

**Explanation:**

*   Since no index satisfies the condition, the output is -1.

**Constraints:**

*   `1 <= nums.length <= 100`
*   `0 <= nums[i] <= 1000`

## Solution

```java
public class Solution {
    private int sum(int num) {
        int s = 0;
        while (num > 0) {
            s += num % 10;
            num /= 10;
        }
        return s;
    }

    public int smallestIndex(int[] nums) {
        for (int i = 0; i < nums.length; i++) {
            if (i == sum(nums[i])) {
                return i;
            }
        }
        return -1;
    }
}
```