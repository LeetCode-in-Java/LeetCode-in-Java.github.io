[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 22\. Generate Parentheses

Medium

Given `n` pairs of parentheses, write a function to _generate all combinations of well-formed parentheses_.

**Example 1:**

**Input:** n = 3

**Output:** ["((()))","(()())","(())()","()(())","()()()"] 

**Example 2:**

**Input:** n = 1

**Output:** ["()"] 

**Constraints:**

*   `1 <= n <= 8`

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<String> generateParenthesis(int n) {
        StringBuilder sb = new StringBuilder();
        List<String> ans = new ArrayList<>();
        return generate(sb, ans, n, n);
    }

    private List<String> generate(StringBuilder sb, List<String> str, int open, int close) {
        if (open == 0 && close == 0) {
            str.add(sb.toString());
            return str;
        }
        if (open > 0) {
            sb.append('(');
            generate(sb, str, open - 1, close);
            sb.deleteCharAt(sb.length() - 1);
        }
        if (close > 0 && open < close) {
            sb.append(')');
            generate(sb, str, open, close - 1);
            sb.deleteCharAt(sb.length() - 1);
        }
        return str;
    }
}
```