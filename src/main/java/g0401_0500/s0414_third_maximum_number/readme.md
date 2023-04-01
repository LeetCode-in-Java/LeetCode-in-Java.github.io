[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 414\. Third Maximum Number

Easy

Given an integer array `nums`, return _the **third distinct maximum** number in this array. If the third maximum does not exist, return the **maximum** number_.

**Example 1:**

**Input:** nums = [3,2,1]

**Output:** 1

**Explanation:** The first distinct maximum is 3. The second distinct maximum is 2. The third distinct maximum is 1. 

**Example 2:**

**Input:** nums = [1,2]

**Output:** 2

**Explanation:** The first distinct maximum is 2. The second distinct maximum is 1. The third distinct maximum does not exist, so the maximum (2) is returned instead. 

**Example 3:**

**Input:** nums = [2,2,3,1]

**Output:** 1

**Explanation:** The first distinct maximum is 3. The second distinct maximum is 2 (both 2's are counted together since they have the same value). The third distinct maximum is 1. 

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>4</sup></code>
*   <code>-2<sup>31</sup> <= nums[i] <= 2<sup>31</sup> - 1</code>

**Follow up:** Can you find an `O(n)` solution?

## Solution

```java
public class Solution {
    public int thirdMax(int[] nums) {
        long max1 = Long.MIN_VALUE;
        long max2 = Long.MIN_VALUE;
        long max3 = Long.MIN_VALUE;
        for (int i : nums) {
            max1 = Math.max(max1, i);
        }
        for (int i : nums) {
            if (i == max1) {
                continue;
            }
            max2 = Math.max(max2, i);
        }
        for (int i : nums) {
            if (i == max1 || i == max2) {
                continue;
            }
            max3 = Math.max(max3, i);
        }
        return (int) (max3 == Long.MIN_VALUE ? max1 : max3);
    }
}
```