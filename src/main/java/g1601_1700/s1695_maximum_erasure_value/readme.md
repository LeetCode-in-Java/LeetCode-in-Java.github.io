[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1695\. Maximum Erasure Value

Medium

You are given an array of positive integers `nums` and want to erase a subarray containing **unique elements**. The **score** you get by erasing the subarray is equal to the **sum** of its elements.

Return _the **maximum score** you can get by erasing **exactly one** subarray._

An array `b` is called to be a subarray of `a` if it forms a contiguous subsequence of `a`, that is, if it is equal to `a[l],a[l+1],...,a[r]` for some `(l,r)`.

**Example 1:**

**Input:** nums = [4,2,4,5,6]

**Output:** 17

**Explanation:** The optimal subarray here is [2,4,5,6].

**Example 2:**

**Input:** nums = [5,2,1,2,5,2,1,2,5]

**Output:** 8

**Explanation:** The optimal subarray here is [5,2,1] or [1,2,5].

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>4</sup></code>

## Solution

```java
public class Solution {
    public int maximumUniqueSubarray(int[] nums) {
        int ans = 0;
        int sum = 0;
        boolean[] seen = new boolean[10001];
        int j = 0;
        for (int num : nums) {
            while (seen[num]) {
                seen[nums[j]] = false;
                sum -= nums[j++];
            }
            seen[num] = true;
            sum += num;
            ans = Math.max(sum, ans);
        }
        return ans;
    }
}
```