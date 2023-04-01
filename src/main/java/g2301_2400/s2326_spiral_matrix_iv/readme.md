[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2326\. Spiral Matrix IV

Medium

You are given two integers `m` and `n`, which represent the dimensions of a matrix.

You are also given the `head` of a linked list of integers.

Generate an `m x n` matrix that contains the integers in the linked list presented in **spiral** order **(clockwise)**, starting from the **top-left** of the matrix. If there are remaining empty spaces, fill them with `-1`.

Return _the generated matrix_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/05/09/ex1new.jpg)

**Input:** m = 3, n = 5, head = [3,0,2,6,8,1,7,9,4,2,5,5,0]

**Output:** [[3,0,2,6,8],[5,0,-1,-1,1],[5,2,4,9,7]]

**Explanation:** The diagram above shows how the values are printed in the matrix.

Note that the remaining spaces in the matrix are filled with -1.

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/05/11/ex2.jpg)

**Input:** m = 1, n = 4, head = [0,1,2]

**Output:** [[0,1,2,-1]]

**Explanation:** The diagram above shows how the values are printed from left to right in the matrix.

The last space in the matrix is set to -1.

**Constraints:**

*   <code>1 <= m, n <= 10<sup>5</sup></code>
*   <code>1 <= m * n <= 10<sup>5</sup></code>
*   The number of nodes in the list is in the range `[1, m * n]`.
*   `0 <= Node.val <= 1000`

## Solution

```java
import com_github_leetcode.ListNode;

/*
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
public class Solution {
    private enum Direction {
        RIGHT,
        DOWN,
        LEFT,
        UP
    }

    public int[][] spiralMatrix(int m, int n, ListNode head) {
        int[][] arr = new int[m][n];
        int i = 0;
        int j = -1;
        Direction direction = Direction.RIGHT;
        // Boundaries
        // ++ after Left to right Horizontal traversed
        int a = 0;
        // -- after Down to Up vertical traversed
        int b = n - 1;
        // -- after Right to Left horizontal teversed
        int c = m - 1;
        // ++ after Down to Up vertical traversed
        int d = 0;
        for (int k = 0; k < m * n; ++k) {
            int val = -1;
            if (head != null) {
                val = head.val;
                head = head.next;
            }
            switch (direction) {
                case RIGHT:
                    ++j;
                    if (j == b) {
                        direction = Direction.DOWN;
                        ++a;
                    }
                    break;
                case DOWN:
                    ++i;
                    if (i == c) {
                        direction = Direction.LEFT;
                    }
                    break;
                case LEFT:
                    --j;
                    if (j == d) {
                        --c;
                        direction = Direction.UP;
                    }
                    break;
                case UP:
                default:
                    --i;
                    if (i == a) {
                        --b;
                        ++d;
                        direction = Direction.RIGHT;
                    }
                    break;
            }
            arr[i][j] = val;
        }
        return arr;
    }
}
```