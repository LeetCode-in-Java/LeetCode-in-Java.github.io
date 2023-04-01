[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1111\. Maximum Nesting Depth of Two Valid Parentheses Strings

Medium

A string is a _valid parentheses string_ (denoted VPS) if and only if it consists of `"("` and `")"` characters only, and:

*   It is the empty string, or
*   It can be written as `AB` (`A` concatenated with `B`), where `A` and `B` are VPS's, or
*   It can be written as `(A)`, where `A` is a VPS.

We can similarly define the _nesting depth_ `depth(S)` of any VPS `S` as follows:

*   `depth("") = 0`
*   `depth(A + B) = max(depth(A), depth(B))`, where `A` and `B` are VPS's
*   `depth("(" + A + ")") = 1 + depth(A)`, where `A` is a VPS.

For example, `""`, `"()()"`, and `"()(()())"` are VPS's (with nesting depths 0, 1, and 2), and `")("` and `"(()"` are not VPS's.

Given a VPS seq, split it into two disjoint subsequences `A` and `B`, such that `A` and `B` are VPS's (and `A.length + B.length = seq.length`).

Now choose **any** such `A` and `B` such that `max(depth(A), depth(B))` is the minimum possible value.

Return an `answer` array (of length `seq.length`) that encodes such a choice of `A` and `B`: `answer[i] = 0` if `seq[i]` is part of `A`, else `answer[i] = 1`. Note that even though multiple answers may exist, you may return any of them.

**Example 1:**

**Input:** seq = "(()())"

**Output:** [0,1,1,1,1,0]

**Example 2:**

**Input:** seq = "()(())()"

**Output:** [0,0,0,1,1,0,1,1]

**Constraints:**

*   `1 <= seq.size <= 10000`

## Solution

```java
public class Solution {
    public int[] maxDepthAfterSplit(String seq) {
        int n = seq.length();
        int[] ans = new int[n];
        char[] chars = seq.toCharArray();
        int depth = 0;
        for (int i = 0; i < n; i++) {
            if (chars[i] == '(') {
                depth++;
                if (depth % 2 == 0) {
                    ans[i] = 0;
                } else {
                    ans[i] = 1;
                }
            } else {
                if (depth % 2 == 0) {
                    ans[i] = 0;
                } else {
                    ans[i] = 1;
                }
                depth--;
            }
        }
        return ans;
    }
}
```