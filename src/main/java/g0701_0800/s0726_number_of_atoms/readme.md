## 726\. Number of Atoms

Hard

Given a string `formula` representing a chemical formula, return _the count of each atom_.

The atomic element always starts with an uppercase character, then zero or more lowercase letters, representing the name.

One or more digits representing that element's count may follow if the count is greater than `1`. If the count is `1`, no digits will follow.

*   For example, `"H2O"` and `"H2O2"` are possible, but `"H1O2"` is impossible.

Two formulas are concatenated together to produce another formula.

*   For example, `"H2O2He3Mg4"` is also a formula.

A formula placed in parentheses, and a count (optionally added) is also a formula.

*   For example, `"(H2O2)"` and `"(H2O2)3"` are formulas.

Return the count of all elements as a string in the following form: the first name (in sorted order), followed by its count (if that count is more than `1`), followed by the second name (in sorted order), followed by its count (if that count is more than `1`), and so on.

The test cases are generated so that all the values in the output fit in a **32-bit** integer.

**Example 1:**

**Input:** formula = "H2O"

**Output:** "H2O"

**Explanation:** The count of elements are {'H': 2, 'O': 1}.

**Example 2:**

**Input:** formula = "Mg(OH)2"

**Output:** "H2MgO2"

**Explanation:** The count of elements are {'H': 2, 'Mg': 1, 'O': 2}.

**Example 3:**

**Input:** formula = "K4(ON(SO3)2)2"

**Output:** "K4N2O14S4"

**Explanation:** The count of elements are {'K': 4, 'N': 2, 'O': 14, 'S': 4}.

**Constraints:**

*   `1 <= formula.length <= 1000`
*   `formula` consists of English letters, digits, `'('`, and `')'`.
*   `formula` is always valid.

## Solution

```java
import java.util.ArrayList;
import java.util.Comparator;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.Stack;

@SuppressWarnings({"java:S135", "java:S1149"})
public class Solution {
    private boolean isLower(char c) {
        return c >= 97 && c <= 122;
    }

    private boolean isDigit(char c) {
        return c >= 48 && c <= 57;
    }

    public String countOfAtoms(String formula) {
        int product = 1;
        Stack<Integer> mlrStack = new Stack<>();
        Map<String, Integer> count = new HashMap<>();
        int i = formula.length() - 1;
        while (i >= 0) {
            char c = formula.charAt(i);
            if (c == '(') {
                product /= mlrStack.pop();
                i--;
                continue;
            }
            int rank = 1;
            int mlr = 0;
            while (isDigit(c)) {
                mlr += rank * (c - 48);
                rank *= 10;
                c = formula.charAt(--i);
            }
            if (mlr == 0) {
                ++mlr;
            }
            mlrStack.push(mlr);
            product *= mlr;
            if (c == ')') {
                i--;
                continue;
            }
            StringBuilder atom = new StringBuilder();
            while (isLower(c)) {
                atom.insert(0, c);
                c = formula.charAt(--i);
            }
            atom.insert(0, c);
            String name = atom.toString();
            count.put(name, count.getOrDefault(name, 0) + product);
            product /= mlrStack.pop();
            i--;
        }
        List<String> atomList = new ArrayList<>(count.keySet());
        atomList.sort(Comparator.naturalOrder());
        StringBuilder res = new StringBuilder();
        for (String atom : atomList) {
            res.append(atom);
            if (count.get(atom) > 1) {
                res.append(count.get(atom));
            }
        }
        return res.toString();
    }
}
```