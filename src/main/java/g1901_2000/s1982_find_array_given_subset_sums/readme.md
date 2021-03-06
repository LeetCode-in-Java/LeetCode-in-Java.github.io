[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1982\. Find Array Given Subset Sums

Hard

You are given an integer `n` representing the length of an unknown array that you are trying to recover. You are also given an array `sums` containing the values of all <code>2<sup>n</sup></code> **subset sums** of the unknown array (in no particular order).

Return _the array_ `ans` _of length_ `n` _representing the unknown array. If **multiple** answers exist, return **any** of them_.

An array `sub` is a **subset** of an array `arr` if `sub` can be obtained from `arr` by deleting some (possibly zero or all) elements of `arr`. The sum of the elements in `sub` is one possible **subset sum** of `arr`. The sum of an empty array is considered to be `0`.

**Note:** Test cases are generated such that there will **always** be at least one correct answer.

**Example 1:**

**Input:** n = 3, sums = [-3,-2,-1,0,0,1,2,3]

**Output:** [1,2,-3]

**Explanation:** [1,2,-3] is able to achieve the given subset sums: 

- \[]: sum is 0 

- \[1]: sum is 1 

- \[2]: sum is 2 

- \[1,2]: sum is 3 

- \[-3]: sum is -3 

- \[1,-3]: sum is -2 

- \[2,-3]: sum is -1 

- \[1,2,-3]: sum is 0 
  
Note that any permutation of [1,2,-3] and also any permutation of [-1,-2,3] will also be accepted.

**Example 2:**

**Input:** n = 2, sums = [0,0,0,0]

**Output:** [0,0]

**Explanation:** The only correct answer is [0,0].

**Example 3:**

**Input:** n = 4, sums = [0,0,5,5,4,-1,4,9,9,-1,4,3,4,8,3,8]

**Output:** [0,-1,4,5]

**Explanation:** [0,-1,4,5] is able to achieve the given subset sums.

**Constraints:**

*   `1 <= n <= 15`
*   <code>sums.length == 2<sup>n</sup></code>
*   <code>-10<sup>4</sup> <= sums[i] <= 10<sup>4</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int[] recoverArray(int n, int[] sums) {
        Arrays.sort(sums);
        int m = sums.length;
        int zeroShift = 0;
        int[] res = new int[n];
        for (int i = 0; i < n; ++i) {
            int diff = sums[1] - sums[0];
            int p = 0;
            int k = 0;
            int zpos = m;
            for (int j = 0; j < m; ++j) {
                if (k < p && sums[k] == sums[j]) {
                    k++;
                } else {
                    if (zeroShift == sums[j]) {
                        zpos = p;
                    }
                    sums[p++] = sums[j] + diff;
                }
            }
            if (zpos >= m / 2) {
                res[i] = -diff;
            } else {
                res[i] = diff;
                zeroShift += diff;
            }
            m /= 2;
        }
        return res;
    }
}
```