[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1096\. Brace Expansion II

Hard

Under the grammar given below, strings can represent a set of lowercase words. Let `R(expr)` denote the set of words the expression represents.

The grammar can best be understood through simple examples:

*   Single letters represent a singleton set containing that word.
    *   `R("a") = {"a"}`
    *   `R("w") = {"w"}`
*   When we take a comma-delimited list of two or more expressions, we take the union of possibilities.
    *   `R("{a,b,c}") = {"a","b","c"}`
    *   `R("{ {a,b},{b,c}}") = {"a","b","c"}` (notice the final set only contains each word at most once)
*   When we concatenate two expressions, we take the set of possible concatenations between two words where the first word comes from the first expression and the second word comes from the second expression.
    *   `R("{a,b}{c,d}") = {"ac","ad","bc","bd"}`
    *   `R("a{b,c}{d,e}f{g,h}") = {"abdfg", "abdfh", "abefg", "abefh", "acdfg", "acdfh", "acefg", "acefh"}`

Formally, the three rules for our grammar:

*   For every lowercase letter `x`, we have `R(x) = {x}`.
*   For expressions <code>e<sub>1</sub>, e<sub>2</sub>, ... , e<sub>k</sub></code> with `k >= 2`, we have <code>R({e<sub>1</sub>, e<sub>2</sub>, ...}) = R(e<sub>1</sub>) ∪ R(e<sub>2</sub>) ∪ ...</code>
*   For expressions <code>e<sub>1</sub></code> and <code>e<sub>2</sub></code>, we have <code>R(e<sub>1</sub> + e<sub>2</sub>) = {a + b for (a, b) in R(e<sub>1</sub>) × R(e<sub>2</sub>)}</code>, where `+` denotes concatenation, and `×` denotes the cartesian product.

Given an expression representing a set of words under the given grammar, return _the sorted list of words that the expression represents_.

**Example 1:**

**Input:** expression = "{a,b}{c,{d,e}}"

**Output:** ["ac","ad","ae","bc","bd","be"]

**Example 2:**

**Input:** expression = "{ {a,z},a{b,c},{ab,z}}"

**Output:** ["a","ab","ac","z"]

**Explanation:** Each distinct word is written only once in the final answer.

**Constraints:**

*   `1 <= expression.length <= 60`
*   `expression[i]` consists of `'{'`, `'}'`, `','`or lowercase English letters.
*   The given `expression` represents a set of words based on the grammar given in the description.

## Solution

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;
import java.util.HashSet;
import java.util.List;
import java.util.Set;

public class Solution {
    public List<String> braceExpansionII(String expression) {
        Set<String> res = flatten(expression);
        List<String> sorted = new ArrayList<>(res);
        Collections.sort(sorted);
        return sorted;
    }

    private Set<String> flatten(String expression) {
        Set<String> res = new HashSet<>();
        // A temp set to store cartesian product results.
        Set<String> curSet = new HashSet<>();
        int idx = 0;
        while (idx < expression.length()) {
            if (expression.charAt(idx) == '{') {
                // end will be the index of matching "}"
                int end = findClosingBrace(expression, idx);
                Set<String> set = flatten(expression.substring(idx + 1, end));
                curSet = concatenateSet(curSet, set);
                idx = end + 1;
            } else if (Character.isLowerCase(expression.charAt(idx))) {
                // Create set with single element
                Set<String> set =
                        new HashSet<>(Arrays.asList(Character.toString(expression.charAt(idx))));
                curSet = concatenateSet(curSet, set);
                idx++;
            } else if (expression.charAt(idx) == ',') {
                res.addAll(curSet);
                curSet.clear();
                idx++;
            }
        }
        // Don't forget!
        res.addAll(curSet);
        return res;
    }

    private Set<String> concatenateSet(Set<String> set1, Set<String> set2) {
        if (set1.isEmpty() || set2.isEmpty()) {
            return !set2.isEmpty() ? new HashSet<>(set2) : new HashSet<>(set1);
        }
        Set<String> res = new HashSet<>();
        for (String s1 : set1) {
            for (String s2 : set2) {
                res.add(s1 + s2);
            }
        }
        return res;
    }

    private int findClosingBrace(String expression, int start) {
        int count = 0;
        int idx = start;
        while (idx < expression.length()) {
            if (expression.charAt(idx) == '{') {
                count++;
            } else if (expression.charAt(idx) == '}') {
                count--;
            }
            if (count == 0) {
                break;
            }
            idx++;
        }
        return idx;
    }
}
```