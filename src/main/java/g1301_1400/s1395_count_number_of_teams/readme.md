[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1395\. Count Number of Teams

Medium

There are `n` soldiers standing in a line. Each soldier is assigned a **unique** `rating` value.

You have to form a team of 3 soldiers amongst them under the following rules:

*   Choose 3 soldiers with index (`i`, `j`, `k`) with rating (`rating[i]`, `rating[j]`, `rating[k]`).
*   A team is valid if: (`rating[i] < rating[j] < rating[k]`) or (`rating[i] > rating[j] > rating[k]`) where (`0 <= i < j < k < n`).

Return the number of teams you can form given the conditions. (soldiers can be part of multiple teams).

**Example 1:**

**Input:** rating = [2,5,3,4,1]

**Output:** 3

**Explanation:** We can form three teams given the conditions. (2,3,4), (5,4,1), (5,3,1). 

**Example 2:**

**Input:** rating = [2,1,3]

**Output:** 0

**Explanation:** We can't form any team given the conditions. 

**Example 3:**

**Input:** rating = [1,2,3,4]

**Output:** 4 

**Constraints:**

*   `n == rating.length`
*   `3 <= n <= 1000`
*   <code>1 <= rating[i] <= 10<sup>5</sup></code>
*   All the integers in `rating` are **unique**.

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int numTeams(int[] rating) {
        int[] cp = rating.clone();
        Arrays.sort(cp);
        // count i, j such that i<j and rating[i]<rating[j]
        int[][] count = new int[cp.length][2];
        int[] bit = new int[cp.length];
        for (int i = 0; i < rating.length; i++) {
            count[i][0] = count(bs(cp, rating[i] - 1), bit);
            count[i][1] = i - count[i][0];
            add(bs(cp, rating[i]), bit);
        }
        // reverse count from right to left, that i<j and rating[i]>rating[j]
        int[][] reverseCount = new int[cp.length][2];
        int[] reverseBit = new int[cp.length];
        for (int i = cp.length - 1; i >= 0; i--) {
            reverseCount[i][0] = count(bs(cp, rating[i] - 1), reverseBit);
            reverseCount[i][1] = cp.length - 1 - i - reverseCount[i][0];
            add(bs(cp, rating[i]), reverseBit);
        }
        int result = 0;
        for (int i = 0; i < rating.length; i++) {
            result += count[i][0] * reverseCount[i][1] + count[i][1] * reverseCount[i][0];
        }
        return result;
    }

    private int count(int idx, int[] bit) {
        int sum = 0;
        while (idx >= 0) {
            sum += bit[idx];
            idx = (idx & (idx + 1)) - 1;
        }
        return sum;
    }

    private void add(int idx, int[] bit) {
        if (idx < 0) {
            return;
        }
        while (idx < bit.length) {
            bit[idx] += 1;
            idx = (idx | (idx + 1));
        }
    }

    private int bs(int[] arr, int val) {
        int l = 0;
        int r = arr.length - 1;
        while (l < r) {
            int m = l + (r - l) / 2;
            if (arr[m] == val) {
                return m;
            } else if (arr[m] < val) {
                l = m + 1;
            } else {
                r = m - 1;
            }
        }
        return arr[l] > val ? l - 1 : l;
    }
}
```