[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1748\. Sum of Unique Elements

Easy

You are given an integer array `nums`. The unique elements of an array are the elements that appear **exactly once** in the array.

Return _the **sum** of all the unique elements of_ `nums`.

**Example 1:**

**Input:** nums = [1,2,3,2]

**Output:** 4

**Explanation:** The unique elements are [1,3], and the sum is 4.

**Example 2:**

**Input:** nums = [1,1,1,1,1]

**Output:** 0

**Explanation:** There are no unique elements, and the sum is 0.

**Example 3:**

**Input:** nums = [1,2,3,4,5]

**Output:** 15

**Explanation:** The unique elements are [1,2,3,4,5], and the sum is 15.

**Constraints:**

*   `1 <= nums.length <= 100`
*   `1 <= nums[i] <= 100`

## Solution

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    public int sumOfUnique(int[] nums) {
        Map<Integer, Integer> map = new HashMap<>();
        int sum = 0;
        for (int num : nums) {
            map.put(num, map.getOrDefault(num, 0) + 1);
        }
        for (Map.Entry<Integer, Integer> entry : map.entrySet()) {
            if (entry.getValue() == 1) {
                sum += entry.getKey();
            }
        }
        return sum;
    }
}
```