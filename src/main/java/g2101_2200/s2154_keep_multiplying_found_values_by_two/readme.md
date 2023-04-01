[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2154\. Keep Multiplying Found Values by Two

Easy

You are given an array of integers `nums`. You are also given an integer `original` which is the first number that needs to be searched for in `nums`.

You then do the following steps:

1.  If `original` is found in `nums`, **multiply** it by two (i.e., set `original = 2 * original`).
2.  Otherwise, **stop** the process.
3.  **Repeat** this process with the new number as long as you keep finding the number.

Return _the **final** value of_ `original`.

**Example 1:**

**Input:** nums = [5,3,6,1,12], original = 3

**Output:** 24

**Explanation:** 
- 3 is found in nums. 3 is multiplied by 2 to obtain 6. 

- 6 is found in nums. 6 is multiplied by 2 to obtain 12. 

- 12 is found in nums. 12 is multiplied by 2 to obtain 24. 

- 24 is not found in nums. 
Thus, 24 is returned. 

**Example 2:**

**Input:** nums = [2,7,9], original = 4

**Output:** 4

**Explanation:** 
- 4 is not found in nums. 

Thus, 4 is returned. 

**Constraints:**

*   `1 <= nums.length <= 1000`
*   `1 <= nums[i], original <= 1000`

## Solution

```java
public class Solution {
    public int findFinalValue(int[] nums, int original) {
        int i = 0;
        while (i < nums.length) {
            if (nums[i] == original) {
                original = original * 2;
                i = -1;
            }
            i++;
        }
        return original;
    }
}
```