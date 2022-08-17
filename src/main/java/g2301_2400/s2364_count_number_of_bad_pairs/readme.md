[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2364\. Count Number of Bad Pairs

Medium

You are given a **0-indexed** integer array `nums`. A pair of indices `(i, j)` is a **bad pair** if `i < j` and `j - i != nums[j] - nums[i]`.

Return _the total number of **bad pairs** in_ `nums`.

**Example 1:**

**Input:** nums = [4,1,3,3]

**Output:** 5

**Explanation:** 

The pair (0, 1) is a bad pair since 1 - 0 != 1 - 4.

The pair (0, 2) is a bad pair since 2 - 0 != 3 - 4, 2 != -1. 

The pair (0, 3) is a bad pair since 3 - 0 != 3 - 4, 3 != -1. 

The pair (1, 2) is a bad pair since 2 - 1 != 3 - 1, 1 != 2. 

The pair (2, 3) is a bad pair since 3 - 2 != 3 - 3, 1 != 0. 

There are a total of 5 bad pairs, so we return 5.

**Example 2:**

**Input:** nums = [1,2,3,4,5]

**Output:** 0

**Explanation:** There are no bad pairs.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>

## Solution

```java
import java.util.HashMap;

public class Solution {
    public long countBadPairs(int[] nums) {
        HashMap<Integer, Integer> seen = new HashMap<>();
        long count = 0;
        for (int i = 0; i < nums.length; i++) {
            int diff = i - nums[i];
            if (seen.containsKey(diff)) {
                count += (i - seen.get(diff));
            } else {
                count += i;
            }
            seen.put(diff, seen.getOrDefault(diff, 0) + 1);
        }
        return count;
    }
}
```