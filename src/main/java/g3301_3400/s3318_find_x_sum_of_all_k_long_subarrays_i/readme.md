[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3318\. Find X-Sum of All K-Long Subarrays I

Easy

You are given an array `nums` of `n` integers and two integers `k` and `x`.

The **x-sum** of an array is calculated by the following procedure:

*   Count the occurrences of all elements in the array.
*   Keep only the occurrences of the top `x` most frequent elements. If two elements have the same number of occurrences, the element with the **bigger** value is considered more frequent.
*   Calculate the sum of the resulting array.

**Note** that if an array has less than `x` distinct elements, its **x-sum** is the sum of the array.

Return an integer array `answer` of length `n - k + 1` where `answer[i]` is the **x-sum** of the subarray `nums[i..i + k - 1]`.

**Example 1:**

**Input:** nums = [1,1,2,2,3,4,2,3], k = 6, x = 2

**Output:** [6,10,12]

**Explanation:**

*   For subarray `[1, 1, 2, 2, 3, 4]`, only elements 1 and 2 will be kept in the resulting array. Hence, `answer[0] = 1 + 1 + 2 + 2`.
*   For subarray `[1, 2, 2, 3, 4, 2]`, only elements 2 and 4 will be kept in the resulting array. Hence, `answer[1] = 2 + 2 + 2 + 4`. Note that 4 is kept in the array since it is bigger than 3 and 1 which occur the same number of times.
*   For subarray `[2, 2, 3, 4, 2, 3]`, only elements 2 and 3 are kept in the resulting array. Hence, `answer[2] = 2 + 2 + 2 + 3 + 3`.

**Example 2:**

**Input:** nums = [3,8,7,8,7,5], k = 2, x = 2

**Output:** [11,15,15,15,12]

**Explanation:**

Since `k == x`, `answer[i]` is equal to the sum of the subarray `nums[i..i + k - 1]`.

**Constraints:**

*   `1 <= n == nums.length <= 50`
*   `1 <= nums[i] <= 50`
*   `1 <= x <= k <= nums.length`

## Solution

```java
import java.util.HashMap;
import java.util.Map;
import java.util.PriorityQueue;

public class Solution {
    private static class Pair {
        int num;
        int freq;

        Pair(int num, int freq) {
            this.num = num;
            this.freq = freq;
        }
    }

    public int[] findXSum(int[] nums, int k, int x) {
        int n = nums.length;
        int[] ans = new int[n - k + 1];
        for (int i = 0; i < n - k + 1; i++) {
            HashMap<Integer, Integer> map = new HashMap<>();
            PriorityQueue<Pair> pq =
                    new PriorityQueue<>(
                            (a, b) -> {
                                if (a.freq == b.freq) {
                                    return b.num - a.num;
                                }
                                return b.freq - a.freq;
                            });
            for (int j = i; j < i + k; j++) {
                map.put(nums[j], map.getOrDefault(nums[j], 0) + 1);
            }
            for (Map.Entry<Integer, Integer> entry : map.entrySet()) {
                pq.add(new Pair(entry.getKey(), entry.getValue()));
            }
            int count = x;
            int sum = 0;
            while (!pq.isEmpty() && count > 0) {
                Pair pair = pq.remove();
                sum += pair.num * pair.freq;
                count--;
            }
            ans[i] = sum;
        }
        return ans;
    }
}
```