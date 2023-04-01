[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1238\. Circular Permutation in Binary Representation

Medium

Given 2 integers `n` and `start`. Your task is return **any** permutation `p` of `(0,1,2.....,2^n -1)` such that :

*   `p[0] = start`
*   `p[i]` and `p[i+1]` differ by only one bit in their binary representation.
*   `p[0]` and `p[2^n -1]` must also differ by only one bit in their binary representation.

**Example 1:**

**Input:** n = 2, start = 3

**Output:** [3,2,0,1]

**Explanation:** The binary representation of the permutation is (11,10,00,01). All the adjacent element differ by one bit. Another valid permutation is [3,1,0,2]

**Example 2:**

**Input:** n = 3, start = 2

**Output:** [2,6,7,5,4,0,1,3]

**Explanation:** The binary representation of the permutation is (010,110,111,101,100,000,001,011).

**Constraints:**

*   `1 <= n <= 16`
*   `0 <= start < 2 ^ n`

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<Integer> circularPermutation(int n, int start) {

        List<Integer> l1 = new ArrayList<>();
        for (int i = 0; i < (1 << n); i++) {
            l1.add(start ^ (i ^ (i >> 1)));
        }
        return l1;
    }
}
```