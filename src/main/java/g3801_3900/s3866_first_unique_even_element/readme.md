[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3866\. First Unique Even Element

Easy

You are given an integer array `nums`.

Return an integer denoting the first **even** integer (earliest by array index) that appears **exactly** once in `nums`. If no such integer exists, return -1.

An integer `x` is considered **even** if it is divisible by 2.

**Example 1:**

**Input:** nums = [3,4,2,5,4,6]

**Output:** 2

**Explanation:**

Both 2 and 6 are even and they appear exactly once. Since 2 occurs first in the array, the answer is 2.

**Example 2:**

**Input:** nums = [4,4]

**Output:** \-1

**Explanation:**

No even integer appears exactly once, so return -1.

**Constraints:**

*   `1 <= nums.length <= 100`
*   `1 <= nums[i] <= 100`

## Solution

```java
public class Solution {
    public int firstUniqueEven(int[] nums) {
        int[] arr = new int[100];
        for (int num : nums) {
            arr[num - 1]++;
        }
        for (int num : nums) {
            if (num % 2 == 0 && (arr[num - 1] == 1)) {
                return num;
            }
        }
        return -1;
    }
}
```