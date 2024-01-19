[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2975\. Maximum Square Area by Removing Fences From a Field

Medium

There is a large `(m - 1) x (n - 1)` rectangular field with corners at `(1, 1)` and `(m, n)` containing some horizontal and vertical fences given in arrays `hFences` and `vFences` respectively.

Horizontal fences are from the coordinates `(hFences[i], 1)` to `(hFences[i], n)` and vertical fences are from the coordinates `(1, vFences[i])` to `(m, vFences[i])`.

Return _the **maximum** area of a **square** field that can be formed by **removing** some fences (**possibly none**) or_ `-1` _if it is impossible to make a square field_.

Since the answer may be large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Note:** The field is surrounded by two horizontal fences from the coordinates `(1, 1)` to `(1, n)` and `(m, 1)` to `(m, n)` and two vertical fences from the coordinates `(1, 1)` to `(m, 1)` and `(1, n)` to `(m, n)`. These fences **cannot** be removed.

**Example 1:**

![](https://assets.leetcode.com/uploads/2023/11/05/screenshot-from-2023-11-05-22-40-25.png)

**Input:** m = 4, n = 3, hFences = [2,3], vFences = [2]

**Output:** 4

**Explanation:** Removing the horizontal fence at 2 and the vertical fence at 2 will give a square field of area 4.

**Example 2:**

![](https://assets.leetcode.com/uploads/2023/11/22/maxsquareareaexample1.png)

**Input:** m = 6, n = 7, hFences = [2], vFences = [4]

**Output:** -1

**Explanation:** It can be proved that there is no way to create a square field by removing fences.

**Constraints:**

*   <code>3 <= m, n <= 10<sup>9</sup></code>
*   `1 <= hFences.length, vFences.length <= 600`
*   `1 < hFences[i] < m`
*   `1 < vFences[i] < n`
*   `hFences` and `vFences` are unique.

## Solution

```java
import java.util.Arrays;
import java.util.HashSet;
import java.util.Set;

public class Solution {
    public int maximizeSquareArea(
            final int m, final int n, final int[] hFences, final int[] vFences) {
        int[] hFencesWithBorder = new int[hFences.length + 2];
        System.arraycopy(hFences, 0, hFencesWithBorder, 0, hFences.length);
        hFencesWithBorder[hFences.length] = 1;
        hFencesWithBorder[hFences.length + 1] = m;
        Arrays.sort(hFencesWithBorder);
        Set<Integer> edgeSet = new HashSet<>();
        for (int i = 0; i < hFencesWithBorder.length; i += 1) {
            for (int j = i + 1; j < hFencesWithBorder.length; j += 1) {
                edgeSet.add(hFencesWithBorder[j] - hFencesWithBorder[i]);
            }
        }
        int maxEdge = -1;
        int[] vFencesWithBorder = new int[vFences.length + 2];
        System.arraycopy(vFences, 0, vFencesWithBorder, 0, vFences.length);
        vFencesWithBorder[vFences.length] = 1;
        vFencesWithBorder[vFences.length + 1] = n;
        Arrays.sort(vFencesWithBorder);
        for (int i = 0; i < vFencesWithBorder.length; i += 1) {
            for (int j = i + 1; j < vFencesWithBorder.length; j += 1) {
                int curEdge = vFencesWithBorder[j] - vFencesWithBorder[i];
                if (edgeSet.contains(curEdge) && curEdge > maxEdge) {
                    maxEdge = curEdge;
                }
            }
        }
        int mod = (int) 1e9 + 7;
        return (maxEdge != -1) ? (int) ((long) maxEdge * maxEdge % mod) : -1;
    }
}
```