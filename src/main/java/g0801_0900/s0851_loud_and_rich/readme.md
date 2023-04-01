[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 851\. Loud and Rich

Medium

There is a group of `n` people labeled from `0` to `n - 1` where each person has a different amount of money and a different level of quietness.

You are given an array `richer` where <code>richer[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> indicates that <code>a<sub>i</sub></code> has more money than <code>b<sub>i</sub></code> and an integer array `quiet` where `quiet[i]` is the quietness of the <code>i<sup>th</sup></code> person. All the given data in richer are **logically correct** (i.e., the data will not lead you to a situation where `x` is richer than `y` and `y` is richer than `x` at the same time).

Return _an integer array_ `answer` _where_ `answer[x] = y` _if_ `y` _is the least quiet person (that is, the person_ `y` _with the smallest value of_ `quiet[y]`_) among all people who definitely have equal to or more money than the person_ `x`.

**Example 1:**

**Input:** richer = \[\[1,0],[2,1],[3,1],[3,7],[4,3],[5,3],[6,3]], quiet = [3,2,5,4,6,1,7,0]

**Output:** [5,5,2,5,4,5,6,7]

**Explanation:** 

answer[0] = 5. 

Person 5 has more money than 3, which has more money than 1, which has more money than 0. 

The only person who is quieter (has lower quiet[x]) is person 7, but it is not clear if they have more money than person 0. 

answer[7] = 7. 

Among all people that definitely have equal to or more money than person 7 (which could be persons 3, 4, 5, 6, or 7), the person who is the quietest (has lower quiet[x]) is person 7. 

The other answers can be filled out with similar reasoning.

**Example 2:**

**Input:** richer = [], quiet = [0]

**Output:** [0]

**Constraints:**

*   `n == quiet.length`
*   `1 <= n <= 500`
*   `0 <= quiet[i] < n`
*   All the values of `quiet` are **unique**.
*   `0 <= richer.length <= n * (n - 1) / 2`
*   <code>0 <= a<sub>i</sub>, b<sub>i</sub> < n</code>
*   <code>a<sub>i</sub> != b<sub>i</sub></code>
*   All the pairs of `richer` are **unique**.
*   The observations in `richer` are all logically consistent.

## Solution

```java
public class Solution {
    public int[] loudAndRich(int[][] richer, int[] quiet) {
        int[] result = new int[quiet.length];
        for (int i = 0; i < quiet.length; i++) {
            result[i] = i;
        }
        for (int k = 0; k < quiet.length; k++) {
            boolean changed = false;
            for (int[] r : richer) {
                if (quiet[result[r[0]]] < quiet[result[r[1]]]) {
                    result[r[1]] = result[r[0]];
                    changed = true;
                }
            }
            if (!changed) {
                break;
            }
        }
        return result;
    }
}
```