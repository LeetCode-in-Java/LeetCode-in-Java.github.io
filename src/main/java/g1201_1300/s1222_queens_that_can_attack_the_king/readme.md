## 1222\. Queens That Can Attack the King

Medium

On an **8x8** chessboard, there can be multiple Black Queens and one White King.

Given an array of integer coordinates `queens` that represents the positions of the Black Queens, and a pair of coordinates `king` that represent the position of the White King, return the coordinates of all the queens (in any order) that can attack the King.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/10/01/untitled-diagram.jpg)

**Input:** queens = [[0,1],[1,0],[4,0],[0,4],[3,3],[2,4]], king = [0,0]

**Output:** [[0,1],[1,0],[3,3]]

**Explanation:** 

The queen at [0,1] can attack the king cause they're in the same row. 

The queen at [1,0] can attack the king cause they're in the same column. 

The queen at [3,3] can attack the king cause they're in the same diagnal. 

The queen at [0,4] can't attack the king cause it's blocked by the queen at [0,1].

The queen at [4,0] can't attack the king cause it's blocked by the queen at [1,0]. 

The queen at [2,4] can't attack the king cause it's not in the same row/column/diagnal as the king.

**Example 2:**

**![](https://assets.leetcode.com/uploads/2019/10/01/untitled-diagram-1.jpg)**

**Input:** queens = [[0,0],[1,1],[2,2],[3,4],[3,5],[4,4],[4,5]], king = [3,3]

**Output:** [[2,2],[3,4],[4,4]]

**Example 3:**

**![](https://assets.leetcode.com/uploads/2019/10/01/untitled-diagram-2.jpg)**

**Input:** queens = [[5,6],[7,7],[2,1],[0,7],[1,6],[5,1],[3,7],[0,3],[4,0],[1,2],[6,3],[5,0],[0,4],[2,2],[1,1],[6,4],[5,4],[0,0],[2,6],[4,5],[5,2],[1,4],[7,5],[2,3],[0,5],[4,2],[1,0],[2,7],[0,1],[4,6],[6,1],[0,6],[4,3],[1,7]], king = [3,4]

**Output:** [[2,3],[1,4],[1,6],[3,7],[4,3],[5,4],[4,5]]

**Constraints:**

*   `1 <= queens.length <= 63`
*   `queens[i].length == 2`
*   `0 <= queens[i][j] < 8`
*   `king.length == 2`
*   `0 <= king[0], king[1] < 8`
*   At most one piece is allowed in a cell.

## Solution

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashMap;
import java.util.HashSet;
import java.util.List;
import java.util.Map;
import java.util.Set;

public class Solution {
    public List<List<Integer>> queensAttacktheKing(int[][] queens, int[] king) {
        List<List<Integer>> result = new ArrayList<>();
        Map<Integer, Set<Integer>> queensLoc = new HashMap<>();
        for (int[] queen : queens) {
            Set<Integer> queensY = queensLoc.getOrDefault(queen[0], new HashSet<>());
            queensY.add(queen[1]);
            queensLoc.put(queen[0], queensY);
        }
        dfs(queensLoc, king[0] - 1, king[1], result, "n");
        dfs(queensLoc, king[0] + 1, king[1], result, "s");
        dfs(queensLoc, king[0], king[1] + 1, result, "e");
        dfs(queensLoc, king[0], king[1] - 1, result, "w");
        dfs(queensLoc, king[0] - 1, king[1] - 1, result, "nw");
        dfs(queensLoc, king[0] - 1, king[1] + 1, result, "ne");
        dfs(queensLoc, king[0] + 1, king[1] - 1, result, "sw");
        dfs(queensLoc, king[0] + 1, king[1] + 1, result, "se");
        return result;
    }

    public void dfs(
            Map<Integer, Set<Integer>> queens,
            int x,
            int y,
            List<List<Integer>> result,
            String direction) {
        if (x < 0 || x >= 8 || y < 0 || y >= 8) {
            return;
        }

        if (queens.containsKey(x) && queens.get(x).contains(y)) {
            result.add(new ArrayList<>(Arrays.asList(x, y)));
            return;
        }

        switch (direction) {
            case "n":
                dfs(queens, x - 1, y, result, direction);
                break;
            case "s":
                dfs(queens, x + 1, y, result, direction);
                break;
            case "e":
                dfs(queens, x, y + 1, result, direction);
                break;
            case "w":
                dfs(queens, x, y - 1, result, direction);
                break;
            case "ne":
                dfs(queens, x - 1, y + 1, result, direction);
                break;
            case "nw":
                dfs(queens, x - 1, y - 1, result, direction);
                break;
            case "se":
                dfs(queens, x + 1, y + 1, result, direction);
                break;
            case "sw":
                dfs(queens, x + 1, y - 1, result, direction);
                break;
            default:
        }
    }
}
```