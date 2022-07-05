[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 964\. Least Operators to Express Number

Hard

Given a single positive integer `x`, we will write an expression of the form `x (op1) x (op2) x (op3) x ...` where each operator `op1`, `op2`, etc. is either addition, subtraction, multiplication, or division (`+`, `-`, `*`, or `/)`. For example, with `x = 3`, we might write `3 * 3 / 3 + 3 - 3` which is a value of 3.

When writing such an expression, we adhere to the following conventions:

*   The division operator (`/`) returns rational numbers.
*   There are no parentheses placed anywhere.
*   We use the usual order of operations: multiplication and division happen before addition and subtraction.
*   It is not allowed to use the unary negation operator (`-`). For example, "`x - x`" is a valid expression as it only uses subtraction, but "`-x + x`" is not because it uses negation.

We would like to write an expression with the least number of operators such that the expression equals the given `target`. Return the least number of operators used.

**Example 1:**

**Input:** x = 3, target = 19

**Output:** 5

**Explanation:** 3 \* 3 + 3 \* 3 + 3 / 3.

The expression contains 5 operations.

**Example 2:**

**Input:** x = 5, target = 501

**Output:** 8

**Explanation:** 5 \* 5 \* 5 \* 5 - 5 \* 5 \* 5 + 5 / 5.

The expression contains 8 operations.

**Example 3:**

**Input:** x = 100, target = 100000000

**Output:** 3

**Explanation:** 100 \* 100 \* 100 \* 100. The expression contains 3 operations.

**Constraints:**

*   `2 <= x <= 100`
*   <code>1 <= target <= 2 * 10<sup>8</sup></code>

## Solution

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    private Map<String, Integer> map = new HashMap<>();
    private int x;

    public int leastOpsExpressTarget(int x, int target) {
        this.x = x;
        if (x == target) {
            return 0;
        }
        return dfs(0, target) - 1;
    }

    // ax^0 + bx^1 + cx^2 +....
    // think it as base x problem
    private int dfs(int ex, long target) {
        if (target == 0) {
            return 0;
        }
        if (ex > 40) {
            return 10000000;
        }
        String state = ex + "," + target;
        if (map.containsKey(state)) {
            return map.get(state);
        }
        int res = Integer.MAX_VALUE;
        int mod = (int) (target % x);
        if (mod == 0) {
            if (ex == 0) {
                // not use
                res = Math.min(res, dfs(ex + 1, target));
            } else {
                // not use
                res = Math.min(res, dfs(ex + 1, target / x));
            }
        } else {
            // division is needed
            if (ex == 0) {
                res = Math.min(res, 2 * mod + dfs(ex + 1, target - mod));
                res = Math.min(res, 2 * (x - mod) + dfs(ex + 1, target - mod + x));
            } else {
                res = Math.min(res, (ex - 1) * mod + dfs(ex + 1, (target - mod) / x));
                res = Math.min(res, (ex - 1) * (x - mod) + dfs(ex + 1, (target - mod + x) / x));
            }
        }
        map.put(state, res);
        return res;
    }
}
```