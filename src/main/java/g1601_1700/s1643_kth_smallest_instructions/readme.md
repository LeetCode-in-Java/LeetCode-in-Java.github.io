[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1643\. Kth Smallest Instructions

Hard

Bob is standing at cell `(0, 0)`, and he wants to reach `destination`: `(row, column)`. He can only travel **right** and **down**. You are going to help Bob by providing **instructions** for him to reach `destination`.

The **instructions** are represented as a string, where each character is either:

*   `'H'`, meaning move horizontally (go **right**), or
*   `'V'`, meaning move vertically (go **down**).

Multiple **instructions** will lead Bob to `destination`. For example, if `destination` is `(2, 3)`, both `"HHHVV"` and `"HVHVH"` are valid **instructions**.

However, Bob is very picky. Bob has a lucky number `k`, and he wants the <code>k<sup>th</sup></code> **lexicographically smallest instructions** that will lead him to `destination`. `k` is **1-indexed**.

Given an integer array `destination` and an integer `k`, return _the_ <code>k<sup>th</sup></code> _**lexicographically smallest instructions** that will take Bob to_ `destination`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/12/ex1.png)

**Input:** destination = [2,3], k = 1

**Output:** "HHHVV"

**Explanation:** All the instructions that reach (2, 3) in lexicographic order are as follows: ["HHHVV", "HHVHV", "HHVVH", "HVHHV", "HVHVH", "HVVHH", "VHHHV", "VHHVH", "VHVHH", "VVHHH"].

**Example 2:**

**![](https://assets.leetcode.com/uploads/2020/10/12/ex2.png)**

**Input:** destination = [2,3], k = 2

**Output:** "HHVHV"

**Example 3:**

**![](https://assets.leetcode.com/uploads/2020/10/12/ex3.png)**

**Input:** destination = [2,3], k = 3

**Output:** "HHVVH"

**Constraints:**

*   `destination.length == 2`
*   `1 <= row, column <= 15`
*   `1 <= k <= nCr(row + column, row)`, where `nCr(a, b)` denotes `a` choose `b`.

## Solution

```java
@SuppressWarnings("java:S135")
public class Solution {
    public String kthSmallestPath(int[] destination, int k) {
        StringBuilder sb = new StringBuilder();
        int v = destination[0];
        int n = v + destination[1];
        while (true) {
            int range = choose(--n, v);
            if (k <= range) {
                sb.append('H');
            } else {
                sb.append('V');
                v--;
                k -= range;
            }
            if (v == 0) {
                for (int i = 1; i <= n; i++) {
                    sb.append('H');
                }
                break;
            } else if (v == n) {
                for (int i = 1; i <= v; i++) {
                    sb.append('V');
                }
                break;
            }
        }

        return sb.toString();
    }

    private int choose(int n, int k) {
        if (n - k < k) {
            k = n - k;
        }
        int answer = 1;
        for (int i = 1; i <= k; i++) {
            answer = answer * (n + 1 - i) / i;
        }
        return answer;
    }
}
```