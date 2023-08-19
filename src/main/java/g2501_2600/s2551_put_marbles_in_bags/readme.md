[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2551\. Put Marbles in Bags

Hard

You have `k` bags. You are given a **0-indexed** integer array `weights` where `weights[i]` is the weight of the <code>i<sup>th</sup></code> marble. You are also given the integer `k.`

Divide the marbles into the `k` bags according to the following rules:

*   No bag is empty.
*   If the <code>i<sup>th</sup></code> marble and <code>j<sup>th</sup></code> marble are in a bag, then all marbles with an index between the <code>i<sup>th</sup></code> and <code>j<sup>th</sup></code> indices should also be in that same bag.
*   If a bag consists of all the marbles with an index from `i` to `j` inclusively, then the cost of the bag is `weights[i] + weights[j]`.

The **score** after distributing the marbles is the sum of the costs of all the `k` bags.

Return _the **difference** between the **maximum** and **minimum** scores among marble distributions_.

**Example 1:**

**Input:** weights = [1,3,5,1], k = 2

**Output:** 4

**Explanation:** 

The distribution [1],[3,5,1] results in the minimal score of (1+1) + (3+1) = 6. 

The distribution [1,3],[5,1], results in the maximal score of (1+3) + (5+1) = 10.

Thus, we return their difference 10 - 6 = 4.

**Example 2:**

**Input:** weights = [1, 3], k = 2

**Output:** 0

**Explanation:** The only distribution possible is [1],[3]. Since both the maximal and minimal score are the same, we return 0.

**Constraints:**

*   <code>1 <= k <= weights.length <= 10<sup>5</sup></code>
*   <code>1 <= weights[i] <= 10<sup>9</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    public long putMarbles(int[] weights, int k) {
        long minAns = weights[0] + (long) weights[weights.length - 1];
        long maxAns = weights[0] + (long) weights[weights.length - 1];
        long[] arr = new long[weights.length - 1];
        for (int i = 1; i < weights.length; i++) {
            arr[i - 1] = weights[i] + (long) weights[i - 1];
        }
        Arrays.sort(arr);
        for (int i = 0; i < k - 1; i++) {
            minAns = minAns + arr[i];
        }
        for (int i = arr.length - 1; i > arr.length - k; i--) {
            maxAns = maxAns + arr[i];
        }
        return maxAns - minAns;
    }
}
```