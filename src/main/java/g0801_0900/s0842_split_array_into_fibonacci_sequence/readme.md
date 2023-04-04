[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 842\. Split Array into Fibonacci Sequence

Medium

You are given a string of digits `num`, such as `"123456579"`. We can split it into a Fibonacci-like sequence `[123, 456, 579]`.

Formally, a **Fibonacci-like** sequence is a list `f` of non-negative integers such that:

*   <code>0 <= f[i] < 2<sup>31</sup></code>, (that is, each integer fits in a **32-bit** signed integer type),
*   `f.length >= 3`, and
*   `f[i] + f[i + 1] == f[i + 2]` for all `0 <= i < f.length - 2`.

Note that when splitting the string into pieces, each piece must not have extra leading zeroes, except if the piece is the number `0` itself.

Return any Fibonacci-like sequence split from `num`, or return `[]` if it cannot be done.

**Example 1:**

**Input:** num = "1101111"

**Output:** [11,0,11,11]

**Explanation:** The output [110, 1, 111] would also be accepted.

**Example 2:**

**Input:** num = "112358130"

**Output:** []

**Explanation:** The task is impossible.

**Example 3:**

**Input:** num = "0123"

**Output:** []

**Explanation:** Leading zeroes are not allowed, so "01", "2", "3" is not valid.

**Constraints:**

*   `1 <= num.length <= 200`
*   `num` contains only digits.

## Solution

```java
import java.util.ArrayList;
import java.util.List;

@SuppressWarnings("java:S5413")
public class Solution {
    public List<Integer> splitIntoFibonacci(String num) {
        List<Integer> res = new ArrayList<>();
        solve(num, res, 0);
        return res;
    }

    private boolean solve(String s, List<Integer> res, int idx) {
        if (idx == s.length() && res.size() >= 3) {
            return true;
        }
        for (int i = idx; i < s.length(); i++) {
            if (s.charAt(idx) == '0' && i > idx) {
                return false;
            }
            long num = Long.parseLong(s.substring(idx, i + 1));
            if (num > Integer.MAX_VALUE) {
                return false;
            }
            int size = res.size();
            if (size >= 2 && num > res.get(size - 1) + res.get(size - 2)) {
                return false;
            }
            if (size <= 1 || num == res.get(size - 1) + res.get(size - 2)) {
                res.add((int) num);
                if (solve(s, res, i + 1)) {
                    return true;
                }
                res.remove(res.size() - 1);
            }
        }
        return false;
    }
}
```