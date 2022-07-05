[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 828\. Count Unique Characters of All Substrings of a Given String

Hard

Let's define a function `countUniqueChars(s)` that returns the number of unique characters on `s`.

*   For example if `s = "LEETCODE"` then `"L"`, `"T"`, `"C"`, `"O"`, `"D"` are the unique characters since they appear only once in `s`, therefore `countUniqueChars(s) = 5`.

Given a string `s`, return the sum of `countUniqueChars(t)` where `t` is a substring of s.

Notice that some substrings can be repeated so in this case you have to count the repeated ones too.

**Example 1:**

**Input:** s = "ABC"

**Output:** 10

**Explanation:** All possible substrings are: "A","B","C","AB","BC" and "ABC". 

Evey substring is composed with only unique letters. 

Sum of lengths of all substring is 1 + 1 + 1 + 2 + 2 + 3 = 10

**Example 2:**

**Input:** s = "ABA"

**Output:** 8

**Explanation:** The same as example 1, except `countUniqueChars`("ABA") = 1.

**Example 3:**

**Input:** s = "LEETCODE"

**Output:** 92

**Constraints:**

*   `1 <= s.length <= 10`<sup>5</sup>
*   `s` consists of uppercase English letters only.

## Solution

```java
import java.util.HashMap;

public class Solution {
    public int uniqueLetterString(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }

        int[] left = new int[s.length()];
        int[] right = new int[s.length()];

        HashMap<Character, Integer> map = new HashMap<>();
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            map.putIfAbsent(c, -1);
            left[i] = map.get(c);
            map.put(c, i);
        }
        map.clear();
        for (int i = s.length() - 1; i >= 0; i--) {
            char c = s.charAt(i);
            map.putIfAbsent(c, s.length());
            right[i] = map.get(c);
            map.put(c, i);
        }

        int res = 0;
        for (int i = 0; i < s.length(); i++) {
            int numOfLeft = i - left[i];
            int numOfRight = right[i] - i;
            res += (numOfLeft * numOfRight);
        }
        return res;
    }
}
```