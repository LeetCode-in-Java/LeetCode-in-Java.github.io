[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 76\. Minimum Window Substring

Hard

Given two strings `s` and `t` of lengths `m` and `n` respectively, return _the **minimum window substring** of_ `s` _such that every character in_ `t` _(**including duplicates**) is included in the window. If there is no such substring__, return the empty string_ `""`_._

The testcases will be generated such that the answer is **unique**.

A **substring** is a contiguous sequence of characters within the string.

**Example 1:**

**Input:** s = "ADOBECODEBANC", t = "ABC"

**Output:** "BANC"

**Explanation:** The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t. 

**Example 2:**

**Input:** s = "a", t = "a"

**Output:** "a"

**Explanation:** The entire string s is the minimum window. 

**Example 3:**

**Input:** s = "a", t = "aa"

**Output:** ""

**Explanation:** Both 'a's from t must be included in the window. Since the largest window of s only has one 'a', return empty string. 

**Constraints:**

*   `m == s.length`
*   `n == t.length`
*   <code>1 <= m, n <= 10<sup>5</sup></code>
*   `s` and `t` consist of uppercase and lowercase English letters.

**Follow up:** Could you find an algorithm that runs in `O(m + n)` time?

## Solution

```java
public class Solution {
    public String minWindow(String s, String t) {
        int[] map = new int[128];
        for (int i = 0; i < t.length(); i++) {
            map[t.charAt(i) - 'A']++;
        }
        int count = t.length();
        int begin = 0;
        int end = 0;
        int d = Integer.MAX_VALUE;
        int head = 0;
        while (end < s.length()) {
            if (map[s.charAt(end++) - 'A']-- > 0) {
                count--;
            }
            while (count == 0) {
                if (end - begin < d) {
                    d = end - begin;
                    head = begin;
                }
                if (map[s.charAt(begin++) - 'A']++ == 0) {
                    count++;
                }
            }
        }
        return d == Integer.MAX_VALUE ? "" : s.substring(head, head + d);
    }
}
```

**Time Complexity (Big O Time):**

1. The program begins by creating an integer array `map` of size 128 to store character frequencies. It iterates through the characters in string `t` once, incrementing the corresponding count in the `map`. This initialization step runs in O(t.length()) time.

2. The main part of the program uses two pointers, `begin` and `end`, to slide through string `s`. This part contains a while loop that iterates through the entire string `s`. In the worst case, the loop can iterate over each character in `s` twice (once for `end` and once for `begin`), so it runs in O(2 * s.length()) time.

3. Within the loop, there is a nested while loop that adjusts the `begin` pointer and updates the minimum window. The number of iterations of this inner loop depends on the length of the window and the contents of string `s` and string `t`. In the worst case, this inner loop can iterate through the entire string `s` once. Therefore, the inner loop's time complexity is O(s.length()).

4. The overall time complexity of the program is dominated by the two iterations over `s`. Therefore, the time complexity is O(s.length()).

**Space Complexity (Big O Space):**

1. The program uses additional space for the integer array `map`, which has a fixed size of 128 characters (assuming ASCII characters). This space complexity is constant, O(1), because the size of `map` does not depend on the input strings `s` and `t`.

2. The program uses several integer variables (`count`, `begin`, `end`, `d`, `head`) to keep track of indices and counts. These variables consume a constant amount of space, which is also O(1).

3. The program does not use additional data structures or arrays that scale with the size of the input strings.

4. Therefore, the space complexity of the program is O(1), or constant space.

In summary, the time complexity of the provided program is O(s.length()), and the space complexity is O(1). The program efficiently finds the minimum window in string `s` containing all characters from string `t`.
