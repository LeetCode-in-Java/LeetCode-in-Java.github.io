[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2607\. Make K-Subarray Sums Equal

Medium

You are given a **0-indexed** integer array `arr` and an integer `k`. The array `arr` is circular. In other words, the first element of the array is the next element of the last element, and the last element of the array is the previous element of the first element.

You can do the following operation any number of times:

*   Pick any element from `arr` and increase or decrease it by `1`.

Return _the minimum number of operations such that the sum of each **subarray** of length_ `k` _is equal_.

A **subarray** is a contiguous part of the array.

**Example 1:**

**Input:** arr = [1,4,1,3], k = 2

**Output:** 1

**Explanation:** we can do one operation on index 1 to make its value equal to 3. The array after the operation is [1,3,1,3] 
- Subarray starts at index 0 is [1, 3], and its sum is 4 
- Subarray starts at index 1 is [3, 1], and its sum is 4 
- Subarray starts at index 2 is [1, 3], and its sum is 4 
- Subarray starts at index 3 is [3, 1], and its sum is 4

**Example 2:**

**Input:** arr = [2,5,5,7], k = 3

**Output:** 5

**Explanation:** we can do three operations on index 0 to make its value equal to 5 and two operations on index 3 to make its value equal to 5. The array after the operations is [5,5,5,5] 
- Subarray starts at index 0 is [5, 5, 5], and its sum is 15 
- Subarray starts at index 1 is [5, 5, 5], and its sum is 15 
- Subarray starts at index 2 is [5, 5, 5], and its sum is 15 
- Subarray starts at index 3 is [5, 5, 5], and its sum is 15

**Constraints:**

*   <code>1 <= k <= arr.length <= 10<sup>5</sup></code>
*   <code>1 <= arr[i] <= 10<sup>9</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    public long makeSubKSumEqual(int[] arr, int k) {
        int n = arr.length;
        int h = gcd(n, k);
        int q = n / h;
        long ans = 0;
        for (int i = 0; i < h; i++) {
            int[] x = new int[q];
            for (int j = 0; j < q; j++) {
                x[j] = arr[(h * j) + i];
            }
            Arrays.sort(x);
            int v = q / 2;
            int u = q % 2 == 0 ? (x[v] + x[v - 1]) / 2 : x[v];
            for (int o = 0; o < q; o++) {
                ans += Math.abs(u - x[o]);
            }
        }
        return ans;
    }

    private int gcd(int a, int b) {
        if (b == 0) {
            return a;
        } else {
            return gcd(b, Math.abs(a - b));
        }
    }
}
```