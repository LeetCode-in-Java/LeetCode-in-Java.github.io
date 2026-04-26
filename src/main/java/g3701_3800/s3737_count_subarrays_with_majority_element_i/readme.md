[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3737\. Count Subarrays With Majority Element I

Medium

You are given an integer array `nums` and an integer `target`.

Return the number of **non-empty subarrays** of `nums` in which `target` is the **majority element**.

The **majority element** of a subarray is the element that appears **strictly** **more than half** of the times in that subarray.

**Example 1:**

**Input:** nums = [1,2,2,3], target = 2

**Output:** 5

**Explanation:**

Valid subarrays with `target = 2` as the majority element:

*   `nums[1..1] = [2]`
*   `nums[2..2] = [2]`
*   `nums[1..2] = [2,2]`
*   `nums[0..2] = [1,2,2]`
*   `nums[1..3] = [2,2,3]`

So there are 5 such subarrays.

**Example 2:**

**Input:** nums = [1,1,1,1], target = 1

**Output:** 10

**Explanation:**

All 10 subarrays have 1 as the majority element.

**Example 3:**

**Input:** nums = [1,2,3], target = 4

**Output:** 0

**Explanation:**

`target = 4` does not appear in `nums` at all. Therefore, there cannot be any subarray where 4 is the majority element. Hence the answer is 0.

**Constraints:**

*   `1 <= nums.length <= 1000`
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   <code>1 <= target <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    public int countMajoritySubarrays(int[] a, int target) {
        int n = a.length;
        int pre = n + 1;
        int res = 0;
        int[] count = new int[2 * n + 2];
        int[] acc = new int[2 * n + 2];
        count[pre] = acc[pre] = 1;
        for (int i : a) {
            pre += (i == target ? 1 : -1);
            count[pre]++;
            acc[pre] = acc[pre - 1] + count[pre];
            res += acc[pre - 1];
        }
        return res;
    }
}
```