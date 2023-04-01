[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1248\. Count Number of Nice Subarrays

Medium

Given an array of integers `nums` and an integer `k`. A continuous subarray is called **nice** if there are `k` odd numbers on it.

Return _the number of **nice** sub-arrays_.

**Example 1:**

**Input:** nums = [1,1,2,1,1], k = 3

**Output:** 2

**Explanation:** The only sub-arrays with 3 odd numbers are [1,1,2,1] and [1,2,1,1].

**Example 2:**

**Input:** nums = [2,4,6], k = 1

**Output:** 0

**Explanation:** There is no odd numbers in the array.

**Example 3:**

**Input:** nums = [2,2,2,1,2,2,1,2,2,2], k = 2

**Output:** 16

**Constraints:**

*   `1 <= nums.length <= 50000`
*   `1 <= nums[i] <= 10^5`
*   `1 <= k <= nums.length`

## Solution

```java
public class Solution {
    public int numberOfSubarrays(int[] nums, int k) {
        int oddLen = 0;
        int startIndex = 0;
        int num = 0;
        int endIndex;
        int res = 0;
        boolean hasK;
        for (int i = 0; i < nums.length; i++) {
            hasK = false;
            endIndex = i;
            if (nums[i] % 2 == 1) {
                oddLen++;
            }
            while (oddLen >= k) {
                hasK = true;
                if (nums[startIndex++] % 2 == 1) {
                    oddLen--;
                }
                num++;
            }
            res += num;
            while (hasK && ++endIndex < nums.length && nums[endIndex] % 2 == 0) {
                res += num;
            }
            num = 0;
        }
        return res;
    }
}
```