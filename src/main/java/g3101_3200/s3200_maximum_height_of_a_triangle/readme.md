[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3200\. Maximum Height of a Triangle

Easy

You are given two integers `red` and `blue` representing the count of red and blue colored balls. You have to arrange these balls to form a triangle such that the 1<sup>st</sup> row will have 1 ball, the 2<sup>nd</sup> row will have 2 balls, the 3<sup>rd</sup> row will have 3 balls, and so on.

All the balls in a particular row should be the **same** color, and adjacent rows should have **different** colors.

Return the **maximum** _height of the triangle_ that can be achieved.

**Example 1:**

**Input:** red = 2, blue = 4

**Output:** 3

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/06/16/brb.png)

The only possible arrangement is shown above.

**Example 2:**

**Input:** red = 2, blue = 1

**Output:** 2

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/06/16/br.png)   
 The only possible arrangement is shown above.

**Example 3:**

**Input:** red = 1, blue = 1

**Output:** 1

**Example 4:**

**Input:** red = 10, blue = 1

**Output:** 2

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/06/16/br.png)   
 The only possible arrangement is shown above.

**Constraints:**

*   `1 <= red, blue <= 100`

## Solution

```java
@SuppressWarnings("java:S135")
public class Solution {
    private int count(int v1, int v2) {
        int ct = 1;
        boolean flag = true;
        while (true) {
            if (flag) {
                if (ct <= v1) {
                    v1 -= ct;
                } else {
                    break;
                }
            } else {
                if (ct <= v2) {
                    v2 -= ct;
                } else {
                    break;
                }
            }
            ct++;
            flag = !flag;
        }
        return ct - 1;
    }

    public int maxHeightOfTriangle(int red, int blue) {
        return Math.max(count(red, blue), count(blue, red));
    }
}
```