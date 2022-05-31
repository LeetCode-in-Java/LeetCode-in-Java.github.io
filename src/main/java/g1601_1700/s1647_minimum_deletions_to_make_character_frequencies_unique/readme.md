## 1647\. Minimum Deletions to Make Character Frequencies Unique

Medium

A string `s` is called **good** if there are no two different characters in `s` that have the same **frequency**.

Given a string `s`, return _the **minimum** number of characters you need to delete to make_ `s` _**good**._

The **frequency** of a character in a string is the number of times it appears in the string. For example, in the string `"aab"`, the **frequency** of `'a'` is `2`, while the **frequency** of `'b'` is `1`.

**Example 1:**

**Input:** s = "aab"

**Output:** 0

**Explanation:** `s` is already good.

**Example 2:**

**Input:** s = "aaabbbcc"

**Output:** 2

**Explanation:** You can delete two 'b's resulting in the good string "aaabcc". Another way it to delete one 'b' and one 'c' resulting in the good string "aaabbc".

**Example 3:**

**Input:** s = "ceabaacb"

**Output:** 2

**Explanation:** You can delete both 'c's resulting in the good string "eabaab". Note that we only care about characters that are still in the string at the end (i.e. frequency of 0 is ignored).

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` contains only lowercase English letters.

## Solution

```java
import java.util.HashSet;
import java.util.Set;

public class Solution {
    public int minDeletions(String s) {
        int cnt = 0;
        int[] freq = new int[26];
        Set<Integer> seen = new HashSet<>();
        for (char c : s.toCharArray()) {
            freq[c - 'a']++;
        }
        for (int i = 0; i < 26; i++) {
            while (freq[i] > 0 && seen.contains(freq[i])) {
                freq[i]--;
                cnt++;
            }
            seen.add(freq[i]);
        }
        return cnt;
    }
}
```