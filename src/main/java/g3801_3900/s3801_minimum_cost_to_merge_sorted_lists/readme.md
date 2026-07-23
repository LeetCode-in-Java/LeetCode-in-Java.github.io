[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3801\. Minimum Cost to Merge Sorted Lists

Hard

You are given a 2D integer array `lists`, where each `lists[i]` is a non-empty array of integers **sorted** in **non-decreasing** order.

You may **repeatedly** choose two lists `a = lists[i]` and `b = lists[j]`, where `i != j`, and merge them. The **cost** to merge `a` and `b` is:

`len(a) + len(b) + abs(median(a) - median(b))`, where `len` and `median` denote the list length and median, respectively.

After merging `a` and `b`, remove both `a` and `b` from `lists` and insert the new merged **sorted list** in **any** position. Repeat merges until only **one** list remains.

Return an integer denoting the **minimum total cost** required to merge all lists into one single sorted list.

The **median** of an array is the middle element after sorting it in non-decreasing order. If the array has an even number of elements, the median is the left middle element.

**Example 1:**

**Input:** lists = \[\[1,3,5],[2,4],[6,7,8]]

**Output:** 18

**Explanation:**

Merge `a = [1, 3, 5]` and `b = [2, 4]`:

*   `len(a) = 3` and `len(b) = 2`
*   `median(a) = 3` and `median(b) = 2`
*   `cost = len(a) + len(b) + abs(median(a) - median(b)) = 3 + 2 + abs(3 - 2) = 6`

So `lists` becomes `[[1, 2, 3, 4, 5], [6, 7, 8]]`.

Merge `a = [1, 2, 3, 4, 5]` and `b = [6, 7, 8]`:

*   `len(a) = 5` and `len(b) = 3`
*   `median(a) = 3` and `median(b) = 7`
*   `cost = len(a) + len(b) + abs(median(a) - median(b)) = 5 + 3 + abs(3 - 7) = 12`

So `lists` becomes `[[1, 2, 3, 4, 5, 6, 7, 8]]`, and total cost is `6 + 12 = 18`.

**Example 2:**

**Input:** lists = \[\[1,1,5],[1,4,7,8]]

**Output:** 10

**Explanation:**

Merge `a = [1, 1, 5]` and `b = [1, 4, 7, 8]`:

*   `len(a) = 3` and `len(b) = 4`
*   `median(a) = 1` and `median(b) = 4`
*   `cost = len(a) + len(b) + abs(median(a) - median(b)) = 3 + 4 + abs(1 - 4) = 10`

So `lists` becomes `[[1, 1, 1, 4, 5, 7, 8]]`, and total cost is 10.

**Example 3:**

**Input:** lists = \[\[1],[3]]

**Output:** 4

**Explanation:**

Merge `a = [1]` and `b = [3]`:

*   `len(a) = 1` and `len(b) = 1`
*   `median(a) = 1` and `median(b) = 3`
*   `cost = len(a) + len(b) + abs(median(a) - median(b)) = 1 + 1 + abs(1 - 3) = 4`

So `lists` becomes `[[1, 3]]`, and total cost is 4.

**Example 4:**

**Input:** lists = \[\[1],[1]]

**Output:** 2

**Explanation:**

The total cost is `len(a) + len(b) + abs(median(a) - median(b)) = 1 + 1 + abs(1 - 1) = 2`.

**Constraints:**

*   `2 <= lists.length <= 12`
*   `1 <= lists[i].length <= 500`
*   <code>-10<sup>9</sup> <= lists[i][j] <= 10<sup>9</sup></code>
*   `lists[i]` is sorted in non-decreasing order.
*   The **sum** of `lists[i].length` will not exceed 2000.

## Solution

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;
import java.util.List;

public class Solution {
    private int numLt(int[][] lists, int enabled, int guess) {
        int result = 0;
        for (int i = 0; i < lists.length; i++) {
            if (((enabled >> i) & 1) == 1) {
                int[] list = lists[i];
                int low = 0;
                int high = list.length;
                while (low < high) {
                    int mid = (low + high) / 2;
                    if (list[mid] < guess) {
                        low = mid + 1;
                    } else {
                        high = mid;
                    }
                }
                result += low;
            }
        }
        return result;
    }

    public long minMergeCost(int[][] lists) {
        int n = lists.length;
        List<Integer> allNums = getSortedNumbers(lists);
        long[][] subsets = buildSubsetInfo(lists, allNums);
        long[] dp = new long[1 << n];
        Arrays.fill(dp, Long.MAX_VALUE);
        for (int subset = 0; subset < (1 << n); subset++) {
            if (Integer.bitCount(subset) <= 1) {
                dp[subset] = 0;
                continue;
            }
            dp[subset] = calculateMinCost(subset, subsets, dp);
        }
        return dp[(1 << n) - 1];
    }

    private List<Integer> getSortedNumbers(int[][] lists) {
        List<Integer> allNums = new ArrayList<>();
        for (int[] lst : lists) {
            for (int num : lst) {
                allNums.add(num);
            }
        }
        Collections.sort(allNums);
        return allNums;
    }

    private long[][] buildSubsetInfo(int[][] lists, List<Integer> allNums) {
        int n = lists.length;
        long[][] subsets = new long[1 << n][2];
        for (int subset = 1; subset < (1 << n); subset++) {
            int resultLen = getSubsetLength(lists, subset);
            int medianLt = (resultLen - 1) / 2;
            subsets[subset][0] = resultLen;
            subsets[subset][1] = findMedian(lists, subset, medianLt, allNums);
        }
        return subsets;
    }

    private int getSubsetLength(int[][] lists, int subset) {
        int resultLen = 0;
        for (int i = 0; i < lists.length; i++) {
            if (((subset >> i) & 1) == 1) {
                resultLen += lists[i].length;
            }
        }
        return resultLen;
    }

    private int findMedian(int[][] lists, int subset, int medianLt, List<Integer> allNums) {
        int low = 0;
        int high = allNums.size() - 1;
        int actualMedian = -1;
        while (low <= high) {
            int mid = (low + high) / 2;
            int num = allNums.get(mid);
            if (numLt(lists, subset, num) <= medianLt) {
                actualMedian = num;
                low = mid + 1;
            } else {
                high = mid - 1;
            }
        }
        return actualMedian;
    }

    private long calculateMinCost(int subset, long[][] subsets, long[] dp) {
        long minCost = Long.MAX_VALUE;
        int a = (subset - 1) & subset;
        while (a > 0) {
            int b = subset ^ a;
            long aLen = subsets[a][0];
            long aMedian = subsets[a][1];
            long bLen = subsets[b][0];
            long bMedian = subsets[b][1];
            long cost = aLen + bLen + Math.abs(aMedian - bMedian);
            minCost = Math.min(minCost, dp[a] + dp[b] + cost);
            a = (a - 1) & subset;
        }
        return minCost;
    }
}
```