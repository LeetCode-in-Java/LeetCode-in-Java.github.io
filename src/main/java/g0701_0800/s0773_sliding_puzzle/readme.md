## 773\. Sliding Puzzle

Hard

On an `2 x 3` board, there are five tiles labeled from `1` to `5`, and an empty square represented by `0`. A **move** consists of choosing `0` and a 4-directionally adjacent number and swapping it.

The state of the board is solved if and only if the board is `[[1,2,3],[4,5,0]]`.

Given the puzzle board `board`, return _the least number of moves required so that the state of the board is solved_. If it is impossible for the state of the board to be solved, return `-1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/06/29/slide1-grid.jpg)

**Input:** board = [[1,2,3],[4,0,5]]

**Output:** 1

**Explanation:** Swap the 0 and the 5 in one move. 

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/06/29/slide2-grid.jpg)

**Input:** board = [[1,2,3],[5,4,0]]

**Output:** -1

**Explanation:** No number of moves will make the board solved. 

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/06/29/slide3-grid.jpg)

**Input:** board = [[4,1,2],[5,0,3]]

**Output:** 5

**Explanation:**

    5 is the smallest number of moves that solves the board.
    An example path:
    After move 0: [[4,1,2],[5,0,3]]
    After move 1: [[4,1,2],[0,5,3]]
    After move 2: [[0,1,2],[4,5,3]]
    After move 3: [[1,0,2],[4,5,3]]
    After move 4: [[1,2,0],[4,5,3]]
    After move 5: [[1,2,3],[4,5,0]] 

**Constraints:**

*   `board.length == 2`
*   `board[i].length == 3`
*   `0 <= board[i][j] <= 5`
*   Each value `board[i][j]` is **unique**.

## Solution

```java
import java.util.HashSet;
import java.util.LinkedList;
import java.util.Queue;
import java.util.Set;

@SuppressWarnings("java:S1104")
public class Solution {
    private static class Node {
        public String board;
        public int depth;
        public int y;
        public int x;

        public Node(String board, int depth, int y, int x) {
            this.board = board;
            this.depth = depth;
            this.y = y;
            this.x = x;
        }
    }

    public int slidingPuzzle(int[][] board) {
        String targetStr = "123450";
        StringBuilder sb = new StringBuilder();
        int y = 0;
        int x = 0;
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[0].length; j++) {
                if (board[i][j] == 0) {
                    y = i;
                    x = j;
                }
                sb.append(board[i][j]);
            }
        }
        Set<String> seen = new HashSet<>();
        Queue<Node> q = new LinkedList<>();
        q.add(new Node(sb.toString(), 0, y, x));
        int[][] dir = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
        while (!q.isEmpty()) {
            Node next = q.poll();
            String s = next.board;
            if (!seen.contains(s)) {
                if (s.equals(targetStr)) {
                    return next.depth;
                }
                int nextDepth = next.depth + 1;
                y = next.y;
                x = next.x;
                for (int[] vector : dir) {
                    int nextY = y + vector[0];
                    int nextX = x + vector[1];
                    if (0 <= nextY
                            && nextY < board.length
                            && 0 <= nextX
                            && nextX < board[0].length) {
                        String newBoard = swap(s, y, x, nextY, nextX);
                        q.add(new Node(newBoard, nextDepth, nextY, nextX));
                    }
                }
                seen.add(s);
            }
        }
        return -1;
    }

    public String swap(String board, int y1, int x1, int y2, int x2) {
        char[] arr = board.toCharArray();
        char t = board.charAt(y1 * 3 + x1);
        arr[y1 * 3 + x1] = board.charAt(y2 * 3 + x2);
        arr[y2 * 3 + x2] = t;
        return new String(arr);
    }
}
```