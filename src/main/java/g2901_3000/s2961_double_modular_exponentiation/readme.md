[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2961\. Double Modular Exponentiation

Medium

You are given a **0-indexed** 2D array `variables` where <code>variables[i] = [a<sub>i</sub>, b<sub>i</sub>, c<sub>i,</sub> m<sub>i</sub>]</code>, and an integer `target`.

An index `i` is **good** if the following formula holds:

*   `0 <= i < variables.length`
*   <code>((a<sub>i</sub><sup>b<sub>i</sub></sup> % 10)<sup>c<sub>i</sub></sup>) % m<sub>i</sub> == target</code>

Return _an array consisting of **good** indices in **any order**_.

**Example 1:**

**Input:** variables = \[\[2,3,3,10],[3,3,3,1],[6,1,1,4]], target = 2

**Output:** [0,2]

**Explanation:** For each index i in the variables array: 
1) For the index 0, variables[0] = [2,3,3,10], (2<sup>3</sup> % 10)<sup>3</sup> % 10 = 2. 
2) For the index 1, variables[1] = [3,3,3,1], (3<sup>3</sup> % 10)<sup>3</sup> % 1 = 0. 
3) For the index 2, variables[2] = [6,1,1,4], (6<sup>1</sup> % 10)<sup>1</sup> % 4 = 2. 

Therefore we return [0,2] as the answer.

**Example 2:**

**Input:** variables = \[\[39,3,1000,1000]], target = 17

**Output:** []

**Explanation:** For each index i in the variables array: 
1) For the index 0, variables[0] = [39,3,1000,1000], (39<sup>3</sup> % 10)<sup>1000</sup> % 1000 = 1. 

Therefore we return [] as the answer.

**Constraints:**

*   `1 <= variables.length <= 100`
*   <code>variables[i] == [a<sub>i</sub>, b<sub>i</sub>, c<sub>i</sub>, m<sub>i</sub>]</code>
*   <code>1 <= a<sub>i</sub>, b<sub>i</sub>, c<sub>i</sub>, m<sub>i</sub> <= 10<sup>3</sup></code>
*   <code>0 <= target <= 10<sup>3</sup></code>

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    private long myPow(int a, int b, int mod) {
        long ans = 1;
        if (b == 0) {
            return 1;
        }
        if (a <= 1) {
            return a;
        }
        while (b > 0) {
            if (b % 2 == 0) {
                a = a * a % mod;
                b = b / 2;
            } else {
                ans *= a;
                b -= 1;
                ans = ans % mod;
            }
        }
        return ans;
    }

    public List<Integer> getGoodIndices(int[][] variables, int target) {
        int n = variables.length;
        List<Integer> goodIndices = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            int ai = variables[i][0];
            int bi = variables[i][1];
            int ci = variables[i][2];
            int mi = variables[i][3];
            long ans = myPow(ai % 10, bi, 10) % 10;
            ans = myPow((int) ans, ci, mi) % mi;
            if (ans == target) {
                goodIndices.add(i);
            }
        }
        return goodIndices;
    }
}
```