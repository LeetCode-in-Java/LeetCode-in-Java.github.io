## 1793\. Maximum Score of a Good Subarray

Hard

You are given an array of integers `nums` **(0-indexed)** and an integer `k`.

The **score** of a subarray `(i, j)` is defined as `min(nums[i], nums[i+1], ..., nums[j]) * (j - i + 1)`. A **good** subarray is a subarray where `i <= k <= j`.

Return _the maximum possible **score** of a **good** subarray._

**Example 1:**

**Input:** nums = [1,4,3,7,4,5], k = 3

**Output:** 15

**Explanation:** The optimal subarray is (1, 5) with a score of min(4,3,7,4,5) \* (5-1+1) = 3 \* 5 = 15. 

**Example 2:**

**Input:** nums = [5,5,4,5,4,1,1,1], k = 0

**Output:** 20

**Explanation:** The optimal subarray is (0, 4) with a score of min(5,5,4,5,4) \* (4-0+1) = 4 \* 5 = 20. 

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 2 * 10<sup>4</sup></code>
*   `0 <= k < nums.length`

## Solution

```java
public class Solution {
    public int maximumScore(int[] nums, int k) {
        int i = k;
        int j = k;
        int res = nums[k];
        int min = nums[k];
        boolean goLeft;
        while (i >= 1 || j < nums.length - 1) {
            // sub array [i...j] is already traversed. Either goLeft or goRight to increase the
            // sequence
            if (i == 0) {
                goLeft = false;
            } else if (j == nums.length - 1) {
                goLeft = true;
            } else {
                goLeft = nums[j + 1] <= nums[i - 1];
            }
            min = goLeft ? Math.min(min, nums[i - 1]) : Math.min(min, nums[j + 1]);
            if (goLeft) {
                while (i >= 1 && min <= nums[i - 1]) {
                    i--;
                }
            } else {
                while (j < nums.length - 1 && min <= nums[j + 1]) {
                    j++;
                }
            }
            res = Math.max(res, min * (j - i + 1));
        }
        return res;
    }
}
```