[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 497\. Random Point in Non-overlapping Rectangles

Medium

You are given an array of non-overlapping axis-aligned rectangles `rects` where <code>rects[i] = [a<sub>i</sub>, b<sub>i</sub>, x<sub>i</sub>, y<sub>i</sub>]</code> indicates that <code>(a<sub>i</sub>, b<sub>i</sub>)</code> is the bottom-left corner point of the <code>i<sup>th</sup></code> rectangle and <code>(x<sub>i</sub>, y<sub>i</sub>)</code> is the top-right corner point of the <code>i<sup>th</sup></code> rectangle. Design an algorithm to pick a random integer point inside the space covered by one of the given rectangles. A point on the perimeter of a rectangle is included in the space covered by the rectangle.

Any integer point inside the space covered by one of the given rectangles should be equally likely to be returned.

**Note** that an integer point is a point that has integer coordinates.

Implement the `Solution` class:

*   `Solution(int[][] rects)` Initializes the object with the given rectangles `rects`.
*   `int[] pick()` Returns a random integer point `[u, v]` inside the space covered by one of the given rectangles.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/07/24/lc-pickrandomrec.jpg)

**Input** ["Solution", "pick", "pick", "pick", "pick", "pick"] [[[[-2, -2, 1, 1], [2, 2, 4, 6]]], [], [], [], [], []]

**Output:** [null, [1, -2], [1, -1], [-1, -2], [-2, -2], [0, 0]]

**Explanation:** 

    Solution solution = new Solution([[-2, -2, 1, 1], [2, 2, 4, 6]]);
    solution.pick(); // return [1, -2]
    solution.pick(); // return [1, -1]
    solution.pick(); // return [-1, -2]
    solution.pick(); // return [-2, -2]
    solution.pick(); // return [0, 0]

**Constraints:**

*   `1 <= rects.length <= 100`
*   `rects[i].length == 4`
*   <code>-10<sup>9</sup> <= a<sub>i</sub> < x<sub>i</sub> <= 10<sup>9</sup></code>
*   <code>-10<sup>9</sup> <= b<sub>i</sub> < y<sub>i</sub> <= 10<sup>9</sup></code>
*   <code>x<sub>i</sub> - a<sub>i</sub> <= 2000</code>
*   <code>y<sub>i</sub> - b<sub>i</sub> <= 2000</code>
*   All the rectangles do not overlap.
*   At most <code>10<sup>4</sup></code> calls will be made to `pick`.

## Solution

```java
import java.util.Random;

@SuppressWarnings("java:S2245")
public class Solution {
    private final int[] weights;
    private final int[][] rects;
    private final Random random;

    public Solution(int[][] rects) {
        this.weights = new int[rects.length];
        this.rects = rects;
        this.random = new Random();
        for (int i = 0; i < rects.length; i++) {
            int[] rect = rects[i];
            int count = (1 + rect[2] - rect[0]) * (1 + rect[3] - rect[1]);
            weights[i] = (i == 0 ? 0 : weights[i - 1]) + count;
        }
    }

    public int[] pick() {
        int picked = 1 + random.nextInt(weights[weights.length - 1]);
        int idx = findGreaterOrEqual(picked);
        return getRandomPoint(idx);
    }

    private int findGreaterOrEqual(int target) {
        int left = 0;
        int right = weights.length - 1;
        while (left + 1 < right) {
            int mid = left + (right - left) / 2;
            if (weights[mid] >= target) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return weights[left] >= target ? left : right;
    }

    private int[] getRandomPoint(int idx) {
        int[] r = rects[idx];
        int left = r[0];
        int right = r[2];
        int bot = r[1];
        int top = r[3];
        return new int[] {
            left + random.nextInt(right - left + 1), bot + random.nextInt(top - bot + 1)
        };
    }
}

/*
 * Your Solution object will be instantiated and called as such:
 * Solution obj = new Solution(rects);
 * int[] param_1 = obj.pick();
 */
```