[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1806\. Minimum Number of Operations to Reinitialize a Permutation

Medium

You are given an **even** integer `n`. You initially have a permutation `perm` of size `n` where `perm[i] == i` **(0-indexed)**.

In one operation, you will create a new array `arr`, and for each `i`:

*   If `i % 2 == 0`, then `arr[i] = perm[i / 2]`.
*   If `i % 2 == 1`, then `arr[i] = perm[n / 2 + (i - 1) / 2]`.

You will then assign `arr` to `perm`.

Return _the minimum **non-zero** number of operations you need to perform on_ `perm` _to return the permutation to its initial value._

**Example 1:**

**Input:** n = 2

**Output:** 1

**Explanation:** perm = [0,1] initially. 

After the 1<sup>st</sup> operation, perm = [0,1] 

So it takes only 1 operation.

**Example 2:**

**Input:** n = 4

**Output:** 2

**Explanation:** perm = [0,1,2,3] initially. 

After the 1<sup>st</sup> operation, perm = [0,2,1,3] 

After the 2<sup>nd</sup> operation, perm = [0,1,2,3] 

So it takes only 2 operations.

**Example 3:**

**Input:** n = 6

**Output:** 4

**Constraints:**

*   `2 <= n <= 1000`
*   `n` is even.

## Solution

```java
public class Solution {
    public int reinitializePermutation(int n) {
        final int factor = n - 1;
        if (factor < 2) {
            return 1;
        }
        int powerOfTwo = 2;
        int ops = 1;
        while (powerOfTwo != 1) {
            powerOfTwo = ((powerOfTwo << 1) % factor);
            ops++;
        }
        return ops;
    }
}
```