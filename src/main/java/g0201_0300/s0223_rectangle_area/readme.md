[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 223\. Rectangle Area

Medium

Given the coordinates of two **rectilinear** rectangles in a 2D plane, return _the total area covered by the two rectangles_.

The first rectangle is defined by its **bottom-left** corner `(ax1, ay1)` and its **top-right** corner `(ax2, ay2)`.

The second rectangle is defined by its **bottom-left** corner `(bx1, by1)` and its **top-right** corner `(bx2, by2)`.

**Example 1:**

![Rectangle Area](https://assets.leetcode.com/uploads/2021/05/08/rectangle-plane.png)

**Input:** ax1 = -3, ay1 = 0, ax2 = 3, ay2 = 4, bx1 = 0, by1 = -1, bx2 = 9, by2 = 2

**Output:** 45 

**Example 2:**

**Input:** ax1 = -2, ay1 = -2, ax2 = 2, ay2 = 2, bx1 = -2, by1 = -2, bx2 = 2, by2 = 2

**Output:** 16 

**Constraints:**

*   <code>-10<sup>4</sup> <= ax1, ay1, ax2, ay2, bx1, by1, bx2, by2 <= 10<sup>4</sup></code>

## Solution

```java
@SuppressWarnings("java:S107")
public class Solution {
    public int computeArea(int ax1, int ay1, int ax2, int ay2, int bx1, int by1, int bx2, int by2) {
        long left = Math.max(ax1, bx1);
        long right = Math.min(ax2, bx2);
        long top = Math.min(ay2, by2);
        long bottom = Math.max(ay1, by1);
        long area = (right - left) * (top - bottom);
        // if not overlaping, either of these two will be non-posittive
        // if right - left = 0, are will automtically be 0 as well
        if (right - left < 0 || top - bottom < 0) {
            area = 0;
        }
        return (int) ((ax2 - ax1) * (ay2 - ay1) + (bx2 - bx1) * (by2 - by1) - area);
    }
}
```