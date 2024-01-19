[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2981\. Find Longest Special Substring That Occurs Thrice I

Medium

You are given a string `s` that consists of lowercase English letters.

A string is called **special** if it is made up of only a single character. For example, the string `"abc"` is not special, whereas the strings `"ddd"`, `"zz"`, and `"f"` are special.

Return _the length of the **longest special substring** of_ `s` _which occurs **at least thrice**_, _or_ `-1` _if no special substring occurs at least thrice_.

A **substring** is a contiguous **non-empty** sequence of characters within a string.

**Example 1:**

**Input:** s = "aaaa"

**Output:** 2

**Explanation:** The longest special substring which occurs thrice is "aa": substrings "<ins>**aa**</ins>aa", "a<ins>**aa**</ins>a", and "aa<ins>**aa**</ins>".

It can be shown that the maximum length achievable is 2. 

**Example 2:**

**Input:** s = "abcdef"

**Output:** -1

**Explanation:** There exists no special substring which occurs at least thrice. Hence return -1. 

**Example 3:**

**Input:** s = "abcaba"

**Output:** 1

**Explanation:** The longest special substring which occurs thrice is "a": substrings "<ins>**a**</ins>bcaba", "abc<ins>**a**</ins>ba", and "abcab<ins>**a**</ins>".

It can be shown that the maximum length achievable is 1. 

**Constraints:**

*   `3 <= s.length <= 50`
*   `s` consists of only lowercase English letters.

## Solution

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;
import java.util.Map;
import java.util.TreeMap;

public class Solution {
    public int maximumLength(String s) {
        List<List<Integer>> buckets = new ArrayList<>();
        for (int i = 0; i < 26; i++) {
            buckets.add(new ArrayList<>());
        }
        int cur = 1;
        for (int i = 1; i < s.length(); i++) {
            if (s.charAt(i) != s.charAt(i - 1)) {
                int index = s.charAt(i - 1) - 'a';
                buckets.get(index).add(cur);
                cur = 1;
            } else {
                cur++;
            }
        }
        int endIndex = s.charAt(s.length() - 1) - 'a';
        buckets.get(endIndex).add(cur);
        int result = -1;
        for (List<Integer> bucket : buckets) {
            result = Math.max(result, generate(bucket));
        }
        return result;
    }

    private int generate(List<Integer> list) {
        Collections.sort(list, Collections.reverseOrder());
        TreeMap<Integer, Integer> map = new TreeMap<>(Collections.reverseOrder());
        for (int i = 0; i < list.size() && i < 3; i++) {
            int cur = list.get(i);
            int num = map.getOrDefault(cur, 0);
            map.put(cur, num + 1);
            if (cur >= 2) {
                num = map.getOrDefault(cur - 1, 0);
                map.put(cur - 1, num + 2);
            }
            if (cur >= 3) {
                num = map.getOrDefault(cur - 2, 0);
                map.put(cur - 2, num + 3);
            }
        }
        for (Map.Entry<Integer, Integer> entry : map.entrySet()) {
            if (entry.getValue() >= 3) {
                return entry.getKey();
            }
        }
        return -1;
    }
}
```