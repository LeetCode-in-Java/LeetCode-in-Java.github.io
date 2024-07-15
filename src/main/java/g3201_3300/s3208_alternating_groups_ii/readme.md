[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3208\. Alternating Groups II

Medium

There is a circle of red and blue tiles. You are given an array of integers `colors` and an integer `k`. The color of tile `i` is represented by `colors[i]`:

*   `colors[i] == 0` means that tile `i` is **red**.
*   `colors[i] == 1` means that tile `i` is **blue**.

An **alternating** group is every `k` contiguous tiles in the circle with **alternating** colors (each tile in the group except the first and last one has a different color from its **left** and **right** tiles).

Return the number of **alternating** groups.

**Note** that since `colors` represents a **circle**, the **first** and the **last** tiles are considered to be next to each other.

**Example 1:**

**Input:** colors = [0,1,0,1,0], k = 3

**Output:** 3

**Explanation:**

**![](https://assets.leetcode.com/uploads/2024/06/19/screenshot-2024-05-28-183519.png)**

Alternating groups:

![](https://assets.leetcode.com/uploads/2024/05/28/screenshot-2024-05-28-182448.png)![](https://assets.leetcode.com/uploads/2024/05/28/screenshot-2024-05-28-182844.png)![](https://assets.leetcode.com/uploads/2024/05/28/screenshot-2024-05-28-183057.png)

**Example 2:**

**Input:** colors = [0,1,0,0,1,0,1], k = 6

**Output:** 2

**Explanation:**

**![](https://assets.leetcode.com/uploads/2024/06/19/screenshot-2024-05-28-183907.png)**

Alternating groups:

![](https://assets.leetcode.com/uploads/2024/06/19/screenshot-2024-05-28-184128.png)![](https://assets.leetcode.com/uploads/2024/06/19/screenshot-2024-05-28-184240.png)

**Example 3:**

**Input:** colors = [1,1,0,1], k = 4

**Output:** 0

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/06/19/screenshot-2024-05-28-184516.png)

**Constraints:**

*   <code>3 <= colors.length <= 10<sup>5</sup></code>
*   `0 <= colors[i] <= 1`
*   `3 <= k <= colors.length`

## Solution

```java
public class Solution {
    public int numberOfAlternatingGroups(int[] colors, int k) {
        int i = 0;
        int len = 0;
        int total = 0;
        while (i < colors.length - 1) {
            int j = i + 1;
            if (colors[j] != colors[i]) {
                len = 2;
                j++;
                while (j < colors.length && colors[j] != colors[j - 1]) {
                    j++;
                    len++;
                }
                if (j == colors.length) {
                    break;
                }
                total += Math.max(0, (len - k + 1));
            }
            i = j;
            len = 0;
        }
        if (colors[0] != colors[colors.length - 1]) {
            // if(len == colors.length) {
            //     return Math.max(0, colors.length);
            // }
            len = len == 0 ? 2 : len + 1;
            int j = 1;
            while (j < colors.length && colors[j] != colors[j - 1]) {
                j++;
                len++;
            }
            if (j >= k) {
                len -= (j - k + 1);
            }
        }
        total += Math.max(0, (len - k + 1));
        return total;
    }
}
```