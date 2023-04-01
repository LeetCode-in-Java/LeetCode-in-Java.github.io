[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1318\. Minimum Flips to Make a OR b Equal to c

Medium

Given 3 positives numbers `a`, `b` and `c`. Return the minimum flips required in some bits of `a` and `b` to make ( `a` OR `b` == `c` ). (bitwise OR operation).  
Flip operation consists of change **any** single bit 1 to 0 or change the bit 0 to 1 in their binary representation.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/01/06/sample_3_1676.png)

**Input:** a = 2, b = 6, c = 5

**Output:** 3

**Explanation:** After flips a = 1 , b = 4 , c = 5 such that (`a` OR `b` == `c`)

**Example 2:**

**Input:** a = 4, b = 2, c = 7

**Output:** 1

**Example 3:**

**Input:** a = 1, b = 2, c = 3

**Output:** 0

**Constraints:**

*   `1 <= a <= 10^9`
*   `1 <= b <= 10^9`
*   `1 <= c <= 10^9`

## Solution

```java
public class Solution {
    public static int csb(int n) {
        int cnt = 0;
        while (n > 0) {
            int rsb = n & -n;
            n -= rsb;
            cnt++;
        }
        return cnt;
    }

    public int minFlips(int a, int b, int c) {
        int ans = 0;
        int or = a | b;
        ans += csb(or ^ c);
        int and = a & b;
        ans += csb(and & ~c);
        return ans;
    }
}
```