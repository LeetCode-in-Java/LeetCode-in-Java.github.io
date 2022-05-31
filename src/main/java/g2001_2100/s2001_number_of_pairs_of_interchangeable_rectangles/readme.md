## 2001\. Number of Pairs of Interchangeable Rectangles

Medium

You are given `n` rectangles represented by a **0-indexed** 2D integer array `rectangles`, where <code>rectangles[i] = [width<sub>i</sub>, height<sub>i</sub>]</code> denotes the width and height of the <code>i<sup>th</sup></code> rectangle.

Two rectangles `i` and `j` (`i < j`) are considered **interchangeable** if they have the **same** width-to-height ratio. More formally, two rectangles are **interchangeable** if <code>width<sub>i</sub>/height<sub>i</sub> == width<sub>j</sub>/height<sub>j</sub></code> (using decimal division, not integer division).

Return _the **number** of pairs of **interchangeable** rectangles in_ `rectangles`.

**Example 1:**

**Input:** rectangles = [[4,8],[3,6],[10,20],[15,30]]

**Output:** 6

**Explanation:** The following are the interchangeable pairs of rectangles by index (0-indexed): 

- Rectangle 0 with rectangle 1: 4/8 == 3/6. 

- Rectangle 0 with rectangle 2: 4/8 == 10/20. 

- Rectangle 0 with rectangle 3: 4/8 == 15/30. 

- Rectangle 1 with rectangle 2: 3/6 == 10/20. 

- Rectangle 1 with rectangle 3: 3/6 == 15/30. 

- Rectangle 2 with rectangle 3: 10/20 == 15/30.

**Example 2:**

**Input:** rectangles = [[4,5],[7,8]]

**Output:** 0

**Explanation:** There are no interchangeable pairs of rectangles.

**Constraints:**

*   `n == rectangles.length`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   `rectangles[i].length == 2`
*   <code>1 <= width<sub>i</sub>, height<sub>i</sub> <= 10<sup>5</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    private long factorial(long n) {
        long m = 0;
        while (n > 0) {
            m += n;
            n = n - 1;
        }
        return m;
    }

    public long interchangeableRectangles(int[][] rec) {
        double[] ratio = new double[rec.length];
        for (int i = 0; i < rec.length; i++) {
            ratio[i] = (double) rec[i][0] / rec[i][1];
        }
        Arrays.sort(ratio);
        long res = 0;
        int k = 0;
        for (int j = 0; j < ratio.length - 1; j++) {
            if (ratio[j] == ratio[j + 1]) {
                k++;
            }
            if (ratio[j] != ratio[j + 1] || j + 2 == ratio.length) {
                res += factorial(k);
                k = 0;
            }
        }
        return res;
    }
}
```