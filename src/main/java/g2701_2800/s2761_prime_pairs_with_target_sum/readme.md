[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2761\. Prime Pairs With Target Sum

Medium

You are given an integer `n`. We say that two integers `x` and `y` form a prime number pair if:

*   `1 <= x <= y <= n`
*   `x + y == n`
*   `x` and `y` are prime numbers

Return _the 2D sorted list of prime number pairs_ <code>[x<sub>i</sub>, y<sub>i</sub>]</code>. The list should be sorted in **increasing** order of <code>x<sub>i</sub></code>. If there are no prime number pairs at all, return _an empty array_.

**Note:** A prime number is a natural number greater than `1` with only two factors, itself and `1`.

**Example 1:**

**Input:** n = 10

**Output:** [[3,7],[5,5]]

**Explanation:**

In this example, there are two prime pairs that satisfy the criteria.

These pairs are [3,7] and [5,5], and we return them in the sorted order as described in the problem statement.

**Example 2:**

**Input:** n = 2

**Output:** []

**Explanation:**

We can show that there is no prime number pair that gives a sum of 2, so we return an empty array.

**Constraints:**

*   <code>1 <= n <= 10<sup>6</sup></code>

## Solution

```java
import java.util.ArrayList;
import java.util.HashSet;
import java.util.List;

public class Solution {
    private static final HashSet<Integer> PRIMES = new HashSet<>();
    private static final List<Integer> LIST = new ArrayList<>();

    public List<List<Integer>> findPrimePairs(int n) {
        List<List<Integer>> answer = new ArrayList<>();
        for (int a : LIST) {
            int other = n - a;
            if (other < n / 2 || a > n / 2) {
                break;
            }
            if (PRIMES.contains(other)) {
                answer.add(List.of(a, other));
            }
        }
        return answer;
    }

    static {
        int m = 1000001;
        boolean[] visited = new boolean[m];
        for (int i = 2; i < m; i++) {
            if (!visited[i]) {
                PRIMES.add(i);
                LIST.add(i);
                int j = i;
                while (j < m) {
                    visited[j] = true;
                    j += i;
                }
            }
        }
    }
}
```