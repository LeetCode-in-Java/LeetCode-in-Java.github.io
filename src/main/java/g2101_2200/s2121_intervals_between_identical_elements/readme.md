[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2121\. Intervals Between Identical Elements

Medium

You are given a **0-indexed** array of `n` integers `arr`.

The **interval** between two elements in `arr` is defined as the **absolute difference** between their indices. More formally, the **interval** between `arr[i]` and `arr[j]` is `|i - j|`.

Return _an array_ `intervals` _of length_ `n` _where_ `intervals[i]` _is **the sum of intervals** between_ `arr[i]` _and each element in_ `arr` _with the same value as_ `arr[i]`_._

**Note:** `|x|` is the absolute value of `x`.

**Example 1:**

**Input:** arr = [2,1,3,1,2,3,3]

**Output:** [4,2,7,2,4,4,5]

**Explanation:** 

- Index 0: Another 2 is found at index 4. \|0 - 4\| = 4 

- Index 1: Another 1 is found at index 3. \|1 - 3\| = 2 

- Index 2: Two more 3s are found at indices 5 and 6. \|2 - 5\| + \|2 - 6\| = 7 

- Index 3: Another 1 is found at index 1. \|3 - 1\| = 2 

- Index 4: Another 2 is found at index 0. \|4 - 0\| = 4 

- Index 5: Two more 3s are found at indices 2 and 6. \|5 - 2\| + \|5 - 6\| = 4 

- Index 6: Two more 3s are found at indices 2 and 5. \|6 - 2\| + \|6 - 5\| = 5

**Example 2:**

**Input:** arr = [10,5,10,10]

**Output:** [5,0,3,4]

**Explanation:** 

- Index 0: Two more 10s are found at indices 2 and 3. \|0 - 2\| + \|0 - 3\| = 5 

- Index 1: There is only one 5 in the array, so its sum of intervals to identical elements is 0. 

- Index 2: Two more 10s are found at indices 0 and 3. \|2 - 0\| + \|2 - 3\| = 3 

- Index 3: Two more 10s are found at indices 0 and 2. \|3 - 0\| + \|3 - 2\| = 4

**Constraints:**

*   `n == arr.length`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   <code>1 <= arr[i] <= 10<sup>5</sup></code>

## Solution

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class Solution {
    public long[] getDistances(int[] arr) {
        int n = arr.length;
        Map<Integer, List<Integer>> map = new HashMap<>();
        for (int i = 0; i < n; i++) {
            List<Integer> list = map.get(arr[i]);
            if (list == null) {
                list = new ArrayList<>();
            }
            list.add(i);
            map.put(arr[i], list);
        }
        long[] ans = new long[n];
        Arrays.fill(ans, 0);
        for (List<Integer> list : map.values()) {
            long sum = 0;
            int first = list.get(0);
            for (int i = 1; i < list.size(); i++) {
                sum = sum + list.get(i) - first;
            }
            ans[first] = sum;
            int prevElements = 0;
            int nextElements = list.size() - 2;
            for (int i = 1; i < list.size(); i++) {
                int diff = list.get(i) - list.get(i - 1);
                sum = sum + (long) diff * (prevElements - nextElements);
                ans[list.get(i)] = sum;
                prevElements++;
                nextElements--;
            }
        }
        return ans;
    }
}
```