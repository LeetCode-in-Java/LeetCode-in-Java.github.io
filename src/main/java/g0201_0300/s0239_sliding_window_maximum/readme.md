[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 239\. Sliding Window Maximum

Hard

You are given an array of integers `nums`, there is a sliding window of size `k` which is moving from the very left of the array to the very right. You can only see the `k` numbers in the window. Each time the sliding window moves right by one position.

Return _the max sliding window_.

**Example 1:**

**Input:** nums = [1,3,-1,-3,5,3,6,7], k = 3

**Output:** [3,3,5,5,6,7]

**Explanation:**

    Window position        Max
    ---------------       -----
    [1 3 -1] -3 5 3 6 7     3
    1 [3 -1 -3] 5 3 6 7     3
    1 3 [-1 -3 5] 3 6 7     5
    1 3 -1 [-3 5 3] 6 7     5
    1 3 -1 -3 [5 3 6] 7     6
    1 3 -1 -3 5 [3 6 7]     7 

**Example 2:**

**Input:** nums = [1], k = 1

**Output:** [1] 

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>-10<sup>4</sup> <= nums[i] <= 10<sup>4</sup></code>
*   `1 <= k <= nums.length`

## Solution

```java
import java.util.LinkedList;

public class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        int n = nums.length;
        int[] res = new int[n - k + 1];
        int x = 0;
        LinkedList<Integer> dq = new LinkedList<>();
        int i = 0;
        int j = 0;
        while (j < nums.length) {
            while (!dq.isEmpty() && dq.peekLast() < nums[j]) {
                dq.pollLast();
            }
            dq.addLast(nums[j]);
            if (j - i + 1 == k) {
                res[x] = dq.peekFirst();
                ++x;
                if (dq.peekFirst() == nums[i]) {
                    dq.pollFirst();
                }
                ++i;
            }
            ++j;
        }
        return res;
    }
}
```

**Time Complexity (Big O Time):**

The time complexity of this program is primarily determined by the two nested loops:

1. The outer `while` loop runs from `j = 0` to `j = n - 1`, where `n` is the length of the input array `nums`. Therefore, it has a time complexity of O(n).

2. Inside the outer loop, there is an inner `while` loop that removes elements from the back of the deque (`dq`). In the worst case, this inner loop can run up to `k` times, where `k` is the size of the sliding window. So, the inner `while` loop contributes O(k) to the time complexity.

Overall, the time complexity of the program is O(n * k) in the worst case, where `n` is the length of the input array, and `k` is the size of the sliding window. In practice, when `k` is much smaller than `n`, the algorithm approaches O(n).

**Space Complexity (Big O Space):**

1. The program uses an integer array `res` to store the results, which has a length of `n - k + 1`. Therefore, the space complexity of the `res` array is O(n).

2. The program uses a `LinkedList<Integer>` named `dq` as a deque to store elements. In the worst case, the deque can store up to `k` elements at any given time. Therefore, the space complexity of the deque is O(k).

3. The program uses a few integer variables (`x`, `i`, `j`) that consume a constant amount of space regardless of the input size. These variables do not depend on the length of the input array.

Overall, the space complexity of the program is O(n + k), where `n` is the length of the input array, and `k` is the size of the sliding window. In practice, when `k` is much smaller than `n`, the space complexity approaches O(n).

In summary, the provided program has a time complexity of O(n * k) and a space complexity of O(n + k), but in practice, it can be more efficient when `k` is small compared to `n`.
