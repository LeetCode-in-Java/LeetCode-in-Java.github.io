[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1051\. Height Checker

Easy

A school is trying to take an annual photo of all the students. The students are asked to stand in a single file line in **non-decreasing order** by height. Let this ordering be represented by the integer array `expected` where `expected[i]` is the expected height of the <code>i<sup>th</sup></code> student in line.

You are given an integer array `heights` representing the **current order** that the students are standing in. Each `heights[i]` is the height of the <code>i<sup>th</sup></code> student in line (**0-indexed**).

Return _the **number of indices** where_ `heights[i] != expected[i]`.

**Example 1:**

**Input:** heights = [1,1,4,2,1,3]

**Output:** 3

**Explanation:**

heights: [1,1,4,2,1,3]

expected: [1,1,1,2,3,4]

Indices 2, 4, and 5 do not match.

**Example 2:**

**Input:** heights = [5,1,2,3,4]

**Output:** 5

**Explanation:**

heights: [5,1,2,3,4]

expected: [1,2,3,4,5]

All indices do not match.

**Example 3:**

**Input:** heights = [1,2,3,4,5]

**Output:** 0

**Explanation:**

heights: [1,2,3,4,5]

expected: [1,2,3,4,5]

All indices match.

**Constraints:**

*   `1 <= heights.length <= 100`
*   `1 <= heights[i] <= 100`

## Solution

```java
public class Solution {
    public int heightChecker(int[] heights) {
        int heightDiff = 0;
        int[] count = new int[101];
        int[] actualLine = new int[heights.length];
        for (int height : heights) {
            count[height]++;
        }
        int heightLength = 0;
        for (int i = 0; i < count.length; i++) {
            if (count[i] > 0) {
                for (int j = 0; j < count[i]; j++) {
                    actualLine[heightLength] = i;
                    heightLength++;
                }
                count[i] = 0;
            }
        }
        for (int i = 0; i < heights.length; i++) {
            if (actualLine[i] != heights[i]) {
                heightDiff++;
            }
        }
        return heightDiff;
    }
}
```