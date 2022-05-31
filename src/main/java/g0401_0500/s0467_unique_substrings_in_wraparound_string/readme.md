## 467\. Unique Substrings in Wraparound String

Medium

We define the string `s` to be the infinite wraparound string of `"abcdefghijklmnopqrstuvwxyz"`, so `s` will look like this:

*   `"...zabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcd...."`.

Given a string `p`, return _the number of **unique non-empty substrings** of_ `p` _are present in_ `s`.

**Example 1:**

**Input:** p = "a"

**Output:** 1 Explanation: Only the substring "a" of p is in s.

**Example 2:**

**Input:** p = "cac"

**Output:** 2

**Explanation:** There are two substrings ("a", "c") of p in s.

**Example 3:**

**Input:** p = "zab"

**Output:** 6

**Explanation:** There are six substrings ("z", "a", "b", "za", "ab", and "zab") of p in s.

**Constraints:**

*   <code>1 <= p.length <= 10<sup>5</sup></code>
*   `p` consists of lowercase English letters.

## Solution

```java
public class Solution {
    public int findSubstringInWraproundString(String p) {
        char[] str = p.toCharArray();
        int n = str.length;
        int[] map = new int[26];
        int len = 0;
        for (int i = 0; i < n; i++) {
            if (i > 0 && ((str[i - 1] + 1 == str[i]) || (str[i - 1] == 'z' && str[i] == 'a'))) {
                len += 1;
            } else {
                len = 1;
            }
            // we are storing the max len of string for each letter and then we will count all these
            // length.
            map[str[i] - 'a'] = Math.max(map[str[i] - 'a'], len);
        }
        int answer = 0;
        for (int num : map) {
            answer += num;
        }
        return answer;
    }
}
```