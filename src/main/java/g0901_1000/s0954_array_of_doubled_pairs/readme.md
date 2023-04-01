[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 954\. Array of Doubled Pairs

Medium

Given an integer array of even length `arr`, return `true` _if it is possible to reorder_ `arr` _such that_ `arr[2 * i + 1] = 2 * arr[2 * i]` _for every_ `0 <= i < len(arr) / 2`_, or_ `false` _otherwise_.

**Example 1:**

**Input:** arr = [3,1,3,6]

**Output:** false

**Example 2:**

**Input:** arr = [2,1,2,6]

**Output:** false

**Example 3:**

**Input:** arr = [4,-2,2,-4]

**Output:** true

**Explanation:** We can take two groups, [-2,-4] and [2,4] to form [-2,-4,2,4] or [2,4,-2,-4].

**Constraints:**

*   <code>2 <= arr.length <= 3 * 10<sup>4</sup></code>
*   `arr.length` is even.
*   <code>-10<sup>5</sup> <= arr[i] <= 10<sup>5</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    public boolean canReorderDoubled(int[] arr) {
        int max = Math.max(0, Arrays.stream(arr).max().getAsInt());
        int min = Math.min(0, Arrays.stream(arr).min().getAsInt());
        int[] positive = new int[max + 1];
        int[] negative = new int[-min + 1];
        for (int a : arr) {
            if (a < 0) {
                negative[-a]++;
            } else {
                positive[a]++;
            }
        }
        if (positive[0] % 2 != 0) {
            return false;
        }
        return validateFrequencies(positive, max) && validateFrequencies(negative, -min);
    }

    private boolean validateFrequencies(int[] frequencies, int limit) {
        for (int i = 0; i <= limit; i++) {
            if (frequencies[i] == 0) {
                continue;
            }
            if (2 * i > limit || frequencies[2 * i] < frequencies[i]) {
                return false;
            }
            frequencies[2 * i] -= frequencies[i];
        }
        return true;
    }
}
```