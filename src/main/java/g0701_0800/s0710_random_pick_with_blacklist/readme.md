[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 710\. Random Pick with Blacklist

Hard

You are given an integer `n` and an array of **unique** integers `blacklist`. Design an algorithm to pick a random integer in the range `[0, n - 1]` that is **not** in `blacklist`. Any integer that is in the mentioned range and not in `blacklist` should be **equally likely** to be returned.

Optimize your algorithm such that it minimizes the number of calls to the **built-in** random function of your language.

Implement the `Solution` class:

*   `Solution(int n, int[] blacklist)` Initializes the object with the integer `n` and the blacklisted integers `blacklist`.
*   `int pick()` Returns a random integer in the range `[0, n - 1]` and not in `blacklist`.

**Example 1:**

**Input**

["Solution", "pick", "pick", "pick", "pick", "pick", "pick", "pick"]

[[7, [2, 3, 5]], [], [], [], [], [], [], []]

**Output:** [null, 0, 4, 1, 6, 1, 0, 4]

**Explanation:**

    Solution solution = new Solution(7, [2, 3, 5]);
    solution.pick(); // return 0, any integer from [0,1,4,6] should be ok. Note that for every call of pick,
                     // 0, 1, 4, and 6 must be equally likely to be returned (i.e., with probability 1/4).
    solution.pick(); // return 4
    solution.pick(); // return 1
    solution.pick(); // return 6
    solution.pick(); // return 1
    solution.pick(); // return 0
    solution.pick(); // return 4 

**Constraints:**

*   <code>1 <= n <= 10<sup>9</sup></code>
*   <code>0 <= blacklist.length <- min(10<sup>5</sup>, n - 1)</code>
*   `0 <= blacklist[i] < n`
*   All the values of `blacklist` are **unique**.
*   At most <code>2 * 10<sup>4</sup></code> calls will be made to `pick`.

## Solution

```java
import java.util.HashMap;
import java.util.Map;
import java.util.Random;

@SuppressWarnings("java:S2245")
public class Solution {
    private final Map<Integer, Integer> map;
    private final Random r;
    private final int upperLimit;

    public Solution(int n, int[] blacklist) {
        map = new HashMap<>();
        r = new Random();
        upperLimit = n - blacklist.length;
        for (int val : blacklist) {
            map.put(val, -1);
        }
        int j = n - 1;
        for (int val : blacklist) {
            if (val < upperLimit) {
                while (map.containsKey(j)) {
                    j--;
                }
                map.put(val, j);
                j--;
            }
        }
    }

    public int pick() {
        int val = r.nextInt(upperLimit);
        if (map.containsKey(val)) {
            return map.get(val);
        }
        return val;
    }
}
```