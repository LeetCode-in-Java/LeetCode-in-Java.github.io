[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1131\. Maximum of Absolute Value Expression

Medium

Given two arrays of integers with equal lengths, return the maximum value of:

`|arr1[i] - arr1[j]| + |arr2[i] - arr2[j]| + |i - j|`

where the maximum is taken over all `0 <= i, j < arr1.length`.

**Example 1:**

**Input:** arr1 = [1,2,3,4], arr2 = [-1,4,5,6]

**Output:** 13

**Example 2:**

**Input:** arr1 = [1,-2,-5,0,10], arr2 = [0,-2,-1,-7,-4]

**Output:** 20

**Constraints:**

*   `2 <= arr1.length == arr2.length <= 40000`
*   `-10^6 <= arr1[i], arr2[i] <= 10^6`

## Solution

```java
public class Solution {
    private int max(int[] a1, int[] a2, int k1, int k2, int k3) {
        int result = Integer.MIN_VALUE;
        for (int i = 0; i < a1.length; i++) {
            result = Math.max(result, a1[i] * k1 + a2[i] * k2 + i * k3);
        }
        return result;
    }

    private int min(int[] a1, int[] a2, int k1, int k2, int k3) {
        return -max(a1, a2, -k1, -k2, -k3);
    }

    public int maxAbsValExpr(int[] a1, int[] a2) {
        if (a1 == null || a2 == null || a1.length == 0 || a2.length == 0) {
            return 0;
        }
        int result = 0;
        int[][] ksArray = { {1, 1, 1}, {1, 1, -1}, {1, -1, 1}, {1, -1, -1}};
        for (int[] ks : ksArray) {
            int max = max(a1, a2, ks[0], ks[1], ks[2]);
            int min = min(a1, a2, ks[0], ks[1], ks[2]);
            result = Math.max(result, max - min);
        }
        return result;
    }
}
```