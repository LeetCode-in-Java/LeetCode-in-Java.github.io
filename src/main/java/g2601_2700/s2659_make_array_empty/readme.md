[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2659\. Make Array Empty

Hard

You are given an integer array `nums` containing **distinct** numbers, and you can perform the following operations **until the array is empty**:

*   If the first element has the **smallest** value, remove it
*   Otherwise, put the first element at the **end** of the array.

Return _an integer denoting the number of operations it takes to make_ `nums` _empty._

**Example 1:**

**Input:** nums = [3,4,-1]

**Output:** 5

    Operation   Array
    1           [4, -1, 3]
    2           [-1, 3, 4]
    3           [3, 4]
    4           [4]
    5           []

**Example 2:**

**Input:** nums = [1,2,4,3]

**Output:** 5

    Operation   Array
    1           [2, 4, 3]
    2           [4, 3]
    3           [3, 4]
    4           [4]
    5           []

**Example 3:**

**Input:** nums = [1,2,3]

**Output:** 3

    Operation   Array
    1           [2, 3]
    2           [3]
    3           []

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>-10<sup>9 </sup><= nums[i] <= 10<sup>9</sup></code>
*   All values in `nums` are **distinct**.

## Solution

```java
import java.util.Arrays;
import java.util.Comparator;

public class Solution {
    public long countOperationsToEmptyArray(int[] nums) {
        int n = nums.length;
        Integer[] arr = new Integer[n];
        for (int i = 0; i < n; i++) {
            arr[i] = i;
        }
        Arrays.sort(arr, Comparator.comparingInt(i -> nums[i]));
        long ans = n;
        for (int i = 1; i < n; i++) {
            if (arr[i] < arr[i - 1]) {
                ans += n - i;
            }
        }
        return ans;
    }
}
```