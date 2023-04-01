[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 546\. Remove Boxes

Hard

You are given several `boxes` with different colors represented by different positive numbers.

You may experience several rounds to remove boxes until there is no box left. Each time you can choose some continuous boxes with the same color (i.e., composed of `k` boxes, `k >= 1`), remove them and get `k * k` points.

Return _the maximum points you can get_.

**Example 1:**

**Input:** boxes = [1,3,2,2,2,3,4,3,1]

**Output:** 23

**Explanation:**

    [1, 3, 2, 2, 2, 3, 4, 3, 1] ----> [1, 3, 3, 4, 3, 1]
    (3\*3=9 points) ----> [1, 3, 3, 3, 1]
    (1\*1=1 points) ----> [1, 1]
    (3\*3=9 points) ----> []
    (2\*2=4 points)

**Example 2:**

**Input:** boxes = [1,1,1]

**Output:** 9

**Example 3:**

**Input:** boxes = [1]

**Output:** 1

**Constraints:**

*   `1 <= boxes.length <= 100`
*   `1 <= boxes[i] <= 100`

## Solution

```java
public class Solution {
    private Integer[][][] dp;

    public int removeBoxes(int[] boxes) {
        int n = boxes.length;
        dp = new Integer[n][n][n + 1];
        return helper(boxes, 0, n - 1, 0);
    }

    private int helper(int[] boxes, int left, int right, int count) {
        if (left > right) {
            return 0;
        }
        if (dp[left][right][count] != null) {
            return dp[left][right][count];
        }
        int leftDash = left;
        int countDash = count;
        while (leftDash < right && boxes[leftDash] == boxes[leftDash + 1]) {
            leftDash++;
            countDash++;
        }
        int ans;
        ans = helper(boxes, leftDash + 1, right, 0) + (countDash + 1) * (countDash + 1);
        for (int i = leftDash + 1; i <= right; i++) {
            if (boxes[i] == boxes[leftDash]) {
                int temp =
                        helper(boxes, leftDash + 1, i - 1, 0)
                                + helper(boxes, i, right, countDash + 1);
                ans = Math.max(temp, ans);
            }
        }
        dp[left][right][count] = ans;
        return ans;
    }
}
```