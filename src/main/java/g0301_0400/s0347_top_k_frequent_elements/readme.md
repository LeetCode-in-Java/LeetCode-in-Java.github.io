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

**Time Complexity (Big O Time):**

1. Sorting the `nums` array using `Arrays.sort(nums)` takes O(n*log(n)), where 'n' is the length of the input array.
2. The program then iterates through the sorted array 'nums' once. Inside the loop, it checks for distinct elements and counts their frequencies. This loop takes O(n) time.
3. The program uses a priority queue (`queue`) to keep track of the top k frequent elements. Adding an element to the priority queue takes O(log(k)) time, and it's done for each unique element in the input array.
4. After the loop, the program constructs the result array by extracting elements from the priority queue. This step takes O(k*log(k)), as it involves removing k elements from the priority queue.

Considering the dominant operations, the overall time complexity of the program is O(n*log(n)) due to the sorting step.

**Space Complexity (Big O Space):**

1. The program uses a priority queue (`queue`) to store the distinct elements along with their frequencies. The maximum size of this priority queue can be 'k + 1' (to ensure 'k' elements are retained at most).
2. The program uses an integer array `result` of size 'k' to store the top k frequent elements.
3. Other than these data structures, the space complexity is dominated by constant space used for variables and indices.

Therefore, the overall space complexity of the program is O(k) because the priority queue dominates the space complexity, and 'k' is the maximum size it can have.

In summary, the provided program has a time complexity of O(n*log(n)) and a space complexity of O(k), where 'n' is the length of the input array, and 'k' is the value of 'k' specified as an argument.
