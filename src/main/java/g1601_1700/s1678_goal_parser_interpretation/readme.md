[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1678\. Goal Parser Interpretation

Easy

You own a **Goal Parser** that can interpret a string `command`. The `command` consists of an alphabet of `"G"`, `"()"` and/or `"(al)"` in some order. The Goal Parser will interpret `"G"` as the string `"G"`, `"()"` as the string `"o"`, and `"(al)"` as the string `"al"`. The interpreted strings are then concatenated in the original order.

Given the string `command`, return _the **Goal Parser**'s interpretation of_ `command`.

**Example 1:**

**Input:** command = "G()(al)"

**Output:** "Goal"

**Explanation:** The Goal Parser interprets the command as follows:

G -> G

() -> o

(al) -> al

The final concatenated result is "Goal".

**Example 2:**

**Input:** command = "G()()()()(al)"

**Output:** "Gooooal"

**Example 3:**

**Input:** command = "(al)G(al)()()G"

**Output:** "alGalooG"

**Constraints:**

*   `1 <= command.length <= 100`
*   `command` consists of `"G"`, `"()"`, and/or `"(al)"` in some order.

## Solution

```java
public class Solution {
    public String interpret(String command) {
        StringBuilder sb = new StringBuilder();
        int i = 0;
        while (i < command.length()) {
            if (command.charAt(i) == '(' && command.charAt(i + 1) == ')') {
                sb.append("o");
                i++;
            } else if ((command.charAt(i) != '(' || command.charAt(i + 1) == ')')
                    && command.charAt(i) != ')') {
                sb.append(command.charAt(i));
            }
            i++;
        }
        return sb.toString();
    }
}
```