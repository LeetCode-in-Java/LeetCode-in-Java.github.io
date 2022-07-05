[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 410\. Split Array Largest Sum

Hard

Given an array `nums` which consists of non-negative integers and an integer `m`, you can split the array into `m` non-empty continuous subarrays.

Write an algorithm to minimize the largest sum among these `m` subarrays.

**Example 1:**

**Input:** nums = [7,2,5,10,8], m = 2

**Output:** 18

**Explanation:**

    There are four ways to split nums into two subarrays.
    The best way is to split it into [7,2,5] and [10,8],
    where the largest sum among the two subarrays is only 18. 

**Example 2:**

**Input:** nums = [1,2,3,4,5], m = 2

**Output:** 9 

**Example 3:**

**Input:** nums = [1,4,4], m = 3

**Output:** 4 

**Constraints:**

*   `1 <= nums.length <= 1000`
*   <code>0 <= nums[i] <= 10<sup>6</sup></code>
*   `1 <= m <= min(50, nums.length)`

## Solution

```java
public class Solution {
    public int splitArray(int[] nums, int m) {
        int maxVal = 0;
        int minVal = nums[0];

        for (int num : nums) {
            maxVal += num;
            minVal = Math.max(minVal, num);
        }

        while (minVal < maxVal) {
            int midVal = minVal + (maxVal - minVal) / 2;
            // if we can split, try to reduce the midVal so decrease maxVal
            if (canSplit(midVal, nums, m)) {
                maxVal = midVal;
            } else {
                // if we can't split, then try to increase midVal by increasing minVal
                minVal = midVal + 1;
            }
        }

        return minVal;
    }

    private boolean canSplit(int maxSubArrSum, int[] nums, int m) {
        int currSum = 0;
        int currSplits = 1;
        for (int num : nums) {
            currSum += num;
            if (currSum > maxSubArrSum) {
                currSum = num;
                currSplits++;
                // if maxSubArrSum was TOO high that we can split the array into more that 'm' parts
                // then its not ideal
                if (currSplits > m) {
                    return false;
                }
            }
        }
        return true;
    }
}
```