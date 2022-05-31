## 628\. Maximum Product of Three Numbers

Easy

Given an integer array `nums`, _find three numbers whose product is maximum and return the maximum product_.

**Example 1:**

**Input:** nums = [1,2,3]

**Output:** 6

**Example 2:**

**Input:** nums = [1,2,3,4]

**Output:** 24

**Example 3:**

**Input:** nums = [-1,-2,-3]

**Output:** -6

**Constraints:**

*   <code>3 <= nums.length <= 10<sup>4</sup></code>
*   `-1000 <= nums[i] <= 1000`

## Solution

```java
public class Solution {
    public int maximumProduct(int[] nums) {
        int min1 = Integer.MAX_VALUE;
        int min2 = Integer.MAX_VALUE;
        int max1 = Integer.MIN_VALUE;
        int max2 = Integer.MIN_VALUE;
        int max3 = Integer.MIN_VALUE;
        for (int i : nums) {
            if (i > max1) {
                max3 = max2;
                max2 = max1;
                max1 = i;
            } else if (i > max2) {
                max3 = max2;
                max2 = i;
            } else if (i > max3) {
                max3 = i;
            }
            if (i < min1) {
                min2 = min1;
                min1 = i;
            } else if (i < min2) {
                min2 = i;
            }
        }
        return Math.max(min1 * min2 * max1, max1 * max2 * max3);
    }
}
```