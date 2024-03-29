[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2537\. Count the Number of Good Subarrays

Medium

Given an integer array `nums` and an integer `k`, return _the number of **good** subarrays of_ `nums`.

A subarray `arr` is **good** if it there are **at least** `k` pairs of indices `(i, j)` such that `i < j` and `arr[i] == arr[j]`.

A **subarray** is a contiguous **non-empty** sequence of elements within an array.

**Example 1:**

**Input:** nums = [1,1,1,1,1], k = 10

**Output:** 1

**Explanation:** The only good subarray is the array nums itself.

**Example 2:**

**Input:** nums = [3,1,4,3,2,2,4], k = 2

**Output:** 4

**Explanation:** There are 4 different good subarrays: 

- \[3,1,4,3,2,2] that has 2 pairs.

- \[3,1,4,3,2,2,4] that has 3 pairs.

- \[1,4,3,2,2,4] that has 2 pairs. 

- \[4,3,2,2,4] that has 2 pairs.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i], k <= 10<sup>9</sup></code>

## Solution

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    public long countGood(int[] nums, int k) {
        if (nums.length < 2) {
            return 0L;
        }
        Map<Integer, Integer> countMap = new HashMap<>(nums.length, 0.99f);
        long goodSubArrays = 0L;
        long current = 0L;
        int left = 0;
        int right = -1;
        while (left < nums.length) {
            if (current < k) {
                if (++right == nums.length) {
                    break;
                }
                Integer num = nums[right];
                Integer count = countMap.get(num);
                if (count == null) {
                    count = 1;
                } else {
                    current += count;
                    if (current >= k) {
                        goodSubArrays += nums.length - right;
                    }
                    count = count + 1;
                }
                countMap.put(num, count);
            } else {
                Integer num = nums[left++];
                int count = countMap.get(num) - 1;
                if (count > 0) {
                    countMap.put(num, count);
                    current -= count;
                } else {
                    countMap.remove(num);
                }
                if (current >= k) {
                    goodSubArrays += nums.length - right;
                }
            }
        }
        return goodSubArrays;
    }
}
```