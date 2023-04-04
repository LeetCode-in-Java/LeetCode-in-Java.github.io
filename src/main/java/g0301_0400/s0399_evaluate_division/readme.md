[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 399\. Evaluate Division

Medium

You are given an array of variable pairs `equations` and an array of real numbers `values`, where <code>equations[i] = [A<sub>i</sub>, B<sub>i</sub>]</code> and `values[i]` represent the equation <code>A<sub>i</sub> / B<sub>i</sub> = values[i]</code>. Each <code>A<sub>i</sub></code> or <code>B<sub>i</sub></code> is a string that represents a single variable.

You are also given some `queries`, where <code>queries[j] = [C<sub>j</sub>, D<sub>j</sub>]</code> represents the <code>j<sup>th</sup></code> query where you must find the answer for <code>C<sub>j</sub> / D<sub>j</sub> = ?</code>.

Return _the answers to all queries_. If a single answer cannot be determined, return `-1.0`.

**Note:** The input is always valid. You may assume that evaluating the queries will not result in division by zero and that there is no contradiction.

**Example 1:**

**Input:** equations = \[\["a","b"],["b","c"]], values = [2.0,3.0], queries = \[\["a","c"],["b","a"],["a","e"],["a","a"],["x","x"]]

**Output:** [6.00000,0.50000,-1.00000,1.00000,-1.00000]

**Explanation:**

Given: _a / b = 2.0_, _b / c = 3.0_

queries are: _a / c = ?_, _b / a = ?_, _a / e = ?_, _a / a = ?_, _x / x = ?_

return: [6.0, 0.5, -1.0, 1.0, -1.0 ]

**Example 2:**

**Input:** equations = \[\["a","b"],["b","c"],["bc","cd"]], values = [1.5,2.5,5.0], queries = \[\["a","c"],["c","b"],["bc","cd"],["cd","bc"]]

**Output:** [3.75000,0.40000,5.00000,0.20000]

**Example 3:**

**Input:** equations = \[\["a","b"]], values = [0.5], queries = \[\["a","b"],["b","a"],["a","c"],["x","y"]]

**Output:** [0.50000,2.00000,-1.00000,-1.00000]

**Constraints:**

*   `1 <= equations.length <= 20`
*   `equations[i].length == 2`
*   <code>1 <= A<sub>i</sub>.length, B<sub>i</sub>.length <= 5</code>
*   `values.length == equations.length`
*   `0.0 < values[i] <= 20.0`
*   `1 <= queries.length <= 20`
*   `queries[i].length == 2`
*   <code>1 <= C<sub>j</sub>.length, D<sub>j</sub>.length <= 5</code>
*   <code>A<sub>i</sub>, B<sub>i</sub>, C<sub>j</sub>, D<sub>j</sub></code> consist of lower case English letters and digits.

## Solution

```java
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class Solution {
    private Map<String, String> root;
    private Map<String, Double> rate;

    public double[] calcEquation(
            List<List<String>> equations, double[] values, List<List<String>> queries) {
        root = new HashMap<>();
        rate = new HashMap<>();
        int n = equations.size();
        for (List<String> equation : equations) {
            String x = equation.get(0);
            String y = equation.get(1);
            root.put(x, x);
            root.put(y, y);
            rate.put(x, 1.0);
            rate.put(y, 1.0);
        }
        for (int i = 0; i < n; ++i) {
            String x = equations.get(i).get(0);
            String y = equations.get(i).get(1);
            union(x, y, values[i]);
        }
        double[] result = new double[queries.size()];
        for (int i = 0; i < queries.size(); ++i) {
            String x = queries.get(i).get(0);
            String y = queries.get(i).get(1);
            if (!root.containsKey(x) || !root.containsKey(y)) {
                result[i] = -1;
                continue;
            }
            String rootX = findRoot(x, x, 1.0);
            String rootY = findRoot(y, y, 1.0);
            result[i] = rootX.equals(rootY) ? rate.get(x) / rate.get(y) : -1.0;
        }
        return result;
    }

    private void union(String x, String y, double v) {
        String rootX = findRoot(x, x, 1.0);
        String rootY = findRoot(y, y, 1.0);
        root.put(rootX, rootY);
        double r1 = rate.get(x);
        double r2 = rate.get(y);
        rate.put(rootX, v * r2 / r1);
    }

    private String findRoot(String originalX, String x, double r) {
        if (root.get(x).equals(x)) {
            root.put(originalX, x);
            rate.put(originalX, r * rate.get(x));
            return x;
        }
        return findRoot(originalX, root.get(x), r * rate.get(x));
    }
}
```