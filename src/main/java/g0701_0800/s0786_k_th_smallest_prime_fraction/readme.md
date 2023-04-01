[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 786\. K-th Smallest Prime Fraction

Hard

You are given a sorted integer array `arr` containing `1` and **prime** numbers, where all the integers of `arr` are unique. You are also given an integer `k`.

For every `i` and `j` where `0 <= i < j < arr.length`, we consider the fraction `arr[i] / arr[j]`.

Return _the_ <code>k<sup>th</sup></code> _smallest fraction considered_. Return your answer as an array of integers of size `2`, where `answer[0] == arr[i]` and `answer[1] == arr[j]`.

**Example 1:**

**Input:** arr = [1,2,3,5], k = 3

**Output:** [2,5]

**Explanation:**

    The fractions to be considered in sorted order are:
    1/5, 1/3, 2/5, 1/2, 3/5, and 2/3.
    The third fraction is 2/5. 

**Example 2:**

**Input:** arr = [1,7], k = 1

**Output:** [1,7] 

**Constraints:**

*   `2 <= arr.length <= 1000`
*   <code>1 <= arr[i] <= 3 * 10<sup>4</sup></code>
*   `arr[0] == 1`
*   `arr[i]` is a **prime** number for `i > 0`.
*   All the numbers of `arr` are **unique** and sorted in **strictly increasing** order.
*   `1 <= k <= arr.length * (arr.length - 1) / 2`

## Solution

```java
public class Solution {
    public int[] kthSmallestPrimeFraction(int[] arr, int k) {
        int n = arr.length;
        double low = 0;
        double high = 1.0;
        while (low < high) {
            double mid = (low + high) / 2;
            int[] res = getFractionsLessThanMid(arr, n, mid);
            if (res[0] == k) {
                return new int[] {arr[res[1]], arr[res[2]]};
            } else if (res[0] > k) {
                high = mid;
            } else {
                low = mid;
            }
        }
        return new int[] {};
    }

    private int[] getFractionsLessThanMid(int[] arr, int n, double mid) {
        double maxLessThanMid = 0.0;
        // stores indices of max fraction less than mid;
        int x = 0;
        int y = 0;
        // for storing fractions less than mid
        int total = 0;
        int j = 1;
        for (int i = 0; i < n - 1; i++) {
            // while fraction is greater than mid increment j
            while (j < n && arr[i] > arr[j] * mid) {
                j++;
            }
            if (j == n) {
                break;
            }
            // j fractions greater than mid, n-j fractions smaller than mid, add fractions smaller
            // than mid to total
            total += (n - j);
            double fraction = (double) arr[i] / arr[j];
            if (fraction > maxLessThanMid) {
                maxLessThanMid = fraction;
                x = i;
                y = j;
            }
        }
        return new int[] {total, x, y};
    }
}
```