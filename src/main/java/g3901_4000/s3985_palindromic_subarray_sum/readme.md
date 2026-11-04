[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3985\. Palindromic Subarray Sum

Hard

You are given an integer array `nums`.

Return the maximum possible sum of a non-empty subarrays of `nums` that is a palindrome.

**Example 1:**

**Input:** nums = [10,10]

**Output:** 20

**Explanation:**

The whole array `[10,10]` is a palindrome. Therefore, the maximum sum is `10 + 10 = 20`.

**Example 2:**

**Input:** nums = [1,2,3,2,1,5,6]

**Output:** 9

**Explanation:**

The contiguous subarray `[1,2,3,2,1]` is a palindrome. Its sum is `1 + 2 + 3 + 2 + 1 = 9` and it is the maximum sum.

**Example 3:**

**Input:** nums = [7,1,2,1,7,3,4,3,4]

**Output:** 18

**Explanation:**

The contiguous subarray `[7,1,2,1,7]` is a palindrome. Its sum is `7 + 1 + 2 + 1 + 7 = 18` and it is the maximum sum.

**Example 4:**

**Input:** nums = [1,2,3,4,5]

**Output:** 5

**Explanation:**

No subarray with length greater than 1 is a palindrome. The largest element in the array is 5. Therefore, the answer is 5.

**Example 5:**

**Input:** nums = [1000]

**Output:** 1000

**Explanation:**

The subarray with only one element is a palindrome. Therefore, the answer is 1000.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    public long getSum(int[] nums) {
        long[] prefix = buildPrefixSum(nums);
        long oddMax = calculateOdd(nums, prefix);
        long evenMax = calculateEven(nums, prefix);
        return Math.max(oddMax, evenMax);
    }

    private long[] buildPrefixSum(int[] nums) {
        int n = nums.length;
        long[] prefix = new long[n + 1];
        for (int i = 0; i < n; i++) {
            prefix[i + 1] = prefix[i] + nums[i];
        }
        return prefix;
    }

    private long calculateOdd(int[] nums, long[] prefix) {
        int n = nums.length;
        int[] d1 = new int[n];
        int l = 0;
        int r = -1;
        long ans = 0;
        for (int i = 0; i < n; i++) {
            int k = (i > r) ? 1 : Math.min(d1[l + r - i], r - i + 1);
            k = expandOdd(nums, i, k);
            d1[i] = k;
            int left = i - k + 1;
            int right = i + k - 1;
            ans = Math.max(ans, getRangeSum(prefix, left, right));
            if (right > r) {
                l = left;
                r = right;
            }
        }
        return ans;
    }

    private int expandOdd(int[] nums, int i, int k) {
        int n = nums.length;
        while (i - k >= 0 && i + k < n && nums[i - k] == nums[i + k]) {
            k++;
        }
        return k;
    }

    private long calculateEven(int[] nums, long[] prefix) {
        int n = nums.length;
        int[] d2 = new int[n];
        int l = 0;
        int r = -1;
        long ans = 0;
        for (int i = 0; i < n; i++) {
            int k = (i > r) ? 0 : Math.min(d2[l + r - i + 1], r - i + 1);
            k = expandEven(nums, i, k);
            d2[i] = k;
            if (k > 0) {
                int left = i - k;
                int right = i + k - 1;
                ans = Math.max(ans, getRangeSum(prefix, left, right));
                if (right > r) {
                    l = left;
                    r = right;
                }
            }
        }
        return ans;
    }

    private int expandEven(int[] nums, int i, int k) {
        while (i - k - 1 >= 0 && i + k < nums.length && nums[i - k - 1] == nums[i + k]) {
            k++;
        }
        return k;
    }

    private long getRangeSum(long[] prefix, int left, int right) {
        return prefix[right + 1] - prefix[left];
    }
}
```