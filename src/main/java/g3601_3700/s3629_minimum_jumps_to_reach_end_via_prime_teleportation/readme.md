[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3629\. Minimum Jumps to Reach End via Prime Teleportation

Medium

You are given an integer array `nums` of length `n`.

You start at index 0, and your goal is to reach index `n - 1`.

From any index `i`, you may perform one of the following operations:

*   **Adjacent Step**: Jump to index `i + 1` or `i - 1`, if the index is within bounds.
*   **Prime Teleportation**: If `nums[i]` is a prime number `p`, you may instantly jump to any index `j != i` such that `nums[j] % p == 0`.

Return the **minimum** number of jumps required to reach index `n - 1`.

**Example 1:**

**Input:** nums = [1,2,4,6]

**Output:** 2

**Explanation:**

One optimal sequence of jumps is:

*   Start at index `i = 0`. Take an adjacent step to index 1.
*   At index `i = 1`, `nums[1] = 2` is a prime number. Therefore, we teleport to index `i = 3` as `nums[3] = 6` is divisible by 2.

Thus, the answer is 2.

**Example 2:**

**Input:** nums = [2,3,4,7,9]

**Output:** 2

**Explanation:**

One optimal sequence of jumps is:

*   Start at index `i = 0`. Take an adjacent step to index `i = 1`.
*   At index `i = 1`, `nums[1] = 3` is a prime number. Therefore, we teleport to index `i = 4` since `nums[4] = 9` is divisible by 3.

Thus, the answer is 2.

**Example 3:**

**Input:** nums = [4,6,5,8]

**Output:** 3

**Explanation:**

*   Since no teleportation is possible, we move through `0 → 1 → 2 → 3`. Thus, the answer is 3.

**Constraints:**

*   <code>1 <= n == nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>6</sup></code>

## Solution

```java
import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.Arrays;

public class Solution {
    public int minJumps(int[] nums) {
        int n = nums.length;
        if (n == 1) {
            return 0;
        }
        int maxVal = 0;
        for (int v : nums) {
            maxVal = Math.max(maxVal, v);
        }
        boolean[] isPrime = sieve(maxVal);
        @SuppressWarnings("unchecked")
        ArrayList<Integer>[] posOfValue = new ArrayList[maxVal + 1];
        for (int i = 0; i < n; i++) {
            int v = nums[i];
            if (posOfValue[v] == null) {
                posOfValue[v] = new ArrayList<>();
            }
            posOfValue[v].add(i);
        }
        boolean[] primeProcessed = new boolean[maxVal + 1];
        int[] dist = new int[n];
        Arrays.fill(dist, -1);
        ArrayDeque<Integer> q = new ArrayDeque<>();
        q.add(0);
        dist[0] = 0;
        while (!q.isEmpty()) {
            int i = q.poll();
            int d = dist[i];
            if (i == n - 1) {
                return d;
            }
            if (i + 1 < n && dist[i + 1] == -1) {
                dist[i + 1] = d + 1;
                q.add(i + 1);
            }
            if (i - 1 >= 0 && dist[i - 1] == -1) {
                dist[i - 1] = d + 1;
                q.add(i - 1);
            }
            int v = nums[i];
            if (v <= maxVal && isPrime[v] && !primeProcessed[v]) {
                for (int mult = v; mult <= maxVal; mult += v) {
                    ArrayList<Integer> list = posOfValue[mult];
                    if (list != null) {
                        for (int idx : list) {
                            if (dist[idx] == -1) {
                                dist[idx] = d + 1;
                                q.add(idx);
                            }
                        }
                    }
                }
                primeProcessed[v] = true;
            }
        }
        return -1;
    }

    private boolean[] sieve(int n) {
        boolean[] prime = new boolean[n + 1];
        if (n >= 2) {
            Arrays.fill(prime, true);
        }
        if (n >= 0) {
            prime[0] = false;
        }
        if (n >= 1) {
            prime[1] = false;
        }
        for (int i = 2; (long) i * i <= n; i++) {
            if (prime[i]) {
                for (int j = i * i; j <= n; j += i) {
                    prime[j] = false;
                }
            }
        }
        return prime;
    }
}
```