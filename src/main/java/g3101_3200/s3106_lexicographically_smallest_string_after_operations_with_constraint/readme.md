[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3106\. Lexicographically Smallest String After Operations With Constraint

Medium

You are given a string `s` and an integer `k`.

Define a function <code>distance(s<sub>1</sub>, s<sub>2</sub>)</code> between two strings <code>s<sub>1</sub></code> and <code>s<sub>2</sub></code> of the same length `n` as:

*   The **sum** of the **minimum distance** between <code>s<sub>1</sub>[i]</code> and <code>s<sub>2</sub>[i]</code> when the characters from `'a'` to `'z'` are placed in a **cyclic** order, for all `i` in the range `[0, n - 1]`.

For example, `distance("ab", "cd") == 4`, and `distance("a", "z") == 1`.

You can **change** any letter of `s` to **any** other lowercase English letter, **any** number of times.

Return a string denoting the **lexicographically smallest** string `t` you can get after some changes, such that `distance(s, t) <= k`.

**Example 1:**

**Input:** s = "zbbz", k = 3

**Output:** "aaaz"

**Explanation:**

Change `s` to `"aaaz"`. The distance between `"zbbz"` and `"aaaz"` is equal to `k = 3`.

**Example 2:**

**Input:** s = "xaxcd", k = 4

**Output:** "aawcd"

**Explanation:**

The distance between "xaxcd" and "aawcd" is equal to k = 4.

**Example 3:**

**Input:** s = "lol", k = 0

**Output:** "lol"

**Explanation:**

It's impossible to change any character as `k = 0`.

**Constraints:**

*   `1 <= s.length <= 100`
*   `0 <= k <= 2000`
*   `s` consists only of lowercase English letters.

## Solution

```java
public class Solution {
    public String getSmallestString(String s, int k) {
        char[] sArray = s.toCharArray();
        for (int i = 0; i < sArray.length; i++) {
            int distToA = cyclicDistance(sArray[i], 'a');
            if (distToA <= k) {
                sArray[i] = 'a';
                k -= distToA;
            } else if (k > 0) {
                sArray[i] = (char) (sArray[i] - k);
                k = 0;
            }
        }
        return new String(sArray);
    }

    private int cyclicDistance(char ch1, char ch2) {
        int dist = Math.abs(ch1 - ch2);
        return Math.min(dist, 26 - dist);
    }
}
```