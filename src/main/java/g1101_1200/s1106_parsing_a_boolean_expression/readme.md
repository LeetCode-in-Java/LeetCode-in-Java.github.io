[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1106\. Parsing A Boolean Expression

Hard

Return the result of evaluating a given boolean `expression`, represented as a string.

An expression can either be:

*   `"t"`, evaluating to `True`;
*   `"f"`, evaluating to `False`;
*   `"!(expr)"`, evaluating to the logical NOT of the inner expression `expr`;
*   `"&(expr1,expr2,...)"`, evaluating to the logical AND of 2 or more inner expressions `expr1, expr2, ...`;
*   `"|(expr1,expr2,...)"`, evaluating to the logical OR of 2 or more inner expressions `expr1, expr2, ...`

**Example 1:**

**Input:** expression = "!(f)"

**Output:** true

**Example 2:**

**Input:** expression = "\|(f,t)"

**Output:** true

**Example 3:**

**Input:** expression = "&(t,f)"

**Output:** false

**Constraints:**

*   <code>1 <= expression.length <= 2 * 10<sup>4</sup></code>
*   `expression[i]` consists of characters in `{'(', ')', '&', '|', '!', 't', 'f', ','}`.
*   `expression` is a valid expression representing a boolean, as given in the description.

## Solution

```java
import java.util.ArrayList;
import java.util.List;
import java.util.logging.Level;
import java.util.logging.Logger;

public class Solution {
    private String source;
    private int index;

    public boolean parseBoolExpr(String expression) {
        this.source = expression;
        this.index = 0;
        return expr();
    }

    private boolean expr() {
        boolean res;
        if (match('!')) {
            res = not();
        } else if (match('&')) {
            res = and();
        } else if (match('|')) {
            res = or();
        } else {
            res = bool();
        }
        return res;
    }

    private boolean not() {
        consume('!', "Expect '!'");
        return !group().get(0);
    }

    private boolean or() {
        consume('|', "Expect '|'");
        boolean res = false;
        for (boolean e : group()) {
            res |= e;
        }
        return res;
    }

    private boolean and() {
        consume('&', "Expect '&'");
        boolean res = true;
        for (boolean e : group()) {
            res &= e;
        }
        return res;
    }

    private List<Boolean> group() {
        consume('(', "Expect '('");
        List<Boolean> res = new ArrayList<>();
        while (!match(')')) {
            res.add(expr());
            if (match(',')) {
                advance();
            }
        }
        consume(')', "Expect ')'");
        return res;
    }

    private boolean bool() {
        boolean isTrue = match('t');
        advance();
        return isTrue;
    }

    private boolean isAtEnd() {
        return index >= source.length();
    }

    private void advance() {
        if (isAtEnd()) {
            return;
        }
        index++;
    }

    private char peek() {
        return source.charAt(index);
    }

    private boolean match(char ch) {
        if (isAtEnd()) {
            return false;
        }
        return peek() == ch;
    }

    private void consume(char ch, String message) {
        if (!match(ch)) {
            Logger.getLogger(Solution.class.getName()).log(Level.SEVERE, () -> message);
            return;
        }
        advance();
    }
}
```