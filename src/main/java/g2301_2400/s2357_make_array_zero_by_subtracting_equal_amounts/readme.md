[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2357\. Make Array Zero by Subtracting Equal Amounts

Easy

You are given a non-negative integer array `nums`. In one operation, you must:

*   Choose a positive integer `x` such that `x` is less than or equal to the **smallest non-zero** element in `nums`.
*   Subtract `x` from every **positive** element in `nums`.

Return _the **minimum** number of operations to make every element in_ `nums` _equal to_ `0`.

**Example 1:**

**Input:** nums = [1,5,0,3,5]

**Output:** 3

**Explanation:**

In the first operation, choose x = 1. Now, nums = [0,4,0,2,4].

In the second operation, choose x = 2. Now, nums = [0,2,0,0,2].

In the third operation, choose x = 2. Now, nums = [0,0,0,0,0]. 

**Example 2:**

**Input:** nums = [0]

**Output:** 0

**Explanation:** Each element in nums is already 0 so no operations are needed. 

**Constraints:**

*   `1 <= nums.length <= 100`
*   `0 <= nums[i] <= 100`

## Solution

```java
import java.util.HashSet;
import java.util.Set;

public class Solution {
    public int minimumOperations(int[] nums) {
        Set<Integer> set = new HashSet<>();
        for (int a : nums) {
            if (a > 0) {
                set.add(a);
            }
        }
        return set.size();
    }
}
```