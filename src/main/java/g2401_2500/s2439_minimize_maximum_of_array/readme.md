[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2439\. Minimize Maximum of Array

Medium

You are given a **0-indexed** array `nums` comprising of `n` non-negative integers.

In one operation, you must:

*   Choose an integer `i` such that `1 <= i < n` and `nums[i] > 0`.
*   Decrease `nums[i]` by 1.
*   Increase `nums[i - 1]` by 1.

Return _the **minimum** possible value of the **maximum** integer of_ `nums` _after performing **any** number of operations_.

**Example 1:**

**Input:** nums = [3,7,1,6]

**Output:** 5

**Explanation:** One set of optimal operations is as follows: 
1. Choose i = 1, and nums becomes [4,6,1,6]. 
2. Choose i = 3, and nums becomes [4,6,2,5].
3. Choose i = 1, and nums becomes [5,5,2,5]. 

The maximum integer of nums is 5. It can be shown that the maximum number cannot be less than 5. 

Therefore, we return 5.

**Example 2:**

**Input:** nums = [10,1]

**Output:** 10

**Explanation:** It is optimal to leave nums as is, and since 10 is the maximum value, we return 10.

**Constraints:**

*   `n == nums.length`
*   <code>2 <= n <= 10<sup>5</sup></code>
*   <code>0 <= nums[i] <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    public int minimizeArrayValue(int[] nums) {
        long max = 0;
        long sum = 0;
        for (int i = 0; i < nums.length; i++) {
            sum += nums[i];
            max = Math.max(max, (sum + i) / (i + 1));
        }
        return (int) max;
    }
}
```