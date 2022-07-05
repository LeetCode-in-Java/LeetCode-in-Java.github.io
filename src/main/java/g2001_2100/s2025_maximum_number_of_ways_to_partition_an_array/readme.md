[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2025\. Maximum Number of Ways to Partition an Array

Hard

You are given a **0-indexed** integer array `nums` of length `n`. The number of ways to **partition** `nums` is the number of `pivot` indices that satisfy both conditions:

*   `1 <= pivot < n`
*   `nums[0] + nums[1] + ... + nums[pivot - 1] == nums[pivot] + nums[pivot + 1] + ... + nums[n - 1]`

You are also given an integer `k`. You can choose to change the value of **one** element of `nums` to `k`, or to leave the array **unchanged**.

Return _the **maximum** possible number of ways to **partition**_ `nums` _to satisfy both conditions after changing **at most** one element_.

**Example 1:**

**Input:** nums = [2,-1,2], k = 3

**Output:** 1

**Explanation:** One optimal approach is to change nums[0] to k. The array becomes [**3**,-1,2]. 

There is one way to partition the array: 

- For pivot = 2, we have the partition [3,-1 \| 2]: 3 + -1 == 2.

**Example 2:**

**Input:** nums = [0,0,0], k = 1

**Output:** 2

**Explanation:** The optimal approach is to leave the array unchanged. 

There are two ways to partition the array: 

- For pivot = 1, we have the partition [0 \| 0,0]: 0 == 0 + 0. 

- For pivot = 2, we have the partition [0,0 \| 0]: 0 + 0 == 0.

**Example 3:**

**Input:** nums = [22,4,-25,-20,-15,15,-16,7,19,-10,0,-13,-14], k = -33

**Output:** 4

**Explanation:** One optimal approach is to change nums[2] to k. The array becomes [22,4,**\-33**,-20,-15,15,-16,7,19,-10,0,-13,-14]. 

There are four ways to partition the array.

**Constraints:**

*   `n == nums.length`
*   <code>2 <= n <= 10<sup>5</sup></code>
*   <code>-10<sup>5</sup> <= k, nums[i] <= 10<sup>5</sup></code>

## Solution

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class Solution {
    public int waysToPartition(int[] nums, int k) {
        int n = nums.length;
        long[] ps = new long[n];
        ps[0] = nums[0];
        for (int i = 1; i < n; i++) {
            ps[i] = ps[i - 1] + nums[i];
        }
        Map<Long, ArrayList<Integer>> partDiffs = new HashMap<>();
        int maxWays = 0;
        for (int i = 1; i < n; i++) {
            long partL = ps[i - 1];
            long partR = ps[n - 1] - partL;
            long partDiff = partR - partL;
            if (partDiff == 0) {
                maxWays++;
            }
            ArrayList<Integer> idxSet =
                    partDiffs.computeIfAbsent(partDiff, k1 -> new ArrayList<>());
            idxSet.add(i);
        }
        for (int j = 0; j < n; j++) {
            int ways = 0;
            long newDiff = (long) k - nums[j];
            ArrayList<Integer> leftList = partDiffs.get(newDiff);
            if (leftList != null) {
                int i = upperBound(leftList, j);
                ways += leftList.size() - i;
            }
            ArrayList<Integer> rightList = partDiffs.get(-newDiff);
            if (rightList != null) {
                int i = upperBound(rightList, j);
                ways += i;
            }
            maxWays = Math.max(ways, maxWays);
        }
        return maxWays;
    }

    private int upperBound(List<Integer> arr, int val) {
        int ans = -1;
        int n = arr.size();
        int l = 0;
        int r = n;
        while (l <= r) {
            int mid = l + (r - l) / 2;
            if (mid == n) {
                return n;
            }
            if (arr.get(mid) > val) {
                ans = mid;
                r = mid - 1;
            } else {
                l = mid + 1;
            }
        }
        return ans;
    }
}
```