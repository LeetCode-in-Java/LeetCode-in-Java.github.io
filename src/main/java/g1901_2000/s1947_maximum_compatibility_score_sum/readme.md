[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1947\. Maximum Compatibility Score Sum

Medium

There is a survey that consists of `n` questions where each question's answer is either `0` (no) or `1` (yes).

The survey was given to `m` students numbered from `0` to `m - 1` and `m` mentors numbered from `0` to `m - 1`. The answers of the students are represented by a 2D integer array `students` where `students[i]` is an integer array that contains the answers of the <code>i<sup>th</sup></code> student (**0-indexed**). The answers of the mentors are represented by a 2D integer array `mentors` where `mentors[j]` is an integer array that contains the answers of the <code>j<sup>th</sup></code> mentor (**0-indexed**).

Each student will be assigned to **one** mentor, and each mentor will have **one** student assigned to them. The **compatibility score** of a student-mentor pair is the number of answers that are the same for both the student and the mentor.

*   For example, if the student's answers were `[1, 0, 1]` and the mentor's answers were `[0, 0, 1]`, then their compatibility score is 2 because only the second and the third answers are the same.

You are tasked with finding the optimal student-mentor pairings to **maximize** the **sum of the compatibility scores**.

Given `students` and `mentors`, return _the **maximum compatibility score sum** that can be achieved._

**Example 1:**

**Input:** students = \[\[1,1,0],[1,0,1],[0,0,1]], mentors = \[\[1,0,0],[0,0,1],[1,1,0]]

**Output:** 8

**Explanation:** We assign students to mentors in the following way: 

- student 0 to mentor 2 with a compatibility score of 3. 

- student 1 to mentor 0 with a compatibility score of 2. 
  
student 2 to mentor 1 with a compatibility score of 3. 

The compatibility score sum is 3 + 2 + 3 = 8.

**Example 2:**

**Input:** students = \[\[0,0],[0,0],[0,0]], mentors = \[\[1,1],[1,1],[1,1]]

**Output:** 0

**Explanation:** The compatibility score of any student-mentor pair is 0.

**Constraints:**

*   `m == students.length == mentors.length`
*   `n == students[i].length == mentors[j].length`
*   `1 <= m, n <= 8`
*   `students[i][k]` is either `0` or `1`.
*   `mentors[j][k]` is either `0` or `1`.

## Solution

```java
import java.util.Arrays;

public class Solution {
    private int[][] dp;
    private int m;
    private int[][] memo;

    public int maxCompatibilitySum(int[][] students, int[][] mentors) {
        int n = students[0].length;
        m = students.length;
        dp = new int[m][m];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < m; j++) {
                int tmp = 0;
                for (int k = 0; k < n; k++) {
                    tmp += (students[i][k] == mentors[j][k] ? 1 : 0);
                }
                dp[i][j] = tmp;
            }
        }
        memo = new int[m][(1 << m) + 1];
        for (int[] x : memo) {
            Arrays.fill(x, -1);
        }
        return dp(0, 0);
    }

    private int dp(int idx, int mask) {
        if (idx == m) {
            return 0;
        }
        if (memo[idx][mask] != -1) {
            return memo[idx][mask];
        }
        int ans = 0;
        for (int i = 0; i < m; i++) {
            if ((mask & (1 << i)) == 0) {
                ans = Math.max(ans, dp[idx][i] + dp(idx + 1, mask | (1 << i)));
            }
        }
        memo[idx][mask] = ans;
        return ans;
    }
}
```