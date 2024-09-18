[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3287\. Find the Maximum Sequence Value of Array

Hard

You are given an integer array `nums` and a **positive** integer `k`.

The **value** of a sequence `seq` of size `2 * x` is defined as:

*   `(seq[0] OR seq[1] OR ... OR seq[x - 1]) XOR (seq[x] OR seq[x + 1] OR ... OR seq[2 * x - 1])`.

Return the **maximum** **value** of any subsequence of `nums` having size `2 * k`.

**Example 1:**

**Input:** nums = [2,6,7], k = 1

**Output:** 5

**Explanation:**

The subsequence `[2, 7]` has the maximum value of `2 XOR 7 = 5`.

**Example 2:**

**Input:** nums = [4,2,5,6,7], k = 2

**Output:** 2

**Explanation:**

The subsequence `[4, 5, 6, 7]` has the maximum value of `(4 OR 5) XOR (6 OR 7) = 2`.

**Constraints:**

*   `2 <= nums.length <= 400`
*   <code>1 <= nums[i] < 2<sup>7</sup></code>
*   `1 <= k <= nums.length / 2`

## Solution

```java
import java.util.HashSet;
import java.util.Set;

@SuppressWarnings("unchecked")
public class Solution {
    public int maxValue(int[] nums, int k) {
        int n = nums.length;
        Set<Integer>[][] left = new Set[n][k + 1];
        Set<Integer>[][] right = new Set[n][k + 1];
        for (int i = 0; i < n; i++) {
            for (int j = 0; j <= k; j++) {
                left[i][j] = new HashSet<>();
                right[i][j] = new HashSet<>();
            }
        }
        left[0][0].add(0);
        left[0][1].add(nums[0]);
        for (int i = 1; i < n - k; i++) {
            left[i][0].add(0);
            for (int j = 1; j <= k; j++) {
                left[i][j].addAll(left[i - 1][j]);
                for (int v : left[i - 1][j - 1]) {
                    left[i][j].add(v | nums[i]);
                }
            }
        }
        right[n - 1][0].add(0);
        right[n - 1][1].add(nums[n - 1]);
        int result = 0;
        if (k == 1) {
            for (int l : left[n - 2][k]) {
                result = Math.max(result, l ^ nums[n - 1]);
            }
        }
        for (int i = n - 2; i >= k; i--) {
            right[i][0].add(0);
            for (int j = 1; j <= k; j++) {
                right[i][j].addAll(right[i + 1][j]);
                for (int v : right[i + 1][j - 1]) {
                    right[i][j].add(v | nums[i]);
                }
            }
            for (int l : left[i - 1][k]) {
                for (int r : right[i][k]) {
                    result = Math.max(result, l ^ r);
                }
            }
        }
        return result;
    }
}
```