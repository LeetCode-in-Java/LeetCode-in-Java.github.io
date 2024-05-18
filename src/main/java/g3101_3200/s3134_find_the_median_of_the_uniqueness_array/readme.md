[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3134\. Find the Median of the Uniqueness Array

Hard

You are given an integer array `nums`. The **uniqueness array** of `nums` is the sorted array that contains the number of distinct elements of all the subarrays of `nums`. In other words, it is a sorted array consisting of `distinct(nums[i..j])`, for all `0 <= i <= j < nums.length`.

Here, `distinct(nums[i..j])` denotes the number of distinct elements in the subarray that starts at index `i` and ends at index `j`.

Return the **median** of the **uniqueness array** of `nums`.

**Note** that the **median** of an array is defined as the middle element of the array when it is sorted in non-decreasing order. If there are two choices for a median, the **smaller** of the two values is taken.

**Example 1:**

**Input:** nums = [1,2,3]

**Output:** 1

**Explanation:**

The uniqueness array of `nums` is `[distinct(nums[0..0]), distinct(nums[1..1]), distinct(nums[2..2]), distinct(nums[0..1]), distinct(nums[1..2]), distinct(nums[0..2])]` which is equal to `[1, 1, 1, 2, 2, 3]`. The uniqueness array has a median of 1. Therefore, the answer is 1.

**Example 2:**

**Input:** nums = [3,4,3,4,5]

**Output:** 2

**Explanation:**

The uniqueness array of `nums` is `[1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 2, 3, 3, 3]`. The uniqueness array has a median of 2. Therefore, the answer is 2.

**Example 3:**

**Input:** nums = [4,3,5,4]

**Output:** 2

**Explanation:**

The uniqueness array of `nums` is `[1, 1, 1, 1, 2, 2, 2, 3, 3, 3]`. The uniqueness array has a median of 2. Therefore, the answer is 2.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    public int medianOfUniquenessArray(int[] nums) {
        int max = 0;
        for (int x : nums) {
            max = Math.max(max, x);
        }
        int n = nums.length;
        long k = ((long) n * (n + 1) / 2 + 1) / 2;
        int left = 0;
        int right = n / 2;
        while (left <= right) {
            int mid = left + right >> 1;
            if (check(nums, max, mid, k)) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }

    private boolean check(int[] nums, int max, int target, long k) {
        long count = 0;
        int distinct = 0;
        int n = nums.length;
        int left = 0;
        int right = 0;
        int[] freq = new int[max + 1];
        while (right < n) {
            int x = nums[right++];
            if (++freq[x] == 1) {
                distinct++;
            }
            while (distinct > target) {
                x = nums[left++];
                if (--freq[x] == 0) {
                    distinct--;
                }
            }
            count += right - left;
            if (count >= k) {
                return true;
            }
        }
        return false;
    }
}
```