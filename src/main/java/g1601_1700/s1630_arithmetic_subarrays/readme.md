[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1630\. Arithmetic Subarrays

Medium

A sequence of numbers is called **arithmetic** if it consists of at least two elements, and the difference between every two consecutive elements is the same. More formally, a sequence `s` is arithmetic if and only if `s[i+1] - s[i] == s[1] - s[0]` for all valid `i`.

For example, these are **arithmetic** sequences:

1, 3, 5, 7, 9 7, 7, 7, 7 3, -1, -5, -9

The following sequence is not **arithmetic**:

1, 1, 2, 5, 7

You are given an array of `n` integers, `nums`, and two arrays of `m` integers each, `l` and `r`, representing the `m` range queries, where the <code>i<sup>th</sup></code> query is the range `[l[i], r[i]]`. All the arrays are **0-indexed**.

Return _a list of_ `boolean` _elements_ `answer`_, where_ `answer[i]` _is_ `true` _if the subarray_ `nums[l[i]], nums[l[i]+1], ... , nums[r[i]]` _can be **rearranged** to form an **arithmetic** sequence, and_ `false` _otherwise._

**Example 1:**

**Input:** nums = `[4,6,5,9,3,7]`, l = `[0,0,2]`, r = `[2,3,5]`

**Output:** `[true,false,true]`

**Explanation:** 

In the 0<sup>th</sup> query, the subarray is [4,6,5]. 

This can be rearranged as [6,5,4], which is an arithmetic sequence. 

In the 1<sup>st</sup> query, the subarray is [4,6,5,9]. This cannot be rearranged as an arithmetic sequence. 

In the 2<sup>nd</sup> query, the subarray is `[5,9,3,7]. This` can be rearranged as `[3,5,7,9]`, which is an arithmetic sequence.

**Example 2:**

**Input:** nums = [-12,-9,-3,-12,-6,15,20,-25,-20,-15,-10], l = [0,1,6,4,8,7], r = [4,4,9,7,9,10]

**Output:** [false,true,false,false,true,true]

**Constraints:**

*   `n == nums.length`
*   `m == l.length`
*   `m == r.length`
*   `2 <= n <= 500`
*   `1 <= m <= 500`
*   `0 <= l[i] < r[i] < n`
*   <code>-10<sup>5</sup> <= nums[i] <= 10<sup>5</sup></code>

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<Boolean> checkArithmeticSubarrays(int[] nums, int[] l, int[] r) {
        List<Boolean> result = new ArrayList<>();
        int n = l.length;
        for (int i = 0; i < n; i++) {
            result.add(check(nums, l[i], r[i]));
        }
        return result;
    }

    private boolean check(int[] nums, int l, int r) {
        int n = r - l;
        if (n == 0) {
            return true;
        }
        int max = Integer.MIN_VALUE;
        int min = Integer.MAX_VALUE;
        for (int i = l; i <= r; i++) {
            max = Math.max(max, nums[i]);
            min = Math.min(min, nums[i]);
        }
        if ((max - min) % n != 0) {
            return false;
        }
        int diff = (max - min) / n;
        if (diff == 0) {
            return true;
        }
        boolean[] checked = new boolean[max - min + 1];
        for (int i = l; i <= r; i++) {
            int currentDiff = nums[i] - min;
            if (checked[currentDiff] || currentDiff % diff != 0) {
                return false;
            }
            checked[currentDiff] = true;
        }
        return true;
    }
}
```