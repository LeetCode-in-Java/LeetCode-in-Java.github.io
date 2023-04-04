[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 475\. Heaters

Medium

Winter is coming! During the contest, your first job is to design a standard heater with a fixed warm radius to warm all the houses.

Every house can be warmed, as long as the house is within the heater's warm radius range.

Given the positions of `houses` and `heaters` on a horizontal line, return _the minimum radius standard of heaters so that those heaters could cover all houses._

**Notice** that all the `heaters` follow your radius standard, and the warm radius will the same.

**Example 1:**

**Input:** houses = [1,2,3], heaters = [2]

**Output:** 1

**Explanation:** The only heater was placed in the position 2, and if we use the radius 1 standard, then all the houses can be warmed.

**Example 2:**

**Input:** houses = [1,2,3,4], heaters = [1,4]

**Output:** 1

**Explanation:** The two heater was placed in the position 1 and 4. We need to use radius 1 standard, then all the houses can be warmed.

**Example 3:**

**Input:** houses = [1,5], heaters = [2]

**Output:** 3

**Constraints:**

*   <code>1 <= houses.length, heaters.length <= 3 * 10<sup>4</sup></code>
*   <code>1 <= houses[i], heaters[i] <= 10<sup>9</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int findRadius(int[] houses, int[] heaters) {
        int res = 0;
        int m = houses.length;
        int n = heaters.length;
        int hs = 0;
        int ht = 0;
        Arrays.sort(houses);
        Arrays.sort(heaters);
        if (n == 1) {
            return Math.max(Math.abs(houses[0] - heaters[0]), Math.abs(houses[m - 1] - heaters[0]));
        }
        while (hs < m && ht < n - 1) {
            if (houses[hs] <= heaters[ht]) {
                res = Math.max(heaters[ht] - houses[hs], res);
                hs++;
            } else if (houses[hs] <= heaters[ht + 1]) {
                res =
                        Math.max(
                                res,
                                Math.min(houses[hs] - heaters[ht], heaters[ht + 1] - houses[hs]));
                hs++;
            } else {
                ht++;
            }
        }
        if (ht == n - 1) {
            res = Math.max(res, houses[m - 1] - heaters[n - 1]);
        }
        return res;
    }
}
```