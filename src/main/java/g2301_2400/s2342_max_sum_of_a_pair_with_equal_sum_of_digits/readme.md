[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2342\. Max Sum of a Pair With Equal Sum of Digits

Medium

You are given a **0-indexed** array `nums` consisting of **positive** integers. You can choose two indices `i` and `j`, such that `i != j`, and the sum of digits of the number `nums[i]` is equal to that of `nums[j]`.

Return _the **maximum** value of_ `nums[i] + nums[j]` _that you can obtain over all possible indices_ `i` _and_ `j` _that satisfy the conditions._

**Example 1:**

**Input:** nums = [18,43,36,13,7]

**Output:** 54

**Explanation:** The pairs (i, j) that satisfy the conditions are:

- (0, 2), both numbers have a sum of digits equal to 9, and their sum is 18 + 36 = 54.

- (1, 4), both numbers have a sum of digits equal to 7, and their sum is 43 + 7 = 50.

So the maximum sum that we can obtain is 54. 

**Example 2:**

**Input:** nums = [10,12,19,14]

**Output:** -1

**Explanation:** There are no two numbers that satisfy the conditions, so we return -1. 

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>

## Solution

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    public int maximumSum(int[] nums) {
        Map<Integer, Integer> map = new HashMap<>();
        int res = -1;
        for (int num : nums) {
            int s = 0;
            for (char digit : String.valueOf(num).toCharArray()) {
                s += digit - '0';
            }
            if (!map.containsKey(s)) {
                map.put(s, num);
            } else {
                res = Math.max(res, map.get(s) + num);
                map.put(s, Math.max(map.get(s), num));
            }
        }
        return res;
    }
}
```