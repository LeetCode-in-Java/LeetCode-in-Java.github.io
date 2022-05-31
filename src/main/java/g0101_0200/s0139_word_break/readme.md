## 139\. Word Break

Medium

Given a string `s` and a dictionary of strings `wordDict`, return `true` if `s` can be segmented into a space-separated sequence of one or more dictionary words.

**Note** that the same word in the dictionary may be reused multiple times in the segmentation.

**Example 1:**

**Input:** s = "leetcode", wordDict = ["leet","code"]

**Output:** true

**Explanation:** Return true because "leetcode" can be segmented as "leet code". 

**Example 2:**

**Input:** s = "applepenapple", wordDict = ["apple","pen"]

**Output:** true

**Explanation:** Return true because "applepenapple" can be segmented as "apple pen apple". Note that you are allowed to reuse a dictionary word. 

**Example 3:**

**Input:** s = "catsandog", wordDict = ["cats","dog","sand","and","cat"]

**Output:** false 

**Constraints:**

*   `1 <= s.length <= 300`
*   `1 <= wordDict.length <= 1000`
*   `1 <= wordDict[i].length <= 20`
*   `s` and `wordDict[i]` consist of only lowercase English letters.
*   All the strings of `wordDict` are **unique**.

## Solution

```java
import java.util.HashSet;
import java.util.List;
import java.util.Set;

public class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        Set<String> set = new HashSet<>();
        int max = 0;
        boolean[] flag = new boolean[s.length() + 1];
        for (String st : wordDict) {
            set.add(st);
            if (max < st.length()) {
                max = st.length();
            }
        }
        for (int i = 1; i <= max; i++) {
            if (dfs(s, 0, i, max, set, flag)) {
                return true;
            }
        }
        return false;
    }

    private boolean dfs(String s, int start, int end, int max, Set<String> set, boolean[] flag) {
        if (!flag[end] && set.contains(s.substring(start, end))) {
            flag[end] = true;
            if (end == s.length()) {
                return true;
            }
            for (int i = 1; i <= max; i++) {
                if (end + i <= s.length() && dfs(s, end, end + i, max, set, flag)) {
                    return true;
                }
            }
        }
        return false;
    }
}
```