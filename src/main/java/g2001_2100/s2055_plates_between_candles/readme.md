[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2055\. Plates Between Candles

Medium

There is a long table with a line of plates and candles arranged on top of it. You are given a **0-indexed** string `s` consisting of characters `'*'` and `'|'` only, where a `'*'` represents a **plate** and a `'|'` represents a **candle**.

You are also given a **0-indexed** 2D integer array `queries` where <code>queries[i] = [left<sub>i</sub>, right<sub>i</sub>]</code> denotes the **substring** <code>s[left<sub>i</sub>...right<sub>i</sub>]</code> (**inclusive**). For each query, you need to find the **number** of plates **between candles** that are **in the substring**. A plate is considered **between candles** if there is at least one candle to its left **and** at least one candle to its right **in the substring**.

*   For example, `s = "||**||**|*"`, and a query `[3, 8]` denotes the substring `"*||******|"`. The number of plates between candles in this substring is `2`, as each of the two plates has at least one candle **in the substring** to its left **and** right.

Return _an integer array_ `answer` _where_ `answer[i]` _is the answer to the_ <code>i<sup>th</sup></code> _query_.

**Example 1:**

![ex-1](https://assets.leetcode.com/uploads/2021/10/04/ex-1.png)

**Input:** s = "\*\*\|\*\*\|\*\*\*\|", queries = \[\[2,5],[5,9]]

**Output:** [2,3]

**Explanation:** - queries[0] has two plates between candles. - queries[1] has three plates between candles. 

**Example 2:**

![ex-2](https://assets.leetcode.com/uploads/2021/10/04/ex-2.png)

**Input:** s = "\*\*\*\|\*\*\|\*\*\*\*\*\|\*\*\|\|\*\*\|\*", queries = \[\[1,17],[4,5],[14,17],[5,11],[15,16]]

**Output:** [9,0,0,0,0]

**Explanation:** - queries[0] has nine plates between candles. - The other queries have zero plates between candles. 

**Constraints:**

*   <code>3 <= s.length <= 10<sup>5</sup></code>
*   `s` consists of `'*'` and `'|'` characters.
*   <code>1 <= queries.length <= 10<sup>5</sup></code>
*   `queries[i].length == 2`
*   <code>0 <= left<sub>i</sub> <= right<sub>i</sub> < s.length</code>

## Solution

```java
public class Solution {
    public int[] platesBetweenCandles(String s, int[][] queries) {
        char[] sa = s.toCharArray();
        int n = sa.length;
        int[] nextCandle = new int[n + 1];
        nextCandle[n] = -1;
        for (int i = n - 1; i >= 0; i--) {
            nextCandle[i] = sa[i] == '|' ? i : nextCandle[i + 1];
        }
        int[] prevCandle = new int[n];
        int[] prevPlates = new int[n];
        int countPlate = 0;
        if (sa[0] == '*') {
            countPlate = 1;
            prevCandle[0] = -1;
        }
        for (int i = 1; i < n; i++) {
            if (sa[i] == '|') {
                prevCandle[i] = i;
                prevPlates[i] = countPlate;
            } else {
                prevCandle[i] = prevCandle[i - 1];
                countPlate++;
            }
        }
        int len = queries.length;
        int[] res = new int[len];
        for (int i = 0; i < len; i++) {
            int[] query = queries[i];
            int start = query[0];
            int end = query[1];
            int curPlates = 0;
            int endCandle = prevCandle[end];
            if (endCandle >= start) {
                int startCandle = nextCandle[start];

                curPlates = prevPlates[endCandle] - prevPlates[startCandle];
            }
            res[i] = curPlates;
        }
        return res;
    }
}
```