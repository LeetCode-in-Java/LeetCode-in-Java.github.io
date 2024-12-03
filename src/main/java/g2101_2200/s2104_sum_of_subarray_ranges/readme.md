[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2104\. Sum of Subarray Ranges

Medium

You are given an integer array `nums`. The **range** of a subarray of `nums` is the difference between the largest and smallest element in the subarray.

Return _the **sum of all** subarray ranges of_ `nums`_._

A subarray is a contiguous **non-empty** sequence of elements within an array.

**Example 1:**

**Input:** nums = [1,2,3]

**Output:** 4

**Explanation:** The 6 subarrays of nums are the following: 

[1], range = largest - smallest = 1 - 1 = 0 

[2], range = 2 - 2 = 0 

[3], range = 3 - 3 = 0 

[1,2], range = 2 - 1 = 1 

[2,3], range = 3 - 2 = 1 

[1,2,3], range = 3 - 1 = 2 

So the sum of all ranges is 0 + 0 + 0 + 1 + 1 + 2 = 4.

**Example 2:**

**Input:** nums = [1,3,3]

**Output:** 4

**Explanation:** The 6 subarrays of nums are the following: 

[1], range = largest - smallest = 1 - 1 = 0 

[3], range = 3 - 3 = 0 

[3], range = 3 - 3 = 0 

[1,3], range = 3 - 1 = 2 

[3,3], range = 3 - 3 = 0 

[1,3,3], range = 3 - 1 = 2 

So the sum of all ranges is 0 + 0 + 0 + 2 + 0 + 2 = 4.

**Example 3:**

**Input:** nums = [4,-2,-3,4,1]

**Output:** 59

**Explanation:** The sum of all subarray ranges of nums is 59.

**Constraints:**

*   `1 <= nums.length <= 1000`
*   <code>-10<sup>9</sup> <= nums[i] <= 10<sup>9</sup></code>

**Follow-up:** Could you find a solution with `O(n)` time complexity?

## Solution

```java
import java.util.ArrayDeque;
import java.util.Deque;

public class Solution {
    public long subArrayRanges(int[] nums) {
        int n = nums.length;
        long sum = 0;
        Deque<Integer> q = new ArrayDeque<>();

        q.add(-1);
        for (int i = 0; i <= n; i++) {
            while (q.peekLast() != -1 && (i == n || nums[q.peekLast()] <= nums[i])) {
                int cur = q.removeLast();
                int left = q.peekLast();
                int right = i;
                sum += (long) (cur - left) * (right - cur) * nums[cur];
            }
            q.add(i);
        }

        q.clear();
        q.add(-1);
        for (int i = 0; i <= n; i++) {
            while (q.peekLast() != -1 && (i == n || nums[q.peekLast()] >= nums[i])) {
                int cur = q.removeLast();
                int left = q.peekLast();
                int right = i;
                sum -= (long) (cur - left) * (right - cur) * nums[cur];
            }
            q.add(i);
        }
        return sum;
    }
}
```