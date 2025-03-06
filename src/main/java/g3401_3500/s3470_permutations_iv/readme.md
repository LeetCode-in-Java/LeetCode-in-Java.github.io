[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3470\. Permutations IV

Hard

Given two integers, `n` and `k`, an **alternating permutation** is a permutation of the first `n` positive integers such that no **two** adjacent elements are both odd or both even.

Return the **k-th** **alternating permutation** sorted in _lexicographical order_. If there are fewer than `k` valid **alternating permutations**, return an empty list.

**Example 1:**

**Input:** n = 4, k = 6

**Output:** [3,4,1,2]

**Explanation:**

The lexicographically-sorted alternating permutations of `[1, 2, 3, 4]` are:

1.  `[1, 2, 3, 4]`
2.  `[1, 4, 3, 2]`
3.  `[2, 1, 4, 3]`
4.  `[2, 3, 4, 1]`
5.  `[3, 2, 1, 4]`
6.  `[3, 4, 1, 2]` ← 6th permutation
7.  `[4, 1, 2, 3]`
8.  `[4, 3, 2, 1]`

Since `k = 6`, we return `[3, 4, 1, 2]`.

**Example 2:**

**Input:** n = 3, k = 2

**Output:** [3,2,1]

**Explanation:**

The lexicographically-sorted alternating permutations of `[1, 2, 3]` are:

1.  `[1, 2, 3]`
2.  `[3, 2, 1]` ← 2nd permutation

Since `k = 2`, we return `[3, 2, 1]`.

**Example 3:**

**Input:** n = 2, k = 3

**Output:** []

**Explanation:**

The lexicographically-sorted alternating permutations of `[1, 2]` are:

1.  `[1, 2]`
2.  `[2, 1]`

There are only 2 alternating permutations, but `k = 3`, which is out of range. Thus, we return an empty list `[]`.

**Constraints:**

*   `1 <= n <= 100`
*   <code>1 <= k <= 10<sup>15</sup></code>

## Solution

```java
import java.util.ArrayList;
import java.util.List;

@SuppressWarnings("java:S6541")
public class Solution {
    private static final long INF = 1_000_000_000_000_000_000L;

    private long helper(int a, int b) {
        long res = 1;
        for (int i = 0; i < b; i++) {
            res *= a - i;
            if (res > INF) {
                return INF;
            }
        }
        return res;
    }

    private long solve(int odd, int even, int r, int req) {
        if (r == 0) {
            return 1;
        }
        int nOdd;
        int nEven;
        if (req == 1) {
            nOdd = (r + 1) / 2;
            nEven = r / 2;
        } else {
            nEven = (r + 1) / 2;
            nOdd = r / 2;
        }
        if (odd < nOdd || even < nEven) {
            return 0;
        }
        long oddWays = helper(odd, nOdd);
        long evenWays = helper(even, nEven);
        long total = oddWays;
        if (evenWays == 0 || total > INF / evenWays) {
            total = INF;
        } else {
            total *= evenWays;
        }
        return total;
    }

    public int[] permute(int n, long k) {
        List<Integer> ans = new ArrayList<>();
        boolean first = false;
        boolean[] used = new boolean[n + 1];
        int odd = (n + 1) / 2;
        int even = n / 2;
        int last = -1;
        for (int i = 1; i <= n; i++) {
            if (!used[i]) {
                int odd2 = odd;
                int even2 = even;
                int cp = i & 1;
                int next = (cp == 1 ? 0 : 1);
                if (cp == 1) {
                    odd2--;
                } else {
                    even2--;
                }
                int r = n - 1;
                long cnt = solve(odd2, even2, r, next);
                if (k > cnt) {
                    k -= cnt;
                } else {
                    ans.add(i);
                    used[i] = true;
                    odd = odd2;
                    even = even2;
                    last = cp;
                    first = true;
                    break;
                }
            }
        }
        if (!first) {
            return new int[0];
        }
        for (int z = 1; z < n; z++) {
            for (int j = 1; j <= n; j++) {
                if (!used[j] && ((j & 1) != last)) {
                    int odd2 = odd;
                    int even2 = even;
                    int cp = j & 1;
                    if (cp == 1) {
                        odd2--;
                    } else {
                        even2--;
                    }
                    int r = n - (z + 1);
                    int next = (cp == 1 ? 0 : 1);
                    long cnt2 = solve(odd2, even2, r, next);
                    if (k > cnt2) {
                        k -= cnt2;
                    } else {
                        ans.add(j);
                        used[j] = true;
                        odd = odd2;
                        even = even2;
                        last = cp;
                        break;
                    }
                }
            }
        }
        return ans.stream().mapToInt(i -> i).toArray();
    }
}
```