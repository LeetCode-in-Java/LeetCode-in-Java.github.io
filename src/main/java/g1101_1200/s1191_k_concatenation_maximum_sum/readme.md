[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1191\. K-Concatenation Maximum Sum

Medium

Given an integer array `arr` and an integer `k`, modify the array by repeating it `k` times.

For example, if `arr = [1, 2]` and `k = 3` then the modified array will be `[1, 2, 1, 2, 1, 2]`.

Return the maximum sub-array sum in the modified array. Note that the length of the sub-array can be `0` and its sum in that case is `0`.

As the answer can be very large, return the answer **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** arr = [1,2], k = 3

**Output:** 9

**Example 2:**

**Input:** arr = [1,-2,1], k = 5

**Output:** 2

**Example 3:**

**Input:** arr = [-1,-2], k = 7

**Output:** 0

**Constraints:**

*   <code>1 <= arr.length <= 10<sup>5</sup></code>
*   <code>1 <= k <= 10<sup>5</sup></code>
*   <code>-10<sup>4</sup> <= arr[i] <= 10<sup>4</sup></code>

## Solution

```java
public class Solution {
    private long mod = 1000000007;

    public int kConcatenationMaxSum(int[] arr, int k) {
        // int kadane = Kadane(arr);
        // #1 when k 1 simply calculate kadanes
        if (k == 1) {
            return (int) (kadane(arr) % mod);
        }
        // #2 else calculate the total sum and then check if sum is -Ve or +Ve
        long totalSum = 0;
        for (int i : arr) {
            totalSum += i;
        }
        // #3 when negative then calculate of arr 2 times only the answer is in there only
        if (totalSum < 0) {
            // when -ve sum put a extra check here of max from 0
            return (int) Math.max(kadaneTwo(arr) % mod, 0);
        } else {
            // #4 when sum is positve then the ans is kadane of 2 + sum * (k-2);
            // these two are used sUm*(k-2) ensures that all other are also included
            return (int) ((kadaneTwo(arr) + ((k - 2) * totalSum) + mod) % mod);
        }
    }

    private long kadane(int[] arr) {
        long max = arr[0];
        long cur = 0;
        for (int n : arr) {
            cur += n;
            max = Math.max(max, cur);
            if (cur < 0) {
                cur = 0;
            }
        }
        return max;
    }

    private long kadaneTwo(int[] arr) {
        long max = arr[0];
        long cur = 0;
        for (int i = 0; i < arr.length * 2; i++) {
            cur += arr[i % arr.length];
            max = Math.max(max, cur);
            if (cur < 0) {
                cur = 0;
            }
        }
        return max;
    }
}
```