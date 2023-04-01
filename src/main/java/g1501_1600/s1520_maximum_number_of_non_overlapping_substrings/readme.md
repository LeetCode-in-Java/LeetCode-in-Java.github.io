[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1520\. Maximum Number of Non-Overlapping Substrings

Hard

Given a string `s` of lowercase letters, you need to find the maximum number of **non-empty** substrings of `s` that meet the following conditions:

1.  The substrings do not overlap, that is for any two substrings `s[i..j]` and `s[k..l]`, either `j < k` or `i > l` is true.
2.  A substring that contains a certain character `c` must also contain all occurrences of `c`.

Find _the maximum number of substrings that meet the above conditions_. If there are multiple solutions with the same number of substrings, _return the one with minimum total length. _It can be shown that there exists a unique solution of minimum total length.

Notice that you can return the substrings in **any** order.

**Example 1:**

**Input:** s = "adefaddaccc"

**Output:** ["e","f","ccc"]

**Explanation:** The following are all the possible substrings that meet the conditions: 
[ 

"adefaddaccc" 

"adefadda", 

"ef", 

"e", 

"f", 

"ccc", 

] 

If we choose the first string, we cannot choose anything else and we'd get only 1. If we choose "adefadda", we are left with "ccc" which is the only one that doesn't overlap, thus obtaining 2 substrings. Notice also, that it's not optimal to choose "ef" since it can be split into two. Therefore, the optimal way is to choose ["e","f","ccc"] which gives us 3 substrings. No other solution of the same number of substrings exist.

**Example 2:**

**Input:** s = "abbaccd"

**Output:** ["d","bb","cc"]

**Explanation:** Notice that while the set of substrings ["d","abba","cc"] also has length 3, it's considered incorrect since it has larger total length.

**Constraints:**

*   `1 <= s.length <= 10^5`
*   `s` contains only lowercase English letters.

## Solution

```java
import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Deque;
import java.util.List;

public class Solution {
    public List<String> maxNumOfSubstrings(String s) {
        int[] lefts = new int[26];
        int[] rights = new int[26];
        Arrays.fill(lefts, -1);
        for (int i = 0; i < s.length(); i++) {
            int idx = s.charAt(i) - 'a';
            if (lefts[idx] == -1) {
                lefts[idx] = i;
            }
            rights[idx] = i;
        }
        List<String> result = new ArrayList<>();
        Deque<int[]> stack = new ArrayDeque<>();
        int[] top = null;
        for (int i = 0; i < s.length(); i++) {
            int idx = s.charAt(i) - 'a';
            if (i == lefts[idx]) {
                if (top == null || rights[idx] < top[1]) {
                    top = new int[] {i, rights[idx]};
                    stack.offerFirst(top);
                } else if (rights[idx] > top[1]) {
                    top[1] = rights[idx];
                }
            } else if (top != null && lefts[idx] < top[0]) {
                int newEnd = rights[idx];
                while (top != null && top[0] > lefts[idx]) {
                    newEnd = Math.max(newEnd, top[1]);
                    stack.pollFirst();
                    top = stack.peekFirst();
                }
                if (top != null) {
                    top[1] = Math.max(newEnd, top[1]);
                }
            }
            if (top != null && i >= top[1]) {
                result.add(s.substring(top[0], top[1] + 1));
                stack.clear();
                top = null;
            }
        }
        return result;
    }
}
```