[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1812\. Determine Color of a Chessboard Square

Easy

You are given `coordinates`, a string that represents the coordinates of a square of the chessboard. Below is a chessboard for your reference.

![](https://assets.leetcode.com/uploads/2021/02/19/screenshot-2021-02-20-at-22159-pm.png)

Return `true` _if the square is white, and_ `false` _if the square is black_.

The coordinate will always represent a valid chessboard square. The coordinate will always have the letter first, and the number second.

**Example 1:**

**Input:** coordinates = "a1"

**Output:** false

**Explanation:** From the chessboard above, the square with coordinates "a1" is black, so return false.

**Example 2:**

**Input:** coordinates = "h3"

**Output:** true

**Explanation:** From the chessboard above, the square with coordinates "h3" is white, so return true.

**Example 3:**

**Input:** coordinates = "c7"

**Output:** false

**Constraints:**

*   `coordinates.length == 2`
*   `'a' <= coordinates[0] <= 'h'`
*   `'1' <= coordinates[1] <= '8'`

## Solution

```java
public class Solution {
    public boolean squareIsWhite(String coordinates) {
        char x = coordinates.charAt(0);
        int y = Integer.parseInt(coordinates.charAt(1) + "");
        switch (x) {
            case 'a':
            case 'c':
            case 'e':
            case 'g':
                return y % 2 == 0;
            default:
                return y % 2 != 0;
        }
    }
}
```