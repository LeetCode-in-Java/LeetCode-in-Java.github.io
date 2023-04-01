[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1200\. Minimum Absolute Difference

Easy

Given an array of **distinct** integers `arr`, find all pairs of elements with the minimum absolute difference of any two elements.

Return a list of pairs in ascending order(with respect to pairs), each pair `[a, b]` follows

*   `a, b` are from `arr`
*   `a < b`
*   `b - a` equals to the minimum absolute difference of any two elements in `arr`

**Example 1:**

**Input:** arr = [4,2,1,3]

**Output:** [[1,2],[2,3],[3,4]]

**Explanation:** The minimum absolute difference is 1. List all pairs with difference equal to 1 in ascending order.

**Example 2:**

**Input:** arr = [1,3,6,10,15]

**Output:** [[1,3]]

**Example 3:**

**Input:** arr = [3,8,-10,23,19,-4,-14,27]

**Output:** [[-14,-10],[19,23],[23,27]]

**Constraints:**

*   <code>2 <= arr.length <= 10<sup>5</sup></code>
*   <code>-10<sup>6</sup> <= arr[i] <= 10<sup>6</sup></code>

## Solution

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class Solution {
    public List<List<Integer>> minimumAbsDifference(int[] arr) {
        List<List<Integer>> result = new ArrayList<>();
        int min = 10000000;
        Arrays.sort(arr);
        for (int i = 0; i + 1 < arr.length; i++) {
            int diff = arr[i + 1] - arr[i];
            if (diff <= min) {
                if (diff < min) {
                    min = diff;
                    result.clear();
                }
                result.add(Arrays.asList(arr[i], arr[i + 1]));
            }
        }
        return result;
    }
}
```