## 1\. Two Sum

Easy

Given an array of integers `nums` and an integer `target`, return _indices of the two numbers such that they add up to `target`_.

You may assume that each input would have **_exactly_ one solution**, and you may not use the _same_ element twice.

You can return the answer in any order.

**Example 1:**

**Input:** nums = [2,7,11,15], target = 9

**Output:** [0,1]

**Output:** Because nums[0] + nums[1] == 9, we return [0, 1]. 

**Example 2:**

**Input:** nums = [3,2,4], target = 6

**Output:** [1,2] 

**Example 3:**

**Input:** nums = [3,3], target = 6

**Output:** [0,1] 

**Constraints:**

*   <code>2 <= nums.length <= 10<sup>4</sup></code>
*   <code>-10<sup>9</sup> <= nums[i] <= 10<sup>9</sup></code>
*   <code>-10<sup>9</sup> <= target <= 10<sup>9</sup></code>
*   **Only one valid answer exists.**

**Follow-up:** Can you come up with an algorithm that is less than <code>O(n<sup>2</sup>) </code>time complexity?

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int[] twoSum(int[] nums, int target) {
        if (nums == null || nums.length <= 1) {
            return new int[0];
        }
        int[] result = new int[2];
        int left = 0;
        int right = nums.length - 1;
        int[] nums1 = Arrays.copyOf(nums, nums.length);
        Arrays.sort(nums1);
        while (left < right) {
            if (nums1[left] + nums1[right] == target) {
                break;
            } else if (nums1[left] + nums1[right] > target) {
                right--;
            } else if (nums1[left] + nums1[right] < target) {
                left++;
            }
        }

        for (int i = 0; i < nums.length; i++) {
            if (nums1[left] == nums[i]) {
                result[0] = i;
                break;
            }
        }
        for (int j = nums.length - 1; j >= 0; j--) {
            if (nums1[right] == nums[j]) {
                result[1] = j;
                break;
            }
        }

        int tmp = result[0];
        result[0] = Math.min(result[0], result[1]);
        result[1] = Math.max(tmp, result[1]);
        return result;
    }
}
```