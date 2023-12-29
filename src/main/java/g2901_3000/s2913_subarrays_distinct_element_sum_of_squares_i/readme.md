[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2913\. Subarrays Distinct Element Sum of Squares I

Easy

You are given a **0-indexed** integer array `nums`.

The **distinct count** of a subarray of `nums` is defined as:

*   Let `nums[i..j]` be a subarray of `nums` consisting of all the indices from `i` to `j` such that `0 <= i <= j < nums.length`. Then the number of distinct values in `nums[i..j]` is called the distinct count of `nums[i..j]`.

Return _the sum of the **squares** of **distinct counts** of all subarrays of_ `nums`.

A subarray is a contiguous **non-empty** sequence of elements within an array.

**Example 1:**

**Input:** nums = [1,2,1]

**Output:** 15

**Explanation:** Six possible subarrays are: 

[1]: 1 distinct value 

[2]: 1 distinct value 

[1]: 1 distinct value 

[1,2]: 2 distinct values 

[2,1]: 2 distinct values 

[1,2,1]: 2 distinct values 

The sum of the squares of the distinct counts in all subarrays is equal to 1<sup>2</sup> + 1<sup>2</sup> + 1<sup>2</sup> + 2<sup>2</sup> + 2<sup>2</sup> + 2<sup>2</sup> = 15.

**Example 2:**

**Input:** nums = [1,1]

**Output:** 3

**Explanation:** Three possible subarrays are: 

[1]: 1 distinct value 

[1]: 1 distinct value 

[1,1]: 1 distinct value 

The sum of the squares of the distinct counts in all subarrays is equal to 1<sup>2</sup> + 1<sup>2</sup> + 1<sup>2</sup> = 3.

**Constraints:**

*   `1 <= nums.length <= 100`
*   `1 <= nums[i] <= 100`

## Solution

```java
import java.util.List;

public class Solution {
    public int sumCounts(List<Integer> nums) {
        final int n = nums.size();
        if (n == 1) {
            return 1;
        }
        int[] numsArr = new int[n];
        for (int i = 0; i < n; i++) {
            numsArr[i] = nums.get(i);
        }
        int[] prev = new int[n];
        int[] foundAt = new int[101];
        boolean dupFound = false;
        int j = 0;
        while (j < n) {
            if ((prev[j] = foundAt[numsArr[j]] - 1) >= 0) {
                dupFound = true;
            }
            foundAt[numsArr[j]] = ++j;
        }
        if (!dupFound) {
            return (((((n + 4) * n + 5) * n) + 2) * n) / 12;
        }
        int result = 0;
        for (int start = n - 1; start >= 0; start--) {
            int distinctCount = 0;
            for (int i = start; i < n; i++) {
                if (prev[i] < start) {
                    distinctCount++;
                }
                result += distinctCount * distinctCount;
            }
        }
        return result;
    }
}
```