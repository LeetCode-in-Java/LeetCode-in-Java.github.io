[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3833\. Count Dominant Indices

Easy

You are given an integer array `nums` of length `n`.

An element at index `i` is called **dominant** if: `nums[i] > average(nums[i + 1], nums[i + 2], ..., nums[n - 1])`

Your task is to count the number of indices `i` that are **dominant**.

The **average** of a set of numbers is the value obtained by adding all the numbers together and dividing the sum by the total number of numbers.

**Note**: The **rightmost** element of any array is **not** **dominant**.

**Example 1:**

**Input:** nums = [5,4,3]

**Output:** 2

**Explanation:**

*   At index `i = 0`, the value 5 is dominant as `5 > average(4, 3) = 3.5`.
*   At index `i = 1`, the value 4 is dominant over the subarray `[3]`.
*   Index `i = 2` is not dominant as there are no elements to its right. Thus, the answer is 2.

**Example 2:**

**Input:** nums = [4,1,2]

**Output:** 1

**Explanation:**

*   At index `i = 0`, the value 4 is dominant over the subarray `[1, 2]`.
*   At index `i = 1`, the value 1 is not dominant.
*   Index `i = 2` is not dominant as there are no elements to its right. Thus, the answer is 1.

**Constraints:**

*   `1 <= nums.length <= 100`
*   `1 <= nums[i] <= 100`

## Solution

```java
public class Solution {
    public int dominantIndices(int[] a) {
        int n = a.length;
        long sum = 0;
        int cnt = 0;
        for (int i = n - 1; i > 0; i--) {
            sum += a[i];
            if (a[i - 1] > sum / (n - i)) {
                cnt++;
            }
        }
        return cnt;
    }
}
```