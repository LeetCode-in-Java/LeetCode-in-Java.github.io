## 990\. Satisfiability of Equality Equations

Medium

You are given an array of strings `equations` that represent relationships between variables where each string `equations[i]` is of length `4` and takes one of two different forms: <code>"x<sub>i</sub>==y<sub>i</sub>"</code> or <code>"x<sub>i</sub>!=y<sub>i</sub>"</code>.Here, <code>x<sub>i</sub></code> and <code>y<sub>i</sub></code> are lowercase letters (not necessarily different) that represent one-letter variable names.

Return `true` _if it is possible to assign integers to variable names so as to satisfy all the given equations, or_ `false` _otherwise_.

**Example 1:**

**Input:** equations = ["a==b","b!=a"]

**Output:** false

**Explanation:** If we assign say, a = 1 and b = 1, then the first equation is satisfied, but not the second.

There is no way to assign the variables to satisfy both equations.

**Example 2:**

**Input:** equations = ["b==a","a==b"]

**Output:** true

**Explanation:** We could assign a = 1 and b = 1 to satisfy both equations.

**Constraints:**

*   `1 <= equations.length <= 500`
*   `equations[i].length == 4`
*   `equations[i][0]` is a lowercase letter.
*   `equations[i][1]` is either `'='` or `'!'`.
*   `equations[i][2]` is `'='`.
*   `equations[i][3]` is a lowercase letter.

## Solution

```java
import java.util.HashMap;

public class Solution {
    private int[] par;

    public boolean equationsPossible(String[] equations) {
        int counter = 0;
        HashMap<Character, Integer> map = new HashMap<>();
        for (String str : equations) {
            char ch = str.charAt(0);
            if (!map.containsKey(ch)) {
                map.put(ch, counter);
                counter++;
            }
            ch = str.charAt(3);
            if (!map.containsKey(ch)) {
                map.put(ch, counter);
                counter++;
            }
        }
        par = new int[counter];
        for (int i = 0; i < par.length; i++) {
            par[i] = i;
        }
        for (String str : equations) {
            String oper = str.substring(1, 3);
            if (oper.equals("==")) {
                int px = find(map.get(str.charAt(0)));
                int py = find(map.get(str.charAt(3)));
                if (px != py) {
                    par[px] = py;
                }
            }
        }
        for (String str : equations) {
            String oper = str.substring(1, 3);
            if (oper.equals("!=")) {
                int px = find(map.get(str.charAt(0)));
                int py = find(map.get(str.charAt(3)));
                if (px == py) {
                    return false;
                }
            }
        }
        return true;
    }

    private int find(int x) {
        if (par[x] == x) {
            return x;
        }
        par[x] = find(par[x]);
        return par[x];
    }
}
```