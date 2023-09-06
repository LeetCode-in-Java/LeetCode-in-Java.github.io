[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2653\. Sliding Subarray Beauty

Medium

Given an integer array `nums` containing `n` integers, find the **beauty** of each subarray of size `k`.

The **beauty** of a subarray is the <code>x<sup>th</sup></code> **smallest integer** in the subarray if it is **negative**, or `0` if there are fewer than `x` negative integers.

Return _an integer array containing_ `n - k + 1` _integers, which denote the_ **beauty** _of the subarrays **in order** from the first index in the array._

*   A subarray is a contiguous **non-empty** sequence of elements within an array.


**Example 1:**

**Input:** nums = [1,-1,-3,-2,3], k = 3, x = 2

**Output:** [-1,-2,-2]

**Explanation:** There are 3 subarrays with size k = 3. 

The first subarray is `[1, -1, -3]` and the 2<sup>nd</sup> smallest negative integer is -1.

The second subarray is `[-1, -3, -2]` and the 2<sup>nd</sup> smallest negative integer is -2.

The third subarray is `[-3, -2, 3] `and the 2<sup>nd</sup> smallest negative integer is -2.

**Example 2:**

**Input:** nums = [-1,-2,-3,-4,-5], k = 2, x = 2

**Output:** [-1,-2,-3,-4]

**Explanation:** There are 4 subarrays with size k = 2. 

For `[-1, -2]`, the 2<sup>nd</sup> smallest negative integer is -1. 

For `[-2, -3]`, the 2<sup>nd</sup> smallest negative integer is -2. 

For `[-3, -4]`, the 2<sup>nd</sup> smallest negative integer is -3. 

For `[-4, -5]`, the 2<sup>nd</sup> smallest negative integer is -4.

**Example 3:**

**Input:** nums = [-3,1,2,-3,0,-3], k = 2, x = 1

**Output:** [-3,0,-3,-3,-3]

**Explanation:** There are 5 subarrays with size k = 2**.** 

For `[-3, 1]`, the 1<sup>st</sup> smallest negative integer is -3. 

For `[1, 2]`, there is no negative integer so the beauty is 0. 

For `[2, -3]`, the 1<sup>st</sup> smallest negative integer is -3. 

For `[-3, 0]`, the 1<sup>st</sup> smallest negative integer is -3. 

For `[0, -3]`, the 1<sup>st</sup> smallest negative integer is -3.

**Constraints:**

*   `n == nums.length`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   `1 <= k <= n`
*   `1 <= x <= k`
*   `-50 <= nums[i] <= 50`

## Solution

```java
@SuppressWarnings("java:S135")
public class Solution {
    public int[] getSubarrayBeauty(int[] nums, int k, int x) {
        int[] arr = new int[101];
        int j = 0;
        int[] ans = new int[nums.length - k + 1];
        int ind = 0;
        for (int i = 0; i < nums.length; i++) {
            arr[nums[i] + 50]++;
            if (i < k - 1) {
                continue;
            }
            if (ind > 0 && ans[ind - 1] < nums[i] && ans[ind - 1] < nums[j - 1]) {
                arr[nums[j++] + 50]--;
                ans[ind] = ans[ind - 1];
                ind++;
                continue;
            }
            int count = 0;
            int si = -1;
            while (count < x) {
                si++;
                if (arr[si] != 0) {
                    count += arr[si];
                }
            }
            arr[nums[j++] + 50]--;
            ans[ind++] = (si - 50 < 1) ? si - 50 : 0;
        }
        return ans;
    }
}
```