[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

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
    public int maxAbsValExpr(int[] arr1, int[] arr2) {
        if (arr1.length != arr2.length) {
            return 0;
        }
        int max1 = Integer.MIN_VALUE;
        int max2 = Integer.MIN_VALUE;
        int max3 = Integer.MIN_VALUE;
        int max4 = Integer.MIN_VALUE;
        int min1 = Integer.MAX_VALUE;
        int min2 = Integer.MAX_VALUE;
        int min3 = Integer.MAX_VALUE;
        int min4 = Integer.MAX_VALUE;
        for (int i = 0; i < arr1.length; i++) {
            max1 = Math.max(arr1[i] + arr2[i] + i, max1);
            min1 = Math.min(arr1[i] + arr2[i] + i, min1);
            max2 = Math.max(i - arr1[i] - arr2[i], max2);
            min2 = Math.min(i - arr1[i] - arr2[i], min2);
            max3 = Math.max(arr1[i] - arr2[i] + i, max3);
            min3 = Math.min(arr1[i] - arr2[i] + i, min3);
            max4 = Math.max(arr2[i] - arr1[i] + i, max4);
            min4 = Math.min(arr2[i] - arr1[i] + i, min4);
        }
        return Math.max(Math.max(max1 - min1, max2 - min2), Math.max(max3 - min3, max4 - min4));
    }
}
```