## 1330\. Reverse Subarray To Maximize Array Value

Hard

You are given an integer array `nums`. The _value_ of this array is defined as the sum of `|nums[i] - nums[i + 1]|` for all `0 <= i < nums.length - 1`.

You are allowed to select any subarray of the given array and reverse it. You can perform this operation **only once**.

Find maximum possible value of the final array.

**Example 1:**

**Input:** nums = [2,3,1,5,4]

**Output:** 10

**Explanation:** By reversing the subarray [3,1,5] the array becomes [2,5,1,3,4] whose value is 10.

**Example 2:**

**Input:** nums = [2,4,9,24,2,1,10]

**Output:** 68

**Constraints:**

*   <code>1 <= nums.length <= 3 * 10<sup>4</sup></code>
*   <code>-10<sup>5</sup> <= nums[i] <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {

    private int getAbsoluteDifference(int a, int b) {
        return Math.abs(a - b);
    }

    public int maxValueAfterReverse(int[] nums) {
        int n = nums.length;
        int result = 0;
        for (int i = 0; i < n - 1; i++) {
            result += getAbsoluteDifference(nums[i], nums[i + 1]);
        }

        int minLine = Integer.MIN_VALUE;
        int maxLine = Integer.MAX_VALUE;
        for (int i = 0; i < n - 1; i++) {
            minLine = Math.max(minLine, Math.min(nums[i], nums[i + 1]));
            maxLine = Math.min(maxLine, Math.max(nums[i], nums[i + 1]));
        }
        int diff = Math.max(0, (minLine - maxLine) * 2);
        for (int i = 1; i < n - 1; i++) {
            diff =
                    Math.max(
                            diff,
                            getAbsoluteDifference(nums[0], nums[i + 1])
                                    - getAbsoluteDifference(nums[i], nums[i + 1]));
        }

        for (int i = 0; i < n - 1; i++) {
            diff =
                    Math.max(
                            diff,
                            getAbsoluteDifference(nums[n - 1], nums[i])
                                    - getAbsoluteDifference(nums[i + 1], nums[i]));
        }
        return result + diff;
    }
}
```