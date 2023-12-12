[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2829\. Determine the Minimum Sum of a k-avoiding Array

Medium

You are given two integers, `n` and `k`.

An array of **distinct** positive integers is called a **k-avoiding** array if there does not exist any pair of distinct elements that sum to `k`.

Return _the **minimum** possible sum of a k-avoiding array of length_ `n`.

**Example 1:**

**Input:** n = 5, k = 4

**Output:** 18

**Explanation:** Consider the k-avoiding array [1,2,4,5,6], which has a sum of 18. It can be proven that there is no k-avoiding array with a sum less than 18.

**Example 2:**

**Input:** n = 2, k = 6

**Output:** 3

**Explanation:** We can construct the array [1,2], which has a sum of 3. It can be proven that there is no k-avoiding array with a sum less than 3.

**Constraints:**

*   `1 <= n, k <= 50`

## Solution

```java
public class Solution {
    public int minimumSum(int n, int k) {
        int[] arr = new int[n];
        int a = k / 2;
        int sum = 0;
        if (a > n) {
            for (int i = 0; i < n; i++) {
                arr[i] = i + 1;
                sum += arr[i];
            }
        } else {
            for (int i = 0; i < a; i++) {
                arr[i] = i + 1;
                sum += arr[i];
            }
            for (int j = a; j < n; j++) {
                arr[j] = k++;
                sum += arr[j];
            }
        }
        return sum;
    }
}
```