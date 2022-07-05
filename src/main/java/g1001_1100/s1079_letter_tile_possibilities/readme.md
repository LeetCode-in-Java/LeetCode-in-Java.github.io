[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1079\. Letter Tile Possibilities

Medium

You have `n` `tiles`, where each tile has one letter `tiles[i]` printed on it.

Return _the number of possible non-empty sequences of letters_ you can make using the letters printed on those `tiles`.

**Example 1:**

**Input:** tiles = "AAB"

**Output:** 8

**Explanation:** The possible sequences are "A", "B", "AA", "AB", "BA", "AAB", "ABA", "BAA".

**Example 2:**

**Input:** tiles = "AAABBC"

**Output:** 188

**Example 3:**

**Input:** tiles = "V"

**Output:** 1

**Constraints:**

*   `1 <= tiles.length <= 7`
*   `tiles` consists of uppercase English letters.

## Solution

```java
import java.util.Arrays;

public class Solution {
    private int count;

    public int numTilePossibilities(String tiles) {
        count = 0;
        char[] chars = tiles.toCharArray();
        Arrays.sort(chars);
        boolean[] visited = new boolean[chars.length];
        dfs(chars, 0, visited);
        return count;
    }

    private void dfs(char[] chars, int length, boolean[] visited) {
        if (length == chars.length) {
            return;
        }
        for (int i = 0; i < chars.length; i++) {
            if (visited[i] || i - 1 >= 0 && chars[i] == chars[i - 1] && !visited[i - 1]) {
                continue;
            }
            count++;
            visited[i] = true;
            dfs(chars, length + 1, visited);
            visited[i] = false;
        }
    }
}
```