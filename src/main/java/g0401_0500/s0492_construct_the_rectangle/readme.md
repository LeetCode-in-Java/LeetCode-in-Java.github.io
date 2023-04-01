[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 492\. Construct the Rectangle

Easy

A web developer needs to know how to design a web page's size. So, given a specific rectangular web page’s area, your job by now is to design a rectangular web page, whose length L and width W satisfy the following requirements:

1.  The area of the rectangular web page you designed must equal to the given target area.
2.  The width `W` should not be larger than the length `L`, which means `L >= W`.
3.  The difference between length `L` and width `W` should be as small as possible.

Return _an array `[L, W]` where `L` and `W` are the length and width of the web page you designed in sequence._

**Example 1:**

**Input:** area = 4

**Output:** [2,2]

**Explanation:** The target area is 4, and all the possible ways to construct it are [1,4], [2,2], [4,1]. But according to requirement 2, [1,4] is illegal; according to requirement 3, [4,1] is not optimal compared to [2,2]. So the length L is 2, and the width W is 2.

**Example 2:**

**Input:** area = 37

**Output:** [37,1]

**Example 3:**

**Input:** area = 122122

**Output:** [427,286]

**Constraints:**

*   <code>1 <= area <= 10<sup>7</sup></code>

## Solution

```java
public class Solution {
    /*
      Algorithm:
     - start with an index i from the square root all the way to 1;
     - if at any time, area % i == 0 (so i is a divisor of area), then it's the closest solution.
    */
    public int[] constructRectangle(int area) {
        int low = (int) Math.sqrt(area);
        while (low > 0) {
            if (area % low == 0) {
                return new int[] {area / low, low};
            }
            low--;
        }
        return new int[] {0, 0};
    }
}
```