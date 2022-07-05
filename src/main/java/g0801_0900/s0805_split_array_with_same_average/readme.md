[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 805\. Split Array With Same Average

Hard

You are given an integer array `nums`.

You should move each element of `nums` into one of the two arrays `A` and `B` such that `A` and `B` are non-empty, and `average(A) == average(B)`.

Return `true` if it is possible to achieve that and `false` otherwise.

**Note** that for an array `arr`, `average(arr)` is the sum of all the elements of `arr` over the length of `arr`.

**Example 1:**

**Input:** nums = [1,2,3,4,5,6,7,8]

**Output:** true

**Explanation:** We can split the array into [1,4,5,8] and [2,3,6,7], and both of them have an average of 4.5.

**Example 2:**

**Input:** nums = [3,1]

**Output:** false

**Constraints:**

*   `1 <= nums.length <= 30`
*   <code>0 <= nums[i] <= 10<sup>4</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    public boolean splitArraySameAverage(int[] nums) {
        int m = nums.length;
        int sum = 0;
        for (int n : nums) {
            sum += n;
        }
        Arrays.sort(nums);
        for (int len = 1; len <= m / 2; len++) {
            if (sum * len % m == 0 && dfs(nums, sum * len / m, len, 0)) {
                return true;
            }
        }
        return false;
    }

    private boolean dfs(int[] nums, int sum, int len, int idx) {
        if (len == 0) {
            return sum == 0;
        }
        if (sum < 0 || idx >= nums.length) {
            return false;
        }
        if (nums[idx] > sum / len) {
            return false;
        }
        for (int i = idx; i < nums.length; i++) {
            if (i > idx && nums[i] == nums[i - 1]) {
                continue;
            }
            if (dfs(nums, sum - nums[i], len - 1, i + 1)) {
                return true;
            }
        }
        return false;
    }
}
```