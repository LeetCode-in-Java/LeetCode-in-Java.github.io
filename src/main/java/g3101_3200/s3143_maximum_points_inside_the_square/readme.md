[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3143\. Maximum Points Inside the Square

Medium

You are given a 2D array `points` and a string `s` where, `points[i]` represents the coordinates of point `i`, and `s[i]` represents the **tag** of point `i`.

A **valid** square is a square centered at the origin `(0, 0)`, has edges parallel to the axes, and **does not** contain two points with the same tag.

Return the **maximum** number of points contained in a **valid** square.

Note:

*   A point is considered to be inside the square if it lies on or within the square's boundaries.
*   The side length of the square can be zero.

**Example 1:**

![](https://assets.leetcode.com/uploads/2024/03/29/3708-tc1.png)

**Input:** points = \[\[2,2],[-1,-2],[-4,4],[-3,1],[3,-3]], s = "abdca"

**Output:** 2

**Explanation:**

The square of side length 4 covers two points `points[0]` and `points[1]`.

**Example 2:**

![](https://assets.leetcode.com/uploads/2024/03/29/3708-tc2.png)

**Input:** points = \[\[1,1],[-2,-2],[-2,2]], s = "abb"

**Output:** 1

**Explanation:**

The square of side length 2 covers one point, which is `points[0]`.

**Example 3:**

**Input:** points = \[\[1,1],[-1,-1],[2,-2]], s = "ccd"

**Output:** 0

**Explanation:**

It's impossible to make any valid squares centered at the origin such that it covers only one point among `points[0]` and `points[1]`.

**Constraints:**

*   <code>1 <= s.length, points.length <= 10<sup>5</sup></code>
*   `points[i].length == 2`
*   <code>-10<sup>9</sup> <= points[i][0], points[i][1] <= 10<sup>9</sup></code>
*   `s.length == points.length`
*   `points` consists of distinct coordinates.
*   `s` consists only of lowercase English letters.

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int maxPointsInsideSquare(int[][] points, String s) {
        int[] tags = new int[26];
        Arrays.fill(tags, Integer.MAX_VALUE);
        int secondMin = Integer.MAX_VALUE;
        for (int i = 0; i < s.length(); i++) {
            int dist = Math.max(Math.abs(points[i][0]), Math.abs(points[i][1]));
            char c = s.charAt(i);
            if (tags[c - 'a'] == Integer.MAX_VALUE) {
                tags[c - 'a'] = dist;
            } else if (dist < tags[c - 'a']) {
                secondMin = Math.min(secondMin, tags[c - 'a']);
                tags[c - 'a'] = dist;
            } else {
                secondMin = Math.min(secondMin, dist);
            }
        }
        int count = 0;
        for (int dist : tags) {
            if (dist < secondMin) {
                count++;
            }
        }
        return count;
    }
}
```