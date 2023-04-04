[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2225\. Find Players With Zero or One Losses

Medium

You are given an integer array `matches` where <code>matches[i] = [winner<sub>i</sub>, loser<sub>i</sub>]</code> indicates that the player <code>winner<sub>i</sub></code> defeated player <code>loser<sub>i</sub></code> in a match.

Return _a list_ `answer` _of size_ `2` _where:_

*   `answer[0]` is a list of all players that have **not** lost any matches.
*   `answer[1]` is a list of all players that have lost exactly **one** match.

The values in the two lists should be returned in **increasing** order.

**Note:**

*   You should only consider the players that have played **at least one** match.
*   The testcases will be generated such that **no** two matches will have the **same** outcome.

**Example 1:**

**Input:** matches = \[\[1,3],[2,3],[3,6],[5,6],[5,7],[4,5],[4,8],[4,9],[10,4],[10,9]]

**Output:** [[1,2,10],[4,5,7,8]]

**Explanation:** Players 1, 2, and 10 have not lost any matches. Players 4, 5, 7, and 8 each have lost one match. Players 3, 6, and 9 each have lost two matches. Thus, answer[0] = [1,2,10] and answer[1] = [4,5,7,8].

**Example 2:**

**Input:** matches = \[\[2,3],[1,3],[5,4],[6,4]]

**Output:** [[1,2,5,6],[]]

**Explanation:** Players 1, 2, 5, and 6 have not lost any matches. Players 3 and 4 each have lost two matches. Thus, answer[0] = [1,2,5,6] and answer[1] = [].

**Constraints:**

*   <code>1 <= matches.length <= 10<sup>5</sup></code>
*   `matches[i].length == 2`
*   <code>1 <= winner<sub>i</sub>, loser<sub>i</sub> <= 10<sup>5</sup></code>
*   <code>winner<sub>i</sub> != loser<sub>i</sub></code>
*   All `matches[i]` are **unique**.

## Solution

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class Solution {
    public List<List<Integer>> findWinners(int[][] matches) {
        Map<Integer, Integer> map = new HashMap<>();
        List<Integer> list1 = new ArrayList<>();
        List<Integer> list2 = new ArrayList<>();
        List<List<Integer>> res = new ArrayList<>();
        for (int[] match : matches) {
            int winner = match[0];
            int loser = match[1];
            map.putIfAbsent(winner, 0);
            map.put(loser, map.getOrDefault(loser, 0) + 1);
        }
        for (Map.Entry<Integer, Integer> entry : map.entrySet()) {
            if (entry.getValue() == 0) {
                list1.add(entry.getKey());
            }
            if (entry.getValue() == 1) {
                list2.add(entry.getKey());
            }
        }
        Collections.sort(list1);
        Collections.sort(list2);
        res.add(list1);
        res.add(list2);
        return res;
    }
}
```