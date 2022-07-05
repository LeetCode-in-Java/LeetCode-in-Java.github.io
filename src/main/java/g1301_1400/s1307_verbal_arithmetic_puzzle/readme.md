[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1307\. Verbal Arithmetic Puzzle

Hard

Given an equation, represented by `words` on the left side and the `result` on the right side.

You need to check if the equation is solvable under the following rules:

*   Each character is decoded as one digit (0 - 9).
*   Every pair of different characters must map to different digits.
*   Each `words[i]` and `result` are decoded as one number **without** leading zeros.
*   Sum of numbers on the left side (`words`) will equal to the number on the right side (`result`).

Return `true` _if the equation is solvable, otherwise return_ `false`.

**Example 1:**

**Input:** words = ["SEND","MORE"], result = "MONEY"

**Output:** true

**Explanation:** Map 'S'-> 9, 'E'->5, 'N'->6, 'D'->7, 'M'->1, 'O'->0, 'R'->8, 'Y'->'2' Such that: "SEND" + "MORE" = "MONEY" , 9567 + 1085 = 10652

**Example 2:**

**Input:** words = ["SIX","SEVEN","SEVEN"], result = "TWENTY"

**Output:** true

**Explanation:** Map 'S'-> 6, 'I'->5, 'X'->0, 'E'->8, 'V'->7, 'N'->2, 'T'->1, 'W'->'3', 'Y'->4 Such that: "SIX" + "SEVEN" + "SEVEN" = "TWENTY" , 650 + 68782 + 68782 = 138214

**Example 3:**

**Input:** words = ["LEET","CODE"], result = "POINT"

**Output:** false

**Constraints:**

*   `2 <= words.length <= 5`
*   `1 <= words[i].length, result.length <= 7`
*   `words[i], result` contain only uppercase English letters.
*   The number of different characters used in the expression is at most `10`.

## Solution

```java
import java.util.Arrays;

public class Solution {
    private int[] map;
    private char[][] grid;
    private boolean solved;
    private boolean[] usedDigit;
    private boolean[] mustNotBeZero;
    private int cols;
    private int resultRow;

    public boolean isSolvable(String[] words, String result) {
        this.solved = false;
        int rows = words.length + 1;
        this.cols = result.length();
        this.grid = new char[rows][cols];
        this.mustNotBeZero = new boolean[26];
        this.usedDigit = new boolean[10];
        this.resultRow = rows - 1;
        this.map = new int[26];
        Arrays.fill(map, -1);
        int maxLength = 0;
        for (int i = 0; i < words.length; i++) {
            int j = words[i].length();
            if (j > maxLength) {
                maxLength = j;
            }
            if (j > 1) {
                mustNotBeZero[words[i].charAt(0) - 'A'] = true;
            }
            if (j > cols) {
                return false;
            }
            for (char c : words[i].toCharArray()) {
                grid[i][--j] = c;
            }
        }
        if (maxLength + 1 < cols) {
            return false;
        }
        int j = cols;
        if (j > 1) {
            mustNotBeZero[result.charAt(0) - 'A'] = true;
        }
        for (char c : result.toCharArray()) {
            grid[resultRow][--j] = c;
        }
        backtrack(0, 0, 0);
        return solved;
    }

    private boolean canPlace(int ci, int d) {
        return (!usedDigit[d] && map[ci] == -1) || (map[ci] == d);
    }

    private void placeNum(int ci, int d) {
        usedDigit[d] = true;
        map[ci] = d;
    }

    private void removeNum(int ci, int d) {
        usedDigit[d] = false;
        map[ci] = -1;
    }

    private void placeNextNum(int r, int c, int sum) {
        if (r == resultRow && c == cols - 1) {
            solved = sum == 0;
        } else {
            if (r == resultRow) {
                backtrack(0, c + 1, sum);
            } else {
                backtrack(r + 1, c, sum);
            }
        }
    }

    private void backtrack(int r, int c, int sum) {
        char unused = '\u0000';
        if (grid[r][c] == unused) {
            placeNextNum(r, c, sum);
        } else {
            int ci = grid[r][c] - 'A';
            if (r == resultRow) {
                int d = sum % 10;
                if (map[ci] == -1) {
                    if (canPlace(ci, d)) {
                        placeNum(ci, d);
                        placeNextNum(r, c, sum / 10);
                        if (solved) {
                            return;
                        }
                        removeNum(ci, d);
                    }
                } else {
                    if (map[ci] == d) {
                        placeNextNum(r, c, sum / 10);
                    }
                }
            } else {
                if (map[ci] == -1) {
                    for (int d = mustNotBeZero[ci] ? 1 : 0; d < 10; d++) {
                        if (canPlace(ci, d)) {
                            placeNum(ci, d);
                            placeNextNum(r, c, sum + d);
                            if (solved) {
                                return;
                            }
                            removeNum(ci, d);
                        }
                    }
                } else {
                    placeNextNum(r, c, sum + map[ci]);
                }
            }
        }
    }
}
```