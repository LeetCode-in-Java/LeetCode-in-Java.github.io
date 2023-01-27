[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2488\. Count Subarrays With Median K

Hard

You are given an array `nums` of size `n` consisting of **distinct** integers from `1` to `n` and a positive integer `k`.

Return _the number of non-empty subarrays in_ `nums` _that have a **median** equal to_ `k`.

**Note**:

*   The median of an array is the **middle** element after sorting the array in **ascending** order. If the array is of even length, the median is the **left** middle element.
    *   For example, the median of `[2,3,1,4]` is `2`, and the median of `[8,4,3,5,1]` is `4`.
*   A subarray is a contiguous part of an array.

**Example 1:**

**Input:** nums = [3,2,1,4,5], k = 4

**Output:** 3

**Explanation:** The subarrays that have a median equal to 4 are: [4], [4,5] and [1,4,5].

**Example 2:**

**Input:** nums = [2,3,1], k = 3

**Output:** 1

**Explanation:** [3] is the only subarray that has a median equal to 3.

**Constraints:**

*   `n == nums.length`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   `1 <= nums[i], k <= n`
*   The integers in `nums` are distinct.

## Solution

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    public int countSubarrays(int[] nums, int k) {
        int idx;
        int n = nums.length;
        int ans = 0;
        for (idx = 0; idx < n; idx++) {
            if (nums[idx] == k) {
                break;
            }
        }
        int[][] arr = new int[n - idx][2];
        int j = 1;
        for (int i = idx + 1; i < n; i++) {
            if (nums[i] < k) {
                arr[j][0] = arr[j - 1][0] + 1;
                arr[j][1] = arr[j - 1][1];
            } else {
                arr[j][1] = arr[j - 1][1] + 1;
                arr[j][0] = arr[j - 1][0];
            }
            j++;
        }
        Map<Integer, Integer> map = new HashMap<>();
        for (int[] ints : arr) {
            int d2 = ints[1] - ints[0];
            map.put(d2, map.getOrDefault(d2, 0) + 1);
        }
        int s1 = 0;
        int g1 = 0;
        for (int i = idx; i >= 0; i--) {
            if (nums[i] < k) {
                s1++;
            } else if (nums[i] > k) {
                g1++;
            }
            int d1 = g1 - s1;
            int cur = map.getOrDefault(-d1, 0) + map.getOrDefault(1 - d1, 0);
            ans += cur;
        }
        return ans;
    }
}
```