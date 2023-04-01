[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 611\. Valid Triangle Number

Medium

Given an integer array `nums`, return _the number of triplets chosen from the array that can make triangles if we take them as side lengths of a triangle_.

**Example 1:**

**Input:** nums = [2,2,3,4]

**Output:** 3

**Explanation:** Valid combinations are: 2,3,4 (using the first 2) 2,3,4 (using the second 2) 2,2,3

**Example 2:**

**Input:** nums = [4,2,3,4]

**Output:** 4

**Constraints:**

*   `1 <= nums.length <= 1000`
*   `0 <= nums[i] <= 1000`

## Solution

```java
public class Solution {
    public int triangleNumber(int[] nums) {
        int n;
        int max = 0;
        int[] count = new int[1001];
        for (int i : nums) {
            count[i]++;
            max = Math.max(max, i);
        }
        count[0] = 0;
        int idx = 0;
        for (int i = 1; i <= max; ++i) {
            for (int j = 0; j < count[i]; ++j, ++idx) {
                nums[idx] = i;
            }
            count[i] += count[i - 1];
        }
        n = idx;
        int r = 0;
        for (int i = 0; i < n - 2; ++i) {
            for (int j = i + 1; j < n - 1; ++j) {
                if (nums[i] + nums[j] > max) {
                    r += (n - j) * (n - j - 1) / 2;
                    break;
                }
                r += count[nums[i] + nums[j] - 1] - j - 1;
            }
        }
        return r;
    }
}
```