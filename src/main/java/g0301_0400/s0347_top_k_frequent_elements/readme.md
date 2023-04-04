[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 347\. Top K Frequent Elements

Medium

Given an integer array `nums` and an integer `k`, return _the_ `k` _most frequent elements_. You may return the answer in **any order**.

**Example 1:**

**Input:** nums = [1,1,1,2,2,3], k = 2

**Output:** [1,2]

**Example 2:**

**Input:** nums = [1], k = 1

**Output:** [1]

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   `k` is in the range `[1, the number of unique elements in the array]`.
*   It is **guaranteed** that the answer is **unique**.

**Follow up:** Your algorithm's time complexity must be better than `O(n log n)`, where n is the array's size.

## Solution

```java
import java.util.Arrays;
import java.util.PriorityQueue;
import java.util.Queue;

public class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Arrays.sort(nums);
        // Min heap of <number, frequency>
        Queue<int[]> queue = new PriorityQueue<>(k + 1, (a, b) -> (a[1] - b[1]));
        // Filter with min heap
        int j = 0;
        for (int i = 0; i <= nums.length; i++) {
            if (i == nums.length || nums[i] != nums[j]) {
                queue.offer(new int[] {nums[j], i - j});
                if (queue.size() > k) {
                    queue.poll();
                }
                j = i;
            }
        }
        // Convert to int array
        int[] result = new int[k];
        for (int i = k - 1; i >= 0; i--) {
            result[i] = queue.poll()[0];
        }
        return result;
    }
}
```