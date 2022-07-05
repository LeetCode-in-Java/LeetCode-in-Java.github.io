[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1156\. Swap For Longest Repeated Character Substring

Medium

You are given a string `text`. You can swap two of the characters in the `text`.

Return _the length of the longest substring with repeated characters_.

**Example 1:**

**Input:** text = "ababa"

**Output:** 3

**Explanation:** We can swap the first 'b' with the last 'a', or the last 'b' with the first 'a'. Then, the longest repeated character substring is "aaa" with length 3.

**Example 2:**

**Input:** text = "aaabaaa"

**Output:** 6

**Explanation:** Swap 'b' with the last 'a' (or the first 'a'), and we get longest repeated character substring "aaaaaa" with length 6.

**Example 3:**

**Input:** text = "aaaaa"

**Output:** 5

**Explanation:** No need to swap, longest repeated character substring is "aaaaa" with length is 5.

**Constraints:**

*   <code>1 <= text.length <= 2 * 10<sup>4</sup></code>
*   `text` consist of lowercase English characters only.

## Solution

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class Solution {
    private static class Pair {
        char character;
        int count;

        public Pair(char c, int count) {
            this.character = c;
            this.count = count;
        }
    }

    public int maxRepOpt1(String text) {
        List<Pair> pairs = new ArrayList<>();
        Map<Character, Integer> map = new HashMap<>();
        // collect counts for each char-block
        int i = 0;
        while (i < text.length()) {
            char c = text.charAt(i);
            int count = 0;
            while (i < text.length() && text.charAt(i) == c) {
                count++;
                i++;
            }
            pairs.add(new Pair(c, count));
            map.put(c, map.getOrDefault(c, 0) + count);
        }
        int max = 0;
        // case 1, swap 1 item to the boundary of a consecutive cha-block to achieve possible max
        // length
        // we need total count to make sure whether a swap is possible!
        for (Pair p : pairs) {
            int totalCount = map.get(p.character);
            if (totalCount > p.count) {
                max = Math.max(max, p.count + 1);
            } else {
                max = Math.max(max, p.count);
            }
        }
        // case 2, find xxxxYxxxxx pattern
        // we need total count to make sure whether a swap is possible!
        for (int j = 1; j < pairs.size() - 1; j++) {
            if (pairs.get(j - 1).character == pairs.get(j + 1).character
                    && pairs.get(j).count == 1) {
                int totalCount = map.get(pairs.get(j - 1).character);
                int groupSum = pairs.get(j - 1).count + pairs.get(j + 1).count;
                if (totalCount > groupSum) {
                    max = Math.max(max, groupSum + 1);
                } else {
                    max = Math.max(max, groupSum);
                }
            }
        }
        return max;
    }
}
```