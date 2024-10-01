[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3301\. Maximize the Total Height of Unique Towers

Medium

You are given an array `maximumHeight`, where `maximumHeight[i]` denotes the **maximum** height the <code>i<sup>th</sup></code> tower can be assigned.

Your task is to assign a height to each tower so that:

1.  The height of the <code>i<sup>th</sup></code> tower is a positive integer and does not exceed `maximumHeight[i]`.
2.  No two towers have the same height.

Return the **maximum** possible total sum of the tower heights. If it's not possible to assign heights, return `-1`.

**Example 1:**

**Input:** maximumHeight = [2,3,4,3]

**Output:** 10

**Explanation:**

We can assign heights in the following way: `[1, 2, 4, 3]`.

**Example 2:**

**Input:** maximumHeight = [15,10]

**Output:** 25

**Explanation:**

We can assign heights in the following way: `[15, 10]`.

**Example 3:**

**Input:** maximumHeight = [2,2,1]

**Output:** \-1

**Explanation:**

It's impossible to assign positive heights to each index so that no two towers have the same height.

**Constraints:**

*   <code>1 <= maximumHeight.length <= 10<sup>5</sup></code>
*   <code>1 <= maximumHeight[i] <= 10<sup>9</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    public long maximumTotalSum(int[] maximumHeight) {
        Arrays.sort(maximumHeight);
        long result = maximumHeight[maximumHeight.length - 1];
        long previousHeight = maximumHeight[maximumHeight.length - 1];
        for (int i = maximumHeight.length - 2; i >= 0; i--) {
            if (previousHeight == 1) {
                return -1;
            }
            long height = maximumHeight[i];
            if (height >= previousHeight) {
                result = result + previousHeight - 1;
                previousHeight = previousHeight - 1;
            } else {
                result = result + height;
                previousHeight = height;
            }
        }
        return result;
    }
}
```