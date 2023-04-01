[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1871\. Jump Game VII

Medium

You are given a **0-indexed** binary string `s` and two integers `minJump` and `maxJump`. In the beginning, you are standing at index `0`, which is equal to `'0'`. You can move from index `i` to index `j` if the following conditions are fulfilled:

*   `i + minJump <= j <= min(i + maxJump, s.length - 1)`, and
*   `s[j] == '0'`.

Return `true` _if you can reach index_ `s.length - 1` _in_ `s`_, or_ `false` _otherwise._

**Example 1:**

**Input:** s = "011010", minJump = 2, maxJump = 3

**Output:** true

**Explanation:**

In the first step, move from index 0 to index 3.

In the second step, move from index 3 to index 5. 

**Example 2:**

**Input:** s = "01101110", minJump = 2, maxJump = 3

**Output:** false 

**Constraints:**

*   <code>2 <= s.length <= 10<sup>5</sup></code>
*   `s[i]` is either `'0'` or `'1'`.
*   `s[0] == '0'`
*   `1 <= minJump <= maxJump < s.length`

## Solution

```java
public class Solution {
    public boolean canReach(String s, int minJump, int maxJump) {
        int i;
        int j = 0;
        int n = s.length();
        char[] li = s.toCharArray();
        for (i = 0; i < n; i++) {
            // o == ok
            if (i == 0 || li[i] == 'o') {
                for (j = Math.max(j, i + minJump); j < Math.min(n, i + maxJump + 1); j++) {
                    if (li[j] == '0') {
                        li[j] = 'o';
                    }
                }
            }
            if (j > n) {
                break;
            }
        }
        return li[n - 1] == 'o';
    }
}
```