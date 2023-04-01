[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 912\. Sort an Array

Medium

Given an array of integers `nums`, sort the array in ascending order.

**Example 1:**

**Input:** nums = [5,2,3,1]

**Output:** [1,2,3,5]

**Example 2:**

**Input:** nums = [5,1,1,2,0,0]

**Output:** [0,0,1,1,2,5]

**Constraints:**

*   <code>1 <= nums.length <= 5 * 10<sup>4</sup></code>
*   <code>-5 * 10<sup>4</sup> <= nums[i] <= 5 * 10<sup>4</sup></code>

## Solution

```java
public class Solution {
    public int[] sortArray(int[] nums) {
        return mergeSort(nums, 0, nums.length - 1);
    }

    private int[] mergeSort(int[] arr, int lo, int hi) {
        if (lo == hi) {
            int[] sortedArr = new int[1];
            sortedArr[0] = arr[lo];
            return sortedArr;
        }
        int mid = (lo + hi) / 2;
        int[] leftArray = mergeSort(arr, lo, mid);
        int[] rightArray = mergeSort(arr, mid + 1, hi);
        return mergeSortedArray(leftArray, rightArray);
    }

    private int[] mergeSortedArray(int[] a, int[] b) {
        int[] ans = new int[a.length + b.length];
        int i = 0;
        int j = 0;
        int k = 0;
        while (i < a.length && j < b.length) {
            if (a[i] < b[j]) {
                ans[k++] = a[i++];
            } else {
                ans[k++] = b[j++];
            }
        }
        while (i < a.length) {
            ans[k++] = a[i++];
        }
        while (j < b.length) {
            ans[k++] = b[j++];
        }
        return ans;
    }
}
```