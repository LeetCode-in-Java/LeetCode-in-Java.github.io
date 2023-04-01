[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1224\. Maximum Equal Frequency

Hard

Given an array `nums` of positive integers, return the longest possible length of an array prefix of `nums`, such that it is possible to remove **exactly one** element from this prefix so that every number that has appeared in it will have the same number of occurrences.

If after removing one element there are no remaining elements, it's still considered that every appeared number has the same number of ocurrences (0).

**Example 1:**

**Input:** nums = [2,2,1,1,5,3,3,5]

**Output:** 7

**Explanation:** For the subarray [2,2,1,1,5,3,3] of length 7, if we remove nums[4] = 5, we will get [2,2,1,1,3,3], so that each number will appear exactly twice.

**Example 2:**

**Input:** nums = [1,1,1,2,2,2,3,3,3,4,4,4,5]

**Output:** 13

**Constraints:**

*   <code>2 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    public int maxEqualFreq(int[] nums) {
        int[] count = new int[100001];
        int[] freq = new int[100001];
        int n = nums.length;
        for (int num : nums) {
            count[num]++;
            freq[count[num]]++;
        }
        for (int i = n - 1; i > 0; i--) {
            if (freq[count[nums[i]]] * count[nums[i]] == i) {
                return i + 1;
            }
            freq[count[nums[i]]]--;
            count[nums[i]]--;
            if (freq[count[nums[i - 1]]] * count[nums[i - 1]] == i) {
                return i + 1;
            }
        }
        return 1;
    }
}
```