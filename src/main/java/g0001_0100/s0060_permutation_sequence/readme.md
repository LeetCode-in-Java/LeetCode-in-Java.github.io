[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 60\. Permutation Sequence

Hard

The set `[1, 2, 3, ..., n]` contains a total of `n!` unique permutations.

By listing and labeling all of the permutations in order, we get the following sequence for `n = 3`:

1.  `"123"`
2.  `"132"`
3.  `"213"`
4.  `"231"`
5.  `"312"`
6.  `"321"`

Given `n` and `k`, return the `kth` permutation sequence.

**Example 1:**

**Input:** n = 3, k = 3

**Output:** "213" 

**Example 2:**

**Input:** n = 4, k = 9

**Output:** "2314" 

**Example 3:**

**Input:** n = 3, k = 1

**Output:** "123" 

**Constraints:**

*   `1 <= n <= 9`
*   `1 <= k <= n!`

## Solution

```java
public class Solution {
    public String getPermutation(int n, int k) {
        char[] res = new char[n];
        // We want the permutation sequence to be zero-indexed
        k = k - 1;
        // The set bits indicate the available digits
        int a = (1 << n) - 1;
        int m = 1;
        for (int i = 2; i < n; i++) {
            // m = (n - 1)!
            m *= i;
        }
        for (int i = 0; i < n; i++) {
            int b = a;
            for (int j = 0; j < k / m; j++) {
                b &= b - 1;
            }
            // b is the bit corresponding to the digit we want
            b &= ~b + 1;
            res[i] = (char) ('1' + Integer.bitCount(b - 1));
            // Remove b from the set of available digits
            a &= ~b;
            k %= m;
            m /= Math.max(1, n - i - 1);
        }
        return String.valueOf(res);
    }
}
```