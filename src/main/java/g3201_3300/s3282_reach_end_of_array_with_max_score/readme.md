[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3282\. Reach End of Array With Max Score

Medium

You are given an integer array `nums` of length `n`.

Your goal is to start at index `0` and reach index `n - 1`. You can only jump to indices **greater** than your current index.

The score for a jump from index `i` to index `j` is calculated as `(j - i) * nums[i]`.

Return the **maximum** possible **total score** by the time you reach the last index.

**Example 1:**

**Input:** nums = [1,3,1,5]

**Output:** 7

**Explanation:**

First, jump to index 1 and then jump to the last index. The final score is `1 * 1 + 2 * 3 = 7`.

**Example 2:**

**Input:** nums = [4,3,1,3,2]

**Output:** 16

**Explanation:**

Jump directly to the last index. The final score is `4 * 4 = 16`.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```java
import java.util.List;

public class Solution {
    public long findMaximumScore(List<Integer> nums) {
        long res = 0;
        long ma = 0;
        for (int num : nums) {
            res += ma;
            ma = Math.max(ma, num);
        }
        return res;
    }
}
```