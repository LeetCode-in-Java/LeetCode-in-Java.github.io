[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1207\. Unique Number of Occurrences

Easy

Given an array of integers `arr`, return `true` if the number of occurrences of each value in the array is **unique**, or `false` otherwise.

**Example 1:**

**Input:** arr = [1,2,2,1,1,3]

**Output:** true

**Explanation:** The value 1 has 3 occurrences, 2 has 2 and 3 has 1. No two values have the same number of occurrences.

**Example 2:**

**Input:** arr = [1,2]

**Output:** false

**Example 3:**

**Input:** arr = [-3,0,1,-3,1,1,1,-3,10,0]

**Output:** true

**Constraints:**

*   `1 <= arr.length <= 1000`
*   `-1000 <= arr[i] <= 1000`

## Solution

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    public boolean uniqueOccurrences(int[] arr) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int j : arr) {
            if (map.containsKey(j)) {
                map.put(j, map.get(j) + 1);
            } else {
                map.put(j, 1);
            }
        }
        // map for check unique number of count
        Map<Integer, Integer> uni = new HashMap<>();
        for (Integer val : map.values()) {
            if (uni.containsKey(val)) {
                return false;
            } else {
                uni.put(val, 1);
            }
        }
        return true;
    }
}
```