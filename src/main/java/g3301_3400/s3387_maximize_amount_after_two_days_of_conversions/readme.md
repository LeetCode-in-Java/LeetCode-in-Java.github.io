[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3387\. Maximize Amount After Two Days of Conversions

Medium

You are given a string `initialCurrency`, and you start with `1.0` of `initialCurrency`.

You are also given four arrays with currency pairs (strings) and rates (real numbers):

*   <code>pairs1[i] = [startCurrency<sub>i</sub>, targetCurrency<sub>i</sub>]</code> denotes that you can convert from <code>startCurrency<sub>i</sub></code> to <code>targetCurrency<sub>i</sub></code> at a rate of `rates1[i]` on **day 1**.
*   <code>pairs2[i] = [startCurrency<sub>i</sub>, targetCurrency<sub>i</sub>]</code> denotes that you can convert from <code>startCurrency<sub>i</sub></code> to <code>targetCurrency<sub>i</sub></code> at a rate of `rates2[i]` on **day 2**.
*   Also, each `targetCurrency` can be converted back to its corresponding `startCurrency` at a rate of `1 / rate`.

You can perform **any** number of conversions, **including zero**, using `rates1` on day 1, **followed** by any number of additional conversions, **including zero**, using `rates2` on day 2.

Return the **maximum** amount of `initialCurrency` you can have after performing any number of conversions on both days **in order**.

**Note:** Conversion rates are valid, and there will be no contradictions in the rates for either day. The rates for the days are independent of each other.

**Example 1:**

**Input:** initialCurrency = "EUR", pairs1 = \[\["EUR","USD"],["USD","JPY"]], rates1 = [2.0,3.0], pairs2 = \[\["JPY","USD"],["USD","CHF"],["CHF","EUR"]], rates2 = [4.0,5.0,6.0]

**Output:** 720.00000

**Explanation:**

To get the maximum amount of **EUR**, starting with 1.0 **EUR**:

*   On Day 1:
    *   Convert **EUR** to **USD** to get 2.0 **USD**.
    *   Convert **USD** to **JPY** to get 6.0 **JPY**.
*   On Day 2:
    *   Convert **JPY** to **USD** to get 24.0 **USD**.
    *   Convert **USD** to **CHF** to get 120.0 **CHF**.
    *   Finally, convert **CHF** to **EUR** to get 720.0 **EUR**.

**Example 2:**

**Input:** initialCurrency = "NGN", pairs1 = \[\["NGN","EUR"]], rates1 = [9.0], pairs2 = \[\["NGN","EUR"]], rates2 = [6.0]

**Output:** 1.50000

**Explanation:**

Converting **NGN** to **EUR** on day 1 and **EUR** to **NGN** using the inverse rate on day 2 gives the maximum amount.

**Example 3:**

**Input:** initialCurrency = "USD", pairs1 = \[\["USD","EUR"]], rates1 = [1.0], pairs2 = \[\["EUR","JPY"]], rates2 = [10.0]

**Output:** 1.00000

**Explanation:**

In this example, there is no need to make any conversions on either day.

**Constraints:**

*   `1 <= initialCurrency.length <= 3`
*   `initialCurrency` consists only of uppercase English letters.
*   `1 <= n == pairs1.length <= 10`
*   `1 <= m == pairs2.length <= 10`
*   <code>pairs1[i] == [startCurrency<sub>i</sub>, targetCurrency<sub>i</sub>]</code>
*   <code>pairs2[i] == [startCurrency<sub>i</sub>, targetCurrency<sub>i</sub>]</code>
*   <code>1 <= startCurrency<sub>i</sub>.length, targetCurrency<sub>i</sub>.length <= 3</code>
*   <code>startCurrency<sub>i</sub></code> and <code>targetCurrency<sub>i</sub></code> consist only of uppercase English letters.
*   `rates1.length == n`
*   `rates2.length == m`
*   `1.0 <= rates1[i], rates2[i] <= 10.0`
*   The input is generated such that there are no contradictions or cycles in the conversion graphs for either day.

## Solution

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.HashSet;
import java.util.List;
import java.util.Map;
import java.util.Set;

@SuppressWarnings("java:S3824")
public class Solution {
    private double res;
    private Map<String, List<Pair>> map1;
    private Map<String, List<Pair>> map2;

    private static class Pair {
        String tarcurr;
        double rate;

        Pair(String t, double r) {
            tarcurr = t;
            rate = r;
        }
    }

    private void solve(
            String currCurrency, double value, String targetCurrency, int day, Set<String> used) {
        if (currCurrency.equals(targetCurrency)) {
            res = Math.max(res, value);
            if (day == 2) {
                return;
            }
        }
        List<Pair> list;
        if (day == 1) {
            list = map1.getOrDefault(currCurrency, new ArrayList<>());
        } else {
            list = map2.getOrDefault(currCurrency, new ArrayList<>());
        }
        for (Pair p : list) {
            if (used.add(p.tarcurr)) {
                solve(p.tarcurr, value * p.rate, targetCurrency, day, used);
                used.remove(p.tarcurr);
            }
        }
        if (day == 1) {
            solve(currCurrency, value, targetCurrency, day + 1, new HashSet<>());
        }
    }

    public double maxAmount(
            String initialCurrency,
            List<List<String>> pairs1,
            double[] rates1,
            List<List<String>> pairs2,
            double[] rates2) {
        map1 = new HashMap<>();
        map2 = new HashMap<>();
        int size = pairs1.size();
        for (int i = 0; i < size; i++) {
            List<String> curr = pairs1.get(i);
            String c1 = curr.get(0);
            String c2 = curr.get(1);
            if (!map1.containsKey(c1)) {
                map1.put(c1, new ArrayList<>());
            }
            map1.get(c1).add(new Pair(c2, rates1[i]));
            if (!map1.containsKey(c2)) {
                map1.put(c2, new ArrayList<>());
            }
            map1.get(c2).add(new Pair(c1, 1d / rates1[i]));
        }
        size = pairs2.size();
        for (int i = 0; i < size; i++) {
            List<String> curr = pairs2.get(i);
            String c1 = curr.get(0);
            String c2 = curr.get(1);
            if (!map2.containsKey(c1)) {
                map2.put(c1, new ArrayList<>());
            }
            map2.get(c1).add(new Pair(c2, rates2[i]));
            if (!map2.containsKey(c2)) {
                map2.put(c2, new ArrayList<>());
            }
            map2.get(c2).add(new Pair(c1, 1d / rates2[i]));
        }
        res = 1d;
        solve(initialCurrency, 1d, initialCurrency, 1, new HashSet<>());
        return res;
    }
}
```