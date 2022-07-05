[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1089\. Duplicate Zeros

Easy

Given a fixed-length integer array `arr`, duplicate each occurrence of zero, shifting the remaining elements to the right.

**Note** that elements beyond the length of the original array are not written. Do the above modifications to the input array in place and do not return anything.

**Example 1:**

**Input:** arr = [1,0,2,3,0,4,5,0]

**Output:** [1,0,0,2,3,0,0,4]

**Explanation:** After calling your function, the input array is modified to: [1,0,0,2,3,0,0,4]

**Example 2:**

**Input:** arr = [1,2,3]

**Output:** [1,2,3]

**Explanation:** After calling your function, the input array is modified to: [1,2,3]

**Constraints:**

*   <code>1 <= arr.length <= 10<sup>4</sup></code>
*   `0 <= arr[i] <= 9`

## Solution

```java
public class Solution {
    public void duplicateZeros(int[] arr) {
        int countZero = 0;
        for (int k : arr) {
            if (k == 0) {
                countZero++;
            }
        }
        int len = arr.length + countZero;
        // We just need O(1) space if we scan from back
        // i point to the original array, j point to the new location
        int i = arr.length - 1;
        int j = len - 1;
        while (i < j) {
            // copy twice when hit '0'
            if (arr[i] == 0) {
                if (j < arr.length) {
                    arr[j] = arr[i];
                }
                j--;
            }
            if (j < arr.length) {
                arr[j] = arr[i];
            }
            i--;
            j--;
        }
    }
}
```