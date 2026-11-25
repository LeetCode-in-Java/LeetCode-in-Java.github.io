[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 4038\. Count Integers Appearing in a Single Block

Easy

You are given an integer array `nums`.

An integer `x` is **special** if all occurrences of `x` in `nums` appear in a single **contiguous** block.

Return the number of **distinct** special integers in `nums`.

**Example 1:**

**Input:** nums = [1,2,2,1]

**Output:** 1

**Explanation:**

*   1 appears at indices 0 and 3, forming two separate blocks, so it is not special.
*   2 appears in a single contiguous block at indices `[1, 2]`, so it is special.

Therefore, there is one special integer.

**Example 2:**

**Input:** nums = [3,3,1,2,2,1]

**Output:** 2

**Explanation:**

*   3 appears in a single contiguous block at indices `[0, 1]`, so it is special.
*   1 appears at indices 2 and 5, forming two separate blocks, so it is not special.
*   2 appears in a single contiguous block at indices `[3, 4]`, so it is special.

Therefore, there are two special integers.

**Constraints:**

*   `1 <= nums.length <= 100`
*   `1 <= nums[i] <= 100`

## Solution

```java
public class Solution {
    public int countSpecialIntegers(int[] nums) {
        boolean[] seen = new boolean[101];
        boolean[] special = new boolean[101];
        int count = 0;
        for (int i = 0; i < nums.length; i++) {
            if (i == 0 || nums[i] != nums[i - 1]) {
                if (seen[nums[i]]) {
                    special[nums[i]] = false;
                } else {
                    seen[nums[i]] = true;
                    special[nums[i]] = true;
                }
            }
        }
        for (int x = 1; x <= 100; x++) {
            if (special[x]) {
                count++;
            }
        }
        return count;
    }
}
```