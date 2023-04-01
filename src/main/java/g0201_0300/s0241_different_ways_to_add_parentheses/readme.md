[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 241\. Different Ways to Add Parentheses

Medium

Given a string `expression` of numbers and operators, return _all possible results from computing all the different possible ways to group numbers and operators_. You may return the answer in **any order**.

**Example 1:**

**Input:** expression = "2-1-1"

**Output:** [0,2]

**Explanation:** ((2-1)-1) = 0 (2-(1-1)) = 2 

**Example 2:**

**Input:** expression = "2\*3-4\*5"

**Output:** [-34,-14,-10,-10,10]

**Explanation:**

    (2*(3-(4*5))) = -34
    ((2*3)-(4*5)) = -14
    ((2*(3-4))*5) = -10
    (2*((3-4)*5)) = -10
    (((2*3)-4)*5) = 10 

**Constraints:**

*   `1 <= expression.length <= 20`
*   `expression` consists of digits and the operator `'+'`, `'-'`, and `'*'`.
*   All the integer values in the input expression are in the range `[0, 99]`.

## Solution

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class Solution {
    public List<Integer> diffWaysToCompute(String expression) {
        return diffWaysToCompute(expression, new HashMap<>());
    }

    private List<Integer> diffWaysToCompute(String expression, Map<String, List<Integer>> map) {
        if (map.containsKey(expression)) {
            return map.get(expression);
        }
        List<Integer> values = new ArrayList<>();
        if (!hasOperator(expression)) {
            // base case
            values.add(Integer.parseInt(expression));
        } else {
            // Recursive case. DFS
            for (int i = 0; i < expression.length(); i++) {
                char symbol = expression.charAt(i);

                if (!Character.isDigit(symbol)) {
                    List<Integer> left = diffWaysToCompute(expression.substring(0, i), map);
                    List<Integer> right = diffWaysToCompute(expression.substring(i + 1), map);
                    for (Integer l : left) {
                        for (Integer r : right) {
                            switch (symbol) {
                                case '+':
                                    values.add(l + r);
                                    break;
                                case '-':
                                    values.add(l - r);
                                    break;
                                case '*':
                                    values.add(l * r);
                                    break;
                                default:
                                    break;
                            }
                        }
                    }
                }
            }
        }
        map.put(expression, values);
        return values;
    }

    private boolean hasOperator(String expression) {
        for (int i = 0; i < expression.length(); i++) {
            switch (expression.charAt(i)) {
                case '+':
                case '-':
                case '*':
                    return true;
                default:
                    break;
            }
        }
        return false;
    }
}
```