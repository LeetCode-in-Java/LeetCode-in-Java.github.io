[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1981\. Minimize the Difference Between Target and Chosen Elements

Medium

You are given an `m x n` integer matrix `mat` and an integer `target`.

Choose one integer from **each row** in the matrix such that the **absolute difference** between `target` and the **sum** of the chosen elements is **minimized**.

Return _the **minimum absolute difference**_.

The **absolute difference** between two numbers `a` and `b` is the absolute value of `a - b`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/08/03/matrix1.png)

**Input:** mat = \[\[1,2,3],[4,5,6],[7,8,9]], target = 13

**Output:** 0

**Explanation:** One possible choice is to: 

- Choose 1 from the first row. 

- Choose 5 from the second row. 

- Choose 7 from the third row. 
  
The sum of the chosen elements is 13, which equals the target, so the absolute difference is 0.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/08/03/matrix1-1.png)

**Input:** mat = \[\[1],[2],[3]], target = 100

**Output:** 94

**Explanation:** The best possible choice is to: 

- Choose 1 from the first row. 

- Choose 2 from the second row. 

- Choose 3 from the third row. 
  
The sum of the chosen elements is 6, and the absolute difference is 94.

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/08/03/matrix1-3.png)

**Input:** mat = \[\[1,2,9,8,7]], target = 6

**Output:** 1

**Explanation:** The best choice is to choose 7 from the first row. The absolute difference is 1.

**Constraints:**

*   `m == mat.length`
*   `n == mat[i].length`
*   `1 <= m, n <= 70`
*   `1 <= mat[i][j] <= 70`
*   `1 <= target <= 800`

## Solution

```java
public class Solution {
    public int minimizeTheDifference(int[][] mat, int target) {
        int m = mat.length;
        boolean[][] seen = new boolean[m][m * 70 + 1];
        dfs(0, mat, 0, seen);
        for (int i = 0; true; i++) {
            for (int j = 0, sign = 1; j < 2; j++, sign *= -1) {
                int k = target - i * sign;
                if (k >= 0 && k <= m * 70 && seen[m - 1][k]) {
                    return i;
                }
            }
        }
    }

    private void dfs(int i, int[][] mat, int sum, boolean[][] seen) {
        if (i == mat.length) {
            return;
        }

        for (int j = 0; j < mat[i].length; j++) {
            if (!seen[i][sum + mat[i][j]]) {
                seen[i][sum + mat[i][j]] = true;
                dfs(i + 1, mat, sum + mat[i][j], seen);
            }
        }
    }
}
```