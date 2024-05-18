[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3133\. Minimum Array End

Medium

You are given two integers `n` and `x`. You have to construct an array of **positive** integers `nums` of size `n` where for every `0 <= i < n - 1`, `nums[i + 1]` is **greater than** `nums[i]`, and the result of the bitwise `AND` operation between all elements of `nums` is `x`.

Return the **minimum** possible value of `nums[n - 1]`.

**Example 1:**

**Input:** n = 3, x = 4

**Output:** 6

**Explanation:**

`nums` can be `[4,5,6]` and its last element is 6.

**Example 2:**

**Input:** n = 2, x = 7

**Output:** 15

**Explanation:**

`nums` can be `[7,15]` and its last element is 15.

**Constraints:**

*   <code>1 <= n, x <= 10<sup>8</sup></code>

## Solution

```java
public class Solution {
    public long minEnd(int n, int x) {
        n = n - 1;
        int[] xb = new int[64];
        int[] nb = new int[64];
        for (int i = 0; i < 32; i++) {
            xb[i] = (x >> i) & 1;
            nb[i] = (n >> i) & 1;
        }
        int i = 0;
        int j = 0;
        while (i < 64) {
            if (xb[i] != 1) {
                xb[i] = nb[j++];
            }
            i++;
        }
        long ans = 0;
        long p = 1;
        for (i = 0; i < 64; i++) {
            ans += (xb[i]) * p;
            p *= 2;
        }
        return ans;
    }
}
```