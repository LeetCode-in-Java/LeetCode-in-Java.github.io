## 154\. Find Minimum in Rotated Sorted Array II

Hard

Suppose an array of length `n` sorted in ascending order is **rotated** between `1` and `n` times. For example, the array `nums = [0,1,4,4,5,6,7]` might become:

*   `[4,5,6,7,0,1,4]` if it was rotated `4` times.
*   `[0,1,4,4,5,6,7]` if it was rotated `7` times.

Notice that **rotating** an array `[a[0], a[1], a[2], ..., a[n-1]]` 1 time results in the array `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]`.

Given the sorted rotated array `nums` that may contain **duplicates**, return _the minimum element of this array_.

You must decrease the overall operation steps as much as possible.

**Example 1:**

**Input:** nums = [1,3,5]

**Output:** 1 

**Example 2:**

**Input:** nums = [2,2,2,0,1]

**Output:** 0 

**Constraints:**

*   `n == nums.length`
*   `1 <= n <= 5000`
*   `-5000 <= nums[i] <= 5000`
*   `nums` is sorted and rotated between `1` and `n` times.

**Follow up:** This problem is similar to [Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/description/), but `nums` may contain **duplicates**. Would this affect the runtime complexity? How and why?

## Solution

```java
public class Solution {
    public int findMin(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        return find(0, nums.length - 1, nums);
    }

    private int find(int left, int right, int[] nums) {
        if (left + 1 >= right) {
            return Math.min(nums[left], nums[right]);
        }
        int mid = left + (right - left) / 2;
        if (nums[left] == nums[right] && nums[left] == nums[mid]) {
            return Math.min(find(left, mid, nums), find(mid, right, nums));
        }
        if (nums[left] >= nums[right]) {
            if (nums[mid] >= nums[left]) {
                return find(mid, right, nums);
            } else {
                return find(left, mid, nums);
            }
        } else {
            return find(left, mid, nums);
        }
    }
}
```