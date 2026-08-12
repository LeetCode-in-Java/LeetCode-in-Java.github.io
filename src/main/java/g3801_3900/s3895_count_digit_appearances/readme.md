[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3895\. Count Digit Appearances

Medium

You are given an integer array `nums` and an integer `digit`.

Return the total number of times `digit` appears in the decimal representation of all elements in `nums`.

**Example 1:**

**Input:** nums = [12,54,32,22], digit = 2

**Output:** 4

**Explanation:**

The digit 2 appears once in 12 and 32, and twice in 22. Thus, the total number of times digit 2 appears is 4.

**Example 2:**

**Input:** nums = [1,34,7], digit = 9

**Output:** 0

**Explanation:**

The digit 9 does not appear in the decimal representation of any element in `nums`, so the total number of times digit 9 appears is 0.

**Constraints:**

*   `1 <= nums.length <= 1000`
*   <code>1 <= nums[i] <= 10<sup>6</sup></code>
*   `0 <= digit <= 9`

## Solution

```java
public class Solution {
    public int countDigitOccurrences(int[] nums, int digit) {
        int ans = 0;
        for (int num : nums) {
            while (num > 9) {
                if ((num % 10) == digit) {
                    ans++;
                }
                num /= 10;
            }
            if (num == digit) {
                ans++;
            }
        }
        return ans;
    }
}
```