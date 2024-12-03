[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3361\. Shift Distance Between Two Strings

Medium

You are given two strings `s` and `t` of the same length, and two integer arrays `nextCost` and `previousCost`.

In one operation, you can pick any index `i` of `s`, and perform **either one** of the following actions:

*   Shift `s[i]` to the next letter in the alphabet. If `s[i] == 'z'`, you should replace it with `'a'`. This operation costs `nextCost[j]` where `j` is the index of `s[i]` in the alphabet.
*   Shift `s[i]` to the previous letter in the alphabet. If `s[i] == 'a'`, you should replace it with `'z'`. This operation costs `previousCost[j]` where `j` is the index of `s[i]` in the alphabet.

The **shift distance** is the **minimum** total cost of operations required to transform `s` into `t`.

Return the **shift distance** from `s` to `t`.

**Example 1:**

**Input:** s = "abab", t = "baba", nextCost = [100,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0], previousCost = [1,100,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0]

**Output:** 2

**Explanation:**

*   We choose index `i = 0` and shift `s[0]` 25 times to the previous character for a total cost of 1.
*   We choose index `i = 1` and shift `s[1]` 25 times to the next character for a total cost of 0.
*   We choose index `i = 2` and shift `s[2]` 25 times to the previous character for a total cost of 1.
*   We choose index `i = 3` and shift `s[3]` 25 times to the next character for a total cost of 0.

**Example 2:**

**Input:** s = "leet", t = "code", nextCost = [1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1], previousCost = [1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1]

**Output:** 31

**Explanation:**

*   We choose index `i = 0` and shift `s[0]` 9 times to the previous character for a total cost of 9.
*   We choose index `i = 1` and shift `s[1]` 10 times to the next character for a total cost of 10.
*   We choose index `i = 2` and shift `s[2]` 1 time to the previous character for a total cost of 1.
*   We choose index `i = 3` and shift `s[3]` 11 times to the next character for a total cost of 11.

**Constraints:**

*   <code>1 <= s.length == t.length <= 10<sup>5</sup></code>
*   `s` and `t` consist only of lowercase English letters.
*   `nextCost.length == previousCost.length == 26`
*   <code>0 <= nextCost[i], previousCost[i] <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    public long shiftDistance(String s, String t, int[] nextCost, int[] previousCost) {
        long[][] costs = new long[26][26];
        long cost;
        for (int i = 0; i < 26; i++) {
            cost = nextCost[i];
            int j = i == 25 ? 0 : i + 1;
            while (j != i) {
                costs[i][j] = cost;
                cost += nextCost[j];
                if (j == 25) {
                    j = -1;
                }
                j++;
            }
        }
        for (int i = 0; i < 26; i++) {
            cost = previousCost[i];
            int j = i == 0 ? 25 : i - 1;
            while (j != i) {
                costs[i][j] = Math.min(costs[i][j], cost);
                cost += previousCost[j];
                if (j == 0) {
                    j = 26;
                }
                j--;
            }
        }
        int n = s.length();
        long ans = 0;
        for (int i = 0; i < n; i++) {
            ans += costs[s.charAt(i) - 'a'][t.charAt(i) - 'a'];
        }
        return ans;
    }
}
```