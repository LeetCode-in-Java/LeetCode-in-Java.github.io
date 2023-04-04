[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 187\. Repeated DNA Sequences

Medium

The **DNA sequence** is composed of a series of nucleotides abbreviated as `'A'`, `'C'`, `'G'`, and `'T'`.

*   For example, `"ACGAATTCCG"` is a **DNA sequence**.

When studying **DNA**, it is useful to identify repeated sequences within the DNA.

Given a string `s` that represents a **DNA sequence**, return all the **`10`\-letter-long** sequences (substrings) that occur more than once in a DNA molecule. You may return the answer in **any order**.

**Example 1:**

**Input:** s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT"

**Output:** ["AAAAACCCCC","CCCCCAAAAA"] 

**Example 2:**

**Input:** s = "AAAAAAAAAAAAA"

**Output:** ["AAAAAAAAAA"] 

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s[i]` is either `'A'`, `'C'`, `'G'`, or `'T'`.

## Solution

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Solution {
    public List<String> findRepeatedDnaSequences(String s) {
        if (s.length() < 10) {
            return Collections.emptyList();
        }
        boolean[] seen = new boolean[1024 * 1024];
        boolean[] added = new boolean[1024 * 1024];
        char[] chars = s.toCharArray();
        int buf = 0;
        int[] map = new int[128];
        map['A'] = 0;
        map['C'] = 1;
        map['G'] = 2;
        map['T'] = 3;
        List<String> ans = new ArrayList<>(s.length() / 2);
        for (int i = 0; i < 10; i++) {
            buf = (buf << 2) + map[chars[i]];
        }
        seen[buf] = true;
        for (int i = 10; i < chars.length; i++) {
            buf = ((buf << 2) & 0xFFFFF) + map[chars[i]];
            if (seen[buf]) {
                if (!added[buf]) {
                    ans.add(new String(chars, i - 9, 10));
                    added[buf] = true;
                }
            } else {
                seen[buf] = true;
            }
        }
        return ans;
    }
}
```