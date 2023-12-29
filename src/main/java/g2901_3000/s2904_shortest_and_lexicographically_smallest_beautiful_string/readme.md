[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2904\. Shortest and Lexicographically Smallest Beautiful String

Medium

You are given a binary string `s` and a positive integer `k`.

A substring of `s` is **beautiful** if the number of `1`'s in it is exactly `k`.

Let `len` be the length of the **shortest** beautiful substring.

Return _the lexicographically **smallest** beautiful substring of string_ `s` _with length equal to_ `len`. If `s` doesn't contain a beautiful substring, return _an **empty** string_.

A string `a` is lexicographically **larger** than a string `b` (of the same length) if in the first position where `a` and `b` differ, `a` has a character strictly larger than the corresponding character in `b`.

*   For example, `"abcd"` is lexicographically larger than `"abcc"` because the first position they differ is at the fourth character, and `d` is greater than `c`.

**Example 1:**

**Input:** s = "100011001", k = 3

**Output:** "11001"

**Explanation:** There are 7 beautiful substrings in this example: 
1. The substring "<ins>100011</ins>001". 
2. The substring "<ins>1000110</ins>01".
3. The substring "<ins>10001100</ins>1". 
4. The substring "1<ins>00011001</ins>". 
5. The substring "10<ins>0011001</ins>". 
6. The substring "100<ins>011001</ins>". 
7. The substring "1000<ins>11001</ins>". 

The length of the shortest beautiful substring is 5.

The lexicographically smallest beautiful substring with length 5 is the substring "11001".

**Example 2:**

**Input:** s = "1011", k = 2

**Output:** "11"

**Explanation:** There are 3 beautiful substrings in this example:
1. The substring "<ins>101</ins>1".
2. The substring "1<ins>011</ins>". 
3. The substring "10<ins>11</ins>". 

The length of the shortest beautiful substring is 2. 

The lexicographically smallest beautiful substring with length 2 is the substring "11".

**Example 3:**

**Input:** s = "000", k = 1

**Output:** ""

**Explanation:** There are no beautiful substrings in this example.

**Constraints:**

*   `1 <= s.length <= 100`
*   `1 <= k <= s.length`

## Solution

```java
public class Solution {
    private int n = 0;

    private int nextOne(String s, int i) {
        for (i++; i < n; i++) {
            if (s.charAt(i) == '1') {
                return i;
            }
        }
        return -1;
    }

    public String shortestBeautifulSubstring(String s, int k) {
        n = s.length();
        int i = nextOne(s, -1);
        int j = i;
        int c = 1;
        while (c != k && j != -1) {
            j = nextOne(s, j);
            c++;
        }
        if (c != k || j == -1) {
            return "";
        }
        int min = j - i + 1;
        String r = s.substring(i, i + min);
        i = nextOne(s, i);
        j = nextOne(s, j);
        while (j != -1) {
            int temp = j - i + 1;
            if (temp < min) {
                min = j - i + 1;
                r = s.substring(i, i + min);
            } else if (temp == min) {
                String r1 = s.substring(i, i + min);
                if (r1.compareTo(r) < 0) {
                    r = r1;
                }
            }
            i = nextOne(s, i);
            j = nextOne(s, j);
        }
        return r;
    }
}
```