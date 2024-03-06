[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3040\. Maximum Number of Operations With the Same Score II

Medium

Given an array of integers called `nums`, you can perform **any** of the following operation while `nums` contains **at least** `2` elements:

*   Choose the first two elements of `nums` and delete them.
*   Choose the last two elements of `nums` and delete them.
*   Choose the first and the last elements of `nums` and delete them.

The **score** of the operation is the sum of the deleted elements.

Your task is to find the **maximum** number of operations that can be performed, such that **all operations have the same score**.

Return _the **maximum** number of operations possible that satisfy the condition mentioned above_.

**Example 1:**

**Input:** nums = [3,2,1,2,3,4]

**Output:** 3

**Explanation:** We perform the following operations: 
- Delete the first two elements, with score 3 + 2 = 5, nums = [1,2,3,4]. 
- Delete the first and the last elements, with score 1 + 4 = 5, nums = [2,3].
- Delete the first and the last elements, with score 2 + 3 = 5, nums = []. 

We are unable to perform any more operations as nums is empty.

**Example 2:**

**Input:** nums = [3,2,6,1,4]

**Output:** 2

**Explanation:** We perform the following operations: 
- Delete the first two elements, with score 3 + 2 = 5, nums = [6,1,4]. 
- Delete the last two elements, with score 1 + 4 = 5, nums = [6]. 

It can be proven that we can perform at most 2 operations.

**Constraints:**

*   `2 <= nums.length <= 2000`
*   `1 <= nums[i] <= 1000`

## Solution

```java
import java.util.HashMap;
import java.util.Map;
import java.util.Objects;

public class Solution {
    private int[] nums;

    private int maxOps = 1;

    private final Map<Pos, Integer> dp = new HashMap<>();

    private static class Pos {
        int start;
        int end;
        int sum;

        Pos(int start, int end, int sum) {
            this.start = start;
            this.end = end;
            this.sum = sum;
        }

        @Override
        public boolean equals(Object o) {
            if (o == null) {
                return false;
            }
            if (!(o instanceof Pos)) {
                return false;
            }
            return start == ((Pos) o).start && end == ((Pos) o).end && sum == ((Pos) o).sum;
        }

        @Override
        public int hashCode() {
            return Objects.hash(start, end, sum);
        }
    }

    public int maxOperations(int[] nums) {
        this.nums = nums;
        var length = nums.length;

        maxOperations(2, length - 1, nums[0] + nums[1], 1);
        maxOperations(0, length - 3, nums[length - 2] + nums[length - 1], 1);
        maxOperations(1, length - 2, nums[0] + nums[length - 1], 1);

        return maxOps;
    }

    private void maxOperations(int start, int end, int sum, int nOps) {
        if (start >= end) {
            return;
        }

        if ((((end - start) / 2) + nOps) < maxOps) {
            return;
        }

        Pos pos = new Pos(start, end, sum);
        Integer posNops = dp.get(pos);
        if (posNops != null && posNops >= nOps) {
            return;
        }
        dp.put(pos, nOps);
        if (nums[start] + nums[start + 1] == sum) {
            maxOps = Math.max(maxOps, nOps + 1);
            maxOperations(start + 2, end, sum, nOps + 1);
        }
        if (nums[end - 1] + nums[end] == sum) {
            maxOps = Math.max(maxOps, nOps + 1);
            maxOperations(start, end - 2, sum, nOps + 1);
        }
        if (nums[start] + nums[end] == sum) {
            maxOps = Math.max(maxOps, nOps + 1);
            maxOperations(start + 1, end - 1, sum, nOps + 1);
        }
    }
}
```