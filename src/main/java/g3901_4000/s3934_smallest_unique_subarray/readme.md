[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3934\. Smallest Unique Subarray

Hard

You are given an integer array `nums`.

Find the **minimum** length of a subarray that is **not** **identical** to any other **subarray** in `nums`.

Return an integer denoting the **minimum possible length** of such a **subarray**.

Two **subarrays** are considered identical if they have the same length and the same elements in corresponding positions.

**Example 1:**

**Input:** nums = [3,3,3]

**Output:** 3

**Explanation:**

*   Subarrays of length 1: `[3]` → appears 3 times
*   Subarrays of length 2: `[3, 3]` → appears 2 times
*   Subarrays of length 3: `[3, 3, 3]` → appears once

The subarray `[3, 3, 3]` is unique, so the smallest unique subarray length is 3.

**Example 2:**

**Input:** nums = [2,1,2,3,3]

**Output:** 1

**Explanation:**

Subarrays of length 1:

*   `[2]` → appears 2 times
*   `[1]` → appears once
*   `[3]` → appears 2 times

The subarray `[1]` is unique, so the smallest unique subarray length is 1.

**Example 3:**

**Input:** nums = [1,1,2,2,1]

**Output:** 2

**Explanation:**

Subarrays of length 1:

*   `[1]` → appears 3 times
*   `[2]` → appears 2 times

Subarrays of length 2:

*   `[1, 1]` → appears once
*   `[1, 2]` → appears once
*   `[2, 2]` → appears once
*   `[2, 1]` → appears once

There is at least one subarray of length 2 that is unique, so the smallest unique subarray length is 2.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int smallestUniqueSubarray(int[] nums) {
        int n = nums.length;
        if (n == 1) {
            return 1;
        }
        int[] sa = buildSuffixArray(nums);
        int[] lcp = buildLCP(nums, sa);
        return findSmallestUniqueLength(sa, lcp, n);
    }

    private int findSmallestUniqueLength(int[] sa, int[] lcp, int n) {
        int ans = n;
        for (int rank = 0; rank < n; rank++) {
            int maxLcp = getMaxLcp(rank, lcp, n);
            int suffixLength = n - sa[rank];
            int uniqueLength = maxLcp + 1;

            if (uniqueLength <= suffixLength) {
                ans = Math.min(ans, uniqueLength);
            }
        }
        return ans;
    }

    private int getMaxLcp(int rank, int[] lcp, int n) {
        int maxLcp = 0;
        if (rank > 0) {
            maxLcp = Math.max(maxLcp, lcp[rank - 1]);
        }
        if (rank < n - 1) {
            maxLcp = Math.max(maxLcp, lcp[rank]);
        }
        return maxLcp;
    }

    // ------------------------------------------------------------
    // Build suffix array using prefix doubling + counting sort
    // ------------------------------------------------------------
    private int[] buildSuffixArray(int[] nums) {
        int n = nums.length;
        int m = n + 1;
        int[] a = addSentinel(nums);
        SuffixArrayData data = initializeSuffixArray(a, m);
        for (int len = 1; len < m && data.classes < m; len *= 2) {
            buildNextSuffixArray(data, len, m);
            if (shouldStopDoubling(len, m)) {
                break;
            }
        }
        return removeSentinel(data.sa, n);
    }

    private int[] addSentinel(int[] nums) {
        int[] a = Arrays.copyOf(nums, nums.length + 1);
        a[nums.length] = 0;
        return a;
    }

    private SuffixArrayData initializeSuffixArray(int[] a, int m) {
        int[] sa = new int[m];
        int[] rank = new int[m];
        int[] tmpRank = new int[m];
        int[] tmpSa = new int[m];
        int maxValue = Math.max(100000, m) + 1;
        int[] count = new int[maxValue];
        sortInitialValues(a, sa, count);
        int classes = assignInitialRanks(a, sa, rank);
        return new SuffixArrayData(sa, rank, tmpRank, tmpSa, count, classes);
    }

    private void sortInitialValues(int[] a, int[] sa, int[] count) {
        for (int x : a) {
            count[x]++;
        }
        buildPrefixCounts(count);
        for (int i = a.length - 1; i >= 0; i--) {
            sa[--count[a[i]]] = i;
        }
    }

    private void buildPrefixCounts(int[] count) {
        for (int i = 1; i < count.length; i++) {
            count[i] += count[i - 1];
        }
    }

    private void buildPrefixCounts(int[] count, int classes) {
        for (int i = 1; i < classes; i++) {
            count[i] += count[i - 1];
        }
    }

    private int assignInitialRanks(int[] a, int[] sa, int[] rank) {
        int classes = 1;
        rank[sa[0]] = 0;
        for (int i = 1; i < a.length; i++) {
            if (a[sa[i]] != a[sa[i - 1]]) {
                classes++;
            }
            rank[sa[i]] = classes - 1;
        }
        return classes;
    }

    private SuffixArrayData buildNextSuffixArray(SuffixArrayData data, int len, int m) {
        shiftSuffixes(data.sa, data.tmpSa, len, m);
        countingSortByRank(data, m);
        int newClasses = assignNewRanks(data, len, m);
        int[] swap = data.rank;
        data.rank = data.tmpRank;
        data.tmpRank = swap;
        data.classes = newClasses;
        return data;
    }

    private void shiftSuffixes(int[] sa, int[] tmpSa, int len, int m) {
        for (int i = 0; i < m; i++) {
            int shifted = sa[i] - len;
            if (shifted < 0) {
                shifted += m;
            }
            tmpSa[i] = shifted;
        }
    }

    private void countingSortByRank(SuffixArrayData data, int m) {
        Arrays.fill(data.count, 0, data.classes, 0);
        for (int i = 0; i < m; i++) {
            data.count[data.rank[data.tmpSa[i]]]++;
        }
        buildPrefixCounts(data.count, data.classes);
        for (int i = m - 1; i >= 0; i--) {
            int x = data.tmpSa[i];
            data.sa[--data.count[data.rank[x]]] = x;
        }
    }

    private int assignNewRanks(SuffixArrayData data, int len, int m) {
        data.tmpRank[data.sa[0]] = 0;
        int newClasses = 1;
        for (int i = 1; i < m; i++) {
            int cur = data.sa[i];
            int prev = data.sa[i - 1];
            if (differentRanks(data.rank, cur, prev, len, m)) {
                newClasses++;
            }
            data.tmpRank[cur] = newClasses - 1;
        }
        return newClasses;
    }

    private boolean differentRanks(int[] rank, int cur, int prev, int len, int m) {
        int curSecond = (cur + len) % m;
        int prevSecond = (prev + len) % m;
        return rank[cur] != rank[prev] || rank[curSecond] != rank[prevSecond];
    }

    private boolean shouldStopDoubling(int len, int m) {
        return len > m / 2;
    }

    private int[] removeSentinel(int[] sa, int n) {
        int[] result = new int[n];
        int idx = 0;
        for (int x : sa) {
            if (x != n) {
                result[idx++] = x;
            }
        }
        return result;
    }

    private int[] buildLCP(int[] nums, int[] sa) {
        int n = nums.length;
        int[] rank = buildRanks(sa, n);
        int[] lcp = new int[n - 1];
        buildLcpValues(nums, sa, rank, lcp, n);
        return lcp;
    }

    private int[] buildRanks(int[] sa, int n) {
        int[] rank = new int[n];
        for (int i = 0; i < n; i++) {
            rank[sa[i]] = i;
        }
        return rank;
    }

    private void buildLcpValues(int[] nums, int[] sa, int[] rank, int[] lcp, int n) {
        int k = 0;
        for (int i = 0; i < n; i++) {
            int r = rank[i];
            if (r == n - 1) {
                k = 0;
                continue;
            }
            int j = sa[r + 1];
            k = calculateLcp(nums, i, j, k);
            lcp[r] = k;
            if (k > 0) {
                k--;
            }
        }
    }

    private int calculateLcp(int[] nums, int i, int j, int k) {
        while (i + k < nums.length && j + k < nums.length && nums[i + k] == nums[j + k]) {
            k++;
        }
        return k;
    }

    private static class SuffixArrayData {
        private int[] sa;
        private int[] rank;
        private int[] tmpRank;
        private final int[] tmpSa;
        private final int[] count;
        private int classes;

        private SuffixArrayData(
                int[] sa, int[] rank, int[] tmpRank, int[] tmpSa, int[] count, int classes) {

            this.sa = sa;
            this.rank = rank;
            this.tmpRank = tmpRank;
            this.tmpSa = tmpSa;
            this.count = count;
            this.classes = classes;
        }
    }
}
```