[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 952\. Largest Component Size by Common Factor

Hard

You are given an integer array of unique positive integers `nums`. Consider the following graph:

*   There are `nums.length` nodes, labeled `nums[0]` to `nums[nums.length - 1]`,
*   There is an undirected edge between `nums[i]` and `nums[j]` if `nums[i]` and `nums[j]` share a common factor greater than `1`.

Return _the size of the largest connected component in the graph_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/12/01/ex1.png)

**Input:** nums = [4,6,15,35]

**Output:** 4

**Example 2:**

![](https://assets.leetcode.com/uploads/2018/12/01/ex2.png)

**Input:** nums = [20,50,9,63]

**Output:** 2

**Example 3:**

![](https://assets.leetcode.com/uploads/2018/12/01/ex3.png)

**Input:** nums = [2,3,6,7,4,12,21,39]

**Output:** 8

**Constraints:**

*   <code>1 <= nums.length <= 2 * 10<sup>4</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>
*   All the values of `nums` are **unique**.

## Solution

```java
public class Solution {
    public int largestComponentSize(int[] nums) {
        int max = 0;
        for (int a : nums) {
            max = Math.max(max, a);
        }
        int[] roots = new int[max + 1];
        int[] sizes = new int[max + 1];
        for (int idx = 1; idx <= max; idx++) {
            roots[idx] = idx;
        }
        for (int a : nums) {
            if (a == 1) {
                sizes[a] = 1;
                continue;
            }
            int sqrt = (int) Math.sqrt(a);
            int thisRoot = getRoot(roots, a);
            sizes[thisRoot]++;
            for (int factor = 1; factor <= sqrt; factor++) {
                if (a % factor == 0) {
                    int otherFactor = a / factor;
                    int otherFactorRoot = getRoot(roots, otherFactor);
                    if (factor != 1) {
                        union(roots, thisRoot, factor, sizes);
                    }
                    union(roots, thisRoot, otherFactorRoot, sizes);
                }
            }
        }
        int maxConnection = 0;
        for (int size : sizes) {
            maxConnection = Math.max(maxConnection, size);
        }
        return maxConnection;
    }

    private void union(int[] roots, int a, int b, int[] sizes) {
        int rootA = getRoot(roots, a);
        int rootB = getRoot(roots, b);
        if (rootA != rootB) {
            sizes[rootA] += sizes[rootB];
            roots[rootB] = rootA;
        }
    }

    private int getRoot(int[] roots, int a) {
        if (roots[a] == a) {
            return a;
        }
        roots[a] = getRoot(roots, roots[a]);
        return roots[a];
    }
}
```