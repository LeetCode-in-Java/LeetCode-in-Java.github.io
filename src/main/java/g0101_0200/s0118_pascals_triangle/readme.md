[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 118\. Pascal's Triangle

Easy

Given an integer `numRows`, return the first numRows of **Pascal's triangle**.

In **Pascal's triangle**, each number is the sum of the two numbers directly above it as shown:

![](https://upload.wikimedia.org/wikipedia/commons/0/0d/PascalTriangleAnimated2.gif)

**Example 1:**

**Input:** numRows = 5

**Output:** [[1],[1,1],[1,2,1],[1,3,3,1],[1,4,6,4,1]] 

**Example 2:**

**Input:** numRows = 1

**Output:** [[1]] 

**Constraints:**

*   `1 <= numRows <= 30`

## Solution

```java
import java.util.ArrayList;
import java.util.List;

@SuppressWarnings("java:S2589")
public class Solution {
    public List<List<Integer>> generate(int numRows) {
        List<List<Integer>> output = new ArrayList<>();
        for (int i = 0; i < numRows; i++) {
            List<Integer> currRow = new ArrayList<>();
            for (int j = 0; j <= i; j++) {
                if (j == 0 || j == i || i <= 1) {
                    currRow.add(1);
                } else {
                    int currCell = output.get(i - 1).get(j - 1) + output.get(i - 1).get(j);
                    currRow.add(currCell);
                }
            }
            output.add(currRow);
        }
        return output;
    }
}
```