[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3727\. Maximum Alternating Sum of Squares

Medium

You are given an integer array `nums`. You may **rearrange the elements** in any order.

The **alternating score** of an array `arr` is defined as:

*   <code>score = arr[0]<sup>2</sup> - arr[1]<sup>2</sup> + arr[2]<sup>2</sup> - arr[3]<sup>2</sup> + ...</code>

Return an integer denoting the **maximum possible alternating score** of `nums` after rearranging its elements.

**Example 1:**

**Input:** nums = [1,2,3]

**Output:** 12

**Explanation:**

A possible rearrangement for `nums` is `[2,1,3]`, which gives the maximum alternating score among all possible rearrangements.

The alternating score is calculated as:

<code>score = 2<sup>2</sup> - 1<sup>2</sup> + 3<sup>2</sup> = 4 - 1 + 9 = 12</code>

**Example 2:**

**Input:** nums = [1,-1,2,-2,3,-3]

**Output:** 16

**Explanation:**

A possible rearrangement for `nums` is `[-3,-1,-2,1,3,2]`, which gives the maximum alternating score among all possible rearrangements.

The alternating score is calculated as:

<code>score = (-3)<sup>2</sup> - (-1)<sup>2</sup> + (-2)<sup>2</sup> - (1)<sup>2</sup> + (3)<sup>2</sup> - (2)<sup>2</sup> = 9 - 1 + 4 - 1 + 9 - 4 = 16</code>

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>-4 * 10<sup>4</sup> <= nums[i] <= 4 * 10<sup>4</sup></code>

## Solution

```java
public class Solution {
    public long maxAlternatingSum(int[] nums) {
        int n = nums.length;
        int need = n / 2;
        int maxa = 40000;
        int[] freq = new int[maxa + 1];
        long total = 0;
        for (int x : nums) {
            int a = Math.abs(x);
            freq[a]++;
            total += 1L * a * a;
        }
        long small = 0;
        for (int a = 0; a <= maxa && need > 0; a++) {
            int take = Math.min(freq[a], need);
            if (take > 0) {
                small += 1L * a * a * take;
                need -= take;
            }
        }
        return total - 2 * small;
    }
}
```