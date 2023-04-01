[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 354\. Russian Doll Envelopes

Hard

You are given a 2D array of integers `envelopes` where <code>envelopes[i] = [w<sub>i</sub>, h<sub>i</sub>]</code> represents the width and the height of an envelope.

One envelope can fit into another if and only if both the width and height of one envelope are greater than the other envelope's width and height.

Return _the maximum number of envelopes you can Russian doll (i.e., put one inside the other)_.

**Note:** You cannot rotate an envelope.

**Example 1:**

**Input:** envelopes = \[\[5,4],[6,4],[6,7],[2,3]]

**Output:** 3

**Explanation:** The maximum number of envelopes you can Russian doll is `3` ([2,3] => [5,4] => [6,7]).

**Example 2:**

**Input:** envelopes = \[\[1,1],[1,1],[1,1]]

**Output:** 1

**Constraints:**

*   `1 <= envelopes.length <= 5000`
*   `envelopes[i].length == 2`
*   <code>1 <= w<sub>i</sub>, h<sub>i</sub> <= 10<sup>4</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int maxEnvelopes(int[][] envelopes) {
        Arrays.sort(envelopes, (a, b) -> a[0] != b[0] ? a[0] - b[0] : b[1] - a[1]);
        int[] tails = new int[envelopes.length];
        int size = 0;
        for (int[] enve : envelopes) {
            int i = 0;
            int j = size;
            while (i != j) {
                int mid = i + ((j - i) >> 1);
                if (tails[mid] < enve[1]) {
                    i = mid + 1;
                } else {
                    j = mid;
                }
            }
            tails[i] = enve[1];
            if (i == size) {
                size++;
            }
        }
        return size;
    }
}
```