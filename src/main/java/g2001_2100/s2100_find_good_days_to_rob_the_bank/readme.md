[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2100\. Find Good Days to Rob the Bank

Medium

You and a gang of thieves are planning on robbing a bank. You are given a **0-indexed** integer array `security`, where `security[i]` is the number of guards on duty on the <code>i<sup>th</sup></code> day. The days are numbered starting from `0`. You are also given an integer `time`.

The <code>i<sup>th</sup></code> day is a good day to rob the bank if:

*   There are at least `time` days before and after the <code>i<sup>th</sup></code> day,
*   The number of guards at the bank for the `time` days **before** `i` are **non-increasing**, and
*   The number of guards at the bank for the `time` days **after** `i` are **non-decreasing**.

More formally, this means day `i` is a good day to rob the bank if and only if `security[i - time] >= security[i - time + 1] >= ... >= security[i] <= ... <= security[i + time - 1] <= security[i + time]`.

Return _a list of **all** days **(0-indexed)** that are good days to rob the bank_. _The order that the days are returned in does **not** matter._

**Example 1:**

**Input:** security = [5,3,3,3,5,6,2], time = 2

**Output:** [2,3]

**Explanation:**

On day 2, we have security[0] >= security[1] >= security[2] <= security[3] <= security[4].

On day 3, we have security[1] >= security[2] >= security[3] <= security[4] <= security[5].

No other days satisfy this condition, so days 2 and 3 are the only good days to rob the bank. 

**Example 2:**

**Input:** security = [1,1,1,1,1], time = 0

**Output:** [0,1,2,3,4]

**Explanation:** Since time equals 0, every day is a good day to rob the bank, so return every day. 

**Example 3:**

**Input:** security = [1,2,3,4,5,6], time = 2

**Output:** []

**Explanation:**

No day has 2 days before it that have a non-increasing number of guards.

Thus, no day is a good day to rob the bank, so return an empty list. 

**Constraints:**

*   <code>1 <= security.length <= 10<sup>5</sup></code>
*   <code>0 <= security[i], time <= 10<sup>5</sup></code>

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<Integer> goodDaysToRobBank(int[] security, int time) {
        int n = security.length;
        // dec: # of non-increasing elements before [i]
        // inc: # of non-decreasing elements after [i]
        int[] dec = new int[n];
        int[] inc = new int[n];
        for (int i = 1; i < n; i++) {
            if (security[i] <= security[i - 1]) {
                dec[i] = dec[i - 1] + 1;
            }
            // no need for else, because array elements are inited as 0
        }
        for (int i = n - 2; i >= 0; i--) {
            if (security[i] <= security[i + 1]) {
                inc[i] = inc[i + 1] + 1;
            }
            // no need for else, because array elements are inited as 0
        }
        List<Integer> res = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            if (dec[i] >= time && inc[i] >= time) {
                res.add(i);
            }
        }
        return res;
    }
}
```