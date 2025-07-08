[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3605\. Minimum Stability Factor of Array

Hard

You are given an integer array `nums` and an integer `maxC`.

A **subarray** is called **stable** if the _highest common factor (HCF)_ of all its elements is **greater than or equal to** 2.

The **stability factor** of an array is defined as the length of its **longest** stable subarray.

You may modify **at most** `maxC` elements of the array to any integer.

Return the **minimum** possible stability factor of the array after at most `maxC` modifications. If no stable subarray remains, return 0.

**Note:**

*   The **highest common factor (HCF)** of an array is the largest integer that evenly divides all the array elements.
*   A **subarray** of length 1 is stable if its only element is greater than or equal to 2, since `HCF([x]) = x`.

**Example 1:**

**Input:** nums = [3,5,10], maxC = 1

**Output:** 1

**Explanation:**

*   The stable subarray `[5, 10]` has `HCF = 5`, which has a stability factor of 2.
*   Since `maxC = 1`, one optimal strategy is to change `nums[1]` to `7`, resulting in `nums = [3, 7, 10]`.
*   Now, no subarray of length greater than 1 has `HCF >= 2`. Thus, the minimum possible stability factor is 1.

**Example 2:**

**Input:** nums = [2,6,8], maxC = 2

**Output:** 1

**Explanation:**

*   The subarray `[2, 6, 8]` has `HCF = 2`, which has a stability factor of 3.
*   Since `maxC = 2`, one optimal strategy is to change `nums[1]` to 3 and `nums[2]` to 5, resulting in `nums = [2, 3, 5]`.
*   Now, no subarray of length greater than 1 has `HCF >= 2`. Thus, the minimum possible stability factor is 1.

**Example 3:**

**Input:** nums = [2,4,9,6], maxC = 1

**Output:** 2

**Explanation:**

*   The stable subarrays are:
    *   `[2, 4]` with `HCF = 2` and stability factor of 2.
    *   `[9, 6]` with `HCF = 3` and stability factor of 2.
*   Since `maxC = 1`, the stability factor of 2 cannot be reduced due to two separate stable subarrays. Thus, the minimum possible stability factor is 2.

**Constraints:**

*   <code>1 <= n == nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   `0 <= maxC <= n`

## Solution

```java
public class Solution {
    public int minStable(int[] nums, int maxC) {
        int n = nums.length;
        int cnt = 0;
        int idx = 0;
        while (idx < n) {
            cnt += nums[idx] >= 2 ? 1 : 0;
            idx++;
        }
        if (cnt <= maxC) {
            return 0;
        }
        int[] logs = new int[n + 1];
        int maxLog = 0;
        int temp = n;
        while (temp > 0) {
            maxLog++;
            temp >>= 1;
        }
        int[][] table = new int[maxLog + 1][n];
        buildLogs(logs, n);
        buildTable(table, nums, n, maxLog);
        return binarySearch(nums, maxC, n, logs, table);
    }

    private void buildLogs(int[] logs, int n) {
        int i = 2;
        while (i <= n) {
            logs[i] = logs[i >> 1] + 1;
            i++;
        }
    }

    private void buildTable(int[][] table, int[] nums, int n, int maxLog) {
        System.arraycopy(nums, 0, table[0], 0, n);
        int level = 1;
        while (level <= maxLog) {
            int start = 0;
            while (start + (1 << level) <= n) {
                table[level][start] =
                        gcd(table[level - 1][start], table[level - 1][start + (1 << (level - 1))]);
                start++;
            }
            level++;
        }
    }

    private int binarySearch(int[] nums, int maxC, int n, int[] logs, int[][] table) {
        int left = 1;
        int right = n;
        int result = n;
        while (left <= right) {
            int mid = left + ((right - left) >> 1);
            boolean valid = isValid(nums, maxC, mid, logs, table);
            result = valid ? mid : result;
            int newLeft = valid ? left : mid + 1;
            int newRight = valid ? mid - 1 : right;
            left = newLeft;
            right = newRight;
        }
        return result;
    }

    private boolean isValid(int[] arr, int limit, int segLen, int[] logs, int[][] table) {
        int n = arr.length;
        int window = segLen + 1;
        int cuts = 0;
        int prevCut = -1;
        int pos = 0;
        while (pos + window - 1 < n && cuts <= limit) {
            int rangeGcd = getRangeGcd(pos, pos + window - 1, logs, table);
            if (rangeGcd >= 2) {
                boolean shouldCut = prevCut < pos;
                cuts += shouldCut ? 1 : 0;
                prevCut = shouldCut ? pos + window - 1 : prevCut;
            }
            pos++;
        }
        return cuts <= limit;
    }

    private int getRangeGcd(int left, int right, int[] logs, int[][] table) {
        int k = logs[right - left + 1];
        return gcd(table[k][left], table[k][right - (1 << k) + 1]);
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```