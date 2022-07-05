[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 119\. Pascal's Triangle II

Easy

Given an integer `rowIndex`, return the `rowIndexth` (**0-indexed**) row of the **Pascal's triangle**.

In **Pascal's triangle**, each number is the sum of the two numbers directly above it as shown:

![](https://upload.wikimedia.org/wikipedia/commons/0/0d/PascalTriangleAnimated2.gif)

**Example 1:**

**Input:** rowIndex = 3

**Output:** [1,3,3,1] 

**Example 2:**

**Input:** rowIndex = 0

**Output:** [1] 

**Example 3:**

**Input:** rowIndex = 1

**Output:** [1,1] 

**Constraints:**

*   `0 <= rowIndex <= 33`

**Follow up:** Could you optimize your algorithm to use only `O(rowIndex)` extra space?

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<Integer> getRow(int rowIndex) {
        int[] buffer = new int[rowIndex + 1];
        buffer[0] = 1;
        computeRow(buffer, 1);
        // Copy buffer to List of Integer.
        List<Integer> ans = new ArrayList<>(buffer.length);
        for (int j : buffer) {
            ans.add(j);
        }
        return ans;
    }

    private void computeRow(int[] buffer, int k) {
        if (k >= buffer.length) {
            return;
        }
        int previous = buffer[0];
        for (int i = 1; i < k; i++) {
            int tmp = previous + buffer[i];
            previous = buffer[i];
            buffer[i] = tmp;
        }
        buffer[k] = 1;
        computeRow(buffer, k + 1);
    }
}
```