[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1004\. Max Consecutive Ones III

Medium

Given a binary array `nums` and an integer `k`, return _the maximum number of consecutive_ `1`_'s in the array if you can flip at most_ `k` `0`'s.

**Example 1:**

**Input:** nums = [1,1,1,0,0,0,1,1,1,1,0], k = 2

**Output:** 6

**Explanation:** [1,1,1,0,0,**1**,1,1,1,1,**1**] Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.

**Example 2:**

**Input:** nums = [0,0,1,1,0,0,1,1,1,0,1,1,0,0,0,1,1,1,1], k = 3

**Output:** 10

**Explanation:** [0,0,1,1,**1**,**1**,1,1,1,**1**,1,1,0,0,0,1,1,1,1] Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   `nums[i]` is either `0` or `1`.
*   `0 <= k <= nums.length`

## Solution

```java
public class Solution {
    public int longestOnes(int[] a, int k) {
        int result = 0;
        int i = 0;
        for (int j = 0; j < a.length; j++) {
            if (a[j] == 0) {
                k--;
            }
            while (k < 0) {
                if (a[i] == 0) {
                    k++;
                }
                i++;
            }
            result = Math.max(result, j - i + 1);
        }
        return result;
    }
}
```