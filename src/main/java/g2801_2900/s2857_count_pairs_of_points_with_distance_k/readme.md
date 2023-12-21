[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2857\. Count Pairs of Points With Distance k

Medium

You are given a **2D** integer array `coordinates` and an integer `k`, where <code>coordinates[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> are the coordinates of the <code>i<sup>th</sup></code> point in a 2D plane.

We define the **distance** between two points <code>(x<sub>1</sub>, y<sub>1</sub>)</code> and <code>(x<sub>2</sub>, y<sub>2</sub>)</code> as `(x1 XOR x2) + (y1 XOR y2)` where `XOR` is the bitwise `XOR` operation.

Return _the number of pairs_ `(i, j)` _such that_ `i < j` _and the distance between points_ `i` _and_ `j` _is equal to_ `k`.

**Example 1:**

**Input:** coordinates = \[\[1,2],[4,2],[1,3],[5,2]], k = 5

**Output:** 2

**Explanation:** We can choose the following pairs: 
- (0,1): Because we have (1 XOR 4) + (2 XOR 2) = 5. 
- (2,3): Because we have (1 XOR 5) + (3 XOR 2) = 5.

**Example 2:**

**Input:** coordinates = \[\[1,3],[1,3],[1,3],[1,3],[1,3]], k = 0

**Output:** 10

**Explanation:** Any two chosen pairs will have a distance of 0. There are 10 ways to choose two pairs.

**Constraints:**

*   `2 <= coordinates.length <= 50000`
*   <code>0 <= x<sub>i</sub>, y<sub>i</sub> <= 10<sup>6</sup></code>
*   `0 <= k <= 100`

## Solution

```java
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class Solution {
    public int countPairs(List<List<Integer>> c, int k) {
        int ans = 0;
        Map<Long, Integer> map = new HashMap<>();
        for (List<Integer> p : c) {
            int p0 = p.get(0);
            int p1 = p.get(1);
            for (int i = 0; i <= k; i++) {
                int x1 = i ^ p0;
                int y1 = (k - i) ^ p1;
                long key2 = hash(x1, y1);
                if (map.containsKey(key2)) {
                    ans += map.get(key2);
                }
            }
            long key = hash(p0, p1);
            map.put(key, map.getOrDefault(key, 0) + 1);
        }
        return ans;
    }

    private long hash(int x1, int y1) {
        long r = (long) 1e8;
        return x1 * r + y1;
    }
}
```