## 1036\. Escape a Large Maze

Hard

There is a 1 million by 1 million grid on an XY-plane, and the coordinates of each grid square are `(x, y)`.

We start at the <code>source = [s<sub>x</sub>, s<sub>y</sub>]</code> square and want to reach the <code>target = [t<sub>x</sub>, t<sub>y</sub>]</code> square. There is also an array of `blocked` squares, where each <code>blocked[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> represents a blocked square with coordinates <code>(x<sub>i</sub>, y<sub>i</sub>)</code>.

Each move, we can walk one square north, east, south, or west if the square is **not** in the array of `blocked` squares. We are also not allowed to walk outside of the grid.

Return `true` _if and only if it is possible to reach the_ `target` _square from the_ `source` _square through a sequence of valid moves_.

**Example 1:**

**Input:** blocked = \[\[0,1],[1,0]], source = [0,0], target = [0,2]

**Output:** false

**Explanation:** The target square is inaccessible starting from the source square because we cannot move. 

We cannot move north or east because those squares are blocked. 

We cannot move south or west because we cannot go outside of the grid.

**Example 2:**

**Input:** blocked = [], source = [0,0], target = [999999,999999]

**Output:** true

**Explanation:** Because there are no blocked cells, it is possible to reach the target square.

**Constraints:**

*   `0 <= blocked.length <= 200`
*   `blocked[i].length == 2`
*   <code>0 <= x<sub>i</sub>, y<sub>i</sub> < 10<sup>6</sup></code>
*   `source.length == target.length == 2`
*   <code>0 <= s<sub>x</sub>, s<sub>y</sub>, t<sub>x</sub>, t<sub>y</sub> < 10<sup>6</sup></code>
*   `source != target`
*   It is guaranteed that `source` and `target` are not blocked.

## Solution

```java
import java.util.HashSet;
import java.util.Set;

public class Solution {
    public boolean isEscapePossible(int[][] blocked, int[] source, int[] target) {
        if (blocked.length == 0) {
            return true;
        }
        Set<Integer> blocks = new HashSet<>();
        for (int[] b : blocked) {
            if (target[0] * 1000000 + target[1] != b[0] * 1000000 + b[1]) {
                blocks.add(b[0] * 1000000 + b[1]);
            }
        }
        return dfs(blocks, source, source[0], source[1], new HashSet<>(), target)
                && dfs(blocks, target, target[0], target[1], new HashSet<>(), source);
    }

    private boolean dfs(
            Set<Integer> blocks, int[] start, int i, int j, Set<Integer> visited, int[] target) {
        if (i < 0
                || j < 0
                || i > 999999
                || j > 999999
                || blocks.contains(i * 1000000 + j)
                || visited.contains(i * 1000000 + j)) {
            return false;
        }
        if (i == target[0] && j == target[1]) {
            return true;
        }
        visited.add(i * 1000000 + j);
        if (visited.size() > blocks.size() * (blocks.size() + 1)) {
            return true;
        }
        return dfs(blocks, start, i + 1, j, visited, target)
                || dfs(blocks, start, i - 1, j, visited, target)
                || dfs(blocks, start, i, j + 1, visited, target)
                || dfs(blocks, start, i, j - 1, visited, target);
    }
}
```