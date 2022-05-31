## 856\. Score of Parentheses

Medium

Given a balanced parentheses string `s`, return _the **score** of the string_.

The **score** of a balanced parentheses string is based on the following rule:

*   `"()"` has score `1`.
*   `AB` has score `A + B`, where `A` and `B` are balanced parentheses strings.
*   `(A)` has score `2 * A`, where `A` is a balanced parentheses string.

**Example 1:**

**Input:** s = "()"

**Output:** 1

**Example 2:**

**Input:** s = "(())"

**Output:** 2

**Example 3:**

**Input:** s = "()()"

**Output:** 2

**Constraints:**

*   `2 <= s.length <= 50`
*   `s` consists of only `'('` and `')'`.
*   `s` is a balanced parentheses string.

## Solution

```java
import java.util.ArrayDeque;
import java.util.Deque;

public class Solution {
    public int scoreOfParentheses(String s) {
        Deque<Integer> stack = new ArrayDeque<>();
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == '(') {
                stack.push(-1);
            } else {
                int curr = 0;
                while (stack.peek() != -1) {
                    curr += stack.pop();
                }
                stack.pop();
                stack.push(curr == 0 ? 1 : curr * 2);
            }
        }
        int score = 0;
        while (!stack.isEmpty()) {
            score += stack.pop();
        }
        return score;
    }
}
```