[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1626\. Best Team With No Conflicts

Medium

You are the manager of a basketball team. For the upcoming tournament, you want to choose the team with the highest overall score. The score of the team is the **sum** of scores of all the players in the team.

However, the basketball team is not allowed to have **conflicts**. A **conflict** exists if a younger player has a **strictly higher** score than an older player. A conflict does **not** occur between players of the same age.

Given two lists, `scores` and `ages`, where each `scores[i]` and `ages[i]` represents the score and age of the <code>i<sup>th</sup></code> player, respectively, return _the highest overall score of all possible basketball teams_.

**Example 1:**

**Input:** scores = [1,3,5,10,15], ages = [1,2,3,4,5]

**Output:** 34

**Explanation:** You can choose all the players.

**Example 2:**

**Input:** scores = [4,5,6,5], ages = [2,1,2,1]

**Output:** 16

**Explanation:** It is best to choose the last 3 players. Notice that you are allowed to choose multiple people of the same age.

**Example 3:**

**Input:** scores = [1,2,3,5], ages = [8,9,10,1]

**Output:** 6

**Explanation:** It is best to choose the first 3 players.

**Constraints:**

*   `1 <= scores.length, ages.length <= 1000`
*   `scores.length == ages.length`
*   <code>1 <= scores[i] <= 10<sup>6</sup></code>
*   `1 <= ages[i] <= 1000`

## Solution

```java
import java.util.Arrays;

public class Solution {
    private static class Pair {
        int score;
        int age;

        Pair(int score, int age) {
            this.score = score;
            this.age = age;
        }
    }

    private final int[][] memo = new int[1001][1001];

    public int bestTeamScore(int[] scores, int[] ages) {
        int n = ages.length;
        Pair[] p = new Pair[n];
        for (int[] x : memo) {
            Arrays.fill(x, -1);
        }
        for (int i = 0; i < n; i++) {
            p[i] = new Pair(scores[i], ages[i]);
        }
        Arrays.sort(p, (a, b) -> (a.score == b.score) ? (a.age - b.age) : a.score - b.score);
        return find(p, 0, 0, n);
    }

    private int find(Pair[] p, int i, int max, int n) {
        if (i >= n) {
            return 0;
        }
        if (memo[i][max] != -1) {
            return memo[i][max];
        }
        if (p[i].age >= max) {
            int x1 = p[i].score + find(p, i + 1, p[i].age, n);
            int x2 = find(p, i + 1, max, n);
            memo[i][max] = Math.max(x1, x2);
        } else {
            memo[i][max] = find(p, i + 1, max, n);
        }
        return memo[i][max];
    }
}
```