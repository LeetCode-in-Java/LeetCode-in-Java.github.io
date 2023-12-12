[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2833\. Furthest Point From Origin

Easy

You are given a string `moves` of length `n` consisting only of characters `'L'`, `'R'`, and `'_'`. The string represents your movement on a number line starting from the origin `0`.

In the <code>i<sup>th</sup></code> move, you can choose one of the following directions:

*   move to the left if `moves[i] = 'L'` or `moves[i] = '_'`
*   move to the right if `moves[i] = 'R'` or `moves[i] = '_'`

Return _the **distance from the origin** of the **furthest** point you can get to after_ `n` _moves_.

**Example 1:**

**Input:** moves = "L\_RL\_\_R"

**Output:** 3

**Explanation:** The furthest point we can reach from the origin 0 is point -3 through the following sequence of moves "LLRLLLR".

**Example 2:**

**Input:** moves = "\_R\_\_LL\_"

**Output:** 5

**Explanation:** The furthest point we can reach from the origin 0 is point -5 through the following sequence of moves "LRLLLLL".

**Example 3:**

**Input:** moves = "\_\_\_\_\_\_\_"

**Output:** 7

**Explanation:** The furthest point we can reach from the origin 0 is point 7 through the following sequence of moves "RRRRRRR".

**Constraints:**

*   `1 <= moves.length == n <= 50`
*   `moves` consists only of characters `'L'`, `'R'` and `'_'`.

## Solution

```java
public class Solution {
    public int furthestDistanceFromOrigin(String m) {
        int count = 0;
        int res = 0;
        for (int i = 0; i < m.length(); i++) {
            if (m.charAt(i) == 'L') {
                res -= 1;
            } else if (m.charAt(i) == 'R') {
                res += 1;
            } else {
                count++;
            }
        }
        return Math.abs(res) + count;
    }
}
```