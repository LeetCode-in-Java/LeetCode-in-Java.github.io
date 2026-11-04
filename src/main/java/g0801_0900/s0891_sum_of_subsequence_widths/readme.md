[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 891\. Sum of Subsequence Widths

Hard

The **width** of a sequence is the difference between the maximum and minimum elements in the sequence.

Given an array of integers `nums`, return _the sum of the **widths** of all the non-empty **subsequences** of_ `nums`. Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

A **subsequence** is a sequence that can be derived from an array by deleting some or no elements without changing the order of the remaining elements. For example, `[3,6,2,7]` is a subsequence of the array `[0,3,1,6,2,2,7]`.

**Example 1:**

**Input:** nums = [2,1,3]

**Output:** 6

**Explanation:**

The subsequences are [1], [2], [3], [2,1], [2,3], [1,3], [2,1,3].

The corresponding widths are 0, 0, 0, 1, 1, 2, 2.

The sum of these widths is 6. 

**Example 2:**

**Input:** nums = [2]

**Output:** 0 

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    public int sumSubseqWidths(int[] nums) {
        int[] numCnt = new int[100001];
        int[] prefixCnt = new int[100001];
        int[] postfixCnt = new int[100001];
        long mod = (long) 1e9 + 7;
        int n = nums.length + 1;
        long[] pow = new long[n];
        pow[0] = 1;
        for (int i = 1; i < n; i++) {
            pow[i] = (2 * pow[i - 1]) % mod;
        }
        for (int num : nums) {
            numCnt[num]++;
        }
        for (int i = 1; i < prefixCnt.length; i++) {
            prefixCnt[i] = prefixCnt[i - 1] + numCnt[i - 1];
        }
        for (int i = postfixCnt.length - 2; i > 0; i--) {
            postfixCnt[i] = postfixCnt[i + 1] + numCnt[i + 1];
        }
        long result = 0;
        for (int num : nums) {
            if (numCnt[num] != -1) {
                result += (num * (pow[prefixCnt[num]] * (pow[numCnt[num]] - 1))) % mod;
                result -= (num * (pow[postfixCnt[num]] * (pow[numCnt[num]] - 1))) % mod;
                result %= mod;
                numCnt[num] = -1;
            }
        }
        result = (result + mod) % mod;
        return (int) result;
    }
}
```