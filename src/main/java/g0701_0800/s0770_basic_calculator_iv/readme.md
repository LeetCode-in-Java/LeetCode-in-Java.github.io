[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 770\. Basic Calculator IV

Hard

Given an expression such as `expression = "e + 8 - a + 5"` and an evaluation map such as `{"e": 1}` (given in terms of `evalvars = ["e"]` and `evalints = [1]`), return a list of tokens representing the simplified expression, such as `["-1*a","14"]`

*   An expression alternates chunks and symbols, with a space separating each chunk and symbol.
*   A chunk is either an expression in parentheses, a variable, or a non-negative integer.
*   A variable is a string of lowercase letters (not including digits.) Note that variables can be multiple letters, and note that variables never have a leading coefficient or unary operator like `"2x"` or `"-x"`.

Expressions are evaluated in the usual order: brackets first, then multiplication, then addition and subtraction.

*   For example, `expression = "1 + 2 * 3"` has an answer of `["7"]`.

The format of the output is as follows:

*   For each term of free variables with a non-zero coefficient, we write the free variables within a term in sorted order lexicographically.
    *   For example, we would never write a term like `"b*a*c"`, only `"a*b*c"`.
*   Terms have degrees equal to the number of free variables being multiplied, counting multiplicity. We write the largest degree terms of our answer first, breaking ties by lexicographic order ignoring the leading coefficient of the term.
    *   For example, `"a*a*b*c"` has degree `4`.
*   The leading coefficient of the term is placed directly to the left with an asterisk separating it from the variables (if they exist.) A leading coefficient of 1 is still printed.
*   An example of a well-formatted answer is `["-2*a*a*a", "3*a*a*b", "3*b*b", "4*a", "5*c", "-6"]`.
*   Terms (including constant terms) with coefficient `0` are not included.
    *   For example, an expression of `"0"` has an output of `[]`.

**Example 1:**

**Input:** expression = "e + 8 - a + 5", evalvars = ["e"], evalints = [1]

**Output:** ["-1\*a","14"] 

**Example 2:**

**Input:** expression = "e - 8 + temperature - pressure", evalvars = ["e", "temperature"], evalints = [1, 12]

**Output:** ["-1\*pressure","5"] 

**Example 3:**

**Input:** expression = "(e + 8) \* (e - 8)", evalvars = [], evalints = []

**Output:** ["1\*e\*e","-64"] 

**Constraints:**

*   `1 <= expression.length <= 250`
*   `expression` consists of lowercase English letters, digits, `'+'`, `'-'`, `'*'`, `'('`, `')'`, `' '`.
*   `expression` does not contain any leading or trailing spaces.
*   All the tokens in `expression` are separated by a single space.
*   `0 <= evalvars.length <= 100`
*   `1 <= evalvars[i].length <= 20`
*   `evalvars[i]` consists of lowercase English letters.
*   `evalints.length == evalvars.length`
*   `-100 <= evalints[i] <= 100`

## Solution

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class Solution {
    private static class Result {
        private Map<List<String>, Integer> map;

        Result() {
            map = new HashMap<>();
        }

        Result(Map<List<String>, Integer> map) {
            this.map = map;
        }

        void update(List<String> key, int val) {
            map.put(key, map.getOrDefault(key, 0) + val);
        }

        Map<List<String>, Integer> getMap() {
            return map;
        }

        List<String> toList() {
            List<List<String>> keyList = new ArrayList<>(map.keySet());
            Map<List<String>, String> list2String = new HashMap<>();
            for (List<String> key : keyList) {
                StringBuilder sb = new StringBuilder();
                for (String k : key) {
                    sb.append(k).append("*");
                }
                list2String.put(key, sb.toString());
            }
            keyList.sort(
                    (a, b) ->
                            (a.size() == b.size()
                                    ? list2String.get(a).compareTo(list2String.get(b))
                                    : b.size() - a.size()));
            List<String> res = new ArrayList<>();
            for (List<String> key : keyList) {
                if (map.get(key) == 0) {
                    continue;
                }
                StringBuilder sb = new StringBuilder();
                sb.append(map.get(key));
                for (String k : key) {
                    sb.append("*").append(k);
                }
                res.add(sb.toString());
            }
            return res;
        }
    }

    private Map<String, Integer> evalMap;
    private int i = 0;

    public List<String> basicCalculatorIV(String expression, String[] evalvars, int[] evalints) {
        evalMap = new HashMap<>();
        for (int j = 0; j < evalvars.length; j++) {
            evalMap.put(evalvars[j], evalints[j]);
        }
        i = -1;
        next(expression);
        Result res = expression(expression);
        return res.toList();
    }

    private Result expression(String s) {
        Result res = term(s);
        while (i < s.length() && (s.charAt(i) == '+' || s.charAt(i) == '-')) {
            int c = s.charAt(i);
            next(s);
            if (c == '+') {
                res = add(res, term(s));
            } else {
                res = subtract(res, term(s));
            }
        }
        return res;
    }

    private Result term(String s) {
        Result res = factor(s);
        while (i < s.length() && s.charAt(i) == '*') {
            next(s);
            res = multiply(res, factor(s));
        }
        return res;
    }

    private Result multiply(Result r1, Result r2) {
        Map<List<String>, Integer> map1 = r1.getMap();
        Map<List<String>, Integer> map2 = r2.getMap();
        Map<List<String>, Integer> map = new HashMap<>();
        for (Map.Entry<List<String>, Integer> entry1 : map1.entrySet()) {
            for (Map.Entry<List<String>, Integer> entry2 : map2.entrySet()) {
                List<String> key = new ArrayList<>(entry1.getKey());
                key.addAll(entry2.getKey());
                Collections.sort(key);
                map.put(key, map.getOrDefault(key, 0) + entry1.getValue() * entry2.getValue());
            }
        }
        return new Result(map);
    }

    private Result add(Result r1, Result r2) {
        Map<List<String>, Integer> map1 = r1.getMap();
        Map<List<String>, Integer> map2 = r2.getMap();
        Map<List<String>, Integer> map = new HashMap<>();
        for (Map.Entry<List<String>, Integer> entry1 : map1.entrySet()) {
            map.put(entry1.getKey(), map.getOrDefault(entry1.getKey(), 0) + entry1.getValue());
        }
        for (Map.Entry<List<String>, Integer> entry2 : map2.entrySet()) {
            map.put(entry2.getKey(), map.getOrDefault(entry2.getKey(), 0) + entry2.getValue());
        }
        return new Result(map);
    }

    private Result subtract(Result r1, Result r2) {
        Map<List<String>, Integer> map1 = r1.getMap();
        Map<List<String>, Integer> map2 = r2.getMap();
        Map<List<String>, Integer> map = new HashMap<>();
        for (Map.Entry<List<String>, Integer> entry1 : map1.entrySet()) {
            map.put(entry1.getKey(), map.getOrDefault(entry1.getKey(), 0) + entry1.getValue());
        }
        for (Map.Entry<List<String>, Integer> entry2 : map2.entrySet()) {
            map.put(entry2.getKey(), map.getOrDefault(entry2.getKey(), 0) - entry2.getValue());
        }
        return new Result(map);
    }

    private Result factor(String s) {
        Result res = new Result();
        if (s.charAt(i) == '(') {
            next(s);
            res = expression(s);
            next(s);
            return res;
        }
        if (Character.isLowerCase(s.charAt(i))) {
            return identifier(s);
        }
        res.update(new ArrayList<>(), number(s));
        return res;
    }

    private Result identifier(String s) {
        Result res = new Result();
        StringBuilder sb = new StringBuilder();
        while (i < s.length() && Character.isLowerCase(s.charAt(i))) {
            sb.append(s.charAt(i));
            i++;
        }
        i--;
        next(s);
        String variable = sb.toString();
        if (evalMap.containsKey(variable)) {
            res.update(new ArrayList<>(), evalMap.get(variable));
        } else {
            List<String> key = new ArrayList<>();
            key.add(variable);
            res.update(key, 1);
        }
        return res;
    }

    private int number(String s) {
        int res = 0;
        while (i < s.length() && s.charAt(i) >= '0' && s.charAt(i) <= '9') {
            res = res * 10 + (s.charAt(i) - '0');
            i++;
        }
        i--;
        next(s);
        return res;
    }

    private void next(String s) {
        i++;
        while (i < s.length() && s.charAt(i) == ' ') {
            i++;
        }
    }
}
```