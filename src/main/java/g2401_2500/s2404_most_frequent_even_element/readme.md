[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2404\. Most Frequent Even Element

Easy

Given an integer array `nums`, return _the most frequent even element_.

If there is a tie, return the **smallest** one. If there is no such element, return `-1`.

**Example 1:**

**Input:** nums = [0,1,2,2,4,4,1]

**Output:** 2

**Explanation:**

The even elements are 0, 2, and 4. Of these, 2 and 4 appear the most.

We return the smallest one, which is 2.

**Example 2:**

**Input:** nums = [4,4,4,9,2,4]

**Output:** 4

**Explanation:** 4 is the even element appears the most. 

**Example 3:**

**Input:** nums = [29,47,21,41,13,37,25,7]

**Output:** -1

**Explanation:** There is no even element. 

**Constraints:**

*   `1 <= nums.length <= 2000`
*   <code>0 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    public int mostFrequentEven(int[] nums) {
        Map<Integer, Integer> frequencyMap = new HashMap<>();
        int mostFrequent = Integer.MAX_VALUE;
        int maxFrequency = 0;
        for (int num : nums) {
            if ((num & 1) == 0) {
                maxFrequency =
                        Math.max(
                                maxFrequency,
                                frequencyMap.compute(num, (n, freq) -> freq == null ? 1 : ++freq));
            }
        }
        for (Map.Entry<Integer, Integer> entry : frequencyMap.entrySet()) {
            if (entry.getValue() == maxFrequency) {
                mostFrequent = Math.min(mostFrequent, entry.getKey());
            }
        }
        return mostFrequent == Integer.MAX_VALUE ? -1 : mostFrequent;
    }
}
```