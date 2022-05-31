## 1889\. Minimum Space Wasted From Packaging

Hard

You have `n` packages that you are trying to place in boxes, **one package in each box**. There are `m` suppliers that each produce boxes of **different sizes** (with infinite supply). A package can be placed in a box if the size of the package is **less than or equal to** the size of the box.

The package sizes are given as an integer array `packages`, where `packages[i]` is the **size** of the <code>i<sup>th</sup></code> package. The suppliers are given as a 2D integer array `boxes`, where `boxes[j]` is an array of **box sizes** that the <code>j<sup>th</sup></code> supplier produces.

You want to choose a **single supplier** and use boxes from them such that the **total wasted space** is **minimized**. For each package in a box, we define the space **wasted** to be `size of the box - size of the package`. The **total wasted space** is the sum of the space wasted in **all** the boxes.

*   For example, if you have to fit packages with sizes `[2,3,5]` and the supplier offers boxes of sizes `[4,8]`, you can fit the packages of size-`2` and size-`3` into two boxes of size-`4` and the package with size-`5` into a box of size-`8`. This would result in a waste of `(4-2) + (4-3) + (8-5) = 6`.

Return _the **minimum total wasted space** by choosing the box supplier **optimally**, or_ `-1` _if it is **impossible** to fit all the packages inside boxes._ Since the answer may be **large**, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** packages = [2,3,5], boxes = [[4,8],[2,8]]

**Output:** 6

**Explanation:**: It is optimal to choose the first supplier, using two size-4 boxes and one size-8 box.

The total waste is (4-2) + (4-3) + (8-5) = 6. 

**Example 2:**

**Input:** packages = [2,3,5], boxes = [[1,4],[2,3],[3,4]]

**Output:** -1

**Explanation:** There is no box that the package of size 5 can fit in. 

**Example 3:**

**Input:** packages = [3,5,8,10,11,12], boxes = [[12],[11,9],[10,5,14]]

**Output:** 9

**Explanation:** It is optimal to choose the third supplier, using two size-5 boxes, two size-10 boxes, and two size-14 boxes.

The total waste is (5-3) + (5-5) + (10-8) + (10-10) + (14-11) + (14-12) = 9. 

**Constraints:**

*   `n == packages.length`
*   `m == boxes.length`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   <code>1 <= m <= 10<sup>5</sup></code>
*   <code>1 <= packages[i] <= 10<sup>5</sup></code>
*   <code>1 <= boxes[j].length <= 10<sup>5</sup></code>
*   <code>1 <= boxes[j][k] <= 10<sup>5</sup></code>
*   <code>sum(boxes[j].length) <= 10<sup>5</sup></code>
*   The elements in `boxes[j]` are **distinct**.

## Solution

```java
import java.util.Arrays;

@SuppressWarnings("java:S135")
public class Solution {
    private static final int MOD = 1_000_000_007;

    public int minWastedSpace(int[] packages, int[][] boxes) {
        int numPackages = packages.length;
        Arrays.sort(packages);
        long[] preSum = new long[numPackages];
        preSum[0] = packages[0];
        for (int i = 1; i < packages.length; i++) {
            preSum[i] = packages[i] + preSum[i - 1];
        }
        long ans = Long.MAX_VALUE;
        for (int[] box : boxes) {
            Arrays.sort(box);
            // Box of required size not present
            if (packages[numPackages - 1] > box[box.length - 1]) {
                continue;
            }
            // Find the total space wasted
            long totalWastedSpace = 0;
            int prev = -1;
            for (int j : box) {
                if (prev == packages.length - 1) {
                    break;
                }
                if (j < packages[0] || j < packages[prev + 1]) {
                    continue;
                }
                // Find up to which package the current box can fit
                int upper = findUpperBound(packages, j);
                if (upper == -1) {
                    continue;
                }
                // The current box will be able to handle the packages from
                // prev + 1 to the upper index
                long totalSpace = ((long) upper - (long) prev) * j;
                long packageSum = preSum[upper] - (prev >= 0 ? preSum[prev] : 0);
                long spaceWastedCurr = totalSpace - packageSum;
                totalWastedSpace += spaceWastedCurr;
                prev = upper;
            }
            ans = Math.min(ans, totalWastedSpace);
        }
        return ans == Long.MAX_VALUE ? -1 : (int) (ans % MOD);
    }

    private int findUpperBound(int[] packages, int key) {
        int l = 0;
        int h = packages.length;
        while (l < h) {
            int m = l + (h - l) / 2;
            if (packages[m] <= key) {
                l = m + 1;
            } else {
                h = m;
            }
        }
        return h - 1;
    }
}
```