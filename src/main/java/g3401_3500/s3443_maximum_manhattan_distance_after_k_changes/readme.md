[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3443\. Maximum Manhattan Distance After K Changes

Medium

You are given a string `s` consisting of the characters `'N'`, `'S'`, `'E'`, and `'W'`, where `s[i]` indicates movements in an infinite grid:

*   `'N'` : Move north by 1 unit.
*   `'S'` : Move south by 1 unit.
*   `'E'` : Move east by 1 unit.
*   `'W'` : Move west by 1 unit.

Initially, you are at the origin `(0, 0)`. You can change **at most** `k` characters to any of the four directions.

Find the **maximum** **Manhattan distance** from the origin that can be achieved **at any time** while performing the movements **in order**.

The **Manhattan Distance** between two cells <code>(x<sub>i</sub>, y<sub>i</sub>)</code> and <code>(x<sub>j</sub>, y<sub>j</sub>)</code> is <code>|x<sub>i</sub> - x<sub>j</sub>| + |y<sub>i</sub> - y<sub>j</sub>|</code>.

**Example 1:**

**Input:** s = "NWSE", k = 1

**Output:** 3

**Explanation:**

Change `s[2]` from `'S'` to `'N'`. The string `s` becomes `"NWNE"`.

| Movement         | Position (x, y) | Manhattan Distance | Maximum |
|-----------------|----------------|--------------------|---------|
| s[0] == 'N'    | (0, 1)         | 0 + 1 = 1         | 1       |
| s[1] == 'W'    | (-1, 1)        | 1 + 1 = 2         | 2       |
| s[2] == 'N'    | (-1, 2)        | 1 + 2 = 3         | 3       |
| s[3] == 'E'    | (0, 2)         | 0 + 2 = 2         | 3       |

The maximum Manhattan distance from the origin that can be achieved is 3. Hence, 3 is the output.

**Example 2:**

**Input:** s = "NSWWEW", k = 3

**Output:** 6

**Explanation:**

Change `s[1]` from `'S'` to `'N'`, and `s[4]` from `'E'` to `'W'`. The string `s` becomes `"NNWWWW"`.

The maximum Manhattan distance from the origin that can be achieved is 6. Hence, 6 is the output.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `0 <= k <= s.length`
*   `s` consists of only `'N'`, `'S'`, `'E'`, and `'W'`.

## Solution

```java
public class Solution {
    public int maxDistance(String s, int k) {
        int north = 0;
        int south = 0;
        int west = 0;
        int east = 0;
        int result = 0;
        for (char c : s.toCharArray()) {
            if (c == 'N') {
                north++;
            } else if (c == 'S') {
                south++;
            } else if (c == 'E') {
                east++;
            } else if (c == 'W') {
                west++;
            }
            int hMax = Math.max(north, south);
            int vMax = Math.max(west, east);
            int hMin = Math.min(north, south);
            int vMin = Math.min(west, east);
            if (hMin + vMin >= k) {
                int curr = hMax + vMax + k - (hMin + vMin - k);
                result = Math.max(result, curr);
            } else {
                int curr = hMax + vMax + hMin + vMin;
                result = Math.max(result, curr);
            }
        }
        return result;
    }
}
```