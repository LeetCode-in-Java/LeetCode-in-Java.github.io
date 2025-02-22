[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1815\. Maximum Number of Groups Getting Fresh Donuts

Hard

There is a donuts shop that bakes donuts in batches of `batchSize`. They have a rule where they must serve **all** of the donuts of a batch before serving any donuts of the next batch. You are given an integer `batchSize` and an integer array `groups`, where `groups[i]` denotes that there is a group of `groups[i]` customers that will visit the shop. Each customer will get exactly one donut.

When a group visits the shop, all customers of the group must be served before serving any of the following groups. A group will be happy if they all get fresh donuts. That is, the first customer of the group does not receive a donut that was left over from the previous group.

You can freely rearrange the ordering of the groups. Return _the **maximum** possible number of happy groups after rearranging the groups._

**Example 1:**

**Input:** batchSize = 3, groups = [1,2,3,4,5,6]

**Output:** 4

**Explanation:** You can arrange the groups as [6,2,4,5,1,3]. Then the 1<sup>st</sup>, 2<sup>nd</sup>, 4<sup>th</sup>, and 6<sup>th</sup> groups will be happy.

**Example 2:**

**Input:** batchSize = 4, groups = [1,3,2,5,2,2,1,6]

**Output:** 4

**Constraints:**

*   `1 <= batchSize <= 9`
*   `1 <= groups.length <= 30`
*   <code>1 <= groups[i] <= 10<sup>9</sup></code>

## Solution

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    public int maxHappyGroups(int batchSize, int[] groups) {
        if (batchSize == 1) {
            return groups.length;
        }
        int[] withSize = new int[batchSize];
        for (int size : groups) {
            withSize[size % batchSize]++;
        }
        int fromZero = withSize[0];
        withSize[0] = 0;
        int fromEnds = 0;
        for (int l = 1, r = batchSize - 1; l < r; l++, r--) {
            int usable = Math.min(withSize[l], withSize[r]);
            fromEnds += usable;
            withSize[l] -= usable;
            withSize[r] -= usable;
        }
        int fromMid = 0;
        if (batchSize % 2 == 0) {
            fromMid = withSize[batchSize / 2] / 2;
            withSize[batchSize / 2] -= fromMid * 2;
        }
        return get(pruneEnd(withSize), batchSize, 0, new HashMap<>())
                + fromZero
                + fromEnds
                + fromMid;
    }

    private int get(int[] ar, int batchSize, int rem, Map<Long, Integer> cache) {
        long hash = 0;
        for (int e : ar) {
            hash = hash * 69L + e;
        }
        Integer fromCache = cache.get(hash);
        if (fromCache != null) {
            return fromCache;
        }
        if (zeroed(ar)) {
            cache.put(hash, 0);
            return 0;
        }
        int max = 0;
        for (int i = 0; i < ar.length; i++) {
            if (ar[i] == 0) {
                continue;
            }
            ar[i]--;
            int from = get(ar, batchSize, (rem + i) % batchSize, cache);
            if (from > max) {
                max = from;
            }
            ar[i]++;
        }
        int score = max + (rem == 0 ? 1 : 0);
        cache.put(hash, score);
        return score;
    }

    private int[] pruneEnd(int[] in) {
        int endingZeros = 0;
        for (int i = in.length - 1; i >= 0; i--) {
            if (in[i] != 0) {
                break;
            }
            endingZeros++;
        }
        int[] out = new int[in.length - endingZeros];
        System.arraycopy(in, 0, out, 0, out.length);
        return out;
    }

    private boolean zeroed(int[] ar) {
        for (int e : ar) {
            if (e != 0) {
                return false;
            }
        }
        return true;
    }
}
```