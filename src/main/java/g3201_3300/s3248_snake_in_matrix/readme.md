[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3248\. Snake in Matrix

Easy

There is a snake in an `n x n` matrix `grid` and can move in **four possible directions**. Each cell in the `grid` is identified by the position: `grid[i][j] = (i * n) + j`.

The snake starts at cell 0 and follows a sequence of commands.

You are given an integer `n` representing the size of the `grid` and an array of strings `commands` where each `command[i]` is either `"UP"`, `"RIGHT"`, `"DOWN"`, and `"LEFT"`. It's guaranteed that the snake will remain within the `grid` boundaries throughout its movement.

Return the position of the final cell where the snake ends up after executing `commands`.

**Example 1:**

**Input:** n = 2, commands = ["RIGHT","DOWN"]

**Output:** 3

**Explanation:**

![image](https://leetcode-images.github.io/g3201_3300/s3248_snake_in_matrix/image01.png)

**Example 2:**

**Input:** n = 3, commands = ["DOWN","RIGHT","UP"]

**Output:** 1

**Explanation:**

![image](https://leetcode-images.github.io/g3201_3300/s3248_snake_in_matrix/image02.png)

**Constraints:**

*   `2 <= n <= 10`
*   `1 <= commands.length <= 100`
*   `commands` consists only of `"UP"`, `"RIGHT"`, `"DOWN"`, and `"LEFT"`.
*   The input is generated such the snake will not move outside of the boundaries.

## Solution

```java
import java.util.List;

public class Solution {
    public int finalPositionOfSnake(int n, List<String> commands) {
        int x = 0;
        int y = 0;
        for (String command : commands) {
            switch (command) {
                case "UP":
                    if (x > 0) {
                        x--;
                    }
                    break;
                case "DOWN":
                    if (x < n - 1) {
                        x++;
                    }
                    break;
                case "LEFT":
                    if (y > 0) {
                        y--;
                    }
                    break;
                case "RIGHT":
                    if (y < n - 1) {
                        y++;
                    }
                    break;
                default:
                    break;
            }
        }
        return (x * n) + y;
    }
}
```