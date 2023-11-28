[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2815\. Max Pair Sum in an Array

Easy

You are given a **0-indexed** integer array `nums`. You have to find the **maximum** sum of a pair of numbers from `nums` such that the maximum **digit** in both numbers are equal.

Return _the maximum sum or_ `-1` _if no such pair exists_.

**Example 1:**

**Input:** nums = [51,71,17,24,42]

**Output:** 88

**Explanation:**

For i = 1 and j = 2, nums[i] and nums[j] have equal maximum digits with a pair sum of 71 + 17 = 88.

For i = 3 and j = 4, nums[i] and nums[j] have equal maximum digits with a pair sum of 24 + 42 = 66.

It can be shown that there are no other pairs with equal maximum digits, so the answer is 88.

**Example 2:**

**Input:** nums = [1,2,3,4]

**Output:** -1

**Explanation:** No pair exists in nums with equal maximum digits. 

**Constraints:**

*   `2 <= nums.length <= 100`
*   <code>1 <= nums[i] <= 10<sup>4</sup></code>

## Solution

```java
import java.util.Comparator;
import java.util.HashMap;
import java.util.Map;
import java.util.PriorityQueue;

public class Solution {
    public int maxSum(int[] nums) {
        // what we'll return
        int maxSum = -1;
        Map<Integer, PriorityQueue<Integer>> maximumDigitToNumber = new HashMap<>();
        for (int i = 1; i <= 9; ++i) {
            maximumDigitToNumber.put(i, new PriorityQueue<>(Comparator.reverseOrder()));
        }
        for (int n : nums) {
            maximumDigitToNumber.get(getMaximumDigit(n)).add(n);
        }
        for (Map.Entry<Integer, PriorityQueue<Integer>> me : maximumDigitToNumber.entrySet()) {
            if (me.getValue().size() <= 1) {
                continue;
            }
            int sum = me.getValue().poll() + me.getValue().poll();
            maxSum = Math.max(maxSum, sum);
        }
        return maxSum;
    }

    private int getMaximumDigit(int n) {
        int maxDigit = 1;
        for (int nMod10 = n % 10; n > 0; n /= 10, nMod10 = n % 10) {
            maxDigit = Math.max(maxDigit, nMod10);
        }
        return maxDigit;
    }
}
```