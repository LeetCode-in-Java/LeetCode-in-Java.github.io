[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2507\. Smallest Value After Replacing With Sum of Prime Factors

Medium

You are given a positive integer `n`.

Continuously replace `n` with the sum of its **prime factors**.

*   Note that if a prime factor divides `n` multiple times, it should be included in the sum as many times as it divides `n`.

Return _the smallest value_ `n` _will take on._

**Example 1:**

**Input:** n = 15

**Output:** 5

**Explanation:** Initially, n = 15. 

15 = 3 \* 5, so replace n with 3 + 5 = 8. 

8 = 2 \* 2 \* 2, so replace n with 2 + 2 + 2 = 6.

6 = 2 \* 3, so replace n with 2 + 3 = 5.

5 is the smallest value n will take on.

**Example 2:**

**Input:** n = 3

**Output:** 3

**Explanation:** Initially, n = 3. 3 is the smallest value n will take on.

**Constraints:**

*   <code>2 <= n <= 10<sup>5</sup></code>

## Solution

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    private final Map<Integer, Integer> memo = new HashMap<>();

    public int smallestValue(int n) {
        while (get(n) != n) {
            n = get(n);
        }
        return n;
    }

    private int get(int n) {
        if (memo.containsKey(n)) {
            return memo.get(n);
        }
        double r = Math.pow(n, 0.5);
        int r1 = (int) r;
        if (r - r1 == 0) {
            return 2 * get(r1);
        }
        int res = 0;
        for (int i = r1; i >= 2; i--) {
            if (n % i == 0) {
                res = get(i) + get(n / i);
            }
        }
        res = res == 0 ? n : res;
        memo.put(n, res);
        return res;
    }
}
```