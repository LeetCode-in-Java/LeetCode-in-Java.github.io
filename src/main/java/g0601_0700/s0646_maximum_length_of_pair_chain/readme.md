[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 646\. Maximum Length of Pair Chain

Medium

You are given an array of `n` pairs `pairs` where <code>pairs[i] = [left<sub>i</sub>, right<sub>i</sub>]</code> and <code>left<sub>i</sub> < right<sub>i</sub></code>.

A pair `p2 = [c, d]` **follows** a pair `p1 = [a, b]` if `b < c`. A **chain** of pairs can be formed in this fashion.

Return _the length longest chain which can be formed_.

You do not need to use up all the given intervals. You can select pairs in any order.

**Example 1:**

**Input:** pairs = \[\[1,2],[2,3],[3,4]]

**Output:** 2

**Explanation:** The longest chain is [1,2] -> [3,4].

**Example 2:**

**Input:** pairs = \[\[1,2],[7,8],[4,5]]

**Output:** 3

**Explanation:** The longest chain is [1,2] -> [4,5] -> [7,8].

**Constraints:**

*   `n == pairs.length`
*   `1 <= n <= 1000`
*   <code>-1000 <= left<sub>i</sub> < right<sub>i</sub> <= 1000</code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int findLongestChain(int[][] pairs) {
        if (pairs.length == 1) {
            return 1;
        }
        Arrays.sort(pairs, (a, b) -> a[1] - b[1]);
        int min = pairs[0][1];
        int max = 1;
        for (int i = 1; i < pairs.length; i++) {
            if (pairs[i][0] > min) {
                max++;
                min = pairs[i][1];
            }
        }
        return max;
    }
}
```