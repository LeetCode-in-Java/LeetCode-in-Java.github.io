[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1713\. Minimum Operations to Make a Subsequence

Hard

You are given an array `target` that consists of **distinct** integers and another integer array `arr` that **can** have duplicates.

In one operation, you can insert any integer at any position in `arr`. For example, if `arr = [1,4,1,2]`, you can add `3` in the middle and make it `[1,4,3,1,2]`. Note that you can insert the integer at the very beginning or end of the array.

Return _the **minimum** number of operations needed to make_ `target` _a **subsequence** of_ `arr`_._

A **subsequence** of an array is a new array generated from the original array by deleting some elements (possibly none) without changing the remaining elements' relative order. For example, `[2,7,4]` is a subsequence of `[4,2,3,7,2,1,4]` (the underlined elements), while `[2,4,2]` is not.

**Example 1:**

**Input:** target = [5,1,3], `arr` = [9,4,2,3,4]

**Output:** 2

**Explanation:** You can add 5 and 1 in such a way that makes `arr` = [5,9,4,1,2,3,4], then target will be a subsequence of `arr`.

**Example 2:**

**Input:** target = [6,4,8,1,3,2], `arr` = [4,7,6,2,3,8,6,1]

**Output:** 3

**Constraints:**

*   <code>1 <= target.length, arr.length <= 10<sup>5</sup></code>
*   <code>1 <= target[i], arr[i] <= 10<sup>9</sup></code>
*   `target` contains no duplicates.

## Solution

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class Solution {
    public int minOperations(int[] target, int[] arr) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < target.length; i++) {
            map.put(target[i], i);
        }
        List<Integer> list = new ArrayList<>();
        for (int num : arr) {
            if (map.containsKey(num)) {
                list.add(map.get(num));
            }
        }
        return target.length - longestIncreasingSubsequence(list);
    }

    private int longestIncreasingSubsequence(List<Integer> list) {
        int n = list.size();
        int l = 0;
        int[] arr = new int[n];
        for (int num : list) {
            int index = Arrays.binarySearch(arr, 0, l, num);
            if (index < 0) {
                index = ~index;
            }
            arr[index] = num;
            if (index == l) {
                l++;
            }
        }

        return l;
    }
}
```