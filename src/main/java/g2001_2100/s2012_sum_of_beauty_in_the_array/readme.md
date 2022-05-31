## 2012\. Sum of Beauty in the Array

Medium

You are given a **0-indexed** integer array `nums`. For each index `i` (`1 <= i <= nums.length - 2`) the **beauty** of `nums[i]` equals:

*   `2`, if `nums[j] < nums[i] < nums[k]`, for **all** `0 <= j < i` and for **all** `i < k <= nums.length - 1`.
*   `1`, if `nums[i - 1] < nums[i] < nums[i + 1]`, and the previous condition is not satisfied.
*   `0`, if none of the previous conditions holds.

Return _the **sum of beauty** of all_ `nums[i]` _where_ `1 <= i <= nums.length - 2`.

**Example 1:**

**Input:** nums = [1,2,3]

**Output:** 2

**Explanation:** For each index i in the range 1 <= i <= 1: - The beauty of nums[1] equals 2.

**Example 2:**

**Input:** nums = [2,4,6,4]

**Output:** 1

**Explanation:** For each index i in the range 1 <= i <= 2: 

- The beauty of nums[1] equals 1.

- The beauty of nums[2] equals 0.

**Example 3:**

**Input:** nums = [3,2,1]

**Output:** 0

**Explanation:** For each index i in the range 1 <= i <= 1: - The beauty of nums[1] equals 0.

**Constraints:**

*   <code>3 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    public int sumOfBeauties(int[] nums) {
        int[] maxArr = new int[nums.length];
        maxArr[0] = nums[0];
        for (int i = 1; i < nums.length - 1; i++) {
            maxArr[i] = Math.max(maxArr[i - 1], nums[i]);
        }
        int[] minArr = new int[nums.length];
        minArr[nums.length - 1] = nums[nums.length - 1];
        for (int i = nums.length - 2; i >= 0; i--) {
            minArr[i] = Math.min(minArr[i + 1], nums[i]);
        }

        int sum = 0;
        for (int i = 1; i < nums.length - 1; i++) {
            if (nums[i] > maxArr[i - 1] && nums[i] < minArr[i + 1]) {
                sum += 2;
            } else if (nums[i] > nums[i - 1] && nums[i] < nums[i + 1]) {
                sum += 1;
            }
        }

        return sum;
    }
}
```