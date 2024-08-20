[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3261\. Count Substrings That Satisfy K-Constraint II

Hard

You are given a **binary** string `s` and an integer `k`.

You are also given a 2D integer array `queries`, where <code>queries[i] = [l<sub>i</sub>, r<sub>i</sub>]</code>.

A **binary string** satisfies the **k-constraint** if **either** of the following conditions holds:

*   The number of `0`'s in the string is at most `k`.
*   The number of `1`'s in the string is at most `k`.

Return an integer array `answer`, where `answer[i]` is the number of substrings of <code>s[l<sub>i</sub>..r<sub>i</sub>]</code> that satisfy the **k-constraint**.

**Example 1:**

**Input:** s = "0001111", k = 2, queries = \[\[0,6]]

**Output:** [26]

**Explanation:**

For the query `[0, 6]`, all substrings of `s[0..6] = "0001111"` satisfy the k-constraint except for the substrings `s[0..5] = "000111"` and `s[0..6] = "0001111"`.

**Example 2:**

**Input:** s = "010101", k = 1, queries = \[\[0,5],[1,4],[2,3]]

**Output:** [15,9,3]

**Explanation:**

The substrings of `s` with a length greater than 3 do not satisfy the k-constraint.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s[i]` is either `'0'` or `'1'`.
*   `1 <= k <= s.length`
*   <code>1 <= queries.length <= 10<sup>5</sup></code>
*   <code>queries[i] == [l<sub>i</sub>, r<sub>i</sub>]</code>
*   <code>0 <= l<sub>i</sub> <= r<sub>i</sub> < s.length</code>
*   All queries are distinct.

## Solution

```java
public class Solution {
    public long[] countKConstraintSubstrings(String s, int k, int[][] queries) {
        char[] current = s.toCharArray();
        int n = current.length;
        long[] prefix = new long[n];
        int[] index = new int[n];
        int i = 0;
        int count = 0;
        int count1 = 0;
        int count0 = 0;
        for (int j = 0; j < n; j++) {
            if (current[j] == '0') {
                count0++;
            }
            if (current[j] == '1') {
                count1++;
            }
            while (count0 > k && count1 > k) {
                if (current[i] == '0') {
                    count0--;
                }
                if (current[i] == '1') {
                    count1--;
                }
                i++;
                index[i] = j - 1;
            }
            count += j - i + 1;
            index[i] = j;
            prefix[j] = count;
        }
        while (i < n) {
            index[i++] = n - 1;
        }
        long[] result = new long[queries.length];
        for (i = 0; i < queries.length; i++) {
            int indexFirst = index[queries[i][0]];
            if (indexFirst > queries[i][1]) {
                long num = queries[i][1] - queries[i][0] + 1L;
                result[i] = ((num) * (num + 1)) / 2;
            } else {
                result[i] = prefix[queries[i][1]] - prefix[indexFirst];
                long num = indexFirst - queries[i][0] + 1L;
                result[i] += ((num) * (num + 1)) / 2;
            }
        }
        return result;
    }
}
```