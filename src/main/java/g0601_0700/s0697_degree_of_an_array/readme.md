[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 697\. Degree of an Array

Easy

Given a non-empty array of non-negative integers `nums`, the **degree** of this array is defined as the maximum frequency of any one of its elements.

Your task is to find the smallest possible length of a (contiguous) subarray of `nums`, that has the same degree as `nums`.

**Example 1:**

**Input:** nums = [1,2,2,3,1]

**Output:** 2

**Explanation:**

    The input array has a degree of 2 because both elements 1 and 2 appear twice.
    Of the subarrays that have the same degree:
    [1, 2, 2, 3, 1], [1, 2, 2, 3], [2, 2, 3, 1], [1, 2, 2], [2, 2, 3], [2, 2]
    The shortest length is 2. So return 2. 

**Example 2:**

**Input:** nums = [1,2,2,3,1,4,2]

**Output:** 6

**Explanation:**

    The degree is 3 because the element 2 is repeated 3 times.
    So [2,2,3,1,4,2] is the shortest subarray, therefore returning 6. 

**Constraints:**

*   `nums.length` will be between 1 and 50,000.
*   `nums[i]` will be an integer between 0 and 49,999.

## Solution

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    private static class Value {
        int count;
        int start;
        int end;

        public Value(int c, int s, int e) {
            count = c;
            start = s;
            end = e;
        }
    }

    public int findShortestSubArray(int[] nums) {
        int max = 1;
        Map<Integer, Value> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            int j = nums[i];
            if (map.containsKey(j)) {
                Value v = map.get(j);
                v.count++;
                max = Math.max(max, v.count);
                v.end = i;
            } else {
                map.put(j, new Value(1, i, i));
            }
        }
        int min = Integer.MAX_VALUE;
        for (Map.Entry<Integer, Value> entry : map.entrySet()) {
            Value v = entry.getValue();
            if (v.count == max) {
                min = Math.min(min, v.end - v.start);
            }
        }
        return min + 1;
    }
}
```