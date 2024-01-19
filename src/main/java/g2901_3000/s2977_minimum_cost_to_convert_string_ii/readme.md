[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2977\. Minimum Cost to Convert String II

Hard

You are given two **0-indexed** strings `source` and `target`, both of length `n` and consisting of **lowercase** English characters. You are also given two **0-indexed** string arrays `original` and `changed`, and an integer array `cost`, where `cost[i]` represents the cost of converting the string `original[i]` to the string `changed[i]`.

You start with the string `source`. In one operation, you can pick a **substring** `x` from the string, and change it to `y` at a cost of `z` **if** there exists **any** index `j` such that `cost[j] == z`, `original[j] == x`, and `changed[j] == y`. You are allowed to do **any** number of operations, but any pair of operations must satisfy **either** of these two conditions:

*   The substrings picked in the operations are `source[a..b]` and `source[c..d]` with either `b < c` **or** `d < a`. In other words, the indices picked in both operations are **disjoint**.
*   The substrings picked in the operations are `source[a..b]` and `source[c..d]` with `a == c` **and** `b == d`. In other words, the indices picked in both operations are **identical**.

Return _the **minimum** cost to convert the string_ `source` _to the string_ `target` _using **any** number of operations_. _If it is impossible to convert_ `source` _to_ `target`, _return_ `-1`.

**Note** that there may exist indices `i`, `j` such that `original[j] == original[i]` and `changed[j] == changed[i]`.

**Example 1:**

**Input:** source = "abcd", target = "acbe", original = ["a","b","c","c","e","d"], changed = ["b","c","b","e","b","e"], cost = [2,5,5,1,2,20]

**Output:** 28

**Explanation:** To convert "abcd" to "acbe", do the following operations: 
- Change substring source[1..1] from "b" to "c" at a cost of 5. 
- Change substring source[2..2] from "c" to "e" at a cost of 1. 
- Change substring source[2..2] from "e" to "b" at a cost of 2. 
- Change substring source[3..3] from "d" to "e" at a cost of 20. 

The total cost incurred is 5 + 1 + 2 + 20 = 28. 

It can be shown that this is the minimum possible cost.

**Example 2:**

**Input:** source = "abcdefgh", target = "acdeeghh", original = ["bcd","fgh","thh"], changed = ["cde","thh","ghh"], cost = [1,3,5]

**Output:** 9

**Explanation:** To convert "abcdefgh" to "acdeeghh", do the following operations: 
- Change substring source[1..3] from "bcd" to "cde" at a cost of 1. 
- Change substring source[5..7] from "fgh" to "thh" at a cost of 3. We can do this operation because indices [5,7] are disjoint with indices picked in the first operation. 
- Change substring source[5..7] from "thh" to "ghh" at a cost of 5. We can do this operation because indices [5,7] are disjoint with indices picked in the first operation, and identical with indices picked in the second operation. 

The total cost incurred is 1 + 3 + 5 = 9. 

It can be shown that this is the minimum possible cost.

**Example 3:**

**Input:** source = "abcdefgh", target = "addddddd", original = ["bcd","defgh"], changed = ["ddd","ddddd"], cost = [100,1578]

**Output:** -1

**Explanation:** It is impossible to convert "abcdefgh" to "addddddd".

If you select substring source[1..3] as the first operation to change "abcdefgh" to "adddefgh", you cannot select substring source[3..7] as the second operation because it has a common index, 3, with the first operation. 

If you select substring source[3..7] as the first operation to change "abcdefgh" to "abcddddd", you cannot select substring source[1..3] as the second operation because it has a common index, 3, with the first operation.

**Constraints:**

*   `1 <= source.length == target.length <= 1000`
*   `source`, `target` consist only of lowercase English characters.
*   `1 <= cost.length == original.length == changed.length <= 100`
*   `1 <= original[i].length == changed[i].length <= source.length`
*   `original[i]`, `changed[i]` consist only of lowercase English characters.
*   `original[i] != changed[i]`
*   <code>1 <= cost[i] <= 10<sup>6</sup></code>

## Solution

```java
import java.util.Arrays;
import java.util.HashMap;
import java.util.HashSet;

public class Solution {
    public long minimumCost(
            String source, String target, String[] original, String[] changed, int[] cost) {
        HashMap<String, Integer> index = new HashMap<>();
        for (String o : original) {
            if (!index.containsKey(o)) {
                index.put(o, index.size());
            }
        }
        for (String c : changed) {
            if (!index.containsKey(c)) {
                index.put(c, index.size());
            }
        }
        long[][] dis = new long[index.size()][index.size()];
        for (int i = 0; i < dis.length; i++) {
            Arrays.fill(dis[i], Long.MAX_VALUE);
            dis[i][i] = 0;
        }
        for (int i = 0; i < cost.length; i++) {
            dis[index.get(original[i])][index.get(changed[i])] =
                    Math.min(dis[index.get(original[i])][index.get(changed[i])], cost[i]);
        }
        for (int k = 0; k < dis.length; k++) {
            for (int i = 0; i < dis.length; i++) {
                if (dis[i][k] < Long.MAX_VALUE) {
                    for (int j = 0; j < dis.length; j++) {
                        if (dis[k][j] < Long.MAX_VALUE) {
                            dis[i][j] = Math.min(dis[i][j], dis[i][k] + dis[k][j]);
                        }
                    }
                }
            }
        }
        HashSet<Integer> set = new HashSet<>();
        for (String o : original) {
            set.add(o.length());
        }
        long[] dp = new long[target.length() + 1];
        Arrays.fill(dp, Long.MAX_VALUE);
        dp[0] = 0L;
        for (int i = 0; i < target.length(); i++) {
            if (dp[i] == Long.MAX_VALUE) {
                continue;
            }
            if (target.charAt(i) == source.charAt(i)) {
                dp[i + 1] = Math.min(dp[i + 1], dp[i]);
            }
            for (int t : set) {
                if (i + t >= dp.length) {
                    continue;
                }
                int c1 = index.getOrDefault(source.substring(i, i + t), -1);
                int c2 = index.getOrDefault(target.substring(i, i + t), -1);
                if (c1 >= 0 && c2 >= 0 && dis[c1][c2] < Long.MAX_VALUE) {
                    dp[i + t] = Math.min(dp[i + t], dp[i] + dis[c1][c2]);
                }
            }
        }
        return dp[dp.length - 1] == Long.MAX_VALUE ? -1L : dp[dp.length - 1];
    }
}
```