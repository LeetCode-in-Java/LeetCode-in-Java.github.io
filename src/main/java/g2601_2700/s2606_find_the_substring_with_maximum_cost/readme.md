[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2606\. Find the Substring With Maximum Cost

Medium

You are given a string `s`, a string `chars` of **distinct** characters and an integer array `vals` of the same length as `chars`.

The **cost of the substring** is the sum of the values of each character in the substring. The cost of an empty string is considered `0`.

The **value of the character** is defined in the following way:

*   If the character is not in the string `chars`, then its value is its corresponding position **(1-indexed)** in the alphabet.
    *   For example, the value of `'a'` is `1`, the value of `'b'` is `2`, and so on. The value of `'z'` is `26`.
*   Otherwise, assuming `i` is the index where the character occurs in the string `chars`, then its value is `vals[i]`.

Return _the maximum cost among all substrings of the string_ `s`.

**Example 1:**

**Input:** s = "adaa", chars = "d", vals = [-1000]

**Output:** 2

**Explanation:** The value of the characters "a" and "d" is 1 and -1000 respectively. The substring with the maximum cost is "aa" and its cost is 1 + 1 = 2. It can be proven that 2 is the maximum cost.

**Example 2:**

**Input:** s = "abc", chars = "abc", vals = [-1,-1,-1]

**Output:** 0

**Explanation:** The value of the characters "a", "b" and "c" is -1, -1, and -1 respectively. The substring with the maximum cost is the empty substring "" and its cost is 0. It can be proven that 0 is the maximum cost.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` consist of lowercase English letters.
*   `1 <= chars.length <= 26`
*   `chars` consist of **distinct** lowercase English letters.
*   `vals.length == chars.length`
*   `-1000 <= vals[i] <= 1000`

## Solution

```java
public class Solution {
    public int maximumCostSubstring(String s, String chars, int[] vals) {
        int[] alphabetCost = new int[26];
        for (char ch = 'a'; ch <= 'z'; ch++) {
            alphabetCost[ch - 'a'] = ((ch - 'a') + 1);
        }
        for (int i = 0; i < chars.length(); i++) {
            char chr = chars.charAt(i);
            int chrCost = vals[i];
            alphabetCost[chr - 'a'] = chrCost;
        }
        int currCost = 0;
        int maxCost = Integer.MIN_VALUE;
        for (char ch : s.toCharArray()) {
            int cost = alphabetCost[ch - 'a'];
            currCost = Math.max(currCost + cost, cost);
            maxCost = Math.max(maxCost, currCost);
        }
        return Math.max(maxCost, "".length());
    }
}
```