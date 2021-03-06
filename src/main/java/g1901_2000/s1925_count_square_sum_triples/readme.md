[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1925\. Count Square Sum Triples

Easy

A **square triple** `(a,b,c)` is a triple where `a`, `b`, and `c` are **integers** and <code>a<sup>2</sup> + b<sup>2</sup> = c<sup>2</sup></code>.

Given an integer `n`, return _the number of **square triples** such that_ `1 <= a, b, c <= n`.

**Example 1:**

**Input:** n = 5

**Output:** 2

**Explanation:** The square triples are (3,4,5) and (4,3,5).

**Example 2:**

**Input:** n = 10

**Output:** 4

**Explanation:** The square triples are (3,4,5), (4,3,5), (6,8,10), and (8,6,10).

**Constraints:**

*   `1 <= n <= 250`

## Solution

```java
public class Solution {
    public int countTriples(int n) {
        int count = 0;
        for (int i = 1; i < n; i++) {
            for (int j = 1; j < n; j++) {
                int product = i * i + j * j;
                double sq = Math.sqrt(product);
                if (sq <= n && (sq - Math.floor(sq) == 0)) {
                    count++;
                }
            }
        }
        return count;
    }
}
```