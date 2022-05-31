## 84\. Largest Rectangle in Histogram

Hard

Given an array of integers `heights` representing the histogram's bar height where the width of each bar is `1`, return _the area of the largest rectangle in the histogram_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/04/histogram.jpg)

**Input:** heights = [2,1,5,6,2,3]

**Output:** 10

**Explanation:** The above is a histogram where width of each bar is 1. The largest rectangle is shown in the red area, which has an area = 10 units. 

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/01/04/histogram-1.jpg)

**Input:** heights = [2,4]

**Output:** 4 

**Constraints:**

*   <code>1 <= heights.length <= 10<sup>5</sup></code>
*   <code>0 <= heights[i] <= 10<sup>4</sup></code>

## Solution

```java
public class Solution {
    public int largestRectangleArea(int[] heights) {
        return largestArea(heights, 0, heights.length);
    }

    private int largestArea(int[] a, int start, int limit) {
        if (a == null || a.length == 0) {
            return 0;
        }
        if (start == limit) {
            return 0;
        }
        if (limit - start == 1) {
            return a[start];
        }
        if (limit - start == 2) {
            int maxOfTwoBars = Math.max(a[start], a[start + 1]);
            int areaFromTwo = Math.min(a[start], a[start + 1]) * 2;
            return Math.max(maxOfTwoBars, areaFromTwo);
        }
        if (checkIfSorted(a, start, limit)) {
            int maxWhenSorted = 0;
            for (int i = start; i < limit; i++) {
                if (a[i] * (limit - i) > maxWhenSorted) {
                    maxWhenSorted = a[i] * (limit - i);
                }
            }
            return maxWhenSorted;
        } else {
            int minInd = findMinInArray(a, start, limit);
            return maxOfThreeNums(
                    largestArea(a, start, minInd),
                    a[minInd] * (limit - start),
                    largestArea(a, minInd + 1, limit));
        }
    }

    private int findMinInArray(int[] a, int start, int limit) {
        int min = Integer.MAX_VALUE;
        int minIndex = -1;
        for (int index = start; index < limit; index++) {
            if (a[index] < min) {
                min = a[index];
                minIndex = index;
            }
        }
        return minIndex;
    }

    private boolean checkIfSorted(int[] a, int start, int limit) {
        for (int i = start + 1; i < limit; i++) {
            if (a[i] < a[i - 1]) {
                return false;
            }
        }
        return true;
    }

    private int maxOfThreeNums(int a, int b, int c) {
        return Math.max(Math.max(a, b), c);
    }
}
```