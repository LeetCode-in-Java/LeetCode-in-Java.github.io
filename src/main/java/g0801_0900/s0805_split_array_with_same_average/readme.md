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
    private int[] nums;
    private int[] sums;

    public boolean splitArraySameAverage(int[] nums) {
        int len = nums.length;
        if (len == 1) {
            return false;
        }
        Arrays.sort(nums);
        sums = new int[len + 1];
        for (int i = 0; i < len; i++) {
            sums[i + 1] = sums[i] + nums[i];
        }
        int sum = sums[len];
        this.nums = nums;
        for (int i = 1, stop = len / 2; i <= stop; i++) {
            if ((sum * i) % len == 0 && findSum(i, len, (sum * i) / len)) {
                return true;
            }
        }
        return false;
    }

    private boolean findSum(int k, int pos, int target) {
        if (k == 1) {
            while (true) {
                if (nums[--pos] <= target) {
                    break;
                }
            }
            return nums[pos] == target;
        }
        for (int i = pos; sums[i] - sums[i-- - k] >= target; ) {
            if (sums[k - 1] <= target - nums[i] && findSum(k - 1, i, target - nums[i])) {
                return true;
            }
        }
        return false;
    }
}
```