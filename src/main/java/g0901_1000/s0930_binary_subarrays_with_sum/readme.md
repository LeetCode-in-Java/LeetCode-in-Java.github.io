[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 930\. Binary Subarrays With Sum

Medium

Given a binary array `nums` and an integer `goal`, return _the number of non-empty **subarrays** with a sum_ `goal`.

A **subarray** is a contiguous part of the array.

**Example 1:**

**Input:** nums = [1,0,1,0,1], goal = 2

**Output:** 4

**Explanation:** The 4 subarrays are bolded and underlined below: 
    
[**1,0,1**,0,1] 

[**1,0,1,0**,1] 

[1,**0,1,0,1**] 

[1,0,**1,0,1**]

**Example 2:**

**Input:** nums = [0,0,0,0,0], goal = 0

**Output:** 15

**Constraints:**

*   <code>1 <= nums.length <= 3 * 10<sup>4</sup></code>
*   `nums[i]` is either `0` or `1`.
*   `0 <= goal <= nums.length`

## Solution

```java
public class Solution {
    public int numSubarraysWithSum(int[] nums, int goal) {
        return goal == 0
                ? numSubarraysWithAtMostGoal(nums, 0)
                : numSubarraysWithAtMostGoal(nums, goal)
                        - numSubarraysWithAtMostGoal(nums, goal - 1);
    }

    private int numSubarraysWithAtMostGoal(int[] nums, int goal) {
        int windowSum = 0;
        int left = 0;
        int right = 0;
        int res = 0;
        while (right < nums.length) {
            windowSum += nums[right++];
            while (left < right && windowSum > goal) {
                windowSum -= nums[left++];
            }
            res += (right - left);
        }
        return res;
    }
}
```