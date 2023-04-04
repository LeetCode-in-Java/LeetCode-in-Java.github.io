[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1464\. Maximum Product of Two Elements in an Array

Easy

Given the array of integers `nums`, you will choose two different indices `i` and `j` of that array. _Return the maximum value of_ `(nums[i]-1)*(nums[j]-1)`.

**Example 1:**

**Input:** nums = [3,4,5,2]

**Output:** 12

**Explanation:** If you choose the indices i=1 and j=2 (indexed from 0), you will get the maximum value, that is, (nums[1]-1)\*(nums[2]-1) = (4-1)\*(5-1) = 3\*4 = 12.

**Example 2:**

**Input:** nums = [1,5,4,5]

**Output:** 16

**Explanation:** Choosing the indices i=1 and j=3 (indexed from 0), you will get the maximum value of (5-1)\*(5-1) = 16.

**Example 3:**

**Input:** nums = [3,7]

**Output:** 12

**Constraints:**

*   `2 <= nums.length <= 500`
*   `1 <= nums[i] <= 10^3`

## Solution

```java
public class Solution {
    public int maxProduct(int[] nums) {
        if (nums == null) {
            return 0;
        }

        int first = Integer.MIN_VALUE;
        int second = Integer.MIN_VALUE;
        for (int num : nums) {
            if (num >= first) {
                second = first;
                first = num;
            } else if (num >= second) {
                second = num;
            }
        }
        return (first - 1) * (second - 1);
    }
}
```