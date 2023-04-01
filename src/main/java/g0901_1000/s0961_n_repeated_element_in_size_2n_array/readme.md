[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 961\. N-Repeated Element in Size 2N Array

Easy

You are given an integer array `nums` with the following properties:

*   `nums.length == 2 * n`.
*   `nums` contains `n + 1` **unique** elements.
*   Exactly one element of `nums` is repeated `n` times.

Return _the element that is repeated_ `n` _times_.

**Example 1:**

**Input:** nums = [1,2,3,3]

**Output:** 3

**Example 2:**

**Input:** nums = [2,1,2,5,3,2]

**Output:** 2

**Example 3:**

**Input:** nums = [5,1,5,2,5,3,5,4]

**Output:** 5

**Constraints:**

*   `2 <= n <= 5000`
*   `nums.length == 2 * n`
*   <code>0 <= nums[i] <= 10<sup>4</sup></code>
*   `nums` contains `n + 1` **unique** elements and one of them is repeated exactly `n` times.

## Solution

```java
import java.util.HashSet;
import java.util.Set;

public class Solution {
    public int repeatedNTimes(int[] nums) {
        Set<Integer> set = new HashSet<>();
        for (int num : nums) {
            if (!set.add(num)) {
                return num;
            }
        }
        return -1;
    }
}
```