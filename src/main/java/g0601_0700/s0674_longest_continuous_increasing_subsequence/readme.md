## 674\. Longest Continuous Increasing Subsequence

Easy

Given an unsorted array of integers `nums`, return _the length of the longest **continuous increasing subsequence** (i.e. subarray)_. The subsequence must be **strictly** increasing.

A **continuous increasing subsequence** is defined by two indices `l` and `r` (`l < r`) such that it is `[nums[l], nums[l + 1], ..., nums[r - 1], nums[r]]` and for each `l <= i < r`, `nums[i] < nums[i + 1]`.

**Example 1:**

**Input:** nums = [1,3,5,4,7]

**Output:** 3

**Explanation:** The longest continuous increasing subsequence is [1,3,5] with length 3. Even though [1,3,5,7] is an increasing subsequence, it is not continuous as elements 5 and 7 are separated by element 4.

**Example 2:**

**Input:** nums = [2,2,2,2,2]

**Output:** 1

**Explanation:** The longest continuous increasing subsequence is [2] with length 1. Note that it must be strictly increasing.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>4</sup></code>
*   <code>-10<sup>9</sup> <= nums[i] <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    public int findLengthOfLCIS(int[] nums) {
        int ans = 1;
        int count = 1;
        for (int i = 0; i < nums.length - 1; i++) {
            if (nums[i] < nums[i + 1]) {
                count++;
            } else {
                ans = max(count, ans);
                count = 1;
            }
        }
        return max(ans, count);
    }

    private int max(int n1, int n2) {
        return Math.max(n1, n2);
    }
}
```