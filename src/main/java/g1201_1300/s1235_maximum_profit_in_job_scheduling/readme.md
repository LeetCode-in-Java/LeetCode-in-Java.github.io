[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1235\. Maximum Profit in Job Scheduling

Hard

We have `n` jobs, where every job is scheduled to be done from `startTime[i]` to `endTime[i]`, obtaining a profit of `profit[i]`.

You're given the `startTime`, `endTime` and `profit` arrays, return the maximum profit you can take such that there are no two jobs in the subset with overlapping time range.

If you choose a job that ends at time `X` you will be able to start another job that starts at time `X`.

**Example 1:**

**![](https://assets.leetcode.com/uploads/2019/10/10/sample1_1584.png)**

**Input:** startTime = [1,2,3,3], endTime = [3,4,5,6], profit = [50,10,40,70]

**Output:** 120

**Explanation:** The subset chosen is the first and fourth job. Time range [1-3]+[3-6] , we get profit of 120 = 50 + 70.

**Example 2:**

**![](https://assets.leetcode.com/uploads/2019/10/10/sample22_1584.png)**

**Input:** startTime = [1,2,3,4,6], endTime = [3,5,10,6,9], profit = [20,20,100,70,60]

**Output:** 150

**Explanation:** The subset chosen is the first, fourth and fifth job. Profit obtained 150 = 20 + 70 + 60.

**Example 3:**

**![](https://assets.leetcode.com/uploads/2019/10/10/sample3_1584.png)**

**Input:** startTime = [1,1,1], endTime = [2,3,4], profit = [5,6,4]

**Output:** 6

**Constraints:**

*   <code>1 <= startTime.length == endTime.length == profit.length <= 5 * 10<sup>4</sup></code>
*   <code>1 <= startTime[i] < endTime[i] <= 10<sup>9</sup></code>
*   <code>1 <= profit[i] <= 10<sup>4</sup></code>

## Solution

```java
import java.util.Arrays;
import java.util.Comparator;

public class Solution {
    public int jobScheduling(int[] startTime, int[] endTime, int[] profit) {
        int n = startTime.length;
        int[][] time = new int[n][3];
        for (int i = 0; i < n; i++) {
            time[i][0] = startTime[i];
            time[i][1] = endTime[i];
            time[i][2] = profit[i];
        }
        Arrays.sort(time, Comparator.comparingInt(a -> a[1]));
        int[][] maxP = new int[n][2];
        int lastPos = -1;
        int currProfit;
        for (int i = 0; i < n; i++) {
            currProfit = time[i][2];
            for (int j = lastPos; j >= 0; j--) {
                if (maxP[j][1] <= time[i][0]) {
                    currProfit += maxP[j][0];
                    break;
                }
            }
            if (lastPos == -1 || currProfit > maxP[lastPos][0]) {
                lastPos++;
                maxP[lastPos][0] = currProfit;
                maxP[lastPos][1] = time[i][1];
            }
        }
        return maxP[lastPos][0];
    }
}
```