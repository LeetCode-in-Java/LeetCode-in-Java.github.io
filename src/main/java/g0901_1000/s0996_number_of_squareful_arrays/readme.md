[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 996\. Number of Squareful Arrays

Hard

An array is **squareful** if the sum of every pair of adjacent elements is a **perfect square**.

Given an integer array nums, return _the number of permutations of_ `nums` _that are **squareful**_.

Two permutations `perm1` and `perm2` are different if there is some index `i` such that `perm1[i] != perm2[i]`.

**Example 1:**

**Input:** nums = [1,17,8]

**Output:** 2

**Explanation:** [1,8,17] and [17,8,1] are the valid permutations. 

**Example 2:**

**Input:** nums = [2,2,2]

**Output:** 1 

**Constraints:**

*   `1 <= nums.length <= 12`
*   <code>0 <= nums[i] <= 10<sup>9</sup></code>

## Solution

```java
import java.util.HashSet;
import java.util.Set;

public class Solution {
    int count;

    public int numSquarefulPerms(int[] nums) {
        int n = nums.length;
        if (n < 2) {
            return count;
        }
        backtrack(nums, n, 0);
        return count;
    }

    private void backtrack(int[] nums, int n, int start) {
        if (start == n) {
            count++;
        }
        Set<Integer> set = new HashSet<>();
        for (int i = start; i < n; i++) {
            if (set.contains(nums[i])) {
                continue;
            }
            swap(nums, start, i);
            if (start == 0 || isPerfectSq(nums[start], nums[start - 1])) {
                backtrack(nums, n, start + 1);
            }
            swap(nums, start, i);
            set.add(nums[i]);
        }
    }

    private void swap(int[] array, int a, int b) {
        int temp = array[a];
        array[a] = array[b];
        array[b] = temp;
    }

    private boolean isPerfectSq(int a, int b) {
        int x = a + b;
        double sqrt = Math.sqrt(x);
        return sqrt - (int) sqrt == 0;
    }
}
```