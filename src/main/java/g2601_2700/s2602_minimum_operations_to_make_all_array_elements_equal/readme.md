[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2602\. Minimum Operations to Make All Array Elements Equal

Medium

You are given an array `nums` consisting of positive integers.

You are also given an integer array `queries` of size `m`. For the <code>i<sup>th</sup></code> query, you want to make all of the elements of `nums` equal to `queries[i]`. You can perform the following operation on the array **any** number of times:

*   **Increase** or **decrease** an element of the array by `1`.

Return _an array_ `answer` _of size_ `m` _where_ `answer[i]` _is the **minimum** number of operations to make all elements of_ `nums` _equal to_ `queries[i]`.

**Note** that after each query the array is reset to its original state.

**Example 1:**

**Input:** nums = [3,1,6,8], queries = [1,5]

**Output:** [14,10]

**Explanation:** For the first query we can do the following operations: 
- Decrease nums[0] 2 times, so that nums = [1,1,6,8]. 
- Decrease nums[2] 5 times, so that nums = [1,1,1,8]. 
- Decrease nums[3] 7 times, so that nums = [1,1,1,1]. 

So the total number of operations for the first query is 2 + 5 + 7 = 14. 

For the second query we can do the following operations: 
- Increase nums[0] 2 times, so that nums = [5,1,6,8]. 
- Increase nums[1] 4 times, so that nums = [5,5,6,8]. 
- Decrease nums[2] 1 time, so that nums = [5,5,5,8]. 
- Decrease nums[3] 3 times, so that nums = [5,5,5,5]. 

So the total number of operations for the second query is 2 + 4 + 1 + 3 = 10.

**Example 2:**

**Input:** nums = [2,9,6,3], queries = [10]

**Output:** [20]

**Explanation:** We can increase each value in the array to 10. The total number of operations will be 8 + 1 + 4 + 7 = 20.

**Constraints:**

*   `n == nums.length`
*   `m == queries.length`
*   <code>1 <= n, m <= 10<sup>5</sup></code>
*   <code>1 <= nums[i], queries[i] <= 10<sup>9</sup></code>

## Solution

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class Solution {
    public List<Long> minOperations(int[] nums, int[] queries) {
        Arrays.sort(nums);
        long[] sum = new long[nums.length];
        sum[0] = nums[0];
        for (int i = 1; i < nums.length; ++i) {
            sum[i] = sum[i - 1] + nums[i];
        }
        List<Long> res = new ArrayList<>();
        for (int query : queries) {
            res.add(getOperations(sum, nums, query));
        }
        return res;
    }

    private long getOperations(long[] sum, int[] nums, int target) {
        long res = 0;
        int index = getIndex(nums, target);
        int rightCounts = nums.length - 1 - index;
        if (index > 0) {
            res += (long) index * target - sum[index - 1];
        }
        if (rightCounts > 0) {
            res += sum[nums.length - 1] - sum[index] - (long) rightCounts * target;
        }
        res += Math.abs(target - nums[index]);
        return res;
    }

    private int getIndex(int[] nums, int target) {
        int index = Arrays.binarySearch(nums, target);
        if (index < 0) {
            index = -(index + 1);
        }
        if (index == nums.length) {
            --index;
        }
        return index;
    }
}
```