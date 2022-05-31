## 643\. Maximum Average Subarray I

Easy

You are given an integer array `nums` consisting of `n` elements, and an integer `k`.

Find a contiguous subarray whose **length is equal to** `k` that has the maximum average value and return _this value_. Any answer with a calculation error less than <code>10<sup>-5</sup></code> will be accepted.

**Example 1:**

**Input:** nums = [1,12,-5,-6,50,3], k = 4

**Output:** 12.75000

**Explanation:** Maximum average is (12 - 5 - 6 + 50) / 4 = 51 / 4 = 12.75

**Example 2:**

**Input:** nums = [5], k = 1

**Output:** 5.00000

**Constraints:**

*   `n == nums.length`
*   <code>1 <= k <= n <= 10<sup>5</sup></code>
*   <code>-10<sup>4</sup> <= nums[i] <= 10<sup>4</sup></code>

## Solution

```java
public class Solution {
    public double findMaxAverage(int[] nums, int k) {
        double windowSum = 0;
        int windowStart = 0;
        double max = Integer.MIN_VALUE;
        for (int windowEnd = 0; windowEnd < nums.length; ++windowEnd) {
            windowSum += nums[windowEnd];
            if (windowEnd >= k - 1) {
                double candidate = windowSum / k;
                max = Math.max(candidate, max);
                windowSum -= nums[windowStart];
                windowStart++;
            }
        }
        return max;
    }
}
```