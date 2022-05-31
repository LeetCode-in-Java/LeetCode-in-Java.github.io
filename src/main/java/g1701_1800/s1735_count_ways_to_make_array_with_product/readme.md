## 1735\. Count Ways to Make Array With Product

Hard

You are given a 2D integer array, `queries`. For each `queries[i]`, where <code>queries[i] = [n<sub>i</sub>, k<sub>i</sub>]</code>, find the number of different ways you can place positive integers into an array of size <code>n<sub>i</sub></code> such that the product of the integers is <code>k<sub>i</sub></code>. As the number of ways may be too large, the answer to the <code>i<sup>th</sup></code> query is the number of ways **modulo** <code>10<sup>9</sup> + 7</code>.

Return _an integer array_ `answer` _where_ `answer.length == queries.length`_, and_ `answer[i]` _is the answer to the_ <code>i<sup>th</sup></code> _query._

**Example 1:**

**Input:** queries = [[2,6],[5,1],[73,660]]

**Output:** [4,1,50734910]

**Explanation:** Each query is independent.

[2,6]: There are 4 ways to fill an array of size 2 that multiply to 6: [1,6], [2,3], [3,2], [6,1].

[5,1]: There is 1 way to fill an array of size 5 that multiply to 1: [1,1,1,1,1].

[73,660]: There are 1050734917 ways to fill an array of size 73 that multiply to 660. 1050734917 modulo 10<sup>9</sup> + 7 = 50734910.

**Example 2:**

**Input:** queries = [[1,1],[2,2],[3,3],[4,4],[5,5]]

**Output:** [1,2,3,10,5]

**Constraints:**

*   <code>1 <= queries.length <= 10<sup>4</sup></code>
*   <code>1 <= n<sub>i</sub>, k<sub>i</sub> <= 10<sup>4</sup></code>

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    private static final int MOD = 1_000_000_007;
    private long[][] tri;
    private List<Integer> primes;

    public int[] waysToFillArray(int[][] queries) {
        int len = queries.length;
        int[] res = new int[len];
        primes = getPrimes(100);
        tri = getTri(10015, 15);
        for (int i = 0; i < len; i++) {
            res[i] = calculate(queries[i][0], queries[i][1]);
        }
        return res;
    }

    private List<Integer> getPrimes(int limit) {
        boolean[] notPrime = new boolean[limit + 1];
        List<Integer> res = new ArrayList<>();
        for (int i = 2; i <= limit; i++) {
            if (!notPrime[i]) {
                res.add(i);
                for (int j = i * i; j <= limit; j += i) {
                    notPrime[j] = true;
                }
            }
        }
        return res;
    }

    private long[][] getTri(int m, int n) {
        long[][] res = new long[m + 1][n + 1];
        for (int i = 0; i <= m; i++) {
            res[i][0] = 1;
            for (int j = 1; j <= Math.min(n, i); j++) {
                res[i][j] = (res[i - 1][j - 1] + res[i - 1][j]) % MOD;
            }
        }
        return res;
    }

    private int calculate(int n, int target) {
        long res = 1;
        for (int prime : primes) {
            if (prime > target) {
                break;
            }
            int cnt = 0;
            while (target % prime == 0) {
                cnt++;
                target /= prime;
            }
            res = (res * tri[cnt + n - 1][cnt]) % MOD;
        }
        return target > 1 ? (int) (res * n % MOD) : (int) res;
    }
}
```