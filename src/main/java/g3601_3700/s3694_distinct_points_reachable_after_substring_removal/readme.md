[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3694\. Distinct Points Reachable After Substring Removal

Medium

You are given a string `s` consisting of characters `'U'`, `'D'`, `'L'`, and `'R'`, representing moves on an infinite 2D Cartesian grid.

*   `'U'`: Move from `(x, y)` to `(x, y + 1)`.
*   `'D'`: Move from `(x, y)` to `(x, y - 1)`.
*   `'L'`: Move from `(x, y)` to `(x - 1, y)`.
*   `'R'`: Move from `(x, y)` to `(x + 1, y)`.

You are also given a positive integer `k`.

You **must** choose and remove **exactly one** contiguous substring of length `k` from `s`. Then, start from coordinate `(0, 0)` and perform the remaining moves in order.

Return an integer denoting the number of **distinct** final coordinates reachable.

**Example 1:**

**Input:** s = "LUL", k = 1

**Output:** 2

**Explanation:**

After removing a substring of length 1, `s` can be `"UL"`, `"LL"` or `"LU"`. Following these moves, the final coordinates will be `(-1, 1)`, `(-2, 0)` and `(-1, 1)` respectively. There are two distinct points `(-1, 1)` and `(-2, 0)` so the answer is 2.

**Example 2:**

**Input:** s = "UDLR", k = 4

**Output:** 1

**Explanation:**

After removing a substring of length 4, `s` can only be the empty string. The final coordinates will be `(0, 0)`. There is only one distinct point `(0, 0)` so the answer is 1.

**Example 3:**

**Input:** s = "UU", k = 1

**Output:** 1

**Explanation:**

After removing a substring of length 1, `s` becomes `"U"`, which always ends at `(0, 1)`, so there is only one distinct final coordinate.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` consists of only `'U'`, `'D'`, `'L'`, and `'R'`.
*   `1 <= k <= s.length`

## Solution

```java
import java.util.HashSet;
import java.util.Set;

public class Solution {
    public int distinctPoints(String s, int k) {
        Set<Long> seen = new HashSet<>();
        seen.add(0L);
        int x = 0;
        int y = 0;
        for (int i = k; i < s.length(); i++) {
            // add new step
            switch (s.charAt(i)) {
                case 'U':
                    y++;
                    break;
                case 'D':
                    y--;
                    break;
                case 'L':
                    x++;
                    break;
                case 'R':
                default:
                    x--;
                    break;
            }
            // remove old step
            switch (s.charAt(i - k)) {
                case 'U':
                    y--;
                    break;
                case 'D':
                    y++;
                    break;
                case 'L':
                    x--;
                    break;
                case 'R':
                default:
                    x++;
                    break;
            }
            seen.add(1000000L * x + y);
        }
        return seen.size();
    }
}
```