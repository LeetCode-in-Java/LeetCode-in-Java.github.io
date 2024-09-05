[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3273\. Minimum Amount of Damage Dealt to Bob

Hard

You are given an integer `power` and two integer arrays `damage` and `health`, both having length `n`.

Bob has `n` enemies, where enemy `i` will deal Bob `damage[i]` **points** of damage per second while they are _alive_ (i.e. `health[i] > 0`).

Every second, **after** the enemies deal damage to Bob, he chooses **one** of the enemies that is still _alive_ and deals `power` points of damage to them.

Determine the **minimum** total amount of damage points that will be dealt to Bob before **all** `n` enemies are _dead_.

**Example 1:**

**Input:** power = 4, damage = [1,2,3,4], health = [4,5,6,8]

**Output:** 39

**Explanation:**

*   Attack enemy 3 in the first two seconds, after which enemy 3 will go down, the number of damage points dealt to Bob is `10 + 10 = 20` points.
*   Attack enemy 2 in the next two seconds, after which enemy 2 will go down, the number of damage points dealt to Bob is `6 + 6 = 12` points.
*   Attack enemy 0 in the next second, after which enemy 0 will go down, the number of damage points dealt to Bob is `3` points.
*   Attack enemy 1 in the next two seconds, after which enemy 1 will go down, the number of damage points dealt to Bob is `2 + 2 = 4` points.

**Example 2:**

**Input:** power = 1, damage = [1,1,1,1], health = [1,2,3,4]

**Output:** 20

**Explanation:**

*   Attack enemy 0 in the first second, after which enemy 0 will go down, the number of damage points dealt to Bob is `4` points.
*   Attack enemy 1 in the next two seconds, after which enemy 1 will go down, the number of damage points dealt to Bob is `3 + 3 = 6` points.
*   Attack enemy 2 in the next three seconds, after which enemy 2 will go down, the number of damage points dealt to Bob is `2 + 2 + 2 = 6` points.
*   Attack enemy 3 in the next four seconds, after which enemy 3 will go down, the number of damage points dealt to Bob is `1 + 1 + 1 + 1 = 4` points.

**Example 3:**

**Input:** power = 8, damage = [40], health = [59]

**Output:** 320

**Constraints:**

*   <code>1 <= power <= 10<sup>4</sup></code>
*   <code>1 <= n == damage.length == health.length <= 10<sup>5</sup></code>
*   <code>1 <= damage[i], health[i] <= 10<sup>4</sup></code>

## Solution

```java
import java.util.Arrays;

@SuppressWarnings("java:S1210")
public class Solution {
    public long minDamage(int pw, int[] damage, int[] health) {
        long res = 0;
        long sum = 0;
        for (int e : damage) {
            sum += e;
        }
        Pair[] pairs = new Pair[damage.length];
        for (int e = 0; e < damage.length; e++) {
            pairs[e] = new Pair(damage[e], (health[e] + pw - 1) / pw);
        }
        Arrays.sort(pairs);
        for (Pair pr : pairs) {
            res += pr.val * sum;
            sum -= pr.key;
        }
        return res;
    }

    static class Pair implements Comparable<Pair> {
        int key;
        int val;

        Pair(int key, int val) {
            this.key = key;
            this.val = val;
        }

        @Override
        public int compareTo(Pair p) {
            return val * p.key - key * p.val;
        }
    }
}
```