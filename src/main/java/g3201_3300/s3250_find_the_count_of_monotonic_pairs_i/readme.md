[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3250\. Find the Count of Monotonic Pairs I

Hard

You are given an array of **positive** integers `nums` of length `n`.

We call a pair of **non-negative** integer arrays `(arr1, arr2)` **monotonic** if:

*   The lengths of both arrays are `n`.
*   `arr1` is monotonically **non-decreasing**, in other words, `arr1[0] <= arr1[1] <= ... <= arr1[n - 1]`.
*   `arr2` is monotonically **non-increasing**, in other words, `arr2[0] >= arr2[1] >= ... >= arr2[n - 1]`.
*   `arr1[i] + arr2[i] == nums[i]` for all `0 <= i <= n - 1`.

Return the count of **monotonic** pairs.

Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** nums = [2,3,2]

**Output:** 4

**Explanation:**

The good pairs are:

1.  `([0, 1, 1], [2, 2, 1])`
2.  `([0, 1, 2], [2, 2, 0])`
3.  `([0, 2, 2], [2, 1, 0])`
4.  `([1, 2, 2], [1, 1, 0])`

**Example 2:**

**Input:** nums = [5,5,5,5]

**Output:** 126

**Constraints:**

*   `1 <= n == nums.length <= 2000`
*   `1 <= nums[i] <= 50`

## Solution

```java
public class Solution {
    public int countOfPairs(int[] nums) {
        int[] maxShift = new int[nums.length];
        maxShift[0] = nums[0];
        int currShift = 0;
        for (int i = 1; i < nums.length; i++) {
            currShift = Math.max(currShift, nums[i] - maxShift[i - 1]);
            maxShift[i] = Math.min(maxShift[i - 1], nums[i] - currShift);
            if (maxShift[i] < 0) {
                return 0;
            }
        }
        int[][] cases = getAllCases(nums, maxShift);
        return cases[nums.length - 1][maxShift[nums.length - 1]];
    }

    private int[][] getAllCases(int[] nums, int[] maxShift) {
        int[] currCases;
        int[][] cases = new int[nums.length][];
        cases[0] = new int[maxShift[0] + 1];
        for (int i = 0; i < cases[0].length; i++) {
            cases[0][i] = i + 1;
        }
        for (int i = 1; i < nums.length; i++) {
            currCases = new int[maxShift[i] + 1];
            currCases[0] = 1;
            for (int j = 1; j < currCases.length; j++) {
                int prevCases =
                        j < cases[i - 1].length
                                ? cases[i - 1][j]
                                : cases[i - 1][cases[i - 1].length - 1];
                currCases[j] = (currCases[j - 1] + prevCases) % (1_000_000_000 + 7);
            }
            cases[i] = currCases;
        }
        return cases;
    }
}
```