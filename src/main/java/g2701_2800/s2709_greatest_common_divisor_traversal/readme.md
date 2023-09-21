[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2709\. Greatest Common Divisor Traversal

Hard

You are given a **0-indexed** integer array `nums`, and you are allowed to **traverse** between its indices. You can traverse between index `i` and index `j`, `i != j`, if and only if `gcd(nums[i], nums[j]) > 1`, where `gcd` is the **greatest common divisor**.

Your task is to determine if for **every pair** of indices `i` and `j` in nums, where `i < j`, there exists a **sequence of traversals** that can take us from `i` to `j`.

Return `true` _if it is possible to traverse between all such pairs of indices,_ _or_ `false` _otherwise._

**Example 1:**

**Input:** nums = [2,3,6]

**Output:** true

**Explanation:** In this example, there are 3 possible pairs of indices: (0, 1), (0, 2), and (1, 2). To go from index 0 to index 1, we can use the sequence of traversals 0 -> 2 -> 1, where we move from index 0 to index 2 because gcd(nums[0], nums[2]) = gcd(2, 6) = 2 > 1, and then move from index 2 to index 1 because gcd(nums[2], nums[1]) = gcd(6, 3) = 3 > 1. To go from index 0 to index 2, we can just go directly because gcd(nums[0], nums[2]) = gcd(2, 6) = 2 > 1. Likewise, to go from index 1 to index 2, we can just go directly because gcd(nums[1], nums[2]) = gcd(3, 6) = 3 > 1.

**Example 2:**

**Input:** nums = [3,9,5]

**Output:** false

**Explanation:** No sequence of traversals can take us from index 0 to index 2 in this example. So, we return false.

**Example 3:**

**Input:** nums = [4,3,12,8]

**Output:** true

**Explanation:** There are 6 possible pairs of indices to traverse between: (0, 1), (0, 2), (0, 3), (1, 2), (1, 3), and (2, 3). A valid sequence of traversals exists for each pair, so we return true.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    private Map<Integer, Integer> map = null;
    private int[] set;

    private int findParent(int u) {
        if (u == set[u]) {
            return u;
        }
        set[u] = findParent(set[u]);
        return set[u];
    }

    private void union(int a, int b) {
        int p1 = findParent(a);
        int p2 = findParent(b);
        if (p1 != p2) {
            set[b] = p1;
        }
        set[p2] = p1;
    }

    private void solve(int n, int index) {
        if (n % 2 == 0) {
            int x = map.getOrDefault(2, -1);
            if (x != -1) {
                union(x, index);
            }
            while (n % 2 == 0) {
                n /= 2;
            }
            map.put(2, index);
        }
        int sqrt = (int) Math.sqrt(n);
        for (int i = 3; i <= sqrt; i++) {
            if (n % i == 0) {
                int x = map.getOrDefault(i, -1);
                if (x != -1) {
                    union(x, index);
                }
                while (n % i == 0) {
                    n /= i;
                }
                map.put(i, index);
            }
        }
        if (n > 2) {
            int x = map.getOrDefault(n, -1);
            if (x != -1) {
                union(x, index);
            }
            map.put(n, index);
        }
    }

    public boolean canTraverseAllPairs(int[] nums) {
        set = new int[nums.length];
        map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            set[i] = i;
        }
        for (int i = 0; i < nums.length; i++) {
            solve(nums[i], i);
        }
        int p = findParent(0);
        for (int i = 0; i < nums.length; i++) {
            if (p != findParent(i)) {
                return false;
            }
        }
        return true;
    }
}
```