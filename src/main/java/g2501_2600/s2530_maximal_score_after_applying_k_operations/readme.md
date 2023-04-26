[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2530\. Maximal Score After Applying K Operations

Medium

You are given a **0-indexed** integer array `nums` and an integer `k`. You have a **starting score** of `0`.

In one **operation**:

1.  choose an index `i` such that `0 <= i < nums.length`,
2.  increase your **score** by `nums[i]`, and
3.  replace `nums[i]` with `ceil(nums[i] / 3)`.

Return _the maximum possible **score** you can attain after applying **exactly**_ `k` _operations_.

The ceiling function `ceil(val)` is the least integer greater than or equal to `val`.

**Example 1:**

**Input:** nums = [10,10,10,10,10], k = 5

**Output:** 50

**Explanation:** Apply the operation to each array element exactly once. The final score is 10 + 10 + 10 + 10 + 10 = 50.

**Example 2:**

**Input:** nums = [1,10,3,3,3], k = 3

**Output:** 17

**Explanation:** You can do the following operations: 

Operation 1: Select i = 1, so nums becomes [1,**<ins>4</ins>**,3,3,3]. Your score increases by 10.

Operation 2: Select i = 1, so nums becomes [1,**<ins>2</ins>**,3,3,3]. Your score increases by 4. 

Operation 3: Select i = 2, so nums becomes [1,1,<ins>**1**</ins>,3,3]. Your score increases by 3. 

The final score is 10 + 4 + 3 = 17.

**Constraints:**

*   <code>1 <= nums.length, k <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>

## Solution

```java
import java.util.Arrays;
import java.util.Collections;
import java.util.PriorityQueue;

public class Solution {
    public long maxKelements(int[] nums, int k) {
        PriorityQueue<Integer> p = new PriorityQueue<>(Collections.reverseOrder());
        Arrays.sort(nums);
        for (int i = 0; i < nums.length; i++) {
            p.add(nums[nums.length - i - 1]);
        }
        long score = 0;
        while (k != 0) {
            int v1 = p.poll();
            score += v1;
            int v2 = (int) Math.ceil((double) v1 / 3);
            p.add(v2);
            k--;
        }
        return score;
    }
}
```