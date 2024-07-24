[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3226\. Number of Bit Changes to Make Two Integers Equal

Easy

You are given two positive integers `n` and `k`.

You can choose **any** bit in the **binary representation** of `n` that is equal to 1 and change it to 0.

Return the _number of changes_ needed to make `n` equal to `k`. If it is impossible, return -1.

**Example 1:**

**Input:** n = 13, k = 4

**Output:** 2

**Explanation:**   
 Initially, the binary representations of `n` and `k` are <code>n = (1101)<sub>2</sub></code> and <code>k = (0100)<sub>2</sub></code>.   
 We can change the first and fourth bits of `n`. The resulting integer is <code>n = (<ins>**0**</ins>10<ins>**0**</ins>)<sub>2</sub> = k</code>.

**Example 2:**

**Input:** n = 21, k = 21

**Output:** 0

**Explanation:**   
 `n` and `k` are already equal, so no changes are needed.

**Example 3:**

**Input:** n = 14, k = 13

**Output:** \-1

**Explanation:**   
 It is not possible to make `n` equal to `k`.

**Constraints:**

*   <code>1 <= n, k <= 10<sup>6</sup></code>

## Solution

```java
public class Solution {
    public int minChanges(int n, int k) {
        if ((n | k) != n) {
            return -1;
        }
        int cnt = 0;
        while (n > 0 || k > 0) {
            int bitN = n & 1;
            int bitK = k & 1;
            if (bitN == 1 && bitK == 0) {
                cnt++;
            }
            n >>= 1;
            k >>= 1;
        }
        return cnt;
    }
}
```